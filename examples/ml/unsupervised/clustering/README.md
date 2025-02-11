# KMeans Clustering Example

This notebook demonstrates how to use **SciREX** for unsupervised clustering with **KMeans**, showcasing automatic parameter selection (via Silhouette Score or Elbow Method) and custom cluster counts.

Author : Dev Sahoo (debajyotis@iisc.ac.in)

## Features

- Flexible parameter selection:
  - Automatic parameter optimization
  - User-defined parameter input
  - Hybrid approaches available
- Built-in visualization function through parent class (base.py)
- Comprehensive metrics calculation
- Support for multiple clustering algorithms
- Access to model attributes and parameters

## Overview

- **File**: `example_kmeans.py`
- **Models**:**KMeans** (shown in the example)

**Import the models using:**

- **KMeans**
  ```python
  from scirex.core.ml.unsupervised.clustering.kmeans import Kmeans
  ```
- **DBSCAN**
  ```python
  from scirex.core.ml.unsupervised.clustering.dbscan import Dbscan
  ```
- **HDBSCAN**
  ```python
  from scirex.core.ml.unsupervised.clustering.hdbscan import Hdbscan
  ```
- **GMM** (Gaussian Mixture Model)
  ```python
  from scirex.core.ml.unsupervised.clustering.gmm import Gmm
  ```
- **OPTICS**
  ```python
  from scirex.core.ml.unsupervised.clustering.optics import Optics
  ```
- **Agglomerative Clustering**
  ```python
  from scirex.core.ml.unsupervised.clustering.agglomerative import Agglomerative
  ```

### Parameter Selection Option

All clustering models support both **automatic parameter choosing** and **user-defined** parameters.

For example, **KMeans** can automatically determine the optimal number of clusters (`k`) by using:

1. **Silhouette Score**
2. **Elbow Method**
3. **Custom user-defined `k`**

Similar approaches are applied in other models (e.g., **DBSCAN** auto-selecting `eps` or **Agglomerative** for identifying the optimal number of clusters).

## Key Files and Imports

```python
# Standard library imports
import os
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
from sklearn.datasets import make_blobs, make_moons
from sklearn.preprocessing import StandardScaler

# Import SciREX KMeans model
from scirex.core.ml.unsupervised.clustering.kmeans import Kmeans
```

## Overview

### Datasets

We use two popular scikit-learn datasets to demonstrate how clustering models behave with different cluster shapes:

1. **`make_blobs`**
   - Generates isotropic Gaussian blobs for clustering (well-separated spherical clusters).
   - In the example, we create 4 centers with `n_samples=1000` and standardize them using `StandardScaler`.
2. **`make_moons`**
   - Generates two interleaving half circles (non-spherical clusters).
   - We add a small amount of noise (`noise=0.05`) and standardize similarly.

```python
X_blobs, _ = make_blobs(n_samples=1000, centers=4, random_state=42)
X_blobs = StandardScaler().fit_transform(X_blobs)

X_moons, _ = make_moons(n_samples=1000, noise=0.05, random_state=42)
X_moons = StandardScaler().fit_transform(X_moons)
```

### The `run_clustering` Function

- The script/notebook defines a helper function `run_clustering(model, X, dataset_name)`.
- This function calls `model.run(data=X)`, which handles:
  1. **Fitting** the model (KMeans in this example).
  2. **Calculating** standard clustering metrics: Silhouette Score, Calinski-Harabasz Score, Davies-Bouldin Score, and timing.
  3. **Plotting** the resulting clusters by calling the `.plots(...)` method (inherited from a base class in `base.py`). The plot is saved as a PNG file and then displayed.

```python
def run_clustering(model, X, dataset_name: str):
    """
    Run clustering model and display results.

    Args:
        model: Initialized clustering model instance
        X (np.ndarray): Input data matrix
        dataset_name (str): Name of dataset for display purposes

    Prints:
        - Clustering metrics (Silhouette, Calinski-Harabasz, Davies-Bouldin)
        - Number of clusters (if applicable)

    Displays:
        - Clustering visualization plot
    """
```

### Running KMeans

The main() function demonstrates clustering on synthetic datasets:

```python
def main():
    """
    Main function to demonstrate KMeans clustering on synthetic datasets.
    Generates two datasets (blobs and moons) and applies KMeans clustering.
    """
```

```python
# Instantiate two separate KMeans models

# Let the model determine optimal k
kmeans_blobs = Kmeans(max_k=10)

# Specify number of clusters directly
kmeans_moons = Kmeans(n_clusters=4,max_k=10)

# Run on blobs
run(kmeans_blobs, X_blobs, "blobs")

# Run on moons
run(kmeans_moons, X_moons, "moons")
```

### Example outputes

```python
#For blobs dataset
Optimal k (silhouette) = 4
Optimal k (elbow)      = 3

KMeans fitted with 3 clusters.

--- KMEANS on blobs dataset ---
Silhouette Score:      0.738
Calinski-Harabasz:     3479.989
Davies-Bouldin:        0.376
Number of clusters:    3

#For moons dataset
KMeans fitted with 4 clusters.

--- KMEANS on moons dataset ---
Silhouette Score:      0.429
Calinski-Harabasz:     1230.080
Davies-Bouldin:        0.900
Number of clusters:    4
```

### Accessible Model Attributes

```python
# Core Attributes
kmeans_blobs.n_clusters_       # Final number of clusters used
kmeans_blobs.labels         # Cluster assignments for each point
kmeans_blobs.cluster_centers_ # Coordinates of cluster centers
kmeans_blobs.inertia_        # Sum of squared distances to centroids

# Get all parameters at once
kmeans_blobs.get_model_params()
```
