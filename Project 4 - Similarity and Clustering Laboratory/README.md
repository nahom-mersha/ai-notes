# Image Colour Compression Laboratory

Notes from Project 4 of my AI engineering roadmap.

This project includes an interactive laboratory for image colour compression, pixel-similarity search, clustering, dimensionality reduction, and controlled experiments with distance-based algorithms.

The project uses from-scratch NumPy implementations of Euclidean distance, Manhattan distance, K-nearest-neighbour search, K-means, K-means++ initialization, and PCA. These implementations are tested and compared with scikit-learn.

- Project repository: https://github.com/nahom-mersha/image-colour-compression-lab
- Live Streamlit application: https://image-colour-compression-lab.streamlit.app/

## AI assistance

This is an AI-assisted learning project. I used ChatGPT to help generate and explain code, then reviewed the implementation, ran tests, explored the underlying concepts, and documented what I learned.

## What the project covers

- Image representation as NumPy arrays
- RGB colour quantization
- Euclidean and Manhattan distance
- NumPy broadcasting for pairwise calculations
- Brute-force K-nearest-neighbour search
- K-means clustering and K-means++ initialization
- Reconstruction error, inertia, silhouette score, and runtime
- Feature scaling in distance-based algorithms
- Curse of dimensionality
- PCA using covariance-matrix eigendecomposition
- Comparisons with scikit-learn
- Runtime and complexity analysis
- Interactive delivery with Streamlit

## Main verified findings

- Increasing `k` from 4 to 32 reduced RGB reconstruction MSE from `185.61` to `6.87`, but runtime increased from approximately `6.47` to `52.24` seconds.
- K-means++ produced lower average inertia and MSE than random initialization in the repeated initialization experiment.
- The first principal component explained approximately `99.82%` of the variance in the sampled RGB pixels.
- Adding irrelevant dimensions increased the nearest-to-farthest distance ratio from `0.007` to `0.705` and reduced KNN stability from `1.000` to `0.028`.

## Notes

- [Image Data and Colour Quantization](Image%20Data%20and%20Colour%20Quantization.md)
- [Distance Metrics, Broadcasting, and KNN](Distance%20Metrics,%20Broadcasting,%20and%20KNN.md)
- [K-Means from Scratch and Initialization](K-Means%20from%20Scratch%20and%20Initialization.md)
- [Compression Evaluation and k Trade-offs](Compression%20Evaluation%20and%20k%20Trade-offs.md)
- [Feature Scaling and the Curse of Dimensionality](Feature%20Scaling%20and%20the%20Curse%20of%20Dimensionality.md)
- [PCA from Scratch and Explained Variance](PCA%20from%20Scratch%20and%20Explained%20Variance.md)
- [Scikit-Learn Comparison, Complexity, and Interactive Delivery](Scikit-Learn%20Comparison,%20Complexity,%20and%20Interactive%20Delivery.md)

## Key takeaway

Distance-based algorithms are simple to describe but their behaviour depends strongly on feature representation, scale, initialization, dimensionality, and evaluation criteria. Implementing their mathematical cores helped me understand what professional libraries automate.