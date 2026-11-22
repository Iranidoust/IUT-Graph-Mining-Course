# Graph Mining Course Project Proposal

**Submission Date:** 1405/09/01 (Solar Hijri; 1 Azar 1405)  
**Course:** Graph Mining [4041]  
**Instructor:** Dr. Zeinab Maleki  

## Student Information
- **Student Name(s):** Aryan Ashoori  
- **Student ID(s):** 40410224  
- **Email(s):** aryanashoori0@gmail.com  

## Project Title
Community Detection and Representative Selection in ECG-PPG Feature Dependency Graphs for Virtual Respiratory Rate Sensing

## Abstract
This project will investigate community detection as a method for selecting indirect indicators of respiratory rate. Thirty features will be extracted from electrocardiogram (ECG) and photoplethysmogram (PPG) signals in the public BIDMC dataset. Each feature will form a node in an undirected dependency graph, with edges determined by absolute Spearman correlation. Louvain, greedy modularity maximization, label propagation, Leiden, HP-MOCD, and a consensus Leiden variant will be compared. One representative feature per community will be selected using training-only mutual information. A fixed random forest will estimate respiratory rate from these representatives. Patient-disjoint nested cross-validation will compare graph-based selection against all features and equally sized non-graph selections. Expected outputs are interpretable community maps and an assessment of the relationship between feature reduction and estimation error; improved accuracy is not assumed.

## Problem and Motivation
Respiratory rate is an important vital sign, but manual breath counting provides intermittent measurements and requires staff time. Dedicated respiratory monitoring can require additional equipment or attachments [1]. When ECG and PPG are already recorded, estimating respiratory rate from their respiratory modulations offers a way to obtain repeated estimates without adding a dedicated respiration sensor. This motivates virtual sensing from existing measurements. The project will assess computational performance; it will not measure clinical benefit, patient comfort, or financial savings.

The graph mining problem concerns redundancy among indirect indicators. Respiration affects the baseline, amplitude, and beat intervals of ECG and PPG. Features derived from these modulations may carry overlapping information. Individual feature ranking can retain several similar indicators while overlooking the structure of their dependencies. A feature graph makes these relationships explicit: nodes represent indicators, edges represent statistical dependence, and communities identify groups of related indicators. Community detection and modularity are the main course concepts examined.

The research question is whether selecting representatives from these communities can reduce the number of model inputs while maintaining a similar estimation error. The public BIDMC dataset provides signals and annotated breaths for evaluation [2]. Both ECG and PPG will remain required inputs; feature reduction concerns software descriptors. Graphs will describe association rather than causation, and similar errors will not establish statistical equivalence.

## Objectives
- Construct 30-node feature dependency graphs and report their edge counts, density, and isolated nodes.
- Compare six community detection methods using modularity, community counts, runtime, and partition stability.
- Evaluate representative selection using patient-balanced estimation errors and matched-size non-graph baselines.
- Provide reproducible code, community visualizations, and a clear account of findings and limitations.

## Related Work
Baker et al. [1] estimated respiratory rate from ECG and PPG modulations using respiratory quality indices and neural networks. Their work motivates the physiological indicators in this project; their neural network pipeline will not be reproduced. Rostami et al. [3] combined community detection and a genetic algorithm for feature selection in classification. This project adopts the idea of grouping dependent features but will evaluate a simpler representative selection rule for respiratory rate regression. Blondel et al. [4] introduced Louvain, a modularity-based community detection method that provides a classical graph mining baseline. Together, these studies motivate the sensing task, feature grouping, and community analysis. The proposed contribution is a controlled class-project comparison, including a consensus graph variant, rather than a claim of a globally novel algorithm.

## Proposed Methodology

### Dataset(s)
BIDMC contains 53 eight-minute recordings sampled at 125 Hz [2]. Patient identifiers will group recordings from the same person. Nonoverlapping 60-second windows yield up to 424 samples. Targets will average breath counts from two annotators; windows with disagreement exceeding two breaths will be excluded. Respiration waveforms and monitor respiratory rates will be excluded from model inputs.

### Techniques and Algorithms
- **Graph construction:** Extract baseline, amplitude, and beat-interval sequences from ECG and PPG. Their mean, standard deviation, interquartile range, dominant respiratory frequency, and relative respiratory power yield 30 nodes. Threshold absolute Spearman correlations to create an undirected graph; report edge counts after thresholding. Use an initial respiratory band of 0.1-0.7 Hz. Retain weights for interpretation, but use unweighted topology for all detectors for compatibility with HP-MOCD.
- **Community discovery:** Compare Louvain [4], greedy modularity maximization, label propagation, Leiden [5], and HP-MOCD [6]. The consensus Leiden variant will retain edges present in at least 80% of 30 patient-level bootstrap samples. It tests sensitivity to patient composition.
- **Representative selection:** Select the feature with highest training-only mutual information per community and preserve isolated nodes. Use a shared random forest with 200 trees and at least three samples per leaf.

### Evaluation Plan
Use five patient-disjoint outer folds and three inner folds to select correlation thresholds from 0.5, 0.7, and 0.9. Fit imputation, graphs, and selection only within training. Compare all features, matched-size mutual information ranking, and random selection. Report patient-balanced MAE and RMSE in breaths/min, feature counts, modularity, density, runtime, and partition stability using adjusted Rand index across ten runs. Python, NetworkX, igraph, and scikit-learn will support implementation.

## Challenges and Resources
Two challenges are signal noise and unstable graph structure. Noise can distort beat detection and respiratory indicators; filtering, explicit quality rules, and documented exclusions will address this risk. Thresholding can fragment graphs, while stochastic community detection can change representatives. Inner validation, patient-level bootstrap graphs, and saved partitions will support sensitivity analysis and numerical replay. Isolated features will be preserved, even if this limits dimensionality reduction. The project requires public data, open-source Python tools, and a personal computer. A four-week schedule allocates one week each to data preparation, graph construction, comparative evaluation, and writing. Clinical validation will require a separate study.

## References
[1] S. Baker, W. Xiang, and I. Atkinson, "Determining respiratory rate from photoplethysmogram and electrocardiogram signals using respiratory quality indices and neural networks," *PLOS ONE*, vol. 16, no. 4, e0249843, 2021. doi: [10.1371/journal.pone.0249843](https://doi.org/10.1371/journal.pone.0249843).

[2] M. Pimentel, A. Johnson, P. Charlton, and D. Clifton, "BIDMC PPG and Respiration Dataset," version 1.0.0, PhysioNet, 2018. doi: [10.13026/C2208R](https://doi.org/10.13026/C2208R).

[3] M. Rostami, K. Berahmand, and S. Forouzandeh, "A novel community detection based genetic algorithm for feature selection," *Journal of Big Data*, vol. 8, article 2, 2021. doi: [10.1186/s40537-020-00398-3](https://doi.org/10.1186/s40537-020-00398-3).

[4] V. D. Blondel, J.-L. Guillaume, R. Lambiotte, and E. Lefebvre, "Fast unfolding of communities in large networks," *Journal of Statistical Mechanics: Theory and Experiment*, vol. 2008, no. 10, P10008, 2008. doi: [10.1088/1742-5468/2008/10/P10008](https://doi.org/10.1088/1742-5468/2008/10/P10008).

[5] V. A. Traag, L. Waltman, and N. J. van Eck, "From Louvain to Leiden: guaranteeing well-connected communities," *Scientific Reports*, vol. 9, article 5233, 2019. doi: [10.1038/s41598-019-41695-z](https://doi.org/10.1038/s41598-019-41695-z).

[6] G. O. Santos et al., "A high-performance evolutionary multiobjective community detection algorithm," *Social Network Analysis and Mining*, vol. 15, article 110, 2025. doi: [10.1007/s13278-025-01519-7](https://doi.org/10.1007/s13278-025-01519-7).

---

**Submission Instructions:**  
- Limit to 2 pages (excluding references), single-spaced, 11-12 pt font.  
- Include figures if helpful (e.g., sample graph).  
- Submit PDF in Github by the deadline announced by the instructor.  

**Student Signature(s):** Aryan Ashoori  
1405/09/01 (Solar Hijri; 1 Azar 1405)
