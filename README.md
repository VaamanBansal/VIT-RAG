# Hybrid RAG-based Reddit Community Assistant

> A domain-specific Retrieval-Augmented Generation (RAG) system that transforms continuously evolving Reddit discussions into a searchable knowledge base using hybrid retrieval and local LLM inference.

## Overview

Reddit communities contain valuable information, but answers are often scattered across thousands of posts and deeply nested comment threads. Traditional keyword search struggles to retrieve semantically relevant discussions, while static RAG systems assume documents rarely change.

This project addresses these challenges by building a **dynamic hybrid retrieval pipeline** that continuously ingests Reddit discussions, incrementally updates its indexes, and generates grounded answers using a local Large Language Model.

The initial implementation focuses on **r/VIT**, but the architecture is designed to support any subreddit.

---

## Problem Statement

Community-driven platforms like Reddit present several retrieval challenges:

- Information is fragmented across multiple posts and comments.
- Duplicate questions receive different answers over time.
- New discussions continuously replace outdated information.
- Keyword search often misses semantically relevant discussions.
- Rebuilding embeddings for the entire dataset after every update is computationally expensive.

This project aims to solve these challenges through incremental indexing and hybrid retrieval.

---

## Solution

The application periodically fetches newly created Reddit posts and comments using the Reddit Data API.

Instead of rebuilding the entire database:

- only newly fetched content is processed,
- embeddings are generated for new chunks,
- FAISS indexes are updated incrementally,
- BM25 indexes are refreshed,
- retrieved candidates are reranked using a cross-encoder,
- and a local LLM generates grounded responses.

This keeps the knowledge base current while minimizing indexing costs.

---

## Architecture

```
                Reddit API
                     │
                     ▼
      Incremental Data Collection
                     │
                     ▼
      Text Cleaning & Chunking
                     │
        ┌────────────┴────────────┐
        ▼                         ▼
 Dense Embeddings             BM25 Index
 (BGE Small)                  (Sparse Search)
        │                         │
        ▼                         ▼
      FAISS                 Keyword Retrieval
        └────────────┬────────────┘
                     ▼
             Hybrid Retrieval
                     │
                     ▼
        Cross Encoder Reranker
                     │
                     ▼
            Local LLM (Qwen)
                     │
                     ▼
              Generated Answer
```

---

## Features

- Hybrid Retrieval (Dense + Sparse)
- FAISS Vector Search
- BM25 Keyword Retrieval
- Cross-Encoder Reranking
- Local LLM Inference using Ollama
- Incremental Index Updates
- Domain-Specific Knowledge Base
- Streamlit Interface
- Modular Data Ingestion Pipeline

---

## Tech Stack

### Backend

- Python
- LangChain

### Retrieval

- FAISS
- rank-bm25

### Embeddings

- Sentence Transformers
- BAAI/bge-small-en-v1.5

### Reranker

- BAAI/bge-reranker-v2-m3

### LLM

- Ollama
- Qwen 3

### Frontend

- Streamlit

### Data Source

- Reddit Data API

---

## Workflow

1. Fetch new Reddit posts and comments.
2. Clean and preprocess text.
3. Split discussions into semantic chunks.
4. Generate embeddings.
5. Update FAISS incrementally.
6. Update BM25 index.
7. Retrieve candidate passages using hybrid search.
8. Rerank candidates.
9. Generate a grounded answer using the local LLM.

---

## Future Work

- Multi-subreddit support
- Citation-based responses
- Conversation memory
- Temporal relevance scoring
- Duplicate discussion detection
- Automatic stale content removal
- Query classification
- Retrieval evaluation dashboard

---

## Current Status

🚧 **Work in Progress**

The retrieval pipeline and indexing architecture are currently under active development. Additional documentation, evaluation metrics, deployment instructions, and a live demo will be added as the project progresses.

---

## Author

**Vaaman Bansal**

B.Tech Computer Science Student  
VIT Vellore
