# Graph Mining Course Project Proposal

**Submission Date:** November 18, 2026  
**Course:** Graph Mining [4051]  
**Instructor:** Dr. Zeinab Maleki  

## Student Information
- **Student Name(s):** Hossein Karimy Moghaddam, Hossein Ghorbani
- **Student ID(s):** 40412294, 40411654
- **Email(s):** h.karimy@ec.iut.ac.ir, h.ghorbanikahrizsangi@ec.iut.ac.ir  

## Project Title
Neuro-Symbolic Graph Neural Networks: Enhancing Imbalanced Node Classification via Rule-Based Feature Augmentation and Semantic Graph Rewiring

## Abstract
Graph Neural Networks (GNNs) rely heavily on message passing, making them highly dependent on the quality of initial node features and the underlying graph topology. These models often struggle with real-world applications featuring noisy structures and severe class imbalance, such as fraud detection. This project proposes a Neuro-Symbolic approach to address these challenges by integrating explicit rule-based knowledge into GNNs. Building upon the REFUEL framework, symbolic classification rules will be extracted from node attributes. These rules will be utilized in two synergistic ways: (1) augmenting node representations by appending rule-satisfaction logic to feature vectors, and (2) semantically rewiring the graph by establishing virtual edges between structurally disconnected nodes that satisfy similar rules. The expected outcome is a more robust, interpretable model capable of achieving higher classification precision on imbalanced graph datasets.

## Problem and Motivation
The core operation in modern graph mining techniques, particularly Graph Neural Networks, is neighborhood aggregation (Message Passing). However, this mechanism fundamentally assumes that connected nodes are informative and share similar labels (homophily). In real-world graphs, structural noise, missing links, and severe class imbalance degrade the quality of aggregated messages. In scenarios like fraud or spam detection, minority classes are often overshadowed by majority classes, leading to the over-smoothing problem where the unique characteristics of rare nodes are lost.

While traditional methods attempt to solve this via loss-weighting or latent space oversampling, they often ignore the semantic potential hidden within the raw node attributes. The motivation of this project is to bridge symbolic AI with deep learning. By extracting explicit, human-readable rules (e.g., IF-THEN conditions) from the data, we can inject a new layer of semantic understanding into the GNN. This dual approach addresses two fundamental graph mining concepts: Node Representation Learning (by enriching the feature matrix with rule satisfaction criteria) and Graph Topology Optimization (by dynamically linking distant nodes that exhibit high semantic similarity based on shared rules), effectively overcoming topological bottlenecks in imbalanced datasets.

## Objectives
- Objective 1: Reproduce the baseline results of the REFUEL framework on standard imbalanced graph datasets to establish a solid benchmark.
- Objective 2: Implement a rule extraction module to generate symbolic rules from node attributes and integrate them as augmented features for the GNN.
- Objective 3: Develop and apply a semantic graph rewiring algorithm that connects nodes sharing similar extracted rules to improve message passing.


## Related Work
This project builds primarily upon the REFUEL framework (Yang et al., 2024), which demonstrates the efficacy of rule extraction for aiding neural models in imbalanced node classification. To ensure optimal rule generation, the project draws inspiration from RACER (Basiri et al., 2017), an algorithm that efficiently handles multi-valued features to create generalized rules. Furthermore, recent advancements in Graph Rewiring and Neuro-Symbolic AI highlight the necessity of modifying graph topology based on latent similarities to overcome heterophily. This project unifies these concepts by utilizing symbolic rules not merely for node prediction, but as a topological restructuring tool for GNNs.

## Proposed Methodology

### Dataset(s)
The project will utilize three real-world datasets adopted in the base paper, specifically chosen to represent highly imbalanced node classification scenarios:
- The Elliptic Dataset: A Bitcoin transaction graph with over 200k nodes and severe imbalance (only 2% illicit nodes).
- The Cora Dataset: A citation graph manipulated to construct artificial minority classes of varying sizes.
- The FraudYelp Dataset: A review graph where 15% of the nodes represent a spam minority class.
- Preprocessing: Feature discretization for categorical rule extraction and 10-fold cross-validation splitting.

### Techniques and Algorithms
The methodology is structured to allow flexible implementation of two complementary phases:
- Rule Extraction Phase: Utilizing a rule-based classifier (e.g., REFUEL's extraction method or RACER) to generate a set of general, high-fitness rules from node attributes.
- Phase A (Feature Augmentation): Encoding the extracted rules into binary vectors. A node's initial feature matrix will be concatenated with these rule-satisfaction embeddings, providing the GNN with pre-processed symbolic knowledge.
- Phase B (Semantic Graph Rewiring): Computing rule-based similarity (e.g., Jaccard similarity of satisfied rules) between node pairs. A new semantic adjacency matrix will be generated by adding edges between highly similar nodes to facilitate rule-guided message passing.


### Evaluation Plan
Standard accuracy is inadequate for highly skewed datasets where the cost of a false positive in the minority class is exceptionally high. Following the base paper, evaluation metrics will primarily include Precision on the Minority Class (to measure the exactness of predicting rare fraudulent instances), Macro-Averaged Precision (to assess overall model performance without sacrificing the majority class), and F1 Score (to reflect the trade-off between precision and recall on the minority class). All metrics will be reported as averages over a 10-fold cross-validation.

## Challenges and Resources
The implementation of this project faces several key challenges, primarily stemming from the integration of the RACER algorithm with large-scale graph data. First, since RACER exclusively processes categorical data, discretizing the continuous numeric features of standard graph datasets (e.g., the 166 numeric attributes in the Elliptic dataset) may lead to unwanted information loss. Second, RACER's methodology involves initializing a unique rule for every training instance; scaling this rule composition phase for large graphs poses significant computational and memory overheads. Finally, the semantic graph rewiring phase requires computing pairwise rule similarities, resulting in a quadratic O(N^2) complexity, which, alongside the risk of excessively densifying the graph and triggering the over-smoothing problem in GCNs, presents substantial scalability challenges.

## References
- Yang, L., et al. (2024). REFUEL: rule extraction for imbalanced neural node classification. Machine Learning, Springer.
- Basiri, J., Taghiyareh, F., & Faili, H. (2017). RACER: accurate and efficient classification based on rule aggregation approach. Neural Computing and Applications, 29, 93-104.
- Wang, Z., et al. (2021). Relaxed Graph Convolutional Networks.
- Zhao, J., et al. (2021). GraphSMOTE: Imbalanced Node Classification on Graphs with Directed Graphs. WSDM.
- Weber, M., et al. (2019). Anti-Money Laundering in Bitcoin: Experimenting with Graph Convolutional Networks for Financial Forensics. KDD.

---

**Student Signature(s):** Hossein Karimy Moghaddam, Hossein Ghorbani Kahrisangi
**Date:** November 18, 2026
