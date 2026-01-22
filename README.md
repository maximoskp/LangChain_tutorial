# LangChain_tutorial
This tutorial covers how to use LangChain for Agents, RAG, Knowledge Graphs, GraphRAG and other subjects around these concepts. Local models from Ollama are employed for no-cost testing and security.

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

### Overall approach

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