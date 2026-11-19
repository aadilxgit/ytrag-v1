# ytrag-v1

A small Retrieval-Augmented Generation (RAG) pipeline built in Jupyter notebooks. It loads PDFs, chunks them, embeds the chunks, stores them in ChromaDB, and answers questions with a Groq-hosted LLM using the retrieved context.

## Pipeline

1. **Ingest**: load every PDF under `data/` with `PyPDFLoader`.
2. **Chunk**: `RecursiveCharacterTextSplitter` (1000 chars, 200 overlap).
3. **Embed**: `all-MiniLM-L6-v2` via `sentence-transformers`.
4. **Store**: persistent ChromaDB collection in `data/vector_store/`.
5. **Retrieve**: `RAGRetriever` runs similarity search with a score threshold.
6. **Generate**: `ChatGroq` (`openai/gpt-oss-120b`) answers from the retrieved context. `rag_simple` is the basic version; `rag_advanced` adds score filtering and optional context return.

## Flow

![RAG pipeline flowchart](docs/flow.png)

## Layout

```
notebook/
  document.ipynb      # LangChain Document structure + sample text files
  pdf_loader.ipynb    # full pipeline: ingest -> chunk -> embed -> store -> retrieve -> LLM
data/
  pdf/                # sample PDFs (attention, embeddings, object detection)
  text_files/         # sample .txt files (Python intro, machine learning)
  vector_store/       # generated ChromaDB files (git-ignored)
main.py               # placeholder entry point
pyproject.toml        # dependencies (uv)
requirements.txt      # same dependencies, pip-style
```

## Setup

Requires Python 3.13+ and a [Groq API key](https://console.groq.com/keys).

```bash
uv sync                      # or: pip install -r requirements.txt
cp .env.example .env         # then put your key in .env
```

Open `notebook/pdf_loader.ipynb` and run the cells top to bottom. The vector store is rebuilt on the first run.

## Status

The retrieval + generation pipeline works in the notebook. Not done yet: moving the notebook code into a Python module/CLI (`main.py` is still a stub) and any UI or YouTube ingestion.
