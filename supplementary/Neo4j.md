# Neo4j: A Practical Introduction for Knowledge Graphs

## What is Neo4j?

**Neo4j** is a **graph database system**, not just a plotting or visualization library.

It consists of three tightly integrated parts:

* **Graph database**
  Stores data as **nodes** and **relationships**, optimized for graph traversal and querying.
* **Query language (Cypher)**
  A declarative language designed specifically for graphs (e.g. pattern matching).
* **Browser-based visualization tool**
  An interactive UI for exploring, querying, and debugging graphs.

### How Neo4j differs from plotting tools (e.g. Plotly, NetworkX)

| Plotting tools         | Neo4j                    |
| ---------------------- | ------------------------ |
| Visualization-only     | Persistent database      |
| Static or client-side  | Server-based             |
| No query language      | Cypher for graph queries |
| No schema awareness    | Schema + metadata        |
| No traversal semantics | Native graph traversal   |

Neo4j is best thought of as **infrastructure**, not a plotting library.

---

## Execution model and security

* Neo4j runs as a **server process**
* Even for local use, it:

  * listens on ports
  * manages persistent data
  * enforces authentication
* Therefore, a **password is required**

  * this password protects the *database*, **not the operating system**
  * it is not a one-time startup password

Typical usage is:

* local execution
* SSH access
* port forwarding for browser visualization

---

## Installation on Ubuntu (minimal setup)

### 1. Install Java (required)

```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
```

### 2. Install Neo4j (Community Edition)

```bash
wget -O - https://debian.neo4j.com/neotechnology.gpg.key | sudo apt-key add -
echo 'deb https://debian.neo4j.com stable 5' | sudo tee /etc/apt/sources.list.d/neo4j.list
sudo apt update
sudo apt install -y neo4j
```

### 3. Start Neo4j

```bash
sudo systemctl enable neo4j
sudo systemctl start neo4j
```

### 4. Set initial password

```bash
sudo neo4j-admin dbms set-initial-password YOUR_PASSWORD
```

---

## Using Neo4j with Python and LangChain

LangChain provides a Python interface (`Neo4jGraph`) that allows:

* sending graphs to Neo4j
* schema introspection
* querying and reasoning over graphs
* visualization via Neo4j Browser

### Python connection example

```python
from langchain_neo4j import Neo4jGraph

graph = Neo4jGraph(
    url="bolt://localhost:7687",
    username="neo4j",
    password="YOUR_PASSWORD"
)
```

At this point, `graph` **is the database connection**.

---

## APOC: Why it is required

LangChain relies on **APOC** (Awesome Procedures on Cypher) to:

* introspect the graph schema
* discover node labels and relationships
* call `apoc.meta.data()`

Without APOC, LangChain cannot initialize the graph interface.

---

## Installing APOC (Neo4j 5.x)

### ⚠️ Critical rule: version matching

> **APOC version must exactly match the Neo4j version (major + minor).**

Check your Neo4j version:

```bash
neo4j --version
```

Example:

```
neo4j 5.13.0
```

### Download matching APOC Core JAR

```bash
wget https://github.com/neo4j/apoc/releases/download/5.13.0/apoc-5.13.0-core.jar
```

### Move to plugins directory

```bash
sudo mv apoc-5.13.0-core.jar /var/lib/neo4j/plugins/
sudo chown neo4j:neo4j /var/lib/neo4j/plugins/apoc-5.13.0-core.jar
```

---

## Neo4j configuration for APOC (Neo4j 5.x)

Edit the config file:

```bash
sudo nano /etc/neo4j/neo4j.conf
```

Add:

```ini
dbms.security.procedures.allowlist=apoc.*
dbms.security.procedures.unrestricted=apoc.*
```

Restart Neo4j:

```bash
sudo systemctl restart neo4j
```

Verify APOC:

```cypher
RETURN apoc.version();
CALL apoc.meta.data();
```

---

## Sending graphs from Python to Neo4j

### Example: adding a graph document

```python
from langchain_community.graphs.graph_document import GraphDocument
from langchain_core.documents import Document

doc = Document(page_content="Marie Curie was married to Pierre Curie.")

graph_doc = GraphDocument(
    nodes=[
        {"id": "Marie_Curie", "type": "Person"},
        {"id": "Pierre_Curie", "type": "Person"},
    ],
    relationships=[
        {
            "source": "Marie_Curie",
            "target": "Pierre_Curie",
            "type": "MARRIED_TO"
        }
    ],
    source=doc
)

graph.add_graph_documents([graph_doc])
```

---

## Accessing the Neo4j Browser

Neo4j exposes a web UI at:

```
http://localhost:7474
```

### If Neo4j runs on a remote server

You can use **SSH port forwarding**:

```bash
ssh -L 7474:localhost:7474 -L 7687:localhost:7687 user@server_ip
```

👉 Then open in your local browser:

```
http://localhost:7474
```

💡 **VS Code** supports port forwarding automatically when connected via Remote SSH.

---

## Visualizing graphs in the browser

### Show all nodes and relationships

```cypher
MATCH (n)-[r]->(m)
RETURN n, r, m;
```

### Show all nodes (including isolated ones)

```cypher
MATCH (n)
RETURN n;
```

### Clear the database (development use)

```cypher
MATCH (n)
DETACH DELETE n;
```

---

## Summary

* Neo4j is a **database + query language + visualization tool**
* It runs as a **server**, even for local use
* Authentication protects the database, not the OS
* LangChain integrates cleanly via `Neo4jGraph`
* APOC is **required** and must match Neo4j’s version
* Visualization happens via the Neo4j Browser
* SSH / VS Code port forwarding enables remote usage