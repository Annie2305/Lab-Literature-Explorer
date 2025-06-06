# Lab-Literature-Explorer
A private, offline-first RAG pipeline using a local LLM to analyze confidential academic theses, providing verifiable answers with clear source and author attribution.

This repository implements a Retrieval-Augmented Generation (RAG) pipeline that supports author-specific document retrieval and QA. It is designed for analyzing multiple academic thesis PDFs, automatically identifying each author's name, and generating precise, context-aware answers for research questions—grouped by author.

## Features

* **Automatic Author Extraction**: Parses PDF thesis files and automatically extracts the author name for each document.
* **Per-Author Chunking & Embedding**: Splits text into chunks with author metadata, stored in a vector database (ChromaDB).
* **Author-Specific Retrieval**: At query time, retrieves top relevant text chunks for each author separately.
* **Pointwise Reranking**: Improves answer quality by reranking retrieved chunks based on similarity to the question.
* **LLM-Driven Answers**: Uses a local LLM (via Ollama, e.g., Mistral) to generate structured, author-specific answers based on retrieved evidence.

## Folder Structure

```
.
├── docs/              # Put your PDF files here (theses to be ingested)
├── vector_db/         # Auto-generated ChromaDB vector store
├── rag_embedding.py      # Embedding pipeline: PDF → chunks → embeddings
├── rag_retrieval.py        # Query pipeline: user Q → retrieval → LLM answer
├── requirements.txt   # All required Python packages
└── README.md          # This file
```

## Getting Started

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Prepare Your Data

* Place all your thesis PDF files into the `./docs/` directory.

### 3. Ingest Documents

```bash
python rag_embedding.py
```

This will process all PDFs, extract text and author metadata, and build the vector database.

### 4. Run the Chat QA System

Make sure you have [Ollama](https://ollama.com/) installed and running, and have pulled the LLM you want to use (e.g., mistral-nemo):

```bash
ollama pull mistral-nemo
ollama serve  # Make sure Ollama server is running
python rag_retrieval.py
```

Then, enter your research question at the prompt. Answers will be structured by author.

## requirements.txt Example

```
langchain
langchain-chroma
langchain-huggingface
langchain-ollama
chromadb
pypdf
numpy
```

## Sample Usage

```bash
python rag_embedding.py
# ...wait for ingestion...
python rag_retrieval.py
# > Please enter your question: What 3D printing techniques did each author use?
```

## Notes

* Pointwise reranking and author filtering are implemented in the `rag_retrieval.py` file.
* You may adjust chunk size, similarity threshold, or LLM prompt for best results.
* For non-English or non-standard PDFs, you may need to tweak the author extraction regex in `rag_embedding.py`.

---

## License

MIT

