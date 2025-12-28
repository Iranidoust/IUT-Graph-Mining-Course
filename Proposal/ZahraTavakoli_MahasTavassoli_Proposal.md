# Graph Mining Course Project Proposal
**Submission Date:** 20 December 2025  
**Course:** Graph Mining [4041]  
**Instructor:** Dr. Zeinab Maleki

## Student Information
- **Student Names:** Zahra Tavakoli, Mahsa Tavassoli
- **Student IDs:** 40120063, 40120033
- **Emails:** zahratavakoli763@gmail.com, mtk.mahsa04@gmail.com
## Project Title
Comparing Classical Graph Measures and Graph Neural Networks for Essential Protein Prediction in Yeast PPI Networks

## Abstract
This project investigates the prediction of essential proteins in a Yeast Protein–Protein Interaction (PPI) network by analyzing graph topology. The PPI network is constructed from STRING, and essentiality labels are obtained from the DEG database. After harmonizing identifiers, each protein node receives a binary essentiality label. We compare two families of methods under identical conditions: (1) classical graph-based centrality measures such as degree, PageRank, eigenvector, closeness, and betweenness, and (2) Graph Neural Networks including GCN, GATv2, GraphSAGE, GIN, and GatedGCN trained with structural node features. The goal is to assess whether lightweight GNNs offer measurable improvement over classical ranking-based methods. Expected outcomes include a reproducible benchmark and insights into strengths and limitations of each approach.

## Problem and Motivation
Essential proteins are critical for cell survival, and identifying them supports drug target discovery and functional analysis in systems biology. Given that many biological processes can be modeled as networks, graph mining offers tools to uncover patterns that correlate with protein essentiality. Classical centrality measures are intuitive and computationally efficient but limited to predefined heuristics. In contrast, Graph Neural Networks learn structural patterns adaptively and can model multihop dependencies in the PPI graph.  
In the context of this course, the comparison between classical graph analytics and GNN-based supervised node classification highlights how graph representation learning may or may not outperform traditional metrics. This project uses a real biological dataset (STRING PPI combined with DEG labels) and incorporates course concepts such as centrality, random walks, and node classification. The motivation is to build a clean, interpretable, and feasible experimental setup that demonstrates practical differences between these approaches.

## Objectives
- Evaluate classical graph-based centrality measures for essential protein ranking.
- Implement multiple GNN architectures trained on structural node features.
- Compare classical and GNN methods using both ranking and classification metrics.
- Produce a reproducible, well-documented benchmark on the Yeast PPI dataset.

## Related Work
Classical essential protein prediction studies often rely on centrality metrics such as degree or PageRank, showing that highly connected proteins tend to be essential (Jeong et al., 2001). More recent work applies graph embedding and GNN-based architectures to biological networks, demonstrating improved performance in node classification tasks (Kipf & Welling, 2017; Hamilton et al., 2017). Studies integrating STRING and DEG datasets also show that structural graph features correlate with essentiality. Our project builds on these works by performing a controlled comparison between classical measures and several GNN variants under uniform preprocessing, features, and evaluation settings to highlight practical differences in performance and complexity.

## Proposed Methodology
### Dataset(s)
- **STRING Yeast PPI Network** (Saccharomyces cerevisiae):
    - Format: protein1, protein2, combined_score
    - Preprocessing: retain edges with score ≥ 700, remove self-loops and duplicates, convert to unweighted undirected graph.
- **DEG Essentiality Labels:**
    - Format: protein_id, essentiality
    - Labels: essential = 1, non-essential = 0
- **Label Assignment:** Harmonize STRING and DEG identifiers, label nodes accordingly, remove isolated nodes.
### Techniques and Algorithms
**Classical Graph-Based Metrics (NetworkX):**
- Degree, Eigenvector, PageRank, Closeness, Betweenness (approximate), Random Walk with Restart.
- Output: ranking scores for each protein.

**Graph Neural Networks (PyTorch Geometric):**
- GCN, GATv2, GraphSAGE, GIN, GatedGCN.
- Features: degree, local clustering coefficient, k-core number.
- Training: BCE loss, Adam optimizer, 70/15/15 stratified split, 50–150 epochs.
### Evaluation Plan
- **Classification metrics:** AUC-ROC, F1-score.
- **Ranking metrics:** Precision@K (K=100).
- **Baselines:** all classical centrality scores.
- **GNN outputs:** predicted essentiality probabilities.

## Challenges and Resources
Main challenges include mapping identifiers between STRING and DEG and handling nodes missing label information. GAT-based models may be computationally heavier. Mitigation includes careful preprocessing, approximate centrality computation, and prioritizing efficient GNN architectures such as GCN and GraphSAGE.

## References
- Kipf, T., & Welling, M. (2017). Semi-supervised classification with graph convolutional networks.
- Hamilton, W., Ying, Z., & Leskovec, J. (2017). Inductive representation learning on large graphs.
- Xu, K., et al. (2019). Graph Isomorphism Networks.
- Brody, S., et al. (2021). GATv2.
- Li, Y., et al. (2016). Gated Graph Neural Networks.
- STRING Database, DEG Database.
