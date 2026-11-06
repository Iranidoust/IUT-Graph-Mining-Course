# Graph Mining Course Project Proposal

**Submission Date:** November 18, 2025  
**Course:** Graph Mining \[4041]  
**Instructor:** Dr. Zeinab Maleki

## Student Information

* **Student Name:** Mohammadreza Najafi
* **Student ID:** 40132333
* **Email:** [mohamadrezanajafi8282@gmail.com](mailto:mohamadrezanajafi8282@gmail.com)

## Project Title

**"Hybrid Link Prediction on Wiki-CS using GCN and Node2Vec Embeddings"**

## Abstract

This project investigates the task of **link prediction** on the **Wiki-CS dataset**, using a combination of **Graph Neural Networks (GCN)** and **Node2Vec embeddings**. Three models will be implemented and compared: (1) a GCN-based local structural model, (2) an embedding-only MLP model using Node2Vec, and (3) a **hybrid model**, inspired by Gupta et al. (2021), which integrates both transductive (Node2Vec) and inductive (GCN) representations. The study aims to evaluate whether combining global structural embeddings with local neighborhood-aware GCN embeddings leads to improved performance. Evaluation metrics include **AUC**, **Accuracy**, and **F1-score**.

## Problem and Motivation

Link prediction is essential in many graph-based domains such as **social networks**, **biology**, and **recommender systems**. Understanding whether two nodes are likely to form a connection has important applications in predicting missing or future interactions.

The motivation behind this project comes from the need to effectively combine both **local** and **global** structural information in graphs. While **GCN** captures local neighborhood information, **Node2Vec** encodes global structural similarities. According to Gupta et al. (2021), combining transductive embeddings like Node2Vec with inductive GNN models improves link prediction accuracy. This project explores such hybridization specifically on the **Wiki-CS graph**, where this type of integration has not been extensively evaluated.

## Objectives

* **Objective 1:** Analyze the Wiki-CS dataset and extract structural characteristics such as node degrees, centrality measures, and clustering coefficients.
* **Objective 2:** Implement three models: GCN-only, Node2Vec+MLP, and a hybrid GCN+Node2Vec model.
* **Objective 3:** Compare the performance of all models using AUC, Accuracy, and F1-score.
* **Objective 4:** Investigate how integrating global node embeddings with GCN-based representations affects link prediction performance.

## Related Work

* **Gupta et al. (2021):** Demonstrated that combining **transductive embeddings** (e.g., Node2Vec) with **inductive GNNs** enhances link prediction accuracy. This paper directly motivates the hybrid architecture used in this project.
* **Zhang \& Chen (2018):** Introduced GNN-based methods for link prediction, showing that deep graph models outperform classical heuristics by leveraging node features and graph structure.
* **Grover \& Leskovec (2016):** Proposed **Node2Vec**, a biased random-walk method that generates expressive node embeddings capturing global graph structure.
* **Mernyei \& Cangea (2020):** Introduced the **Wiki-CS dataset**, designing it as a benchmark for GNN research. Their work includes initial baselines for node classification and link prediction.

## Proposed Methodology

### Dataset

* **Dataset:** Wiki-CS (Wikipedia-based citation graph)
* **Nodes:** 11,701
* **Edges:** 216,123 (undirected)
* **Preprocessing:** Remove 10% of edges for testing; generate negative samples from unconnected node pairs.

### Models

#### 1\. GCN Model (Local Structure)

GCN learns representations by aggregating features from a node’s neighborhood. It captures **local structural dependencies**, making it useful for link prediction.

#### 2\. Node2Vec + MLP (Global Structure)

Node2Vec performs biased random walks to extract global structural embeddings. These embeddings are fed into an MLP classifier to predict links.

#### 3\. Hybrid Model (GCN + Node2Vec)

Inspired by Gupta et al. (2021), the hybrid model combines both:

* **GCN embeddings** capturing local structure
* **Node2Vec embeddings** capturing global structure

The embeddings of each node are **concatenated**, and the pairwise concatenation of two nodes is fed into an **MLP classifier** to predict link existence.

This fusion allows the model to leverage both neighborhood information and structural patterns across the entire graph.

### Evaluation Plan

* **Metrics:** AUC, Accuracy, F1-score
* **Baselines:** Node2Vec+MLP, GCN-only
* **Comparison:** Evaluate whether the hybrid model outperforms standalone models.

## Challenges and Resources

* **Challenge 1:** Balancing the importance of global vs. local information. Hyperparameter tuning and ablation studies will be performed.
* **Challenge 2:** Computational cost of training GNNs on large graphs. This will be addressed using **Google Colab GPU acceleration**.

## References

1. Gupta, C., Jain, Y., De, A., \& Chakrabarti, S. (2021). *Integrating Transductive and Inductive Embeddings Improves Link Prediction Accuracy*.
2. Zhang, M., \& Chen, Y. (2018). *Link Prediction Based on Graph Neural Networks*. NeurIPS.
3. Grover, A., \& Leskovec, J. (2016). *node2vec: Scalable Feature Learning for Networks*. KDD.
4. Mernyei, P., \& Cangea, C. (2020). *Wiki-CS: A Wikipedia-Based Benchmark for Graph Neural Networks*. arXiv:2007.02901.

**Student Signature:** Mohammadreza Najafi  
**Date:** November 18, 2025



