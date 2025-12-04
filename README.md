# Hybrid Movie Recommendation with User Clustering and TF-IDF Genres

This repository contains my final project for the DTU course **02807 Computational Tools for Data Science**.

The goal of the project is to build and evaluate a small **movie recommender system** that combines:
- a **collaborative** component based on **k-means clustering of users**, and  
- a **content-based** component using **TF-IDF over movie genres**.

The project compares three models:
1. **Popularity baseline** (global top-rated movies),
2. **Cluster-only recommender** (movies popular within a user's k-means cluster),
3. **Hybrid recommender** combining cluster scores and genre similarity.

Although the hybrid design is conceptually appealing, the experiments show that it underperforms the simpler baselines. The project therefore focuses on analysing *why* this happens (data sparsity, weak content features, and limitations of k-means on sparse rating vectors).

---

## Data

This project uses two public datasets:

- **MovieLens "latest small"**  
  100,004 ratings from 671 users on 9,066 movies.  
  Available from: https://grouplens.org/datasets/movielens/

- **The Movies Dataset (Kaggle)**  
  Movie metadata including titles and JSON-encoded genre lists.  
  Available from: https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset

For licensing and size reasons, **raw data files are not included** in this repository.

Please download:

- `ratings_small.csv` from MovieLens,
- `movies_metadata.csv` from the Kaggle Movies Dataset,

and place them in:

```text
data/
  ratings_small.csv
  movies_metadata.csv
