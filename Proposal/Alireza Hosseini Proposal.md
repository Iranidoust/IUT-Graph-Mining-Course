# Graph Mining Course Project Proposal

**Submission Date:** 1404/09/16  
**Course:** Graph Mining  
**Instructor:** Dr. Zeinab Maleki  

---

## Student Information
- **Student Name:** Alireza Hosseini  
- **Student ID:** 40007513  
- **Email:** a.hosseini@ec.iut.ac.ir  

---

## Project Title
**Learning Variable Branching Strategies in Mixed-Integer Linear Programming Using Graph Convolutional Neural Networks (GCNNs)**

---

## Abstract
Mixed-Integer Linear Programming (MILP) is a fundamental optimization framework used in logistics, scheduling, finance, and engineering. A core bottleneck in solving MILPs via Branch-and-Bound is selecting the variable on which to branch, as this decision heavily influences the size and depth of the search tree. Recent advances have shown that MILP instances can be represented as bipartite graphs consisting of variable and constraint nodes, enabling the use of Graph Neural Networks (GNNs) for learning branching strategies.  

This project proposes a simplified GCNN-based model that learns branching scores by imitating strong branching—an expensive but highly effective heuristic. The expected outcome is a prototype model that demonstrates how graph-based learning can improve decision quality in heuristic-guided optimization.

---

## Problem and Motivation
The efficiency of Branch-and-Bound algorithms strongly depends on the choice of branching variables. Traditional heuristics such as Most Fractional or Pseudocost branching operate using fixed manual rules that may fail to generalize across diverse MILP structures.  

However, the structure of an MILP naturally forms a bipartite graph: variable nodes represent decision variables and constraint nodes represent linear constraints, with edges capturing coefficient relationships. This graphical formulation allows us to leverage message-passing mechanisms from GNNs to capture neighborhood influence, variable-constraint interactions, and structural dependencies that classical heuristics overlook.

From a graph mining perspective, this problem encapsulates multiple core concepts: graph representation learning, node-level prediction, heterogeneous graph processing, and message passing. Additionally, the potential real-world impact is substantial—faster MILP solvers improve performance in transportation networks, supply chain design, task scheduling, energy grid optimization, and scientific simulation.

Motivated by these ideas, this project aims to implement a minimal GCNN that learns to approximate strong branching and examine whether graph-based learning improves branching quality on small synthetic datasets.

---

## Objectives
1. Convert MILP instances into bipartite graphs with normalized variable and constraint node features.  
2. Implement a lightweight GCNN model for predicting variable branching scores.  
3. Train the model via supervised learning using strong branching as ground truth.  
4. Compare the learned branching policy against classical baselines such as Most Fractional.  

---

## Related Work
- **Gasse et al. (2019)** introduced bipartite graph representation for MILP and trained GCNNs to imitate strong branching, reducing search tree size.
- **ML4CO Competition (2021)** showed graph-based learning methods outperform many handcrafted heuristics on heterogeneous MILPs.
- Other related studies explore GNNs for NP-complete problems and graph-based optimization.

Our project adapts these ideas into a smaller-scale implementation suitable for a course setting.

---

## Proposed Methodology

### Dataset(s)
- MILP instances generated via **Ecole** (IndependentSetGenerator)  
- Size: ~20–50 variable nodes, ~20–60 constraint nodes  
- Dataset: ~200 for training, 50 for evaluation  
- Preprocessing:
  - Normalize node features  
  - Extract bipartite adjacency lists  
  - Generate strong branching labels  

### Techniques and Algorithms
- **Graph Representation:** Bipartite MILP graphs (variable–constraint)
- **GCNN Architecture:** Message passing between heterogeneous node types
- **Learning Strategy:** Supervised regression of strong branching scores
- **Tools:** PyTorch Geometric, Ecole, SCIP

### Evaluation Plan
- **Metrics:** MSE, ranking correlation, optional solver-based metrics (e.g., tree size)
- **Baselines:** Random branching, Most Fractional branching

---

## Challenges and Resources
- Strong branching label generation is computationally expensive.
- Solution: restrict dataset to small synthetic instances and use shallow GCNN architectures.
- Data integration between PyTorch Geometric and Ecole may introduce complexity → modular preprocessing pipeline will be used.

---

## References
- Gasse, M., Chételat, D., Ferroni, N., Charlin, L., & Lodi, A. (2019). *Learning to branch in mixed integer programming*. NeurIPS. https://arxiv.org/abs/1906.01366
- Nair, V., et al. (2021). *Is GCNN all you need?* ML4CO Competition. https://arxiv.org/abs/2112.12251
- Gasse, M., et al. (2020). *Machine learning for combinatorial optimization: A methodological tour d’horizon*. https://arxiv.org/abs/2003.04673
- Prates, M. O., et al. (2019). *Learning to solve NP-complete problems: A GNN for decision TSP*. AAAI. https://arxiv.org/abs/1809.02721
- Ecole Documentation. https://doc.ecole.ai
- PyTorch Geometric Documentation. https://pytorch-geometric.readthedocs.io/

---
