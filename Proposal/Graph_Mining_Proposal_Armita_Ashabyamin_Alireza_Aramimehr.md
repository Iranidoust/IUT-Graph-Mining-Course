# Graph Mining Course Project Proposal

**Submission Date:** November 19, 2026  
**Course:** Graph Mining 4051  
**Instructor:** Dr. Zeinab Maleki

## Student Information

**Student Name(s):** Armita Ashab-yamin, Alireza Aramimehr  
**Student ID(s):** 40404084, 40415314  
**Email(s):** a.ashabyamin@ec.iut.ac.ir, a.aramimehr@ec.iut.ac.ir

## Project Title

**Healthy-Only Multiplex Graph Learning with Contrastive Embeddings and Deep SVDD for Detecting Atypical EEG Connectivity in Autism Spectrum Disorder**

## Abstract

Autism Spectrum Disorder (ASD) is associated with highly heterogeneous patterns of brain connectivity. Consequently, supervised classifiers that learn an average ASD pattern may not adequately represent individual differences. This project proposes a healthy-only graph anomaly detection framework for studying atypical EEG connectivity. Each participant's resting-state EEG will be represented as a multiplex graph, with frequency bands providing complementary network views. A graph encoder will be trained using data from typically developing participants, with contrastive learning used to obtain subject-level representations. Deep Support Vector Data Description (Deep SVDD) will then be used to estimate how far an unseen participant's representation deviates from the learned healthy reference. The resulting score will be interpreted as a measure of atypicality, not as a clinical diagnosis. The project will use publicly available Healthy Brain Network (HBN) EEG data, subject to data availability and eligibility criteria. Performance will be assessed using ROC-AUC and PR-AUC, alongside comparisons with single-band, one-class, and reconstruction-based baselines. Ablation experiments will examine the contribution of multiplex integration, contrastive pre-training, and individual frequency-band views.

## Problem and Motivation

EEG studies of ASD report heterogeneous and sometimes inconsistent case-control findings. Group-level averages can obscure individual differences, motivating methods that assess each participant relative to a reference population. A recent normative-modeling study [1] reported that extreme deviations from healthy EEG norms overlap only partially across individuals with neurodevelopmental and psychiatric conditions. This supports investigating individual-level deviation scores rather than relying exclusively on group-average differences.

Many graph-based ASD approaches are supervised classifiers trained using diagnostic labels [5], [6]. In contrast, a healthy-only anomaly detection model learns a reference representation from participants without the target diagnosis. It does not use ASD examples to learn the representation or determine the anomaly threshold; ASD labels are reserved for the final evaluation. This makes the task different from conventional diagnosis classification: the model estimates deviation from the learned reference rather than making a clinical diagnosis.

**Research question:** Can a graph model trained exclusively on typically developing participants assign higher anomaly scores to held-out participants with ASD than to held-out healthy participants, and does integrating multiple frequency-band graph views improve detection compared with using a single band?

The project is directly related to graph mining. EEG connectivity can be represented as weighted graphs, where nodes correspond to recording channels or other consistently defined signal units and edges represent estimated connectivity. A multiplex representation retains multiple frequency-band views of the same underlying node set. The project will investigate graph representation learning, graph-level embeddings, multiplex network integration, and graph anomaly detection. The precise signal-processing and graph-construction choices will be finalized after inspecting the available data and reviewing their suitability.

## Objectives

- **Data and graph representation:** Identify eligible HBN resting-state EEG recordings, perform appropriate data-quality checks and preprocessing, and construct consistent graph representations across the selected frequency-band views. Report relevant data and graph characteristics.
- **Healthy-only learning pipeline:** Develop a graph-based representation-learning pipeline using healthy training participants, incorporating multiplex information, contrastive learning, and Deep SVDD-based anomaly scoring.
- **Baseline comparison:** Compare the proposed approach with single-band and other suitable one-class or reconstruction-based baselines using ROC-AUC, PR-AUC, and sensitivity at a specified false-positive rate. Report uncertainty estimates where feasible.
- **Ablation and analysis:** Examine the contribution of multiplex integration, contrastive pre-training, and individual frequency-band views. Report negative or inconclusive findings as well as improvements.

## Related Work

Graph-based ASD studies commonly formulate the task as supervised diagnosis. Wang et al. [6] combine multi-site clustering with nested feature extraction using resting-state fMRI, while Ren et al. [5] propose Brain-Shapelet to capture instantaneous abnormalities in brain activity for ASD diagnosis. These studies provide relevant examples of graph or brain-signal analysis, but they do not implement the same healthy-only anomaly detection setting proposed here.

Ebadi et al. [1] report heterogeneity in EEG deviations from normative models across neurodevelopmental and psychiatric conditions, motivating individual-level reference-based analysis. Deep SVDD [2] learns representations that map normal data toward a compact region in latent space and provides a basis for one-class anomaly scoring. OCGTL [3] addresses graph-level anomaly detection and is relevant to the design considerations of graph anomaly models. GraphCL [4] presents graph contrastive learning strategies and augmentation approaches that can inform the representation-learning component.

To the best of our knowledge, the combination of healthy-only training, multiplex frequency-band EEG graphs, graph contrastive representation learning, and Deep SVDD scoring has not been established as a standard unified pipeline for ASD EEG anomaly detection. This project will combine and evaluate these existing ideas; it does not claim to introduce an entirely new learning architecture.

## Proposed Methodology

### Dataset

The primary dataset will be resting-state EEG recordings from the publicly available Healthy Brain Network (HBN) study. The project will focus on participants within the age range appropriate to the selected cohort and on participants for whom the required EEG and clinical information are available. Eligibility criteria and diagnostic-group definitions will be documented using the dataset's accompanying metadata and clinical records.

The analysis will distinguish between typically developing participants used for model training, typically developing participants reserved for calibration, and held-out participants used for final evaluation. Where sample sizes permit, the held-out evaluation set will include both healthy and ASD participants. Participant-level separation will be maintained so that recordings from the same individual cannot appear in more than one split. The final sample size, channel configuration, and exact inclusion criteria will be reported after data inspection rather than assumed in advance.

### Data preparation and graph representation

The EEG recordings will undergo appropriate quality assessment and preprocessing before connectivity graphs are generated. The preparation stage is expected to include checks for missing or unusable recordings, inspection of signal quality, treatment of artifacts and noisy segments, and consistent handling of recording metadata. The exact preprocessing pipeline will be selected after examining the dataset and relevant methodological guidance.

The project intends to represent each participant using multiple graph views associated with EEG frequency bands. The views will share a consistent node definition so that information across bands can be integrated. Connectivity estimates will be used as edge weights. The specific connectivity measure, filtering details, treatment of negative or weak connections, and any graph sparsification procedure will be selected after preliminary data inspection and documented in the final implementation. These choices will be applied consistently across participants and justified in terms of data quality and methodological suitability.

Before model training, graph and feature characteristics will be inspected for invalid values, missing data, scale differences, and other potential sources of inconsistency. Relevant graph statistics, such as density and edge-weight distributions, will be summarized for the selected views.

### Learning framework

The proposed framework will contain three main components:

- **Multiplex graph encoder:** A graph neural network will learn representations from the frequency-band graph views. A shared or otherwise consistently parameterized encoder design will be considered, with a mechanism for combining information across views into a participant-level embedding. The final encoder and integration mechanism will be selected based on implementation feasibility and validation using healthy training data.
- **Contrastive representation learning:** Contrastive learning will be investigated to encourage useful and stable participant-level representations. Two augmented views of a graph or multiplex graph will be used during training. Augmentation strategies will be selected conservatively so that they do not destroy meaningful connectivity structure. The choice and strength of augmentations will be documented and assessed through validation and ablation experiments.
- **Deep SVDD anomaly scoring:** Deep SVDD will be applied to the learned participant-level embeddings. A reference center will be established using the healthy training data, and anomaly scores will be based on distance from that reference. The implementation will include checks for representation collapse, such as monitoring embedding variance and score distributions. If the Deep SVDD objective proves unstable, alternative distance-based scoring on the learned embeddings, such as Mahalanobis distance or k-nearest-neighbor distance, will be considered and clearly reported as alternatives rather than silently substituted.

The central training principle will remain healthy-only: ASD examples will not be used to train the encoder, learn the contrastive representations, fit preprocessing transformations, select features, or determine the anomaly threshold.

### Data leakage prevention and calibration

All data-dependent preprocessing operations, including feature scaling and any fitted correction or transformation, will be estimated using the healthy training partition only and then applied unchanged to calibration and test participants. If age or other covariates are considered, their treatment will be defined without using the held-out test labels or test outcomes. Splits will be made at the participant level.

A separate healthy calibration subset will be used to select an operating threshold corresponding to the target false-positive rate. The test set will remain untouched until the analysis pipeline and thresholding procedure have been fixed. Any model selection will be performed without using ASD test labels.

### Tools and implementation

The planned implementation environment includes Python, MNE, NetworkX, PyTorch, and PyTorch Geometric. The final set of libraries and implementation details may be adjusted to the requirements of the selected data representation and model components.

## Evaluation Plan

The primary evaluation will be conducted on held-out HBN participants. The model will produce an anomaly score for each participant. The primary metrics will be:

- ROC-AUC, to measure ranking performance across possible thresholds.
- PR-AUC, to assess precision-recall performance, particularly if the evaluation groups are imbalanced.
- Sensitivity at a prespecified 5% false-positive rate, with the operating threshold determined using healthy calibration participants only.

Where sample sizes permit, uncertainty will be summarized using bootstrap confidence intervals. The number of participants in each split and diagnostic group will be reported alongside the metrics.

The proposed method will be compared with suitable baselines, including a single-band version of the approach, a multiplex model without contrastive pre-training, one-class SVM using an appropriate vector representation, a graph or feature autoencoder reconstruction-based score, and distance-based scoring methods on the learned embeddings. The exact baseline configurations will be chosen to ensure fair comparisons and to remain feasible within the project scope.

Ablation experiments will examine the effect of contrastive pre-training, multiplex integration versus the best-performing single-band view, and alternative graph-view or augmentation choices where feasible. Any exploratory association between anomaly scores and available symptom-severity measures (for example, SRS scores) will be treated as secondary analysis and not as evidence of diagnostic validity.

## Challenges and Resources

- **Data quality and availability:** EEG recordings may vary in quality and completeness. The project will define transparent inclusion and quality-control criteria and report the number of participants excluded at each stage.
- **Heterogeneity and confounding:** Age, sex, recording quality, and other available covariates may influence connectivity estimates. Their potential effects will be examined, and any correction or adjustment will be fitted without using held-out test information.
- **Deep SVDD stability:** One-class objectives can produce uninformative or collapsed representations. Embedding variance and score distributions will be monitored, and alternative distance-based scoring will be considered if necessary.
- **Class imbalance and sample size:** The number of eligible ASD participants may limit the precision of evaluation. Group sizes and uncertainty intervals will therefore be reported, and conclusions will be appropriately qualified.
- **Scope and implementation feasibility:** The final preprocessing, connectivity, graph-construction, and model details will be selected after preliminary data inspection. The project will prioritize a reproducible core pipeline before optional extensions or secondary analyses.

## References

1. A. Ebadi et al., “Beyond homogeneity: charting the landscape of heterogeneity in neurodevelopmental and psychiatric electroencephalography,” *Translational Psychiatry*, vol. 15, art. 223, 2025, doi: [10.1038/s41398-025-03441-0](https://doi.org/10.1038/s41398-025-03441-0).
2. L. Ruff et al., “Deep one-class classification,” in *Proceedings of the 35th International Conference on Machine Learning (ICML)*, PMLR, vol. 80, 2018, pp. 4393–4402.
3. C. Qiu, M. Kloft, S. Mandt, and M. Rudolph, “Raising the bar in graph-level anomaly detection,” in *Proceedings of the International Joint Conference on Artificial Intelligence (IJCAI)*, 2022.
4. Y. You et al., “Graph contrastive learning with augmentations,” in *Advances in Neural Information Processing Systems*, vol. 33, 2020.
5. Y. Ren, Z. Xia, Y. Zhang, and Z. Jiao, “Brain-Shapelet: A framework for capturing instantaneous abnormalities in brain activity for autism spectrum disorder diagnosis,” *IEEE Transactions on Neural Systems and Rehabilitation Engineering*, vol. 33, 2025.
6. N. Wang, D. Yao, L. Ma, and M. Liu, “Multi-site clustering and nested feature extraction for identifying autism spectrum disorder with resting-state fMRI,” *Medical Image Analysis*, vol. 75, art. 102279, 2022.

---

**Student Signature(s):**

Armita Ashab-yamin: ______________________________

Alireza Aramimehr: ______________________________

**Date:** November 19, 2026
