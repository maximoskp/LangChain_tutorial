# Managing Secrets with Environment Variables (`.env` + `.gitignore`)

When working with Neo4j (or any service that requires credentials), **passwords should never be hard-coded** in Python files or committed to Git repositories.

Instead, credentials are stored in **environment variables**, which are loaded at runtime.

---

## Why environment variables?

Environment variables allow you to:

* keep **secrets out of source code**
* safely share repositories publicly
* use different credentials on different machines
* avoid accidental password leaks on GitHub

This is especially important when:

* working with databases
* publishing tutorials
* collaborating with others

---

## The `.env` file (local, private)

A `.env` file is a simple text file that contains environment variables:

```env
NEO4J_URL=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=your_real_password_here
```

### Important properties of `.env`

* contains **real credentials**
* should exist **only on your local machine**
* must **never be committed to Git**

---

## Using `.gitignore` to protect secrets

To ensure `.env` is never committed, add it to `.gitignore`:

```gitignore
.env
```

This tells Git:

> “Ignore this file completely, even if it exists locally.”

As a result:

* your password stays private
* collaborators won’t see your credentials
* GitHub never stores your secrets

---

## The `.env.example` file (safe to share)

Since `.env` is ignored, collaborators need a **template** showing which variables are required.

This is the purpose of `.env.example`.

Example:

```env
# Neo4j Configuration
NEO4J_URL=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=your_password_here
```

### Why `.env.example` is useful

* contains **no real secrets**
* documents required environment variables
* can be safely committed to Git
* allows others to create their own `.env` file

Typical workflow:

1. Clone repository
2. Copy `.env.example` → `.env`
3. Fill in real credentials locally

---

## Loading environment variables in Python

The `python-dotenv` package loads variables from `.env` into the environment.

### Installation

```bash
pip install python-dotenv
```

### Example usage

```python
import os
from dotenv import load_dotenv
from langchain_neo4j import Neo4jGraph

# Load environment variables from .env
load_dotenv()

neo4j_url = os.getenv("NEO4J_URL", "bolt://localhost:7687")
neo4j_user = os.getenv("NEO4J_USER", "neo4j")
neo4j_password = os.getenv("NEO4J_PASSWORD")

if not neo4j_password:
    raise ValueError(
        "NEO4J_PASSWORD environment variable is not set. "
        "Please create a .env file with your credentials."
    )

graph = Neo4jGraph(
    url=neo4j_url,
    username=neo4j_user,
    password=neo4j_password
)
```

---

## What happens at runtime?

1. `.env` is read **locally**
2. Variables are injected into the process environment
3. Python accesses them via `os.getenv(...)`
4. Neo4j credentials are never hard-coded or committed

---

## What Git sees vs. what Python sees

| File                  | Visible to Git | Visible to Python |
| --------------------- | -------------- | ----------------- |
| `.env`                | ❌              | ✅                 |
| `.env.example`        | ✅              | ❌                 |
| Python source         | ✅              | ✅                 |
| Environment variables | ❌              | ✅                 |

---

## Best practices (recommended)

* ✔ Never commit `.env`
* ✔ Always provide `.env.example`
* ✔ Validate required variables at startup
* ✔ Use defaults only for non-sensitive values
* ✔ Treat passwords as disposable (easy to rotate)

---

## Summary

* Environment variables protect secrets from version control
* `.env` stores **real credentials locally**
* `.gitignore` ensures secrets are never committed
* `.env.example` documents required configuration
* `python-dotenv` bridges `.env` and Python

This pattern is **standard practice** for secure, reproducible research code and production systems alike.