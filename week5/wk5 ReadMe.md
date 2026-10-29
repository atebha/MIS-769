# HW 5: Named Entity Recognition with spaCy

## Overview

`HW05.ipynb` demonstrates how named entity recognition (NER) turns free text into structured mentions of organizations, products, locations, people, and amounts. It applies spaCy to Yelp reviews, counts and visualizes the extracted entities, then adapts the workflow to automotive reviews to examine how a general-purpose NER model handles domain-specific names.

## What it does

1. Installs spaCy and its small English model, then loads 5,000 Yelp reviews from Hugging Face.
2. Demonstrates spaCy entity labels on a short example and tests the effects of casing and ambiguous brand/product names.
3. Extracts ORG, PRODUCT, GPE, PERSON, and MONEY entities from Yelp reviews with `nlp.pipe`, truncating each review to 1,000 characters for runtime.
4. Counts the most common organizations, locations, and people, and experiments with money counts, casing normalization, and merging location variants.
5. Plots top entity frequencies in horizontal bar charts and compares alternate visualizations.
6. Loads an automotive-review dataset and investigates its entity labels, including brands, models, model years, engine sizes, and quantities.
7. Applies dataset-specific cleanup and normalization, then charts the most common car brands, models, and engine sizes.
8. Includes written observations about entity errors, business use cases, and domain-specific NER improvement. An optional entity-level sentiment example is included but commented out.

## Data

The general NER example uses the `Yelp/yelp_review_full` training split, limited to 5,000 reviews. The domain playground loads `DaVinciCode/Expert_Car_Reviews_Dataset` from Hugging Face. Its saved results contain 3,463 reviews and use the `full description`, `car brand`, and `car model` columns.

The notebook needs internet access to install spaCy and its `en_core_web_sm` model and to download both Hugging Face datasets. Depending on Hugging Face dataset access settings, authentication may also be required for the car-review dataset.

## How to run

1. Open `HW05.ipynb` in Google Colab or another Jupyter environment with internet access.
2. Run the setup cells to install `spacy`, `datasets`, pandas, matplotlib, and seaborn, and download `en_core_web_sm`.
3. Run the Yelp demo, extraction, analysis, and visualization cells from top to bottom. NER over the 5,000 reviews may take a few minutes on CPU.
4. Run the Playground cells to load and analyze the automotive reviews. Review the printed dataset columns if adapting the pipeline to a different source.
5. Inspect the highlighted examples and document missed, mislabeled, or incorrectly identified entities. The optional sentiment example requires installing Transformers and is not run by default.

## Key findings

- In the saved Yelp run, the selected 5,000 reviews produced 3,357 ORG, 185 PRODUCT, 2,388 GPE, 3,364 PERSON, and 959 MONEY mentions. These are model predictions, not verified counts of correct entities.
- In the car-review playground, Toyota, Honda, and Ford were the most frequent ORG mentions, with 1,231, 793, and 680 mentions respectively.
- Dataset-based normalization classified 47% of ORG mentions as known brands, 28% as vehicle models, and 25% as other mentions. The `PRODUCT` mentions were also mixed: 71% classified as models, 10% as engine codes, and 19% as other.
- Manual review found inconsistent labels for similar automotive names: some car models were tagged as ORG instead of PRODUCT, engine configurations such as V6 were sometimes tagged as locations, and paint colors could be tagged as people.
- In one saved runtime comparison, analyzing 300 characters per review instead of 1,000 reduced elapsed time by about 51%, but also reduced extracted entities by about 46%. The batch-size experiment was also faster with larger `nlp.pipe` batches in that run.

## Limitations

- `en_core_web_sm` is a general-purpose model, not one trained specifically for automotive reviews. It can miss domain terms or inconsistently label brands, model names, trims, and specifications.
- Entity counts are not accuracy measures. The notebook only manually inspects selected examples and does not calculate precision, recall, or F1 against a labeled dataset.
- Truncating reviews can omit entities near the end. The saved comparison shows that reducing the character limit lowers runtime but loses many entity mentions.
- Raw frequency counts can split case, spelling, hyphenation, and alias variants. Normalization can merge genuine variants, but may also combine distinct names; the car-specific cleanup relies on the dataset's own brand/model lists and hand-maintained aliases.
- Entity labels such as ORG, PRODUCT, DATE, QUANTITY, and NORP are not equivalent to business categories. Additional validation and domain rules are required before using counts for reporting or decisions.
- Runtime and batch-size results depend on the machine, spaCy model, and input text; the recorded timings are illustrative rather than benchmarks.

## Tools

Python, Jupyter/Google Colab, spaCy (`en_core_web_sm`), Hugging Face `datasets`, pandas, matplotlib, seaborn, and `collections.Counter`. The optional sentiment example uses Hugging Face Transformers.
