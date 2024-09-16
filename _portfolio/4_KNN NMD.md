---
title: "Clustering Algorithms"
excerpt: "Click the above link for a detailed explanation.<br/><img src='/images/KNN.png'>"
collection: portfolio
---
K-Means is one of the simplest and most popular clustering algorithms. It works by dividing data points into groups (or clusters) based on their similarity. The algorithm starts by selecting a few initial "centroids" (central points), then assigns each data point to the nearest centroid. Once all points are assigned, the centroids are recalculated, and the process repeats until the clusters no longer change. K-Means is fast and works well for data that forms clear, compact groups, making it ideal for problems like image compression or customer segmentation.

In this project, I applied K-Means clustering to distinguish between images of handwritten digits '0' and '7' from the MNIST dataset. The dataset contains 400 images, each represented by 784 pixels (28x28), and the goal was to group these images into two clusters using an **unsupervised learning** algorithm.

However, **K-Means assumes that clusters are spherical and of similar sizes**. This means the algorithm works best when clusters are compact and equally spread out. For more complex data structures—like clusters that are elongated, irregularly shaped, or vary significantly in size—K-Means may struggle to group the data correctly. In cases where the data doesn't follow this simple structure, a more advanced approach is needed.

Spectral Clustering is a more flexible technique that addresses these limitations. Instead of relying solely on the distance between data points (as K-Means does), Spectral Clustering starts by building a graph that represents the **similarity** between points. By analyzing the structure of this graph, it can identify clusters based on **relationships** between data points, even if the clusters are irregularly shaped or not well separated.

This makes Spectral Clustering particularly effective for more complex datasets where clusters may not follow the spherical and equally-sized assumptions of K-Means. For example, it can handle datasets where clusters are connected by nonlinear patterns or where data points form intricate structures, such as those seen in social networks or biological datasets. 

<br/><img src='/images/similarity_matrix.png'>

[Here](https://github.com/metedb/ML-Projects/blob/main/Dibi_Mete_CA1_NMD.ipynb) is the link of the code.
