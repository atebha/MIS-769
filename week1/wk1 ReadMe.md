# HW 1: Exploring Netflix Movies and TV Shows

## Overview

`HW01.ipynb` is an introductory exploratory data analysis (EDA) notebook. It loads a Kaggle dataset of Netflix titles into a pandas DataFrame, inspects its structure and data quality, and explores the mix of movies and TV shows, content ratings, and movie runtimes.

## What it does

1. Loads the Netflix titles CSV into pandas.
2. Previews records and inspects column types and dataset size.
3. Checks for duplicate show IDs, blank-looking text values, and missing values.
4. Counts content ratings and compares the number and proportion of movies and TV shows.
5. Calculates the average movie runtime and expresses it in hours and minutes.
6. Includes written responses discussing the data source, data-quality observations, and an interpretation of the results.

## Data

The notebook uses the Kaggle Netflix Movies and TV Shows dataset. Its saved output shows 8,807 records and 12 columns, including title, type, director, cast, country, release year, rating, duration, genres, and description.

The notebook expects the CSV to be available at a google drive path:

`/colab_notebooks/MyDrive/Colab Notebooks/netflix_titles.csv`

I have also included the raw CSV data as well in this project folder. 

## How to run

1. Open `HW_1.ipynb` in Google Colab.
2. Download the Netflix titles CSV dataset from Kaggle and place `netflix_titles.csv` in the `Colab Notebooks` folder in Google Drive.
    a. Or just load the csv file directly.
3. Run the cells from top to bottom. The notebook mounts Google Drive at `/colab_notebooks` and reads the CSV from the path above.
4. To run locally instead, install the notebook dependencies and update the data-loading cell to point to your local CSV file. The current drive-mount cell uses the Google Colab-only `google.colab` package.

## Key findings

- The dataset contains 6,131 movies and 2,676 TV shows: approximately 69.6% movies and 30.4% TV shows.
- `TV-MA` is the most common rating in the saved results, with 3,207 entries.
- The calculated average movie runtime is about 99.6 minutes, or approximately 1 hour and 40 minutes.
- The saved output reports 8,807 unique show IDs, matching the row count.
- Missing values are concentrated in `director` (2,634), `cast` (825), and `country` (831). The output also reports 10 missing `date_added` values, 4 missing `rating` values, and 3 missing `duration` values.

## Limitations

- This is a basic descriptive analysis of one snapshot of the catalog; it does not establish why Netflix releases a particular mix of content or how that mix changes over time.
- The notebook prints `df.describe` and `df.value_counts` without calling them, so those cells display method representations rather than descriptive statistics or value counts. Later column-specific counts and calculations do run.
- The blank-text check casts values to strings before checking for empty text. As a result, missing values such as `NaN` are not counted as blanks by that check; the separate null check is needed to identify them.
- Some ratings are likely invalid or misparsed: the saved rating counts include values such as `74 min`, `84 min`, and `66 min`. These should be cleaned before drawing conclusions about ratings.
- The movie-duration calculation assumes movie durations are strings ending in ` min` and converts them directly to numbers. Rows with missing or unexpected duration values may cause issues.
- `csv` and `json` are imported but not used, and `matplotlib.pyplot` is imported without any plots being created.

## Tools

Python, Jupyter/Google Colab, pandas, and the Netflix titles CSV dataset. The notebook also imports `csv`, `json`, and `matplotlib`; it uses Google Drive mounting when run in Colab.