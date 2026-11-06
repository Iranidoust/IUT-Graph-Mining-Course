# Graph Mining Course Project Proposal

**Submission Date:** 1404/09/19
**Course:** Graph Mining [4041]
**Instructor:** Dr. Zeinab Maleki

## Student Information

* **Student Name(s):** Mahrokh Mousavi & MohammadHossein Shojaodiny
* **Student ID(s):** 40131663 & 40125043
* **Email(s):** [mahrokhmousavii44@gmail.com](mailto:mahrokhmousavii44@gmail.com) & reynardipg@gmail.com

## Project Title

**Comparing Bi-Directional Graph Convolutional Networks and Standard GCNs for Detecting Satire and Fake News on Reddit**

## Abstract

Misinformation spreads quickly on social media, and detecting it requires models that use both textual and structural information. Many existing studies rely on Twitter, where propagation graphs are shallow and have limited interaction depth. In this project, we focus on Reddit, which contains deeper and more complex discussion trees. We plan to reproduce the Bi-Directional Graph Convolutional Network (Bi-GCN) model and apply it to the Fakeddit dataset to evaluate whether graph structure helps distinguish *Satire* from *Fake News*. These two categories can look similar in text but often differ in how users react to them. Our goal is to compare Bi-GCN with a standard GCN and analyze how much the bottom-up structure contributes to classification performance.

## Problem and Motivation

Most misinformation detection models emphasize text features or shallow interaction graphs such as Twitter threads. Reddit, however, contains deep comment trees where user reactions may carry important signals about intent and content type. Distinguishing satire from fake news is especially difficult because their written content may appear similar, yet the community's response differs. Satire usually attracts humorous reactions, while fake news may receive corrections, debates, or skepticism.
This problem connects to course concepts such as graph classification, message passing, and structural representation learning. By using Reddit instead of Twitter, we examine whether GCN-based models generalize to deeper graph structures and whether bottom-up information — comments reacting to the original post — helps identify satire more accurately.

## Objectives

* **Objective 1:** Build directed propagation trees from the Fakeddit dataset using Reddit comment threads.
* **Objective 2:** Implement the Bi-GCN model to capture both top-down and bottom-up propagation patterns.
* **Objective 3:** Implement a standard GCN as a baseline model.
* **Objective 4:** Compare model performance on the Satire, Fake, and Real classes.

## Related Work

Bian et al. (2020) introduced the Bi-GCN model and showed that using both propagation directions improves rumor detection. Nakamura et al. (2020) released the Fakeddit dataset, which includes large Reddit threads and provides a more challenging benchmark for graph-based methods. Kipf and Welling (2017) proposed the standard GCN architecture, which forms the core building block for both of the models used in this project. Our work builds on these studies by applying them to the specific task of separating satire from fake news on deeper Reddit graphs.


**** 

## Proposed Methodology

### Dataset

**Fakeddit: Fine-Grained Fake News Detection Dataset**

* **Source:** Reddit
* **Structure:** Directed reply trees
* **Subset:** Approximately 10k–20k samples across Fake, Real, and Satire classes
* **Preprocessing:**

  * Parse parent–child relationships to build graph edges
  * Convert text into node embeddings (TF-IDF or BERT)
  * Filter incomplete or extremely short threads

### Techniques and Algorithms

* **Bi-GCN (Main Model):**

  * Applies one GCN layer on the adjacency matrix (top-down) and one on its transpose (bottom-up)
  * Combines both feature sets for classification
* **Standard GCN (Baseline):**

  * Uses only the top-down propagation direction
  * Helps measure the added value of bottom-up information
* **Tools and Libraries:** Python, PyTorch Geometric, NetworkX

### Evaluation Plan

* **Data Split:** 70% training, 10% validation, 20% testing
* **Metrics:** Accuracy, Precision, Recall, F1-Score
* **Analysis:** Compare confusion matrices for both models, with a focus on errors between Satire and Fake classes

## Challenges and Resources

Reddit graphs can be deep, which may cause over-smoothing in GCN layers. To address this, we will use depth pruning or residual connections if needed. Another challenge is converting nested Reddit JSON structures into clean graph formats. We plan to address this by implementing a structured parser in Python.

## References

Bian, T. et al. (2020). *Rumor Detection on Social Media with Bi-Directional Graph Convolutional Networks*. AAAI.
Nakamura, K., Levy, S., & Wang, W. Y. (2020). *Fakeddit: A New Multimodal Fake News Dataset for Fine-Grained Detection*. LREC.
Kipf, T. N., & Welling, M. (2017). *Semi-Supervised Classification with Graph Convolutional Networks*. ICLR.

---

**Student Signature(s):** Mahrokh Mousavi & MohammadHossein Shojaodiny
**Date:** 1404/09/19
