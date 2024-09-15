---
title: "Clustering Algorithms"
excerpt: "Click the above link for a detailed explanation.<br/><img src='/images/500x300.png'>"
collection: portfolio
---
### K-Means Clustering:
K-Means is one of the simplest and most popular clustering algorithms. It works by dividing data points into groups (or clusters) based on their similarity. The algorithm starts by selecting a few initial "centroids" (central points), then assigns each data point to the nearest centroid. Once all points are assigned, the centroids are recalculated, and the process repeats until the clusters no longer change. K-Means is fast and works well for data that forms clear, compact groups, making it ideal for problems like image compression or customer segmentation.

**Advantages**:
- **Speed**: K-Means is efficient and can handle large datasets quickly.
- **Simplicity**: It’s easy to understand and implement, making it a great starting point for clustering tasks.

However, K-Means assumes that clusters are spherical and of similar sizes, which can limit its effectiveness for more complex data structures.

### Spectral Clustering:
Spectral Clustering, on the other hand, is a more advanced technique that can handle complex data patterns. Instead of directly grouping the data points based on distance, it first builds a graph representing the similarity between points. By analyzing this graph, the algorithm can identify clusters based on relationships between the points, even when the clusters are irregularly shaped or not well separated.

**Advantages**:
- **Handles Complex Structures**: Spectral Clustering is particularly powerful for data that doesn’t neatly form spherical clusters, making it ideal for more intricate datasets.
- **Flexibility**: It can identify clusters in data that would be hard for K-Means to detect, such as when clusters are non-convex or intertwined.

However, Spectral Clustering is more computationally expensive, especially with large datasets, because it involves eigenvalue decomposition of matrices.

### MNIST Project (Classifying 0's and 7's)
I applied both K-Means and Spectral Clustering to the MNIST dataset, specifically focusing on classifying handwritten digits '0' and '7'. While K-Means provided fast and straightforward grouping based on pixel intensity patterns, Spectral Clustering revealed deeper relationships in the data that K-Means might miss, particularly in distinguishing subtle variations between digits.

[Here](https://github.com/metedb/ML-Projects/blob/main/Dibi_Mete_CA1_NMD.ipynb) is the link.
