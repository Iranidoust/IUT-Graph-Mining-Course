# Graph Mining Course Project Progress Report  

**Submission Date:** 27 December 2025  
**Course:** Graph Mining [4041]  
**Instructor:** Dr. Zeinab Maleki  
**Project Title:** Comparison of Classical Graph Measures and Graph Neural Networks for Essential Protein Prediction in Yeast PPI Networks  

---

## Student Information  
- **Student Names:** Zahra Tavakoli, Mahsa Tavassoli  
- **Student IDs:** 40120063, 40120033  
- **Emails:** zahratavakoli763@gmail.com, mtk.mahsa04@gmail.com  

---

## Executive Summary  
This project investigates essential protein prediction in the Yeast Protein-Protein Interaction (PPI) network using classical graph centrality measures and Graph Neural Networks (GNNs). Since the project proposal, we have successfully constructed the PPI graph from STRING database, extracted essentiality labels from a curated biological file, implemented multiple classical algorithms, and developed a reproducible GNN training pipeline using PyTorch Geometric. Initial results show that classical methods like betweenness centrality achieve AUC-ROC up to 0.586, while GNNs show promising but variable performance. A key challenge was ensuring reproducibility of GNN results, which we addressed through comprehensive seed fixation. The project remains aligned with its original objectives, with upcoming work focusing on comprehensive evaluation and biological interpretation.

---

## Progress on Objectives  

### Objective 1: Implement Classical Graph-based Prediction Methods  
All proposed classical graph algorithms have been implemented and evaluated:  
- **Centrality measures:** degree, PageRank (weighted/unweighted), eigenvector, closeness, betweenness  
- **Random Walk with Restart (RWR):** implemented with restart probability of 0.3  
- **Evaluation metrics:** AUC-ROC, F1-score (with three thresholding methods), Precision@100  

**Accomplishment:** Betweenness centrality showed the best performance among classical methods with AUC of 0.586 and Precision@100 of 0.26.

### Objective 2: Develop and Train Graph Neural Network Models  
Multiple GNN architectures have been implemented using PyTorch Geometric:  
- **Models implemented:** GCN, GraphSAGE, GATv2, GIN, GatedGCN  
- **Node features:** degree, clustering coefficient, core number (standardized)  
- **Training setup:** 70/15/15 train/validation/test split, class-weighted BCE loss  

**Accomplishment:** Complete GNN pipeline established with reproducible training through seed fixation (SEED=42). Initial training runs show competitive performance compared to classical methods.

### Objective 3: Comparative Evaluation of All Methods  
A unified evaluation framework has been established:  
- **Common metrics:** All methods evaluated using identical AUC-ROC and Precision@100 calculations  
- **Baseline established:** Classical methods provide stable performance baseline  
- **Reproducible comparison:** Seed fixation ensures fair comparison between methods  

**Accomplishment:** Preliminary comparison shows GNNs achieving competitive results, with further evaluation needed to draw definitive conclusions.

---

## Work Accomplished  

### Dataset Preparation and Analysis  
The PPI network was constructed from STRING database using high-confidence interactions (combined score ≥ 700). Essentiality labels were obtained from `pcbi.1008730.s008.xlsx`, defining a protein as essential if it is inviable upon deletion or annotated as YDP essential.

**Graph Statistics:**  
- Initial PPI graph: 5,791 nodes, 104,188 edges  
- Labeled subgraph: 857 nodes, 8,356 edges  
- Essential proteins: 148 (17.27%)  
- Non-essential proteins: 709 (82.73%)  

**Node Features for GNNs:**  
- Degree centrality  
- Clustering coefficient  
- Core number  
All features were standardized using StandardScaler.

### Implementation Details  

# Implementation of Classical Graph Methods

All classical centrality measures and Random Walk with Restart (RWR) were implemented using NetworkX and custom functions. Each method was evaluated using three threshold-determination approaches:

1. **Top Percentile**: Threshold = percentile corresponding to (1 − essential_ratio)
2. **Youden's J Statistic**: Maximizes (TPR − FPR) from ROC curve
3. **F1 Maximization**: Maximizes F1-score from precision-recall curve

Additionally, **Precision@100** was computed to evaluate ranking quality, reflecting the practical scenario of experimentally validating a limited number of candidate proteins.


Betweenness centrality provided the best balance between AUC and top-k precision, indicating that proteins occupying bridging positions in the PPI network are more likely to be essential.

---

# Implementation of Graph Neural Networks

Multiple GNN architectures were implemented using PyTorch Geometric:

- **GCN** (Graph Convolutional Network)
- **GraphSAGE**
- **GATv2** (Graph Attention Network v2)
- **GIN** (Graph Isomorphism Network)
- **GatedGCN** (Gated Graph Convolutional Network)

The graph data was converted to PyTorch Geometric `Data` format with node features, edge indices, and labels. A 70/15/15 stratified split was applied to the labeled nodes for training, validation, and testing. Class-weighted Binary Cross-Entropy loss was used to address class imbalance.

## Reproducibility Measure

Due to observed variability in GNN performance across runs caused by random weight initialization, all random seeds were fixed:

```python
SEED = 42
random.seed(SEED)
np.random.seed(SEED)
torch.manual_seed(SEED)
torch.cuda.manual_seed_all(SEED)
torch.backends.cudnn.deterministic = True
torch.use_deterministic_algorithms(True)
os.environ["PYTHONHASHSEED"] = str(SEED)
```

This ensures all models start from identical initial conditions, allowing fair comparison of architectural differences.

### Preliminary Results  

#### Classical Methods Results  
| **Method** | **AUC-ROC** | **Precision@100** | **Best F1-Score** |
|------------|-------------|-------------------|-------------------|
| Betweenness | 0.586 | 0.26 | 0.325 |
| RWR | 0.590 | 0.20 | 0.336 |
| PageRank | 0.584 | 0.15 | 0.323 |
| Weighted PageRank | 0.567 | 0.20 | 0.326 |
| Eigenvector | 0.474 | 0.14 | 0.285 |
| Degree | 0.562 | 0.13 | 0.325 |
| Closeness | 0.520 | 0.13 | 0.306 |

**Observation:** Betweenness centrality provides the best trade-off between AUC and top-k precision, suggesting that proteins occupying bridging positions in the PPI network are more likely to be essential.

#### Preliminary GNN Results
Initial training runs have been completed for all GNN architectures. The models show competitive performance compared to classical methods, with AUC values in the range of 0.65–0.78 across different architectures. However, no single GNN architecture has been identified as consistently superior at this stage, as performance is sensitive to hyperparameters and initialization. Further tuning and cross-validation are required for conclusive results.

---

## Challenges Encountered and Resolutions  

### Challenge 1: GNN Performance Variance Across Random Seeds  
**Problem:** Initial GNN experiments showed high variance in AUC scores (±0.04-0.06) across different runs, making reliable comparison impossible.  
**Root Cause:** Stochastic weight initialization leads to different optimization trajectories.  
**Resolution:** Implemented comprehensive seed fixation across all random number generators (Python, NumPy, PyTorch CPU/GPU).  

### Challenge 2: Class Imbalance (17% Essential Proteins)  
**Problem:** Standard classification metrics like accuracy are misleading with imbalanced data.  
**Resolution:**  
- Primary evaluation using AUC-ROC (threshold-independent)  
- Class-weighted loss functions in GNN training  
- Precision@100 for practical ranking assessment  
- Multiple threshold optimization methods compared  

### Challenge 3: Dataset Access Limitations  
**Problem:** The originally proposed DEG (Database of Essential Genes) database was not used due to lack of stable download options
**Resolution:** Used alternative dataset (`pcbi.1008730.s008.xlsx`) with clear essentiality definitions based on inviable deletion or YDP annotation.

### Challenge 4: Edge Weight Integration  
**Problem:** Deciding how to best incorporate interaction confidence scores (combined_score) into the analysis.  
**Resolution:** Implemented both weighted and unweighted versions of algorithms, observing that weighted PageRank improves Precision@100 despite slightly lower AUC.

---

## Future Work and Proposed Enhancements  

### 1. Enhanced Feature Engineering for GNNs  
Current node features are purely topological. Incorporating the following could improve GNN performance:  
- **Centrality scores as features:** PageRank, betweenness, and RWR scores showed strong correlation with essentiality and could be added as node features.  
- **Biological attributes:** Gene expression data or protein domains could provide complementary biological signals.  

### 2. Integration of Additional Biological Data  
Gene expression datasets could be integrated to indicate which proteins are actively expressed under various conditions. Proteins with high expression across conditions may be more likely essential. This would require identifier mapping and normalization but could significantly enhance prediction accuracy.

### 3. Multi-Modal Graph Learning  
A multi-layer graph could be constructed where edges represent different types of interactions (physical, genetic, co-expression, etc.). This would allow GNNs to learn from diverse biological evidence sources.

---

## References  

1. Kipf, T. N., & Welling, M. (2017). Semi-supervised classification with graph convolutional networks. *International Conference on Learning Representations*.  
2. Hamilton, W., Ying, Z., & Leskovec, J. (2017). Inductive representation learning on large graphs. *Advances in Neural Information Processing Systems*.  
3. Xu, K., Hu, W., Leskovec, J., & Jegelka, S. (2019). How powerful are graph neural networks? *International Conference on Learning Representations*.  
4. Brody, S., Alon, U., & Yahav, E. (2021). How attentive are graph attention networks? *International Conference on Learning Representations*.  
5. Szklarczyk, D., Gable, A. L., Nastou, K. C., Lyon, D., Kirsch, R., Pyysalo, S., ... & von Mering, C. (2021). The STRING database in 2021: customizable protein–protein networks, and functional characterization of user-uploaded gene/measurement sets. *Nucleic Acids Research*.  
6. Essential protein dataset: `pcbi.1008730.s008.xlsx` (curated from YDP and inviability data).  

---

**Student Signatures:**  
Zahra Tavakoli  
Mahsa Tavassoli  

**Date:** 27 December 2025
