# Chinese-Vietnamese Sentence Alignment

![Python](https://img.shields.io/badge/Python-3.12-blue)
![NLP](https://img.shields.io/badge/NLP-Sentence%20Alignment-orange)
![PyTorch](https://img.shields.io/badge/PyTorch-LaBSE-red)
![DVC](https://img.shields.io/badge/MLOps-DVC%20%7C%20DagsHub-purple)

A Python project to extract, clean, and align Chinese-Vietnamese bilingual text using **Language-agnostic BERT Sentence Embedding (LaBSE)** and **Dynamic Programming**. 

The project also uses **DVC (Data Version Control)** to track datasets separately from the codebase, effectively handling large files and copyrighted material without bloating the Git repository.

---

## 📌 Overview
Sentence alignment is a crucial pre-processing step for building Machine Translation models or cross-lingual information retrieval systems. This project tackles the challenge of processing highly unstructured heterogeneous data (JSON, PDFs) and creating a clean, high-quality parallel corpus.

## 🚀 Key Features
* **Data Extraction & Cleaning:** Parses text from diverse formats (JSON, PDFs) and filters out document noise like tables of contents or vocabulary lists.
* **Sentence Segmentation:** Uses custom Regex rules to split sentences accurately in both languages, avoiding false splits on abbreviations.
* **Semantic Alignment:** Maps sentences into a shared vector space using **LaBSE** to evaluate semantic similarity rather than just lexical overlap.
* **Dynamic Programming:** Implements a Vecalign-inspired algorithm to find the optimal alignment path. It supports 1-1, 1-2, and 2-1 mappings to handle translation divergences (e.g., when one sentence in Chinese translates to two in Vietnamese).
* **Data Management:** Uses **DVC** and **DagsHub** to host and version-control the datasets remotely, keeping the main GitHub repository lightweight and secure.

---

## ⚙️ Methodology

The alignment strategy recursively maximizes the total similarity score considering possible alignment types. The scoring metric is Cosine Similarity computed on LaBSE embeddings. 

To handle complex sentence structures, the algorithm calculates the average vector for span mappings. For a 1-2 mapping (Span Target), the score is computed using the Cosine Similarity between the source vector and the average of the two target vectors:
$v_{avg} = \frac{v_{T_{j-1}} + v_{T_{j}}}{2}$

A skip penalty ($-0.6$) and a minimum confidence threshold ($0.4$) are applied to ensure only high-quality pairs are retained, maintaining an efficient runtime complexity.

---

## 📂 Repository Structure & Data Access

Due to copyright and data privacy restrictions, the datasets are managed via **DVC** and hosted securely on **DagsHub**. The GitHub repository contains the codebase, documentation, and a small set of structural samples.

```text
chinese-vietnamese-alignment/
├── assets/                   # Structural screenshots of raw data
├── data/                     
│   ├── aligned_data.dvc      # DVC pointer to the final aligned datasets
│   └── cleaned_data.dvc      # DVC pointer to the pre-processed datasets
├── docs/                     # Project report
├── notebooks/                
│   ├── 01_clean_data.ipynb   # Extraction and segmentation pipeline
│   └── 02_Alignment.ipynb    # LaBSE and Dynamic Programming alignment
├── .gitignore                
├── requirements.txt          
└── README.md
```
## 📊 Results Summary

The pipeline processed four distinct datasets, yielding the following alignment distributions:

| Dataset | 1-1 Pairs | 1-2 Pairs | 2-1 Pairs | Untranslated | Total Clean |
| :--- | :--- | :--- | :--- | :--- | :--- |
| WikiHow (JSON) | 21,189 | 3,021 | 3,281 | 5,870 | **27,491** |
| Multimedia (JSON) | 2,456 | 0 | 0 | 3 | **2,456** |
| Letters (PDF) | 99 | 3 | 22 | 10 | **124** |
| Funny Stories (PDF) | 84 | 11 | 8 | 8 | **103** |
| **TOTAL** | **23,828** | **3,035** | **3,311** | **5,891** | **30,174** |
---

## 🛠️ Usage

**1. Clone the repository:**
```bash
git clone https://github.com/MinhHCMUSsv/chinese-vietnamese-alignment.git
cd chinese-vietnamese-alignment
```

**2. Install dependencies:**
It is recommended to use a virtual environment.
```bash
python -m venv venv
# Windows: venv\Scripts\activate
# Mac/Linux: source venv/bin/activate
pip install -r requirements.txt
```

**3. Pull the datasets (Requires DagsHub access):**
```bash
dvc pull
```

**4. Run the pipeline:**
Execute the notebooks in sequence to reproduce the results:
* Run `notebooks/01_clean_data.ipynb`
* Run `notebooks/02_Alignment.ipynb`

---

## 👤 Author
**Tăng Nhật Minh**
* Student ID: 23127425 | Class: 23CNTThuc2
* Ho Chi Minh City University of Science (HCMUS)