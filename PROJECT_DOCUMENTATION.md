# FC Mobile Cluster - Project Documentation

## Project Overview

**FC Mobile Cluster** is a machine learning project that automatically categorizes football players into meaningful types without being explicitly told what those types are. The project uses unsupervised learning techniques (K-Means clustering) to discover natural groupings in player data based on their career stage and market position.

**Problem Statement:** Can a machine automatically sort football players into meaningful types, without being told what the types are?

---

## Data Source

- **Dataset:** FIFA 22 players dataset (`players_22.csv`)
- **Location:** `C:/Users/PAJEET/Documents/Jupyter codes/Data Files/archive/players_22.csv`
- **File Format:** CSV

---

## Features Selected

The project uses **5 key features** to represent each player:

1. **Overall** - Current player rating (what he can do right now)
2. **Potential** - Future player rating (what he will be able to do later)
3. **Wage (wage_eur)** - Actual club salary in euros
4. **Value (value_eur)** - Market value in euros
5. **Age** - Player age (indicates time to reach potential)

### Why These 5 Features?

These features answer a fundamental question: **"What is this player worth, now and in the future?"**

Together, they describe:
- A player's career stage (young prospect vs. established player vs. veteran)
- Market position (elite player vs. squad filler vs. rising talent)

**Why NOT other features?** Features like pace, dribbling, or shooting describe playing style, not career stage. Including them would blur clusters, creating groupings like "fast young players" instead of meaningful career stages.

---

## Data Preprocessing

### 1. Data Cleaning
- Removed rows with missing values in any of the 5 selected features using `dropna()`
- Only complete records were used for clustering

### 2. Feature Scaling (Min-Max Normalization)
- Applied min-max scaling to normalize all features to the range [1, 10]
- **Formula:** `(value - min) / (max - min) × 9 + 1`

### Why Scaling Matters?
Without scaling, the K-Means algorithm would be dominated by the largest-range features:
- Wage values (€200,000) would overshadow age (24) and overall rating (87)
- The algorithm would effectively cluster on wage only, with other features becoming invisible
- Scaling ensures all features have equal vote in determining similarity

---

## Clustering Algorithm

### Implementation Approach

The project implements **K-Means clustering from scratch** to ensure deep understanding of the algorithm mechanics:

#### Key Steps:

1. **Random Centroid Initialization**
   - Function: `random_centriods(data, k)`
   - Creates k random centroids from the data

2. **Assignment Step**
   - Function: `get_labels(data, centriods)`
   - For each player, calculates Euclidean distance to all centroids
   - Assigns player to nearest centroid (lowest distance)

3. **Update Step**
   - Function: `new_centriods(data, labels, k)`
   - Recalculates centroid positions as the mean of assigned players
   - Uses geometric mean to preserve feature relationships

4. **Convergence**
   - Iterates until centroids stop changing (max 100 iterations)
   - Visualizes cluster evolution using PCA for 2D projection

### Dimensionality Reduction for Visualization

- **PCA (Principal Component Analysis)** reduces 5D cluster data to 2D
- Allows visualization of high-dimensional clustering results
- Shows cluster separation and centroid positions

---

## Validation

### Comparison with scikit-learn

To ensure correctness, the manual K-Means implementation was validated against scikit-learn's production-grade KMeans:

- Ran sklearn's `KMeans(n_clusters=3)` on the same scaled data
- Cluster centers matched the manual implementation
- Cluster assignments agreed with manual results
- **Conclusion:** Manual implementation is correct and trustworthy

---

## Results

### Clustering Output (k=3)

The algorithm identified **3 distinct player types**:

1. **Cluster 0** - [Elite/Established Players]
   - High overall rating
   - Stable potential (already near peak)
   - High wages and market value
   - Older age group

2. **Cluster 1** - [Rising Prospects/Young Talents]
   - Lower-to-moderate overall rating
   - High potential (significant room to grow)
   - Lower wages and value
   - Younger age group

3. **Cluster 2** - [Squad Fillers/Support Players]
   - Moderate overall rating
   - Limited potential (won't improve much)
   - Low-to-moderate wages and value
   - Mixed age group

Each cluster summary includes:
- Average overall rating
- Average potential
- Average wage
- Average market value
- Average age
- Total player count per cluster

---

## Technologies & Libraries

- **Python** - Primary programming language
- **Pandas** - Data manipulation and analysis
- **NumPy** - Numerical computations
- **Scikit-learn** - K-Means validation and PCA
- **Matplotlib** - Data visualization
- **Jupyter Notebook** - Development environment

---

## Project Structure

```
FC-mobile-cluster/
├── FCMobile cluster.ipynb      # Main notebook with full analysis
└── PROJECT_DOCUMENTATION.md    # This file
```

---

## Key Learnings

1. **Feature Selection Matters:** Choosing the right features determines if clusters are meaningful
2. **Data Scaling is Critical:** K-Means is distance-based; unscaled features create biased results
3. **Manual Implementation Builds Understanding:** Implementing from scratch reveals algorithm mechanics
4. **Validation is Essential:** Comparing with production libraries ensures correctness
5. **Domain Knowledge Enhances Machine Learning:** Understanding what the features represent helped select features that produce meaningful clusters

---

## Future Enhancements

- Experiment with different values of k (currently k=3)
- Use the elbow method or silhouette analysis to find optimal k
- Try other clustering algorithms (hierarchical clustering, DBSCAN, etc.)
- Add more sophisticated visualization techniques
- Include additional player characteristics (position, nationality, etc.)
- Analyze cluster characteristics to generate player profiles and insights

---

## Author

Created as part of exploratory machine learning analysis on FIFA 22 player data.

**Date Created:** 2026

---

## Summary

This project successfully demonstrates that machine learning can automatically discover meaningful player categories without explicit guidance. By selecting features that capture career stage and market position, and applying K-Means clustering with proper data preprocessing, the algorithm identified distinct player types that align with real-world football player classifications.
