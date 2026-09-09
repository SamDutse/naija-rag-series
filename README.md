# Naija RAG Series

A small collection of walkthrough Jupyter notebooks teaching **Retrieval-Augmented Generation (RAG)** through real-world Nigerian problems — built as portfolio projects and as teaching material for my AI/Data Science students.

Each notebook uses the same core architecture (LangChain + FAISS + sentence-transformer embeddings + a Hugging Face-hosted LLM) applied to a different domain, so you can read them side by side and see how the *pipeline* stays constant while the *knowledge base, persona, and grounding concerns* change with the problem.

| Notebook | Problem | Knowledge base | Persona |
|---|---|---|---|
| [`AgriRAG_Naija_Walkthrough.ipynb`](notebooks/AgriRAG_Naija_Walkthrough.ipynb) | Smallholder farmers lack on-demand access to agronomic advice (~1 extension worker per 3,000+ farmers in Nigeria) | 62 synthetic facts on crops, pests, livestock, storage, and farm support programs | Friendly agricultural extension officer |
| [`NaijaBizRAG_Walkthrough.ipynb`](notebooks/NaijaBizRAG_Walkthrough.ipynb) | First-time entrepreneurs can't easily tell which regulator/agency applies to their business, and get overcharged by unofficial "agents" | 59 synthetic facts on CAC, tax, sector licensing, employment compliance, IP, and financing | Calm, precise compliance advisor |

## Why these two problems

Both notebooks follow the same principle: an LLM is fluent but not automatically trustworthy on specialized, local knowledge. RAG grounds it in a curated set of facts and — just as importantly — teaches it to say "I don't know" when a question falls outside that knowledge base, rather than inventing a plausible-sounding answer. That matters more in some domains than others: giving wrong farming advice is costly, but giving wrong tax or licensing advice can have legal and financial consequences, which is why the NaijaBizRAG notebook carries an explicit non-advice disclaimer and avoids hardcoding any fee amounts that could go stale.

## Repository structure

```
naija-rag-series/
├── README.md
├── LICENSE
├── requirements.txt
├── notebooks/
│   ├── AgriRAG_Naija_Walkthrough.ipynb
│   └── NaijaBizRAG_Walkthrough.ipynb
├── data/
│   ├── agri_documents.py         # the 62-fact synthetic knowledge base, as a standalone importable list
│   └── sme_compliance_documents.py   # the 59-fact synthetic knowledge base, as a standalone importable list
└── docs/
    └── architecture.md           # shared RAG architecture diagram + explanation
```

The `data/` files are the exact same `documents` list embedded in each notebook, pulled out separately so they're easy to reuse, diff, or swap into a different notebook (e.g. plugging the agriculture facts into a different retrieval strategy without re-copying from the `.ipynb`).

## Getting started

1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/naija-rag-series.git
   cd naija-rag-series
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Get a free [Hugging Face access token](https://huggingface.co/settings/tokens) (read access is enough).
4. Open either notebook in Jupyter or Google Colab. If you're in Colab, save your token as a Colab secret named `HF_TOKEN`; otherwise the notebook will prompt you for it.
5. Run the cells top to bottom — each notebook is self-contained and includes short "Exercise" prompts if you want to extend it.

## What each notebook teaches

Both notebooks walk through the same four-stage RAG pattern:

```
question → embed → retrieve (FAISS) → augment prompt with retrieved facts → generate (LLM) → grounded answer
```

See [`docs/architecture.md`](docs/architecture.md) for a shared explanation of this pipeline, and each notebook's own "What's Actually Happening Under the Hood" section for the domain-specific walk-through.

## Extending this series

Each notebook ends with a set of exercises (source citation, similarity-score thresholds, real-document chunking, deployment with Gradio, etc.). If you build a third notebook on the same pattern — a new Nigerian problem, a new knowledge base — it's a natural fit for this repo: add it under `notebooks/`, its data file under `data/`, and a row to the table above.

## Disclaimer

The knowledge bases in this repo are **synthetic and written for teaching purposes**. They are simplified, may be incomplete, and are not guaranteed to be current. In particular, the `NaijaBizRAG` notebook is **not legal, tax, or regulatory advice** — verify anything relevant to an actual business decision with the official CAC, FIRS, or SMEDAN channels, or a qualified professional.

## Author

**Samuel Yaula Dutse** — Lead Data Scientist at Bluehouse Technologies Ltd., AI/Data Science Instructor at Nexus Hub Limited, Jos, Nigeria.

## License

MIT — see [LICENSE](LICENSE).
