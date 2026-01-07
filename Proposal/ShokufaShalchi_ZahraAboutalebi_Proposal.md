# Graph Mining Course Project Proposal

**Submission Date:** 12/11/2025  
**Course:** Graph Mining 4041  
**Instructor:** Dr. Zeinab Maleki  

## Student Information
- **Student Name(s):** Shokufa Shalchi, Zahra Aboutalebi  
- **Student ID(s):** 40124913, 40116943  
- **Email(s):** s.shalchi@ec.iut.ac.ir, z.abootalebi@ec.iut.ac.ir  

## Project Title
Improving Node Classification in Heterophilic Graphs Using Similarity-Weighted H2GCN

## Abstract
Graph Neural Networks (GNNs) often assume that connected nodes are similar (homophily), but this is not true in many real-world networks. The H2GCN model improves node classification by aggregating information from higher-order neighbors while separating ego and neighbor features. In this project, we extend H2GCN by adding a similarity-weighting mechanism ( we try :), so nodes prioritize messages from feature-similar neighbors. We evaluate this model on the GitHub Developer graph, where users are labeled as Web or ML developers. finally we try to test different similarity metrics (e.g., cosine, Euclidean) and compare results against the original H2GCN and GCN models.

## Problem and Motivation
Traditional GNNs like GCN and GraphSAGE assume that connected nodes are similar, which works well in homophilic graphs like citation networks. However, in many real-world graphs — such as GitHub’s social network — users with very different roles follow each other (heterophily). In such graphs, standard GNNs may aggregate noisy or misleading information, reducing classification performance. This issue motivates models that can intelligently filter or weigh neighbor contributions based on similarity. H2GCN addresses this by aggregating higher-order neighbors and separating ego-node features. We propose to enhance this approach using feature similarity — so that nodes learn from those who are actually similar in behavior or content, not just topologically connected. This has practical applications in social recommendation systems, developer classification, and trust analysis in networks. By extending H2GCN with a simple yet powerful similarity-based weighting, we aim to bridge a key gap in heterophilic graph learning.
POV: H2GCN looked interesting and useful^^

## Objectives
- Reproduce and evaluate the original H2GCN model on the GitHub Developer graph ( main goal ).
- Integrate feature similarity (cosine, Jaccard, Euclidean) into neighbor aggregation.
- Compare the improved model with baseline GCN and H2GCN in terms of accuracy and F1-score.
- Analyze the effect of different similarity functions on performance.

## Related Work
The H2GCN model (Zhu et al., NeurIPS 2020) addresses GNN limitations in heterophilic graphs by incorporating higher-order neighborhoods and separating ego and neighbor representations. Geom-GCN (Pei et al., ICLR 2020) introduces geometric distances to capture structurally similar but unconnected nodes. Our approach is most closely aligned with H2GCN, but introduces a lightweight similarity-based weighting mechanism for aggregation. This is inspired by the broader idea behind SNGNN, where feature similarity guides neighborhood selection. Our work simplifies and extends this idea for improved performance on real-world social networks like GitHub.


## Proposed Methodology
This project builds upon the H2GCN model to improve node classification performance in heterophilic graphs by incorporating feature similarity into the message-passing process. Our approach consists of the following components:

### Dataset(s)
Name: GitHub Developer Graph
Source: Built into PyTorch Geometric
Size: ~37,700 nodes, ~289,000 edges
Type: Undirected, social network
Labels: Web vs. ML developer
Preprocessing: Normalize features, convert to PyG format, remove isolated nodes if any

### Techniques and Algorithms
- H2GCN (Baseline):
  Aggregates 1-hop and 2-hop neighbors while separating ego and neighbor features. Designed for heterophilic graphs.
- Similarity-Weighted Aggregation (Our Extension):
  Reweights neighbor messages using feature similarity (e.g., cosine, Jaccard) to prioritize more relevant neighbors.
- GCN (Baseline):
  Standard GNN that assumes homophily. Used for comparison.
- Tools:
  PyTorch, PyTorch Geometric, NumPy, SciPy, Matplotlib

### Evaluation Plan
- Metrics: Accuracy, F1-score, and confusion matrix.
- Baselines: Original H2GCN and GCN without similarity weighting.
- Ablation Study: Compare different similarity functions.

## Challenges and Resources
A key challenge is computing similarity efficiently, especially on large graphs. We will limit similarity computation to direct neighbors to save time and memory.

## References
[1] J. Zhu, Y. Yan, L. Zhao, M. Heimann, L. Akoglu and D. Koutra, "Beyond Homophily in Graph Neural Networks: Current Limitations and Effective Designs," Advances in Neural Information Processing Systems (NeurIPS), 2020. [Online]. Available: https://arxiv.org/abs/2006.11468

[2] H. Pei, B. Wei, K. C.-C. Chang, Y. Lei and B. Yang, "Geom-GCN: Geometric Graph Convolutional Networks," International Conference on Learning Representations (ICLR), 2020. [Online]. Available: https://arxiv.org/abs/2002.05287

[3] M. Zou, Z. Gan, R. Cao, C. Guan, and S. Leng, “Similarity-Navigated GNNs for Node Classification in Heterophilic Graphs,” Information Sciences, vol. 633, pp. 172–185, 2023. [Online]. Available: https://doi.org/10.1016/j.ins.2023.02.103

**Student Signature(s):** [shokufa shalchi / Zahra Aboutalbei]  
[12/11/2025]
