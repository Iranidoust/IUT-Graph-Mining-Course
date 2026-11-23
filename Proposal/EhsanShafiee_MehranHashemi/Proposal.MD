# Graph Mining Course Project Proposal

**Submission Date:** 12/11/2025
**Course:** Graph Mining [4041]  
**Instructor:** [Dr. Zeinab Maleki]  

## Student Information
- **Student Name(s):** Ehsan Shafiee , Mehran Hashemi  
- **Student ID(s):** 40125223  , 40133203 
- **Email(s):** esheshesh83@gmail.com , mehranstock1383@gmail.com

## Project Title
Graph Neural Network-Based Book Recommendation: A Comparative Study of GraphSAGE and Matrix Factorization on the Book-Crossing Dataset

## Abstract
In the domain of personalized recommendation, traditional methods often struggle to capture high-order connectivity between users and items or fail to generalize to unseen nodes. This project proposes a **Graph Neural Network (GNN)** approach to book recommendation using the **GraphSAGE** framework. By modeling the Book-Crossing dataset as a heterogeneous bipartite graph, we aim to predict missing links (ratings) between users and books. The project will implement a GraphSAGE model using PyTorch Geometric to generate node embeddings by aggregating feature information from local neighborhoods. We will compare this deep learning approach against a strong baseline: **Matrix Factorization (MF)**. The expected outcome is a performance evaluation demonstrating how graph-based feature aggregation compares to latent factor models in terms of predictive accuracy (RMSE) and ranking metrics.

## Problem and Motivation
With the exponential growth of digital libraries, users face information overload. Recommender systems are essential for filtering content. The core research question is: Can aggregating local neighborhood information in a user-item graph outperform traditional latent factor models in predicting user preferences?

This problem is significant in graph mining because recommendation can be framed as a link prediction task. While traditional Collaborative Filtering relies solely on the interaction matrix, Graph Mining techniques allow us to utilize structural properties (like centrality and community structure) and node features (e.g., user age, book author) simultaneously.

We utilize the Book-Crossing dataset, a real-world dataset containing explicit ratings. This project is motivated by the need for inductive learning capabilities—where the model can generate embeddings for new users or books without retraining—a key advantage of GraphSAGE over transductive methods like standard Matrix Factorization.

## Objectives
Objective 1: Construct a heterogeneous graph from the Book-Crossing dataset, encoding user and book metadata as node features and ratings as edge weights.

Objective 2: Implement a GraphSAGE (SAmple and aggreGatE) model using PyTorch Geometric to learn low-dimensional embeddings for users and books.

Objective 3: Implement a Matrix Factorization (MF) baseline (using SVD or SGD) to serve as a performance benchmark.

Objective 4: Evaluate and compare both models using Root Mean Square Error (RMSE) for rating prediction and Precision@K for recommendation ranking.

## Related Work
Recommender systems have historically relied on Matrix Factorization (MF) techniques, such as those popularized during the Netflix Prize (Koren et al., 2009), which map users and items to a joint latent factor space. While effective, MF often fails to capture the complex, non-linear structural connectivity of a graph.

More recently, Graph Neural Networks (GNNs) have emerged as a powerful alternative. GraphSAGE (Hamilton et al., 2017) introduced an inductive framework that generates embeddings by sampling and aggregating features from a node’s local neighborhood, rather than learning a distinct embedding for every node. This project builds on recent work like NGCF (Neural Graph Collaborative Filtering), applying the specific aggregation logic of GraphSAGE to the bipartite book-recommendation context.

## Proposed Methodology
### Dataset(s)
We will use the **Book-Crossing** Dataset, which consists of three files: BX-Users.csv, BX-Books.csv, and BX-Book-Ratings.csv.

- **Nodes**: Users (~278,000) and Books (~271,000).

- **Edges**: Explicit ratings (scale 1-10) and implicit interactions (0).

- **Preprocessing**: We will filter for users and books with a minimum interaction threshold (k-core filtering) to reduce sparsity. We will convert metadata (User Location, Book Author, Year) into numerical feature vectors using sentence transformer text embeding models and normalization.

**Implementation Plan (Data Loading)**: We will use pandas to process the CSVs and torch_geometric.data.HeteroData to build the graph.


### Techniques and Algorithms
1. **GraphSAGE (Main Model):** We will use the GraphSAGE operator (SAGEConv) to aggregate messages between Users and Books. Since the graph is bipartite, we will apply the convolution twice (User $\to$ Book, Book $\to$ User) or use a Heterogeneous GNN wrapper.

- *Input*: Node features (Age, Location, Author).
- *Process*: Sample neighbors $\to$ Aggregate features (Mean/Max pool) $\to$ Update node embedding.
- *Prediction*: The dot product of the final user embedding and book embedding predicts the rating.

2. **Matrix Factorization (Baseline):**
We will implement a standard Singular Value Decomposition (SVD) or a simple Embedding-based dot product model using PyTorch or the Surprise library. This assumes the rating matrix $R$ can be approximated by $U \times V^T$.

### Evaluation Plan
We will split the edges (ratings) into Training (80%), Validation (10%), and Testing (10%) sets.

- **Metric 1 (Accuracy):** RMSE (Root Mean Square Error) to measure how close the predicted rating is to the actual rating.

- **Metric 2 (Ranking):** Recall@K and Normalized Discounted Cumulative Gain (NDCG) to see if high-rated books appear at the top of the recommendation list.

## Challenges and Resources
**Challenges:**

**Data Sparsity:** The Book-Crossing dataset is highly sparse (many users rate very few books). Mitigated by K-core filtering and utilizing node content features via GraphSAGE.

**Scalability:** Computing embeddings for all nodes is expensive. We will use PyTorch Geometric’s NeighborLoader for mini-batch training.

## References
1. Hamilton, W., Ying, Z., & Leskovec, J. (2017). Inductive representation learning on large graphs. Advances in neural information processing systems, 30.[link](https://arxiv.org/pdf/1706.02216)

2. Koren, Y., Bell, R., & Volinsky, C. (2009). Matrix factorization techniques for recommender systems. Computer, 42(8), 30-37. [link](https://datajobs.com/data-science-repo/Recommender-Systems-[Netflix].pdf)

3. Shoujin Wang1, Liang Hu2,3, Yan Wang,Xiangnan He4, Quan Z. Graph Learning based Recommender Systems: A Review [link](https://www.ijcai.org/proceedings/2021/0630.pdf)

---