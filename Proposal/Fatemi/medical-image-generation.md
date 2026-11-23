**Graph Mining Course (4041)**  
**Instructor: Dr. Zeinab Maleki**  
**Student Name: Sayed Mohammad Fatemi**  
**Student ID: 40127793**  
**Email: [lucimad.dev@gmail.com](mailto:Lucimad.dev@gmail.com)**

# **Graph-Conditioned Diffusion-Based Data Augmentation for Retinal Fundus Images**

---

## **Abstract**

    **Medical image analysis systems are highly dependent on large and diverse datasets; however, many clinically valuable datasets, especially those collected at a national or regional level, suffer from severe data scarcity. Recent advances in graph-based generative modeling have shown that incorporating explicit structural information into the generation process can significantly improve the realism and usefulness of synthetic data. In this project, we adopt a recent graph-conditioned diffusion model proposed in *IEEE Transactions on Medical Imaging* (SurGrID, 2025\) and apply it to retinal fundus images from an official Iranian dataset provided by the Medical Image and Signal Processing (MISP) Center. Images are first represented as graphs encoding anatomical regions and spatial relationships, which are then used to guide diffusion-based image synthesis. The generated images are employed for data augmentation, and their effectiveness is evaluated using image quality metrics and downstream diabetic retinopathy classification performance.**

---

## **I. Introduction and Motivation**

    **Deep learning has achieved state-of-the-art performance in various medical image analysis tasks; however, its success is strongly tied to the availability of large-scale annotated datasets. In real-world clinical settings, especially within domestic healthcare systems, acquiring such datasets is challenging due to privacy constraints, acquisition costs, and limited patient populations. Retinal fundus imaging for diabetic retinopathy screening is a representative example where limited data diversity can significantly reduce model generalization.**

    **Traditional data augmentation techniques, such as geometric transformations or intensity perturbations, increase dataset size but do not introduce meaningful structural diversity and may distort anatomical consistency. Generative models such as GANs and diffusion models address this limitation by synthesizing new images, but most approaches treat images as unstructured pixel grids and fail to explicitly model anatomical relationships.**

    **Graph mining provides a principled framework for capturing such relationships by representing images as graphs, where nodes correspond to meaningful regions and edges encode spatial or semantic dependencies. Recent graph-conditioned generative models demonstrate that incorporating graph structure into the generation process leads to more realistic and semantically consistent images. Motivated by these advances, this project investigates the application of a graph-conditioned diffusion model to an Iranian retinal fundus dataset, aiming to improve data augmentation quality and downstream diagnostic performance.**

---

## **II. Objectives**

**The main objectives of this project are as follows:**

* **To construct graph representations of retinal fundus images that encode anatomical regions and spatial relationships.**  
* **To apply and fine-tune a graph-conditioned diffusion model (SurGrID) for synthetic fundus image generation.**  
* **To evaluate the realism and fidelity of generated images using standard image quality metrics.**  
* **To assess the impact of graph-based data augmentation on diabetic retinopathy classification performance.**

---

## **III. Related Work**

    **Graph-based generative modeling has recently emerged as an effective approach for structure-aware medical image synthesis. SurGrID \[1\] introduces a graph-conditioned diffusion framework that synthesizes medical and surgical images by conditioning the denoising process on scene graphs encoding object-level and spatial relationships. Related work on latent graph representations combined with adversarial learning has demonstrated improved structure preservation in medical image augmentation \[2\]. Comprehensive surveys on medical image data augmentation further emphasize the limitations of traditional GAN- and diffusion-based approaches that lack explicit structural constraints \[3\]. This project builds directly on SurGrID by reusing its published architecture and source code, while extending its application to retinal fundus imaging and an official Iranian dataset.**

---

## **IV. Proposed Methodology, Dataset(s), and Evaluation Plan**

### **Methodology Overview**

    **The goal of this project is to apply an existing graph-conditioned diffusion model to generate additional training data for a small Iranian retinal fundus image dataset. Since the dataset consists of raw 2D images and not graphs, the first step is to explicitly construct graph representations from images before generation. The overall pipeline consists of: (i) image-to-graph construction, (ii) graph-conditioned image generation using a pretrained model, and (iii) evaluation via image quality and downstream classification.**

### **Dataset(s)**

    **The primary dataset is Colour Fundus Images of Healthy Persons and Patients with Diabetic Retinopathy, provided by the Medical Image and Signal Processing (MISP) Center, Isfahan University of Medical Sciences. The dataset contains approximately 60 RGB retinal fundus images covering healthy and diabetic retinopathy cases.**  
**Images are provided in raw form and do not include graph annotations. In this project, each image is resized to a fixed resolution and converted into an undirected graph. Nodes represent anatomically meaningful regions or fixed image patches, while edges encode spatial adjacency and relative position. Node features are extracted using a pretrained CNN or Vision Transformer.**

### **Techniques and Algorithms**

* **Image-to-Graph Construction: Patch-based or region-based graph modeling with spatial edges**  
* **Graph-Conditioned Diffusion Model: SurGrID (IEEE TMI, 2025), reused without architectural modification**  
* **Feature Extraction: Pretrained CNN / ViT embeddings**  
* **Tools: PyTorch, PyTorch Geometric, NetworkX**

### **Evaluation Plan**

    **Generated images are evaluated using Fréchet Inception Distance (FID) and Structural Similarity Index (SSIM) to measure realism and fidelity. To assess practical usefulness, a diabetic retinopathy classifier is trained using (i) the original dataset and (ii) the augmented dataset. Performance is compared using classification accuracy and AUC, with the non-augmented model serving as the baseline. We can also contact the dataset providers to ask and verify the quality of our generated images.**

---

## **V. Challenges and Resources**

    **The primary challenge is the limited size of the Iranian dataset, which restricts training from scratch. This is mitigated through transfer learning and careful fine-tuning of the pretrained diffusion model. Another challenge is defining anatomically meaningful graph representations, addressed by leveraging established retinal structures and automated region extraction methods. Required resources include GPU access, the MISP dataset, and the publicly released SurGrID source code.**

---

## **References**

**\[1\] SurGrID: Controllable Surgical Simulation via Scene Graph to Image Diffusion, *IEEE Transactions on Medical Imaging*, 2025\.**

**\[2\] K. Arias *et al.*, “Structure-Preserving Medical Image Generation from a Latent Graph Representation,” *IEEE Transactions on Medical Imaging*, 2025\.**

**\[3\] E. Goceri, “Medical Image Data Augmentation: Techniques, Comparisons and Interpretations,” *Artificial Intelligence Review*, 2023\.**

**\[4\] Medical Image and Signal Processing (MISP) Center, Isfahan University of Medical Sciences.**  
[**https://misp.mui.ac.ir/en/colour-fundus-images-healthy-persons-patients-diabetic-retinopathy**](https://misp.mui.ac.ir/en/colour-fundus-images-healthy-persons-patients-diabetic-retinopathy)

**\[5\] SurGrID Official Source Code, GitHub repository:**  
[**http://github.com/MECLabTUDA/SurGrID**](http://github.com/MECLabTUDA/SurGrID)