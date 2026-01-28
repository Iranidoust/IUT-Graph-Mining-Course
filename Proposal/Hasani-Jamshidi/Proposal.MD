# Graph Mining Course Project Proposal

**Submission Date:** 2025-12-07 
**Course:** Graph Mining 4041  
**Instructor:** Dr. Zeinab Maleki  

---

## Student Information
- **Student Name(s):** Masoud Hasani , Korosh Jamshidi
- **Student ID(s):** 40121243 , 40120523
- **Email(s):** masood.hsn82@gmail.com , k.jamshidi@ec.iut.ac.ir

---

## Project Title
**Graph-based Analysis and Recommendation of Amazon Electronics Products**

---

## Abstract
This project aims to analyze Amazon electronics product data using graph mining techniques to uncover insights about user behavior, product relationships, and recommendation potential. We will construct a user–product bipartite graph and a product–product graph based on "also_bought" and "also_viewed" metadata. Graph metrics including PageRank, clustering coefficient, betweenness, and closeness centralities will be computed to identify influential products and users. Additionally, we will explore user similarity and clustering to enhance recommendation strategies. Finally, we will study the degree distribution of the product graph to assess its adherence to a power-law, providing insights into on-demand products and network structure. Expected outcomes include a graph-based recommendation system and a detailed characterization of the product network.

---

## Problem and Motivation
Online retail platforms like Amazon generate massive interaction data between users and products. Understanding these relationships can help improve recommendation systems, identify influential products, and detect patterns in user behavior. Graph mining provides a powerful framework to model these interactions: a bipartite graph captures user–product purchases, while a product–product graph models relationships such as co-purchases and co-views. Centrality metrics (PageRank, betweenness) can identify popular or influential items, while clustering techniques can detect communities of similar products or user groups. Analyzing degree distributions and network structures can also reveal fundamental properties such as adherence to power-law distributions, common in real-world networks. This study combines structural analysis, user modeling, and graph-based recommendation, providing both academic insights and practical implications for e-commerce applications.

---

## Objectives
- Construct user–product and product–product graphs from Amazon electronics data.  
- Compute centrality and clustering metrics to identify influential users and products.  
- Analyze user similarity and clusters to enhance personalized recommendations.  
- Evaluate a graph-based recommendation system using multiple graph metrics and product relationships.

---

## Related Work
Prior work has shown that graph-based approaches improve recommendation accuracy in e-commerce datasets. For instance, Yu et al. (2014) analyzed user-product bipartite networks to enhance collaborative filtering recommendations. Leskovec et al. (2007) introduced the Amazon product co-purchase network and studied structural properties including degree distributions and link prediction. Our project builds on these foundations by combining multiple graph analysis techniques (centrality, clustering, power-law analysis) and leveraging both user and product networks for recommendation, providing a more holistic view of network structure and user behavior.

---

## Proposed Methodology

### Dataset(s)
- **Source:** SNAP Amazon Electronics dataset  
- **Files:** `reviews_Electronics_5.json`, `meta_Electronics.json`, `ratings_Electronics.csv`  
- **Size:** ~1.7M reviews, ~500k products, ~7.8M ratings
- **Graph Types:**  
  - User–Product bipartite graph (undirected, weighted by purchase frequency)  
  - Product–Product co-purchase/view graph (undirected, weighted by link count)  
- **Preprocessing:**  
  - Extract user IDs and product IDs from reviews  
  - Parse metadata for "also_bought" and "also_viewed" relationships  
  - Merge rating data to include user satisfaction  

### Techniques and Algorithms
- **Graph Construction:** Using NetworkX to build bipartite and product graphs.  
- **Centrality Analysis:** PageRank, betweenness, and closeness to identify key nodes.  
- **Clustering:** Compute clustering coefficients and detect product/user communities.  
- **User Similarity:** Jaccard similarity on purchased items to build a user similarity graph.  
- **Recommendation:**  
  - Rank products using PageRank and user similarity  
  - Recommend products via co-purchase and co-view links  
- **Power-Law Analysis:** Plot degree distribution of product graph and fit power-law model.

### Evaluation Plan
- Compare recommended products with actual user purchases in `ratings_Electronics.csv`  
- Metrics: Precision@K, Recall@K, and coverage for recommendations  
- Baselines: Random recommendation and popularity-based recommendation

---

## Challenges and Resources
- **Challenges:**  
  - Large dataset may require efficient memory handling and graph representation  
  - Sparsity of user-product interactions may impact similarity measures  
- **Resources:**  
  - Python, NetworkX, Pandas, Matplotlib/Seaborn for analysis and visualization

---

## References
1. Leskovec, J., et al. (2007). *The Dynamics of Viral Marketing*. ACM Transactions on the Web.  
2. Yu, H., et al. (2014). *Graph-Based Recommender Systems: A Review*. IEEE Access.  
3. SNAP Amazon Reviews Dataset. [http://snap.stanford.edu/data/amazon/productGraph.html](http://snap.stanford.edu/data/amazon/productGraph.html)

---

**Student Signature(s):** [Typed name]  
[Date]
