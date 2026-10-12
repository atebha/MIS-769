# Semantic Search with Sentence Embeddings


## Overview
A semantic search engine built with sentence embeddings. It finds documents by
meaning instead of exact keywords, and is tested on two corpora: **IMDB movie
reviews** and **Amazon microSD card reviews**.

## What it does
1. Encodes text into 384-dimension vectors with `all-MiniLM-L6-v2` (Sentence Transformers).
2. Measures similarity between sentences using cosine similarity.
3. Provides a `semantic_search()` function that returns the top-k most similar documents.
4. Compares semantic search against keyword (substring) search.

## Data
- **IMDB demo:** 2,000 reviews (`stanfordnlp/imdb`, first 2,000 of train)
- **Playground:** Amazon microSD card reviews (`hugginglearners/amazon-reviews-sentiment-analysis`),
  text column `reviewText`, first 2,000 rows
- Each review is truncated to 512 characters before encoding

## How to run
1. Open `HW06.ipynb` in Google Colab or local in a Jupyter Notebook.
2. Run all cells top to bottom (Runtime → Run all); the first cell installs the dependencies.
3. Encoding takes about 1-2 minutes per corpus on CPU.

## Key findings
- **Semantic search wins on paraphrase:** "stopped working after a few weeks" found
  failure reviews that never use that phrase
- **Keyword search has false positives:** many "died" matches were not about a failing card
- **Failure case:** "very slow transfer speed" returned reviews about *fast* speeds;
  the model matched the topic and ignored the direction of the sentiment

## Limitations
- Each document is compressed to one 384-number vector, so topic can outweigh sentiment or negation
- Text is truncated, so details late in a long review are lost
- Compares the query against every document, which won't scale to millions without a vector index (e.g. FAISS)
- Not suited to exact identifiers like SKUs or invoice numbers

## Tools
Python, sentence-transformers, scikit-learn, pandas, matplotlib/seaborn, Hugging Face datasets