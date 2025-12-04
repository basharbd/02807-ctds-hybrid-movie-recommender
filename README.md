

````markdown
# Hybrid Movie Recommendation with User Clustering and TF-IDF Genres

This repository contains my final project for the DTU course **02807 Computational Tools for Data Science**.

The goal of the project is to build and evaluate a small **movie recommender system** that combines:
- a **collaborative** component based on **k-means clustering of users**, and  
- a **content-based** component using **TF-IDF over movie genres**.

The project compares three models:

1. **Popularity baseline** – recommends globally top-rated movies based on mean rating.  
2. **Cluster-only recommender** – recommends movies that are highly rated within a user’s k-means cluster.  
3. **Hybrid recommender** – combines cluster scores with TF–IDF genre similarity for users’ liked movies.

Although the hybrid design is conceptually appealing, the experiments show that it underperforms the simpler baselines. The project therefore focuses on analysing *why* this happens (data sparsity, weak content features, and limitations of k-means on sparse rating vectors).

---

## Data

This project uses two public datasets:

- **MovieLens "latest small"** 100,004 explicit ratings from 671 users on 9,066 movies.  
  Available from: <https://grouplens.org/datasets/movielens/>

- **The Movies Dataset (Kaggle)** Movie metadata including titles and JSON-encoded genre lists.  
  Available from: <https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset>

For licensing and size reasons, **raw data files are not included** in this repository.

Please download:

- `ratings_small.csv` from MovieLens  
- `movies_metadata.csv` from the Kaggle Movies Dataset  

and place them in:

```text
data/
  ratings_small.csv
  movies_metadata.csv
````

There is also a small `data/README_data.md` file describing this.

-----

## Repository Structure

```text
02807-ctds-hybrid-movie-recommender/
├─ README.md
├─ requirements.txt           # Python dependencies (to be installed with pip)
├─ notebook/
│   └─ 02807_hybrid_movie_recommender.ipynb
├─ report/
│   ├─ main.tex               # LaTeX report source
│   ├─ dtu_logo.png
│   ├─ fig_pca_clusters.png   # PCA + k-means visualisation
│   └─ fig_hitrate_models.png # Hit-rate@10 bar plot
└─ data/
    └─ README_data.md         # Instructions only, no raw data
```

-----

## Methods (Short Overview)

**Course topic used:**

  * **General clustering algorithms** – specifically k-means on user–item rating vectors.

**External method:**

  * TF–IDF features built from movie genres (`genres_str`) and cosine similarity for content-based movie similarity.

**Models:**

1.  **Popularity baseline:** rank movies by global mean rating (with a minimum rating-count threshold).
2.  **Cluster-only recommender:** For each user, recommend movies with high mean rating within that user’s k-means cluster, excluding already-rated movies.
3.  **Hybrid recommender:**

For each candidate movie ($m$):

$$
\text{cluster_score}(m) = \text{mean rating of } (m) \text{ inside the user’s cluster}
$$

$$
\text{content_score}(m) = \text{maximum TF–IDF cosine similarity between } (m) \text{ and any movie the user liked (rating ≥ 4.0) in the training set}
$$

**Final score:**

```math
\text{final_score}(m) = 0.7 \cdot \text{cluster_score}(m) + 0.3 \cdot \text{content_score}(m)

**Evaluation metric:**

  * **Hit-rate@10** on held-out test ratings.
  * A user is counted as a “hit” if at least one of their test movies appears in the top-10 recommendations.

-----

## How to Run

1.  **Create a Python environment and install dependencies**

    ```bash
    pip install -r requirements.txt
    ```

2.  **Download the data files** (`ratings_small.csv`, `movies_metadata.csv`) and place them in the `data/` folder as described above.

3.  **Launch Jupyter and open the notebook**

    ```bash
    jupyter notebook notebook/02807_hybrid_movie_recommender.ipynb
    ```

4.  **Run all cells from top to bottom to:**

      * load and clean the data
      * build the user–item matrix
      * train the k-means clustering model
      * compute TF–IDF features and cosine similarity
      * evaluate popularity, cluster-only, and hybrid recommenders
      * generate the PCA and hit-rate figures used in the report

-----

## Results 

**Hit-rate@10 (sample of test users):**

  * **Popularity:** 0.210
  * **Cluster-only:** 0.210
  * **Hybrid:** 0.037

The popularity and cluster-only models perform similarly (partly because the cluster-only model often falls back to the global popularity list when clusters are too small or sparse).

The current hybrid approach performs significantly worse, mainly due to:

1.  extremely sparse user–item data and crude 0-filling for missing ratings
2.  unbalanced and overlapping k-means clusters
3.  weak content features based only on genres (many movies share almost identical genre vectors)
4.  heuristic thresholds and weights in the hybrid scoring function.




