# Retrieval Modes and GraphRAG in Neo4j

This section clarifies the different retrieval modes used in GraphRAG systems, how they are combined in practice, and how Neo4j supports them internally through different indexing mechanisms and retrievers.

---

## 1. Three Fundamental Modes of Retrieval

In modern knowledge-assisted LLM systems, there are **three complementary modes of retrieval**:

### 1.1 Vector (Semantic) Search  
This is the retrieval mode used in **classical RAG**.

- Text is embedded into vectors using an embedding model
- Queries are embedded in the same vector space
- Retrieval is based on **semantic similarity**
- Captures meaning, paraphrases, and conceptual similarity

**Strengths:**  
- Works even when exact keywords do not match  
- Robust to paraphrasing  

**Limitations:**  
- Can miss exact terms, names, numbers, or rare tokens  

---

### 1.2 Graph (Query-Based) Search  
This is **symbolic retrieval** over a structured knowledge graph.

- Nodes and relationships represent entities and facts
- Retrieval is done using **Cypher queries**
- Supports joins, constraints, and multi-hop reasoning

**Strengths:**  
- Precise, explainable, deterministic  
- Ideal for structured knowledge and reasoning  

**Limitations:**  
- Requires schema awareness  
- Not suitable for fuzzy or semantic matching  

---

### 1.3 Full-Text (Exact / Lexical) Search  
This is **keyword-based retrieval**, similar to classical information retrieval.

- Operates on raw text
- Uses tokenization and term statistics (e.g. BM25-style scoring)
- Matches exact words or phrases

**Strengths:**  
- Excellent for names, technical terms, identifiers  
- Deterministic and interpretable  

**Limitations:**  
- No semantic understanding  
- Sensitive to wording  

---

## 2. How GraphRAG Combines These Modes

**GraphRAG is not a single retrieval strategy**, but a family of approaches that combine the above modes.

In practice, the most effective GraphRAG pipelines follow this pattern:

> **Retrieve text first, then reason with the graph.**

Typical combinations include:

- **Vector → Graph**  
  Classical RAG retrieval followed by Cypher queries on the graph nodes linked to retrieved text.

- **Vector → Full-Text → Graph**  
  Semantic recall first, lexical filtering second, graph traversal last.

- **Full-Text → Vector → Graph**  
  Exact keyword filtering first, semantic ranking second, graph reasoning last.

The key idea is that **vector and full-text search identify relevant evidence**, while **graph queries provide structure, aggregation, and reasoning**.

---

## 3. How Neo4j Supports These Retrieval Modes

Neo4j supports these retrieval processes **in parallel**, within the same database, using different **indexing mechanisms**.

Conceptually, you can think of Neo4j as hosting:

### 3.1 The Knowledge Graph (Symbolic Layer)
- Nodes: entities (e.g. `Planet`, `Person`, `Company`)
- Relationships: facts and relations
- Accessed via **Cypher queries**

This is the graph used in query-based GraphRAG.

---

### 3.2 The Vector Index (Semantic Layer)
- Built over nodes that store embeddings (typically `Chunk` nodes)
- Enables **k-nearest-neighbor vector search**
- Used for semantic retrieval

---

### 3.3 The Full-Text Index (Lexical Layer)
- Built over text properties of nodes (again, usually `Chunk`)
- Enables keyword-based search
- Complements vector retrieval

> These are **not separate databases**, but **different indexes co-existing on the same Neo4j graph**, each optimized for a different retrieval mode.

---

## 4. Neo4j Retrievers and the Retrieval Modes They Combine

Neo4j provides four retrievers that progressively combine these capabilities.

---

### 4.1 `VectorRetriever`

**Retrieval modes used:**
- Vector search

**Requirements:**
- Vector index

**Description:**  
Performs pure semantic search over embeddings and returns the matched nodes with similarity scores.  
This is the closest analogue to classical RAG retrieval.

---

### 4.2 `VectorCypherRetriever`

**Retrieval modes used:**
- Vector search  
- Graph (Cypher) traversal

**Requirements:**
- Vector index  
- Knowledge graph

**Description:**  
Performs vector search first, then executes a Cypher retrieval query starting from the matched nodes.  
This is often the **first true GraphRAG retriever** users encounter.

---

### 4.3 `HybridRetriever`

**Retrieval modes used:**
- Vector search  
- Full-text search

**Requirements:**
- Vector index  
- Full-text index

**Description:**  
Combines semantic and lexical retrieval to improve recall and precision, but does not yet exploit graph structure.

---

### 4.4 `HybridCypherRetriever`

**Retrieval modes used:**
- Vector search  
- Full-text search  
- Graph (Cypher) traversal

**Requirements:**
- Vector index  
- Full-text index  
- Knowledge graph

**Description:**  
This retriever represents the **full GraphRAG retrieval pipeline**:
semantic + lexical retrieval followed by graph-based contextualization and reasoning.

---

## 5. Summary

- There are **three fundamental retrieval modes**: vector, graph, and full-text
- GraphRAG systems combine these modes in different ways
- Neo4j supports all three via **co-existing indexes on the same graph**
- The four Neo4j retrievers differ in **which modes they combine**
- `HybridCypherRetriever` represents the most complete and production-ready approach

Understanding these layers clarifies why GraphRAG is more than “RAG + a graph” — it is a structured retrieval and reasoning pipeline.
