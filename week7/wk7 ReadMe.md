# HW07: Build a RAG System

## Overview

This Lab builds a small Retrieval-Augmented Generation (RAG) system. The notebook demonstrates how to chunk documents, embed them, retrieve relevant passages with vector search, and provide those passages to a language model to answer questions. It first uses fictional TechGadgets company FAQ and policy documents, then applies the same workflow to a Dragon Ball CCG rulebook subset example.

## What it does

1. Splits a policy document into overlapping word-based chunks and compares chunk sizes.
2. Embeds FAQ entries and policy chunks with `all-MiniLM-L6-v2` and indexes them in FAISS.
3. Retrieves the nearest passages for a question and inspects similarity distances.
4. Compares context-grounded answers from `google/flan-t5-base` with answers from the
   same model without retrieval.
5. Rebuilds the knowledge base for a user-provided domain and tests how chunk size and
   retrieval count (`k`) affect results.

## Data

- **Default demo:** Fictional TechGadgets FAQ entries and a customer policy handbook
  covering returns, shipping, warranties, and discounts.
- **Domain playground:** A Dragon Ball CCG rulebook excerpt, chunked at different sizes to test questions about victory conditions.
- The notebook also includes deliberately irrelevant or unanswerable questions to show
  how retrieval and generation can fail.

## How to run

1. Open `HW07.ipynb` in Google Colab or a local Jupyter environment.
2. Use a Python kernel with the required packages installed: `numpy`,
   `sentence-transformers`, `transformers`, `torch`, and `faiss-cpu`. If needed, install
   packages from a notebook cell with `%pip install sentence-transformers transformers faiss-cpu`.
3. Run the notebook cells from top to bottom. The first model runs may download the
   embedding and FLAN-T5 models, so internet access is needed initially.
4. Run the experiment cells and review their outputs. For your own domain, add 10–15
   passages to `my_documents`, or chunk a long source document with `chunk_text`.
5. Run the Playground cells in order. They compare chunk sizes and restore the selected
   baseline index after the experiment.

The main models and vector search run locally; the optional frontier-model example
requires an external API account and key and is not needed for the assignment.

## Key findings

- In the Dragon Ball playground, a single-fact question about winning after capturing
  the seventh Dragon Ball was answered correctly with each tested chunk size (30/5,
  40/10, 60/15, and 100/20) at `k=3` and `k=5`.
- The broader question about all ways to win was sensitive to chunk size and `k`.
  The 40/10 setting retrieved all required victory-condition phrases at `k=5`, while
  60/15 did so at `k=3`. Having the relevant facts in retrieved context did not always
  make FLAN-T5 produce a complete answer.
- In the default demo, RAG answered TechGadgets-specific questions more accurately than
  the bare model, which sometimes guessed or answered a different question.
- FAISS always returns the requested number of nearest results, even when none are
  relevant. A distance/relevance check and a reliable abstention strategy are needed
  to handle questions the source documents cannot answer.

## Limitations

- `flan-t5-base` can omit facts, return fragments, or be misled by competing numbers
  in retrieved passages.
- Fixed-size word chunks may split useful context. Overlap helps preserve boundary
  information but increases the number of indexed chunks.
- Exact L2 nearest-neighbor search works for this small example but does not provide
  relevance guarantees or, by itself, an answer for out-of-scope questions.
- The notebook is a learning prototype, not a production deployment. A real system
  needs an ingestion/update process, relevance thresholds, evaluation, monitoring,
  access controls, and a way to abstain or escalate uncertain answers.

## Tools

Python, Jupyter/Google Colab, NumPy, Sentence Transformers (`all-MiniLM-L6-v2`),
Transformers and PyTorch (`google/flan-t5-base`), and FAISS (`IndexFlatL2`).
