# Graph Mining Course Project Progress Report

**Submission Date:** 1404/10/12
**Course:** Graph Mining 4041
**Instructor:** Dr. Zeinab Maleki
**Project Title:** Comparing Bi-Directional Graph Convolutional Networks and Standard GCNs for Detecting Satire and Fake News on Reddit

## Student Information

* **Student Name(s):** Mahrokh Mousavi & MohammadHossein Shojaodiny
* **Student ID(s):** 40131663 & 40125043
* **Email(s):** mahrokhmousavii44@gmail.com & reynardipg@gmail.com

## Executive Summary

Since our proposal, we have focused on setting up the environment and writing the code to handle the Fakeddit dataset. We successfully processed a subset of the data (20,000 posts) and built the conversation trees required for the graph models. We have finished implementing both the Standard GCN and the Bi-GCN models using PyTorch Geometric. Our initial tests show that the Bi-GCN model performs better than the standard one, specifically for the "Satire" class, which aligns with our main goal. We are now ready to run the final comparisons and analyze the results in detail.

## Progress on Objectives

### Objective 1: Build directed propagation trees from the Fakeddit dataset

**Status: Completed.**
We wrote a script to process the data. Since the dataset is very large, we had to scan about 10 million lines of comments to find the ones related to our 20,000 target posts.

* We created a graph for each thread: the post is the root node, and comments are child nodes.
* We used the BERT model (`all-MiniLM-L6-v2`) to turn the text of posts and comments into vectors (embeddings).
* We successfully built over 10,000 valid graphs to use for training.

### Objective 2: Implement the Bi-GCN model

**Status: Completed.**
We coded the Bi-GCN architecture in Python. This model looks at the graph in two ways:

1. **Top-Down:** How the news spreads from the post to the comments.
2. **Bottom-Up:** How the users react to the post (from comments back to the post).
We combine the information from both directions to decide if a post is Fake, Real, or Satire.

### Objective 3: Implement a standard GCN as a baseline model

**Status: Completed.**
We also implemented a simple Standard GCN. This model only looks at the Top-Down flow. We use this as a baseline to see if adding the "user reaction" part (in Bi-GCN) actually makes a difference or not.

### Objective 4: Compare model performance on Satire, Fake, and Real classes

**Status: In Progress.**
We have trained both models (using a 70% Train, 10% Validation, 20% Test split). We checked the Confusion Matrices and F1-Scores. We also added a simple function to test the model on single, random graphs to see the predictions manually. The initial results support our hypothesis.

## Work Accomplished

### Dataset Preparation and Analysis

Working with the Fakeddit dataset was challenging because of the file sizes.

* **Processing:** We couldn't load the comment file all at once because of RAM limits. We wrote a loop to read the file in chunks (200,000 lines at a time), keep the useful comments, and discard the rest.
* **Graph Building:** We filtered out threads that had less than 2 comments because they don't give enough structural information.
* **Saving Progress:** Since generating BERT embeddings takes a long time on Google Colab, we added a feature to save the graphs in batches (every 500 graphs). This way, if the internet disconnected, we didn't lose our progress.

### Implementation Details

We used **Python** with **PyTorch Geometric** and **NetworkX**.

* For the **Standard GCN**, we used two convolutional layers and a global pooling layer.
* For the **Bi-GCN**, we used two parallel sets of layers (one for each direction) and concatenated their outputs before the final classification.
* We also wrote a helper function `predict_single_graph` that takes a graph and prints the predicted label vs. the actual label in plain text. This helps us verify the model's behavior on individual examples.

### Preliminary Results

We trained both models for 20 epochs. Here is a summary of the test results:

| Metric | Standard GCN (Baseline) | Bi-GCN (Proposed) | Improvement |
| --- | --- | --- | --- |
| **Accuracy** | ~84% | **~87%** | +3% |
| **Satire F1** | 0.54 | **0.60** | Significant |
| **Fake F1** | 0.91 | **0.93** | Slight |

**Observation:**
The Confusion Matrices (attached below) show that the Bi-GCN makes fewer mistakes when distinguishing between "Satire" and "Fake News". This suggests that user comments/reactions are indeed helpful for detecting satire.

<img width="522" height="470" alt="Confusion Matrix 1" src="https://github.com/user-attachments/assets/49397814-fc9b-46aa-a9e2-ba5b95f64005" />
<img width="522" height="470" alt="Confusion Matrix 2" src="https://github.com/user-attachments/assets/13712a2b-f09f-4b5d-8730-e55f09ea39da" />


## Challenges Encountered and Resolutions

* **Challenge 1: Large Dataset & Memory Issues**
* **Description:** The comment file was too big to load into memory directly. It caused the system to crash.
* **Resolution:** We implemented a "chunk reading" strategy. We process the file piece by piece instead of loading it all at once.


* **Challenge 2: Long Processing Time**
* **Description:** converting text to embeddings with BERT is very slow on the CPU. It takes hours to process 20,000 posts.
* **Resolution:** We added a checkpoint system. The code saves the processed data to Google Drive periodically. This allowed us to pause and resume the work without starting from zero every time Colab disconnected.



## References

1. Bian, T. et al. (2020). *Rumor Detection on Social Media with Bi-Directional Graph Convolutional Networks*. AAAI.
2. Nakamura, K. et al. (2020). *Fakeddit: A New Multimodal Fake News Dataset*. LREC.
3. Fey, M., & Lenssen, J. E. (2019). *Fast Graph Representation Learning with PyTorch Geometric*.

---

**Student Signature(s):** Mahrokh Mousavi & MohammadHossein Shojaodiny
**Date:** 1404/10/12
