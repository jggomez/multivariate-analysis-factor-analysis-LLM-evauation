# Multivariate Analysis of LLM Evaluation Data

## Problem

A multivariate dataset ($n = 200, p = 3000$) is available, derived from the evaluation of a Large Language Model (LLM), NLA-7B. The $n$ observations correspond to unique prompts (queries), while the $p$ variables represent a heterogeneous set of metrics (internal activations, human evaluations, and derived performance measures).

The dataset exhibits a severe high-dimensionality problem ($p \gg n$), which renders the direct application of classical multivariate methods invalid. The study pursues a dual objective:

1.  To psychometrically validate a 20-item human evaluation sub-instrument, the theoretical structure of which posits four latent constructs (Quality, Safety, Creativity, Bias).
2.  To implement a Factor-Cluster Analysis strategy to identify typological profiles (clusters) of the prompts based on the entirety of the variables, thereby overcoming the challenge.

## Step-by-step

### 1. Environment Setup

*   Installation of necessary libraries: `rich`, `factor-analyzer`, `semopy`.
*   Import of `pandas`, `rich.Console`, `FactorAnalyzer`.

### 2. Data Loading and Initial Inspection

*   The `neuro_lingua_ai.csv` dataset was loaded into a pandas DataFrame.
*   The shape `(200, 3001)` was confirmed, indicating 200 observations and 3001 variables (including `prompt_id`).

### 3. Validation of the Human Evaluation Instrument Structure

*   **Isolation of Human Evaluation Variables:** A subset of 20 variables, `hum_eval_1` to `hum_eval_20`, was extracted for analysis.
*   **Confirmatory Factor Analysis (CFA):**
    *   A four-factor measurement model (Quality, Safety, Creativity, Bias) was specified based on theoretical hypotheses.
    *   The model was fitted using `semopy`.
    *   Goodness-of-fit indices (Chi-square, RMSEA, CFI) were calculated. The model demonstrated a good to excellent fit, supporting the construct validity of the instrument with its four-factor structure.
 
<img width="2009" height="838" alt="model_plot" src="https://github.com/user-attachments/assets/3188ea95-f443-4e26-ba3a-2e3a9c75c2ea" />

### 4. Dimensionality Reduction and Feature Extraction

*   **P >> N Problem:** Acknowledgment of the high dimensionality ($p \gg n$) challenge and the need for dimensionality reduction.
*   **Principal Component Analysis (PCA):**
    *   Data was scaled using `StandardScaler`.
    *   PCA was performed to identify the number of components.
    *   Scree plots and cumulative explained variance plots were used to assess the optimal number of components.
    *   It was decided to reduce the analysis to **2 components** to capture the most dominant structure and avoid overfitting, despite low total explained variance.

<img width="850" height="547" alt="img2" src="https://github.com/user-attachments/assets/2437f554-e393-4f06-a0a0-7063942b9d41" />
<img width="846" height="547" alt="img3" src="https://github.com/user-attachments/assets/0c79c3f2-e50c-4af3-afd0-06eb42fef35f" />

*   **Factor Analysis (FA):**
    *   Factor Analysis with two components and `varimax` rotation was performed on the scaled data.
    *   Loadings were inspected to understand the contribution of original variables to the new factors.

### 5. Identification of Typologies (Factor-Cluster Analysis)

*   **Justification:** Factor-Cluster Analysis was chosen to mitigate the curse of dimensionality, extract signal from noise, and eliminate multicollinearity.
*   **Hierarchical Clustering (Dendrogram):** A dendrogram using Ward's method was generated to visually inspect potential clusters and suggest an optimal number of clusters.
<img width="1617" height="624" alt="img5" src="https://github.com/user-attachments/assets/3c6b193f-0995-4430-921c-0e37930f6343" />

*   **Silhouette Analysis (K-Means):** Silhouette scores were calculated for a range of clusters (k=2 to 10) using K-Means to find the optimal number of clusters, which was determined to be **3**.
<img width="855" height="470" alt="img6" src="https://github.com/user-attachments/assets/eba6b88f-d267-4813-af9b-8f4a63fc4993" />

*   **Robustness Check:** The Adjusted Rand Index (ARI) was calculated between K-Means and Agglomerative Clustering results, showing high robustness (ARI > 0.70).

*   **Clustering Visualization:** Clusters were visualized in the 2-dimensional latent space derived from Factor Analysis.
<img width="844" height="624" alt="img8" src="https://github.com/user-attachments/assets/d7112637-92b4-4db5-89d6-72ed8059d5c5" />

### 6. Characterization and Interpretation of Profiles

*   Cluster centroids were calculated and visualized using a heatmap to understand the average profile of each cluster based on the two latent factors.
<img width="1189" height="490" alt="img9" src="https://github.com/user-attachments/assets/db7d2d72-5a73-449a-87cb-f2d0b101d2dd" />
<img width="797" height="547" alt="img10" src="https://github.com/user-attachments/assets/a65e75c5-8b59-42c7-b292-5e5a5d09bf40" />


## Conclusions and Report

**Key Finding:**
The model does not function as a single monolithic block; instead, it activates **three distinct operating modes** (Typologies) based on two primary axes: Reasoning versus Creativity.

### 1. Identified Typologies
*   **Cluster 1 (Analytical Mode):** Logic, code, and mathematics prompts. These demand high precision.
*   **Cluster 0 (Creative Mode):** Free writing and roleplay prompts. These demand variety and style.
*   **Cluster 2 (Routine Mode):** Simple queries with low resource demand.

### 2. Strategic Implications

**For Fine-Tuning (Training):**
*   **Data Segregation:** Do not mix data. Model performance can be improved by using distinct datasets for analytical prompts and creative prompts.

**For Evaluation:**
*   **Segmented Evaluation:** Discontinue the use of global averages.
    *   Evaluate **Cluster 1** using logic benchmarks/exams.
    *   Evaluate **Cluster 0** using creativity and diversity metrics.
    *   Failure in **Cluster 2** indicates basic inefficiency.
 
## Author

* **Juan Guillermo Gómez**
* Linkedin: [@jggomezt](https://www.linkedin.com/in/jggomezt/)

***
