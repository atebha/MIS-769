# HW 4: Word2Vec and Word Embeddings

## Overview

`HW4.ipynb` introduces Word2Vec embeddings using customer-review text. It trains a model on Yelp reviews, examines similar words and vector analogies, tests common embedding failure modes, compares the results with pre-trained GloVe vectors, and repeats selected experiments with a game-review dataset.

## What it does

1. Installs the notebook dependencies, loads a 20,000-review subset of Yelp data from Hugging Face, and preprocesses reviews by lowercasing, keeping letters and spaces, and removing one- and two-letter tokens.
2. Trains a 100-dimensional Skip-gram Word2Vec model with a five-word context window, a minimum word count of 10, and 15 epochs.
3. Explores nearest neighbors and cosine similarities for review vocabulary, then prompts experiments with CBOW, different training durations, and a wider context window.
4. Investigates embedding weaknesses, including rare words, words with multiple meanings, and antonyms that occur in similar contexts.
5. Tests vector analogies and compares the Yelp-trained model with the pre-trained `glove-twitter-50` model.
6. Includes an optional t-SNE visualization of frequent word vectors.
7. Trains a second Word2Vec model on 20,000 game reviews and tests game-related terms, embedding failures, and domain-specific vocabulary.
8. Provides written reflections on the models, their failure modes, and business uses of embeddings.

## Data

The first model uses the `Yelp/yelp_review_full` training split, limited to 20,000 reviews. The domain playground loads the training split of `auphong2707/game-reviews-sentiment`, also limited to 20,000 records, and expects its text column to be named `review_text`.

The notebook also downloads the pre-trained `glove-twitter-50` vectors through Gensim for comparison. No local CSV files are required, but the Hugging Face datasets and GloVe download require internet access.

## How to run

1. Open `HW4.ipynb` in Google Colab or a local Jupyter environment with internet access.
2. Run cells from top to bottom. The first code cell installs `gensim`, `datasets`, `scikit-learn`, `matplotlib`, and `seaborn`.
3. Wait for the Yelp dataset to download and the Skip-gram model to train. The notebook estimates training at one to two minutes.
4. Run the similarity, failure-hunt, analogy, and GloVe comparison cells. The first GloVe download is approximately 200 MB.
5. For the domain playground, check the columns printed after loading the game-review dataset. The following cells expect `review_text`; update `my_text_column` if the dataset schema differs.
6. Review the optional t-SNE visualization and the written findings.

## Key findings

- The Yelp-trained model illustrates that nearest-neighbor and analogy quality depends on word frequency, the review corpus, and the context patterns in which words appear.
- The notebook explores three common failure modes: rare words may have unreliable neighbors, polysemous words combine multiple senses into one vector, and antonyms may appear similar because they occur in similar contexts.
- In the game-review playground, the written analysis reports that the model captured gaming terms such as `grind`, `buggy`, and `farm`, while struggling with rare misspellings and with meanings specific to gaming.
- The game-review antonym check reports a similarity of 0.738 for `good` and `bad`, illustrating why embedding similarity alone is not a reliable sentiment classifier.
- The notebook's comparison emphasizes that a domain-trained model can learn niche vocabulary, while a large general model such as GloVe has broader coverage. Neither model is universally better; usefulness depends on the task and corpus.

## Limitations

- Word2Vec assigns one vector to each word, so it cannot distinguish different senses of a word based on its sentence. Opposites may also receive high similarity because they appear in similar contexts.
- Word neighbors and vector analogies are exploratory signals, not guaranteed definitions or reliable decision rules. In particular, similarity scores should not be used by themselves to route complaints or classify sentiment.
- The Yelp and game-review datasets are each capped at 20,000 reviews. Rare or domain-specific words may not appear often enough to learn stable representations.
- Preprocessing removes punctuation and short tokens and keeps no sentence-level context beyond ordered word windows; this can discard useful signals.
- Results depend on the sampled corpus, model settings, and training. The notebook sets random seeds, but parallel training with multiple workers may still make results vary.
- The GloVe comparison requires a sizable one-time download, and the optional t-SNE plot is a two-dimensional projection that can distort distances in the original embedding space.

## Tools

Python, Jupyter/Google Colab, Gensim/Word2Vec, Hugging Face `datasets`, scikit-learn, NumPy, pandas, matplotlib, seaborn, and the pre-trained GloVe Twitter vectors.
