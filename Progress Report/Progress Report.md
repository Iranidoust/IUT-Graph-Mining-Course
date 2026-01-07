# Graph Mining Course Project Progress Report  

**Submission Date:** 15/01/2026  
**Course:** Graph Mining [4041]  
**Instructor:** Dr. Zeinab Maleki  
**Project Title:** **Explaining GNN-Based Link Prediction Using Feature and Structural Perturbation (Z-REx Implementation)**  

---

## Student Information
- **Student Name(s):** Arman Abbasi, Mohammad Bazazzadeh  
- **Student ID(s):** 40226263, 40218043  
- **Email(s):** arman.abbasi94@gmail.com, m.bazazzadeh84@gmail.com  

---

## Executive Summary

Since the submission of the project proposal, we have made substantial progress toward implementing a simplified and educational reproduction of the Z-REx explainability framework for GNN-based link prediction. Our primary focus during this phase was to design and validate the core explainability mechanisms—feature perturbation and structural perturbation—independent of large-scale model training. We successfully implemented a prototype system in a Jupyter notebook that constructs a small heterogeneous user–item graph, simulates GNN-style node embeddings, and applies perturbation-based explanations to analyze recommendation decisions.

While the original plan included training a full GNN model on a public dataset, we intentionally deferred end-to-end model training to prioritize conceptual correctness and interpretability of explanations. This deviation aligns with the course scope and allowed us to validate the explanation pipeline in a controlled setting. Preliminary results demonstrate that perturbing specific item features or removing influential edges leads to measurable changes in link prediction scores, supporting the fidelity of the explanation approach. The current implementation closely aligns with the objectives outlined in the proposal and serves as a strong foundation for integrating real datasets and trained GNN embeddings in the final phase of the project.

---

## Progress on Objectives

### Objective 1: Implement a GNN-based link prediction model on a public user–item dataset

Progress toward this objective is **partially complete**. Instead of training a full GNN model at this stage, we implemented a simplified link prediction framework using simulated node embeddings. These embeddings act as placeholders for outputs typically produced by models such as LightGCN or RGCN. The link prediction score is computed using cosine similarity between user and item embeddings, a common technique in recommender systems. This abstraction allowed us to focus on explanation logic while maintaining compatibility with future integration of trained GNN models.

---

### Objective 2: Reproduce Z-REx feature perturbation to rank important item attributes

This objective has been **fully achieved** at the prototype level. We implemented feature perturbation by iteratively zeroing out individual item features and measuring the resulting change in link prediction score. This simulates counterfactual scenarios where a specific item attribute is absent. Feature importance is quantified by the magnitude of score change, producing a ranked list of influential features. This approach closely follows the core idea of Z-REx and enables human-interpretable explanations.

---

### Objective 3: Implement structural perturbation to identify influential edges in a k-hop subgraph

This objective is **largely completed**. Structural perturbation is implemented by removing edges in the local neighborhood of a user–item pair and observing changes in embedding similarity. The graph is modeled using NetworkX, enabling efficient manipulation and visualization. Preliminary results show that removing certain edges leads to significant changes in prediction scores, indicating their influence on the recommendation outcome.

---

## Work Accomplished

### Dataset Preparation and Analysis

Rather than immediately adopting a large public dataset, we constructed a controlled synthetic user–item graph to validate explanation mechanisms. The graph includes multiple users and items connected by interaction edges, reflecting a heterogeneous recommendation setting. Item nodes are assigned explicit feature vectors representing interpretable attributes such as category indicators or popularity proxies. This setup simplifies debugging and ensures clarity in explanation outputs.

---

### Implementation Details

The prototype is implemented in Python using the following tools:

- **NetworkX:** Graph construction and structural perturbation  
- **NumPy:** Numerical computations  
- **scikit-learn:** Cosine similarity for link prediction scoring  

Node embeddings are manually initialized to simulate GNN outputs. Feature perturbation is implemented by modifying item feature vectors, while structural perturbation is performed by temporarily removing edges from the graph. Visualization is used to illustrate graph structure and explanation results.

---

### Preliminary Results

Initial experiments demonstrate that both feature and structural perturbations produce meaningful changes in link prediction scores. Certain item features consistently result in larger score drops when removed, indicating higher importance. Similarly, removing specific edges leads to noticeable changes in predicted similarity.

| Metric                        | Value (Prototype) | Notes                                      |
|-------------------------------|-------------------|--------------------------------------------|
| Max score change              | High              | After removing key item features           |
| Influential edges detected    | Yes               | Via local structural perturbation           |
| Explanation interpretability  | High              | Features and edges are human-readable      |

---

## Challenges Encountered and Resolutions

- **Challenge 1:** Training and tuning a full GNN model within limited project time  
  **Resolution:** We decoupled explanation logic from model training by using simulated embeddings, allowing us to validate Z-REx-style explanations independently.

- **Challenge 2:** Maintaining interpretability in a heterogeneous graph  
  **Resolution:** We focused on item-side feature explanations and edge-level structural perturbations, which align with practical recommender system explanations.

---

## References

1. Mukherjee, K., Harrison, Z., & Balaneshin, S. (2025). *Z-REx: Human-Interpretable GNN Explanations for Real Estate Recommendations*.  
2. Ying, Z., Bourgeois, D., You, J., Zitnik, M., & Leskovec, J. (2019). *GNNExplainer: Generating Explanations for Graph Neural Networks*. NeurIPS.  
3. He, X., et al. (2020). *LightGCN: Simplifying and Powering Graph Convolution for Recommendation*. SIGIR.  
4. Yuan, H., et al. (2021). *On Explainability of Graph Neural Networks via Subgraph Exploration*. ICML.

---

**Student Signature(s):**  
Arman Abbasi  
Mohammad Bazazzadeh  

**Date:** 15/01/2026  
