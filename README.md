# LangChain_tutorial
This tutorial covers how to use LangChain for Agents and RAG. Local models from Ollama are employed.


## OLLAMA Tests

This folder includes basic usage examples as tutorials of Ollama and its interactions with LangChain.

Aim of this tutorial is to examine the extent to which tests can be performed locally on agents, RAG, GraphRAG, etc., using tools like LangChain and models from Ollama.

### Overall approach

The overall approach that this tutorial is using Ollama in python. To this end, the Ollama server needs to start in a new screen

```
ollama serve
```

Then we can download a model, e.g.

```
ollama pull phi3:3.8b
```

And use Ollama in python - check links below.


### Interesting links:

Ollama models:

https://ollama.com/library

Compatibility with tools:

https://ollama.com/search?c=tools

Ollama quick start:

https://docs.ollama.com/quickstart#python

LangChain Ollama integration

Chat models:

https://docs.langchain.com/oss/python/integrations/chat/ollama

Embeddings:

https://docs.langchain.com/oss/python/integrations/text_embedding/ollama