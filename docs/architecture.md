# Shared RAG Architecture

Every notebook in this repo follows the same pipeline. What changes between notebooks is the knowledge base and the system prompt/persona — not the underlying mechanics.

```
User's question
       │
       ▼
 Embed the question  ──────────────►  vector
       │
       ▼
 Search FAISS index for nearest k document vectors
       │
       ▼
 Retrieved facts (context)
       │
       ▼
 System prompt + context + question  ──────────►  LLM
       │
       ▼
 Grounded answer, in a domain-appropriate persona
```

## Components

- **Embedding model:** `sentence-transformers/all-MiniLM-L6-v2` — small and fast enough to run on a free-tier CPU, while still capturing enough semantic similarity for short-fact retrieval.
- **Vector store:** [FAISS](https://github.com/facebookresearch/faiss) (`faiss-cpu`), used locally and in-memory — no external vector database needed for a knowledge base this size.
- **LLM:** `microsoft/Phi-3.5-mini-instruct`, served via a Hugging Face Inference Endpoint through `langchain-huggingface`.
- **Orchestration:** LangChain's `Document`, `FAISS`, and `ChatHuggingFace`/`HuggingFaceEndpoint` wrappers.

## Design principles used across every notebook

1. **Atomic facts.** Each entry in a knowledge base is a single, self-contained sentence rather than a long paragraph. This keeps retrieval precise for a knowledge base of this size. For larger, real-world source documents, chunking (e.g. `RecursiveCharacterTextSplitter`) would replace this approach — see the exercises in each notebook.
2. **Explicit grounding instructions.** Every system prompt tells the model to answer only from the retrieved context, and to say when it doesn't have the answer rather than guessing.
3. **Domain-appropriate persona.** The tone of the system prompt is deliberately different per notebook — encouraging and simple for a farmer audience, calm and precise (with an explicit non-advice disclaimer) for a compliance audience. The architecture doesn't change; the framing does, because the stakes and audience do.
4. **Synthetic but realistic data, clearly labeled.** Every knowledge base in this repo is synthetic, written for teaching purposes, and explicitly flagged as needing verification against real sources before any production use.

## Extending the architecture

Ideas that apply across any notebook in this repo:

- Add `similarity_search_with_score` and a relevance threshold, so low-confidence retrievals are dropped instead of always returning `k` documents.
- Return the retrieved source facts alongside the generated answer, so a user can verify what the answer was grounded in.
- Swap the synthetic knowledge base for chunked real documents.
- Deploy the `rag()` function behind a Gradio interface for a shareable demo.
