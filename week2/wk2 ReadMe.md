# HW 2: Distributed Processing and Clustering with Spark

## Overview

`HW02.ipynb` is a hands-on introduction to Apache Spark and PySpark. It demonstrates how Spark partitions data and applies MapReduce operations, compares processing times under different local core settings, and uses Spark ML's K-Means algorithm to cluster a synthetic streaming-content dataset.

## What it does

1. Installs and configures Java and PySpark, then creates a local Spark session.
2. Creates 100,000 random integers and compares their distribution across four and eight RDD partitions.
3. Uses `map` and `reduceByKey` to count integer frequencies, display the ten most frequent values, and look up a selected number.
4. Benchmarks a MapReduce count over 500,000 random integers using one, two, and four local cores, then plots elapsed time and relative speedup.
5. Generates 5,000 synthetic movie and TV-show records, converts them to a Spark DataFrame, and prepares release year, description length, and movie/show type as clustering features.
6. Scales the features, fits a three-cluster K-Means model, and summarizes cluster sizes and characteristics.
7. Includes written answers about Spark configuration, performance, MapReduce, cluster interpretation, and a text diagram of a Spark workflow.

## Data

The notebook generates its own data; it does not require a separate CSV or external dataset. The partitioning and frequency examples use randomly generated integers. The clustering example creates 5,000 synthetic streaming titles with a content type, release year, genre, and generated description.

## How to run

1. Open `HW02.ipynb` in Google Colab.
2. Run the cells from top to bottom. The setup cell installs Java 11, PySpark 3.5.0, and `findspark`; the later analysis also uses pandas and matplotlib.
3. Allow the performance experiment to complete. It restarts Spark sessions to compare one, two, and four local cores.
4. Review the printed partition counts, frequency results, timing chart, cluster statistics, sample titles, and written responses.

The setup cell uses `apt-get` and a Linux-specific `JAVA_HOME`, so it is designed for Google Colab or a compatible Debian-based environment. A local macOS or Windows setup requires installing Java separately and adjusting the Java configuration.

## Key findings

- The 100,000 integers were split evenly across the requested four and eight partitions, with small count differences because the partition sizes must distribute the full dataset.
- In the saved frequency example, the top value was 117 (136 occurrences). The selected number 458 appeared 109 times. These results come from random data and can vary across runs.
- The saved 500,000-record performance run took 1.838 seconds with one core, 1.928 seconds with two cores, and 3.143 seconds with four cores. In that run, using more cores did not improve performance; measured speedups relative to one core were 0.95x and 0.58x.
- The synthetic dataset contained 2,966 movies and 2,034 TV shows.
- The three clusters separated the TV shows from movies and further divided movies by description length: Cluster 0 contained 1,529 movies with an average description length of about 161 characters; Cluster 1 contained 2,034 TV shows; Cluster 2 contained 1,437 movies with an average description length of about 388 characters.

## Limitations

- The data is synthetic and its titles, genres, and descriptions do not represent a real streaming catalog. The generated descriptions repeat the word “word,” so description length is a constructed feature rather than meaningful text content.
- The core-count benchmark is a single timing run on one environment, with random input generated separately for each run. Its results are illustrative, not a general performance guarantee; startup, scheduling, data generation, and machine load affect timings.
- More local worker threads do not guarantee faster execution, especially for this relatively small workload. The saved run was slower with two and four cores than with one.
- K-Means is configured with three clusters, and its inputs are only release year, description length, and a binary movie/show indicator. It does not interpret text, genre, or title, and the resulting groups depend on those feature choices and scaling.
- The setup cell runs system/package installation commands and assumes a Linux Colab-style environment. It does not check the return codes from those installation commands before continuing.
- Random integer examples are not seeded, so their frequency results change between runs.

## Tools

Python, Jupyter/Google Colab, Apache Spark, PySpark (including Spark ML), Java 11, `findspark`, pandas, and matplotlib.