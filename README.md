# Optimized-IoT-Intrusion-Classification-using-PCA-LDA

## 📌 Introduction
This project addresses the challenges of high-dimensional IoT traffic data for Intrusion Detection Systems (IDS). Using the IoTID20 dataset, it explores dimensionality reduction techniques (PCA, LDA, and hybrid PCA+LDA) to improve classification accuracy while reducing computational cost for real-world IoT deployment.

---

## 📂 Dataset: IoTID20
The **IoTID20 dataset** is a publicly available benchmark for IoT intrusion detection. It contains traffic generated from IoT devices under both normal conditions and attack scenarios such as:

- DoS/DDoS attacks  
- Mirai botnet activity  
- Scanning attacks  
- MITM ARP spoofing  

Each row represents a network flow with features including **packet length, duration, protocol type, and statistical properties**. The target variables include:

- **Label** → Normal (0) or Anomaly (1)  
- **Category (Cat)** → Attack type group  
- **Sub-Category (Sub_Cat)** → Specific attack sub-type  

---

## ⚙️ Preprocessing & Dimensionality Reduction

1. **Data Cleaning**  
   - Removed duplicate rows and irrelevant columns (`Flow_ID`, IPs, Ports, Timestamp).  
   - Converted categorical data to numerical values (binary labels, one-hot encoding for categories/subcategories).  
   - Replaced infinities with column max/min and scaled numerical features to [0,1].

2. **Dimensionality Reduction**  
   - **PCA** → Reduces redundancy by capturing main variance directions.  
   - **LDA** → Projects data to maximize class separability for Label, Category, or Subcategory.  
   - **Hybrid PCA+LDA** → PCA first reduces dimensionality, then LDA enhances discriminative power.  

This preprocessing pipeline ensures computational efficiency while preserving essential information for accurate classification.

---

## 0️⃣|1️⃣ Binary Classification Results

**Model Explanations:**

- **Label LDA Feature** → Uses only the binary label (Normal/Malicious) as a discriminator. Simplest reduction, but loses finer details.  
- **Category LDA Features** → Groups attacks into broader categories (e.g., DoS, Probe). Captures more structure than Label-based.  
- **Subcategory LDA Features** → Uses detailed subcategories of attacks, giving more discriminative power.  
- **PCA + LDA (Category/Subcategory)** → First reduces dimensionality with PCA, then applies LDA. Helps balance speed and accuracy.  

**Model Performance Comparison (Binary Classification):**  

![Binary Classification Performance](imgs/Binary_Comparison.png)  

✅ **Best Models (Binary):**  
- **30 PCA + LDA (Subcategory)** → Highest accuracy (**99.99%**)  
- **Subcategory LDA Features** → Best trade-off between accuracy and speed (fit: 998 ms, predict: 3 ms)  

---

## 🔢 Multi-Class Classification Results

**Model Explanations:**

- **Category LDA components** → Projects data into fewer dimensions based on attack categories (e.g., DoS, Probe, R2L).  
- **Subcategory LDA components** → Uses finer-grained attack subcategories, improving separability between different attack types.  
- **PCA + LDA (Category/Subcategory)** → Combines PCA (to reduce redundancy) and LDA (to enhance class separation).  
- **Category LDA + Cross Validation** → Validates generalization performance across multiple folds, more robust but slower.  

**Model Performance Comparison (Multi-Class Classification):**  

![Multi-Class Classification Performance](imgs/Multiclass_Comparison.png)  

✅ **Best Models (Multi-Class):**  
- **Subcategory LDA components** → Best overall F1-score (**0.996**) with efficient training.  
- **Category LDA + Cross Validation** → Excellent recall, but computationally expensive.
