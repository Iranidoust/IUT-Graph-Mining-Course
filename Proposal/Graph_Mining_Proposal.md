# Graph Mining Course Project Proposal

**Submission Date:** 1405/08/22  
**Course:** Graph Mining [4051]  
**Instructor:** Dr. Zeinab Maleki

## Student Information

- **Student Name(s):** Mohsen Ahmadi, Hadi Atabakhshian
- **Student ID(s):** 40403674, 40410634
- **Email(s):** m.ahamadi@ec.iut.ac.ir, h.atabakhshian@ec.iut.ac.ir

## Project Title

**Financial Lead–Lag Link Prediction via Temporal and Spatio-Temporal Graph Learning**

## Abstract

Financial assets may exhibit *lead–lag relationships*, where a significant price movement in one asset precedes a movement in the same direction in another. As these relationships change over time, they can be represented as directed, time-stamped edges in a dynamic graph. This project formulates the problem as **temporal link prediction**, with assets as nodes and historical relationships as edges. First, the dataset and code of the reference study will be prepared, and GraphMixer will be run to reproduce its main reported results as closely as possible. Two possible extensions will then be investigated: combining a GNN with GRU or LSTM to model changes in graph relationships over time, and assigning edge weights based on the strength of historical relationships between assets. Combining these ideas may also be examined. The aim is to assess their effects on future-link prediction, without assuming that performance will necessarily improve.

## Problem and Motivation

Financial markets consist of related assets, including stocks and commodities. Their price movements do not always occur simultaneously: a movement in one asset on one trading day may be followed by a movement in the same direction in another asset on the following trading day. This pattern is known as a *lead–lag relationship* and does not, by itself, establish causality between assets. Such relationships may strengthen or weaken over time.

Krstev et al. [1] represent assets as nodes and lead–lag events as directed, time-stamped edges. GraphMixer performed well in their experiments; therefore, we do not assume in advance that the model is inadequate. However, two questions remain worth investigating. First, could combining graph-based structural feature extraction with recurrent models such as GRU or LSTM provide a more suitable representation of changes in relationships over time? Second, could explicitly representing *relationship strength* as an edge weight, rather than considering only whether an edge exists, provide useful information to the model? In the reference framework, edge occurrence is determined by a threshold-based condition, although certain price and financial information has also been considered among model features.

Our main question is whether these proposed changes affect the quality of future directed-link prediction. The project relates to dynamic graphs, link prediction, edge attributes, and graph representation learning. It focuses on relationships between assets, rather than price forecasting or investment advice. The final extension will be selected after reproducing the baseline and examining the data and implementation feasibility.

## Objectives

- **Baseline reproduction:** Prepare the data, run the reference GraphMixer implementation, and compare the obtained results with those reported in the paper by the progress report.
- **Graph-based extensions:** Assess the feasibility of combining a GNN with GRU/LSTM and weighting edges according to the strength of historical relationships; select and implement at least one justified approach by the final report.
- **Fair evaluation:** Compare the extended approach with the reproduced GraphMixer baseline using the same chronological splits and evaluation metrics.
- **Result analysis:** Examine the contributions of edge weights or temporal modeling through ablation experiments, and report limitations even if performance does not improve.

## Related Work

Krstev et al. [1] formulate financial lead–lag detection as temporal graph link prediction and compare multiple methods. In their experiments, GraphMixer achieved stronger results than the other evaluated models. GraphMixer [2] uses a relatively lightweight MLP-based architecture to process interaction histories and structural information. TGAT [3] applies time-aware attention, while TGN [4] maintains memory of past interactions. In spatio-temporal learning, DCRNN [5] illustrates the combination of graph operations and recurrent modeling for dynamic data. DyGFormer [6] provides a Transformer-based approach to interaction histories. These methods inform possible design choices, but this project does not commit to implementing all of them. Its proposed direction is to investigate edge weights derived from historical relationship strength and the possible combination of graph representations with GRU or LSTM for this financial task. The precise novelty of these ideas will be checked before final implementation.

## Proposed Methodology

### Dataset(s)

The project will use the public dataset and code of Krstev et al. [1]. It contains **37 assets (29 stocks and 8 commodities)** observed daily from **2019 to 2024**. Assets are nodes, and lead–lag events are directed, time-stamped edges. Under the reference definition, an edge occurs when consecutive-day returns of two assets are directionally aligned and exceed a fixed threshold. Data will be divided chronologically into training, validation, and test sets.

### Techniques and Algorithms

**Phase 1 — Reproduction:** The data and environment will be prepared, and GraphMixer run with the reference chronological splits, negative sampling, and settings. Results and discrepancies will be documented in the progress report.

**Phase 2 — Extension:** Two ideas are proposed:

- **GNN–recurrent combination:** Use a GNN to extract structural features from consecutive graph snapshots, followed by GRU or LSTM to learn their temporal changes. The final architecture will depend on feasibility testing.
- **Weighted-edge graphs:** Assign a weight to each historical relationship based on the magnitude of the two assets' aligned return movements, or another measure of relationship strength computed using past data only. The weight may be used as an edge feature or in graph aggregation; the implementation will be chosen after examining the code.

If feasible, the two extensions may also be combined. At least one justified extension will be implemented by the final report.

### Evaluation Plan

The extended approach will be compared with the **reproduced GraphMixer baseline** using the same positive and negative samples and **AP, ROC-AUC, Recall@K, and MRR**. The baseline and any implemented weighted or spatio-temporal variants will be compared. Edge weights will be computed only after their historical events are fully observed to prevent future-information leakage. Ablation experiments will assess the contribution of edge weights; results will be reported even without performance gains.

## Challenges and Resources

The first challenge is **temporal information leakage**: a lead–lag event is complete only after the follower asset's price movement is observed on the next trading day. Therefore, edge weights and model inputs will use only fully observed historical relationships available at prediction time. A second challenge is adapting the proposed architectures to the GraphMixer code under limited computing resources. To reduce this risk, the baseline will be run first, followed by small-scale feasibility tests of the extensions. The project will use Python, PyTorch, and graph-learning libraries. Initial tests will run on a laptop with 16 GB RAM, while more demanding training will use Kaggle T4 GPUs.

## References

[1] Krstev, I., Rigoni, D., Mishkovski, I., & Pasa, L. (2026). *A Temporal Graph Learning Framework for Lead-Lag Detection in Financial Markets*. **Machine Learning, 115**, Article 173. https://doi.org/10.1007/s10994-026-07113-y

[2] Cong, W., et al. (2023). *Do We Really Need Complicated Model Architectures for Temporal Networks?* **ICLR**. https://arxiv.org/abs/2302.11636

[3] Xu, D., et al. (2020). *Inductive Representation Learning on Temporal Graphs*. **ICLR**. https://arxiv.org/abs/2002.07962

[4] Rossi, E., et al. (2020). *Temporal Graph Networks for Deep Learning on Dynamic Graphs*. https://arxiv.org/abs/2006.10637

[5] Li, Y., Yu, R., Shahabi, C., & Liu, Y. (2018). *Diffusion Convolutional Recurrent Neural Network: Data-Driven Traffic Forecasting*. **ICLR**. https://arxiv.org/abs/1707.01926

[6] Yu, L., Sun, L., Du, B., & Lv, W. (2023). *Towards Better Dynamic Graph Learning: New Architecture and Unified Library*. **NeurIPS**. https://arxiv.org/abs/2303.13047

**Dataset:** https://github.com/ivankrstev7/Temporal_Graphs_Lead_Lag_Data  
**Reference implementation:** https://github.com/ivankrstev7/temporal_graph_lead_lag_detection

---

**Student Signature(s):** Mohsen Ahmadi, Hadi Atabakhshian  
**Date:** 1405/08/22
