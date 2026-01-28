<div style="
    text-align: center;
    font-family: 'Trebuchet MS', 'Segoe UI', sans-serif;
    color: #75797dff;
    line-height: 1.5;
    padding: 25px;
    border-radius: 20px;
    background: linear-gradient(145deg, #fbfbfbff, #5ea0f7ff);
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
">
  <img src="iut-logo.png" width="160" height="160" style="border-radius: 15px; box-shadow: 0 2px 6px rgba(0,0,0,0.2);">  
  <h1 style="
      color: #0F5298;
      font-size: 45px;
      margin-top: 15px;
      margin-bottom: 10px;
  ">
    Computer Engineering Department
  </h1>
  <h2 style="
      color: #2565AE;
      font-size: 28px;
      margin: 0;
  ">
    <br> Graph Mining Course Project — Progress Report
  </h2>
  <h3 style="
      color: #3C99D8;
      font-size: 22px;
      margin: 10px 0 5px 0;
      letter-spacing: 0.5px;
  ">
    <br> Analysis and Recommendation of Amazon Electronics Products
  </h3>
  <h2 style="
      color: #1F75FE;
      font-size: 28px;
      font-weight: bold;
      text-transform: uppercase;
      margin-top: 15px;
      letter-spacing: 1px;
      text-shadow: 1px 1px 4px rgba(0,0,0,0.1);
  ">
    <br> Dr. Maleki & Dr. Iranidoust
  </h2>
  <p style="
      color: #696880;
      font-size: 18px;
      margin-top: 15px;
      font-style: italic;
  ">
    <br> Student Names: Masoud Hasani & Kourosh Jamshidi <br>
    Student IDs: 40121243 & 40120523 <br>
    Emails: masood.hsn82@gmail.com & k.jamshidi@ec.iut.ac.ir <br>
  </p>
    <h2 style="
      color: #44e3fcff;
      font-size: 28px;
      font-weight: bold;
      text-transform: uppercase;
      margin-top: 15px;
      letter-spacing: 1px;
      text-shadow: 1px 1px 4px rgba(0,0,0,0.1);
  ">
    <br> Winter 2026
  </h2>
</div>

# Graph Mining Course Project — Final Report

**Submission Date:** Winter 2026  
**Course:** Graph Mining [4041]  
**Instructor:** Dr. Zeinab Maleki  
**Project Title:** Analysis and Recommendation of Amazon Electronics Products  

## Student Information
- **Student Name(s):** Kourosh Jamshidi, Masoud Hasani  
- **Student ID(s):** 40120523, 40121243  
- **Email(s):** k.jamshidi@ec.iut.ac.ir, masood.hsn82@gmail.com  

---

## Abstract

In this project, we studied the problem of **product recommendation** on the Amazon Electronics dataset using **graph mining and graph-based ranking methods**. The main goal was to predict which items a user is likely to interact with next, based on their historical behavior. We first preprocessed the data and generated a **time-aware 80/20 user-based split**, where the most recent interactions of each user were used for testing to simulate real-world recommendation settings. Then, we modeled the data as multiple graphs, including a **user–item interaction graph**, an **item–item co-purchase graph**, and a **user similarity graph** (KNN with Jaccard similarity).

We implemented and evaluated several baselines and graph mining techniques: **Popularity**, **Random**, **Neighbor-based Collaborative Filtering**, **Two-step Co-purchase**, **Personalized PageRank (PPR)**, and **community-based recommendation** using Leiden-style and Spectral clustering ideas. We also implemented a lightweight **GNN-inspired model** with sparse matrix aggregation and BPR training to improve scalability. Evaluation was performed under multiple scenarios using different **Top-K and negative sampling sizes**, reporting metrics such as **Precision@K, Recall@K, HitRate@K, MRR, Coverage, and Error**.

Our experiments showed that some graph-based methods produced very similar results due to candidate set design and graph structure, while the GNN model faced scalability challenges on the full dataset and required user subsampling for efficient training. Overall, this work highlights both the effectiveness and limitations of graph-based recommendation under realistic evaluation constraints.

---

## Introduction

Recommender systems are one of the most important components of modern e-commerce platforms, helping users discover relevant products while improving engagement and sales. The Amazon Electronics dataset provides a realistic environment for studying recommendation because it contains a large number of users, items, and historical interactions. However, the recommendation task in such settings is challenging due to sparsity, scale, and the need for evaluation methods that reflect real-world usage.

The main objective of this project was to design and compare several recommendation approaches using **graph mining concepts** taught in the course. Instead of treating the data only as a user–item matrix, we modeled the problem as graph structures that enable neighborhood reasoning, random-walk scoring, and community discovery. In particular, we built:
1) a **user–item bipartite interaction graph**,  
2) an **item–item co-purchase graph** extracted from shared user interactions, and  
3) a **user–user similarity graph** based on Jaccard similarity over item sets.

A key part of the project was building a **time-aware train/test split**. Since recommendation is inherently sequential, we used each user’s latest interactions as test data and earlier interactions as training data. This makes the evaluation closer to a real deployment scenario, where the system must recommend future items.

During implementation, we faced several practical issues such as data leakage, seen-item filtering, candidate set construction, and scalability limits. In early experiments, multiple algorithms produced unexpectedly similar metrics. By analyzing the evaluation pipeline, we discovered that incorrect candidate construction or insufficient filtering can make evaluation unrealistically easy and cause methods to converge to similar outputs. We addressed this by ensuring proper negative sampling and removing previously seen items during ranking.

Finally, we extended the project by implementing a lightweight **GNN-inspired recommender**, designed to run efficiently on limited hardware. This part demonstrated how representation learning can be combined with graph structure, while also highlighting the challenges of scaling deep models on large datasets.

---

## Methodology

### Dataset Description

We used the Amazon Electronics interactions dataset (user–item ratings/interactions). Each row represents an interaction between a user and a product, along with a timestamp. The dataset was preprocessed to remove invalid rows (missing user, item, or timestamp) and to keep only useful interactions for graph construction.

We created two main splits:
- **Training set:** earlier interactions for each user  
- **Test set:** the last 20% of interactions for each user (time-aware)

The dataset naturally forms a **bipartite graph** between users and items. From this, we derived additional graphs for graph mining methods:

- **User–Item Graph:** edges represent observed interactions.
- **Item–Item Co-purchase Graph:** items are connected if they co-occur in user histories, with an edge weight representing co-occurrence strength.
- **User–User Similarity Graph:** users are connected using Jaccard similarity of their interacted item sets.

**Time-aware split logic:**  
For each user with at least `MIN_INTERACTIONS`, we sort interactions by timestamp and place the last 20% in test. Users with fewer interactions are kept fully in training to avoid unstable evaluation.

---

### Algorithms and Techniques

We implemented multiple recommendation algorithms:

#### 1) Popularity Baseline
Ranks candidate items by global popularity (frequency in training data).  
This baseline is strong in many real datasets due to popularity bias.

#### 2) Random Baseline
Ranks candidate items randomly.  
This provides a lower-bound baseline.

#### 3) Neighbor-CF (User-based Collaborative Filtering)
Uses a precomputed user–user KNN similarity graph (Jaccard similarity).  
For a target user, it scores items based on how many similar neighbors interacted with them, weighted by similarity.

#### 4) Two-step Co-purchase
Uses the item–item co-purchase graph.  
For a user, it aggregates scores from the neighbors of the items the user has already interacted with.

#### 5) Personalized PageRank (PPR-inspired scoring)
We implemented a simplified personalized propagation approach by collecting high-weight neighbors of the user’s seen items and ranking candidates based on aggregated edge weights. A popularity fallback is used for unseen candidates.

#### 6) Community-based Recommendation (Leiden-style approximation)
We used community structure intuition by giving higher scores to candidates that are densely connected to a user’s seen items in the co-purchase graph (membership-like scoring).

#### 7) Community-based Recommendation (Spectral-style)
Similar to Leiden-style but weight-aware, using co-purchase weights for scoring.

#### 8) Lightweight GNN-inspired Model (Fast Sparse GNN-like)
To incorporate representation learning, we implemented a fast model based on sparse aggregation:
\[
\text{UserRep} = U + \hat{R}I
\]
where \(\hat{R}\) is a row-normalized sparse interaction matrix.  
Training is done with **BPR loss** and negative sampling. Due to dataset scale, we subsampled users for feasibility.

---

### Experimental Setup and Evaluation

We evaluated all algorithms using Top-K ranking metrics:

- **Precision@K**
- **Recall@K**
- **HitRate@K**
- **MRR**
- **Coverage**
- **Error = 1 − Recall**

We tested multiple evaluation scenarios using:
\[
(K,\ \text{Negatives}) \in \{(1,1000), (10,100), (20,50)\}
\]
Negative sampling was designed to avoid leakage by excluding:
- items already seen in training for that user
- positive test items

We also applied **seen-item filtering** to ensure that recommended lists do not include items the user already interacted with in training.

---

## Results

### Quantitative Results

We compared the following methods:
- Popularity
- Random
- Neighbor-CF
- Two-step Co-purchase
- Personalized PPR
- Community-Leiden
- Community-Spectral
- GNN-like model

Across different scenarios, the strongest traditional graph-based methods (Neighbor-CF, Two-step Co-purchase, and community-based scoring) often achieved high Recall and HitRate values. However, we observed that several of these methods produced **very similar outputs**, which resulted in nearly identical metrics. This happened because many of them rely on the same underlying neighborhood structure from the co-purchase graph, meaning they often rank the same candidate items near the top.

In contrast, Popularity and Random baselines performed significantly worse, especially when the candidate set included harder negatives. This confirms that personalization and graph structure are important for recommendation performance.

For Personalized PPR, we expected it to outperform other graph-based methods. However, in practice, it sometimes performed similarly to Popularity or underperformed compared to Two-step methods. The main reason is that our PPR implementation was a restricted approximation and did not fully simulate a multi-step random walk on the entire graph. In addition, candidate set design strongly affects PPR performance: if the candidate list is dominated by popular items or if propagation is restricted too aggressively, PPR becomes less effective.

The lightweight GNN-like model was able to train and produce valid ranking results under subsampling settings. Its performance depended heavily on:
- number of users sampled
- embedding dimension
- number of epochs
- negative sampling strategy

Because the model was simplified to remain fast, it acted more like a one-layer LightGCN approximation rather than a deep GNN. Still, it demonstrated how representation learning can be integrated into graph-based recommendation and produced reasonable Top-K metrics.

---

### Qualitative Analysis

A key qualitative finding of this project is that **evaluation design is as important as the algorithm itself**. In early runs, many methods appeared “too good” or nearly identical. After debugging, we found that if seen-item filtering is missing or if negatives are sampled too easily, evaluation becomes unrealistically optimistic. Once these issues were fixed, the differences between methods became more meaningful, and weak baselines became clearly separated from stronger graph-based approaches.

Additionally, the co-purchase graph structure naturally creates strong item neighborhoods, which explains why multiple neighborhood-based algorithms converge to similar rankings.

---

## Discussion

This project demonstrates how graph mining techniques can be applied to recommendation and how different graph-based approaches compare under realistic evaluation settings. A major strength of the work is that it includes multiple perspectives: baselines, similarity-based collaborative filtering, neighborhood propagation on item graphs, community-aware ranking, and a lightweight GNN-inspired embedding model.

One important observation is that several methods produced nearly identical results, especially Neighbor-CF, Two-step Co-purchase, and community-based scoring. This does not necessarily mean the methods are incorrect; rather, it indicates that they exploit similar information sources. For example, Neighbor-CF and Two-step methods both rely on the principle that “items related to the user’s history are good candidates,” and if the co-purchase graph is dense around popular items, different scoring formulas may still rank the same items.

Another major factor is the candidate set construction. If negative sampling is random across the entire item set, many negatives become extremely easy, making all personalized methods look perfect. On the other hand, using hard negatives (e.g., popularity-based negatives) creates a more challenging and realistic evaluation, which better separates algorithm quality.

Regarding Personalized PPR, we initially expected it to be the best graph-based method. However, PPR requires careful design: true PPR typically performs iterative random-walk propagation on the whole graph, while our restricted implementation only propagated within a limited neighborhood. This made it closer to a weighted neighbor method rather than a full PPR, which explains why it did not dominate.

Finally, the GNN-like model highlighted scalability issues. Training deep graph models on large user–item graphs is expensive, and even simplified models can be heavy without careful optimization. By using sparse matrix multiplication and subsampling, we achieved a runnable pipeline, but the results remain limited by the simplified architecture and short training.

---

## Challenges, Limitations, and Future Work

### Challenges and Issues Encountered
- **Scalability:** Full graph-based deep learning was too heavy to run on the entire dataset, requiring user subsampling and simplified models.
- **Evaluation leakage:** Without filtering already-seen training items, metrics become unrealistically high and many algorithms appear identical.
- **Negative sampling sensitivity:** Random negatives often make evaluation too easy, while hard negatives change results significantly.
- **PPR approximation:** Our Personalized PPR was restricted and did not fully represent true random-walk behavior, which reduced its advantage.
- **Similar ranking outputs:** Many neighborhood-based methods rely on the same co-purchase structure, causing similar Top-K recommendations.

### Future Work
- Implement a **true iterative Personalized PageRank** on the full item graph.
- Add **hard negative sampling strategies** (popularity-based + graph-distance-based).
- Use a stronger GNN architecture (e.g., **LightGCN / GraphSAGE**) with proper batching.
- Evaluate with additional metrics such as **NDCG@K** and diversity/novelty measures.
- Improve community-based methods using real community assignments and intra-community ranking.

---

## Conclusion

In this project, we explored graph mining techniques for recommendation on the Amazon Electronics dataset. We built time-aware train/test splits and represented the data using multiple graph structures including user–item interactions, item co-purchase relations, and user similarity networks. We implemented several recommendation algorithms ranging from simple baselines (Popularity, Random) to graph-based approaches (Neighbor-CF, Two-step Co-purchase, PPR-inspired scoring, and community-based ranking). We also developed a lightweight GNN-inspired model using sparse aggregation and BPR training to demonstrate representation learning in graph-based recommendation.

Our experiments showed that graph-based methods can significantly outperform baselines, but evaluation design (seen-item filtering and negative sampling) strongly impacts reported results. We also observed that multiple neighborhood-based algorithms may produce similar rankings because they exploit the same underlying co-purchase structure. Overall, the project provides practical insights into graph mining for recommender systems and highlights real-world challenges such as scalability, leakage, and evaluation sensitivity.

---

## References
1. Grover, A., & Leskovec, J. (2016). node2vec: Scalable Feature Learning for Networks. *KDD*.  
2. Kipf, T. N., & Welling, M. (2017). Semi-Supervised Classification with Graph Convolutional Networks. *ICLR*.  
3. He, X., Deng, K., Wang, X., Li, Y., Zhang, Y., & Wang, M. (2020). LightGCN: Simplifying and Powering Graph Convolution Network for Recommendation. *SIGIR*.  
4. Page, L., Brin, S., Motwani, R., & Winograd, T. (1999). The PageRank Citation Ranking.  
5. Amazon Product Data. (2014). *Julian McAuley Dataset Repository*.  

---

**Student Signature(s):** Kourosh Jamshidi, Masoud Hasani  
**Date:** Winter 2026
