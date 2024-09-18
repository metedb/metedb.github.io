---
title: "Efficient Search Methods for Optimisation"
excerpt: "Search methods are crucial in optimisation, especially in real-world problems where finding the best solution involves complex or noisy functions that are hard to solve analytically. In scenarios such as engineering design, financial modeling, and scientific simulations, where exact gradients are unavailable or too costly to compute, search methods offer practical solutions. In this project, I combined two optimisation methods: the Golden Section Search and the Hooke-Jeeves method. <br/><br/>

The Golden Section Search minimizes a one-dimensional function by narrowing the interval using a specific ratio. In the Hooke-Jeeves method, designed for multi-variable problems, the Golden Section Search is used in each direction to find the optimal step size. Hooke-Jeeves explores how the function responds to adjustments in different directions, refining the movement using the Golden Section Search. This iterative process moves closer to the optimal solution without relying on gradients, making it especially useful for complex or noisy functions. Code is [available here](https://github.com/metedb/Nonlinear-Optimisation). The image below is taken from [this study](https://www.researchgate.net/publication/227082838_Derivative-Free_Optimization). <br/><img src='/images/hooke-jeeves.png'>" 
collection: portfolio
---

