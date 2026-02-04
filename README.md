# LangChain_tutorial

This tutorial covers the basics on how to use LangChain for Agents and RAG, while it covers how to use LangChain for constructing Knowledge Graphs from text. The tutorial digresses a bit into the GraphRAG territory, introducing shortly Neo4J and showing some very basic graph-language interactions. Additional tutorials will follow that will cover GraphRAG and Agents in more detail, however, this tutorial is a good entry point for people who want to get a firm understanding on the opportunities and limitations of the aforementioned approaches.

Aim of this tutorial is to employ only local models, for no-cost testing and security. To this end, Ollama models that run locally on machines with very small GPUs - or without GPUs at all, though much slower.

## Suggested system setup

For running the tutorials, you will need to have python, conda for creating and managing python environments and VSCode for reading/writing and running the code. Below information is provided for installing those components. Please consult ChatGPT or a chat service of your choice if you find trouble to apply those steps for your system.

It is strongly suggested that Ubuntu, MacOS or WSL (Windows Subsystem for Linux) is used, using VSCode for installing the dependencies and running the notebooks.

Before you start, please make sure that you have VSCode and conda (miniconda is suggested) up and running (in Windows, these tools are installed in WSL).

For information about how to install VSCode (for WSL follow instructions for Linux):

https://code.visualstudio.com/docs/setup/linux

For information about how to install miniconda (for WSL follow instructions for Linux):

https://www.anaconda.com/docs/getting-started/miniconda/install

To install Ollama on Ubuntu (or WSL), open a terminal and copy-past:

`curl -fsSL https://ollama.com/install.sh | sh`

In WSL, before you install Ollama, you may need to install:

`sudo apt-get install zstd`

## Conda environment

It is suggested to create a conda environment for running this tutorial. If the name of the environment is `ollama`, you can create this environment like so:

`conda create -n ollama python=3.12`

Then you can install all the dependencies by navigating to the folder of the tutorial through the terminal and doing:

`pip install -r requirements.txt`

## OLLAMA Tests

This folder includes basic usage examples as tutorials of Ollama and its interactions with LangChain.

Aim of this tutorial is to examine the extent to which tests can be performed locally on agents, RAG, GraphRAG, etc., using tools like LangChain and models from Ollama.

### Ollama server

The overall approach that this tutorial is using Ollama in python. To this end, the Ollama server needs to start in a new screen

```
ollama serve
```

Note: the first time you install Ollama, it might start automatically, so `ollama serve` may produce an error that the port is already used. This may be the case in your installation any time you start your computer (Ollama may start serving automatically).

Then we can download a model, e.g.

```
ollama pull qwen3-vl:4b
```

This model is actually the one we will be using throughout the tutorial - you may try others for experimentation. Running Ollama with this model requires a bit lower than 5GB of GPU RAM. It will run without GPU, but it will be taking more time to run.

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

The complete Sherlock Holmes - Stories in Plain text format
Single short stories are available at the end of the page

https://sherlock-holm.es/ascii/