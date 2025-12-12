# Graph Mining Course Project Proposal

**Submission Date:** 12/11/2025
**Course:** Graph Mining [4041]
**Instructor:** Dr. Zeinab Maleki

## Student Information
- **Student Name(s):** Arman Abbasi, Mohammad bazazzadeh
- **Student ID(s):** 40226263,40218043
- **Email(s):** arman.abbasi94@gmail.com, m.bazazzadeh84@gmail.com

## Project Title
**Explaining GNN-Based Link Prediction Using Feature and Structural Perturbation (Z-REx Implementation)**

## Abstract
Modern recommender systems increasingly rely on Graph Neural Networks (GNNs) due to their ability to model complex relationships through link prediction. However, these systems act as black boxes, limiting transparency and trust. This project aims to reproduce a simplified version of Z-REx, a 2025 framework designed to provide human-interpretable explanations for GNN-based recommendations. Our approach focuses on identifying the important node features and critical subgraph structures that influence link predictions. Using a heterogeneous user–item graph constructed from a public dataset, we will perform feature perturbation and structural perturbation to quantify the impact of specific attributes and edges on recommendation outcomes. The expected result is an explainability module that improves interpretability while preserving recommendation quality.

## Problem and Motivation
Recommender systems based on GNNs generate high-quality predictions by leveraging graph-structured data, but they lack transparency, especially in link prediction tasks, where the goal is to determine how strongly a user should be connected to an item. The black-box nature of GNNs makes it difficult for users and system administrators to understand why a specific recommendation is generated, reducing trust and limiting use in high-stakes environments.

This project focuses on the explainability problem in heterogeneous link prediction. Existing explainers struggle with scalability, heterogeneity, and producing human-interpretable explanations. Z-REx proposes domain-aware perturbation techniques to identify influential features and edges that most contribute to a recommendation.

We will adapt this method to a small-scale public dataset and evaluate whether Z-REx-style explanations can be generated effectively. The motivation is to bridge the gap between accurate GNN models and interpretable graph-based recommendations, enabling responsible and trustworthy recommender systems.

## Objectives
- Implement a simplified GNN-based link prediction model on a public user–item dataset.
- Reproduce the Z-REx feature perturbation method to rank important item attributes.
- Implement structural perturbation to identify influential edges in a k-hop subgraph.
- Compare explanations with baselines such as GNNExplainer or simple heuristics.

## Related Work
GNNExplainer introduced one of the earliest methods for identifying important subgraphs for GNN predictions, but it struggles with large or heterogeneous graphs. SubgraphX uses Monte-Carlo tree search to identify informative subgraphs but suffers from scalability issues. The Z-REx framework specifically addresses the challenges of explaining link prediction for heterogeneous recommendation graphs using perturbation-based explanations. Our project builds directly on Z-REx by implementing a version suitable for smaller datasets and academic experimentation while comparing it to one general GNN explainer.

## Proposed Methodology

### Dataset(s)
The original Z-REx paper uses a proprietary real-estate interaction dataset from Zillow, which is not publicly available. Therefore, for this course project we will instead use a public, open-source dataset such as MovieLens 100K or Amazon Books. These datasets provide user–item interactions, item features, and sufficient structure to construct a heterogeneous user–item graph suitable for link prediction and explanation.

### Techniques and Algorithms
- **GNN-Based Link Prediction:** Implement LightGCN or a 2-layer RGCN. Train using positive and negative sampled user–item pairs.
- **Feature Perturbation:** Zero out one item feature at a time, measure change in nDCG@K, and rank features.
- **Structural Perturbation:** Extract the k-hop subgraph around a user, remove edges one-by-one, measure cosine similarity change, and rank edge influence.
- **Tools:** PyTorch Geometric or DGL, NetworkX for graph manipulation, FAISS optional.

### Evaluation Plan
- **Recommendation quality:** nDCG@K, Recall@K
- **Explainability quality:** Fidelity measured as decrease in nDCG when important features/edges are removed
- **Baselines:** GNNExplainer and random perturbation controls

## Challenges and Resources
Challenges include constructing heterogeneous graphs, performing perturbations efficiently, and scaling structural perturbation. We mitigate this by using small datasets, limiting k-hop neighborhood size, and simplifying Z-REx operations.

## References
- Mukherjee, K., Harrison, Z., & Balaneshin, S. (2025). Z-REx: Human-Interpretable GNN Explanations for Real Estate Recommendations.
- Ying, Z., Bourgeois, D., You, J., Zitnik, M., & Leskovec, J. (2019). GNNExplainer: Generating Explanations for Graph Neural Networks.
- Yuan, H., Yu, H., Wang, J., & Ji, S. (2021). On Explainability of GNNs via Subgraph Exploration.
- He, X., et al. (2020). LightGCN: Simplifying and Powering Graph Convolution for Recommendation.
