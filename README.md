# 🧠 Neural Decoding of Linguistic Meaning from fMRI

<p align="center">
  <img src=technion.png alt="Technion Logo" width="120"/>
</p>

**Institution**: Technion – Israel Institute of Technology  
**Course**: Language, Computation, and Cognition  
**Year**: 2025  

---

## 📖 Overview

This repository contains all code, data, and documentation for my **Language, Computation, and Cognition** project.  
The project investigates how **linguistic meaning** can be decoded from human brain activity (fMRI) using **static** and **contextual** semantic embeddings, and whether such decoders can generalize across participants.

The work replicates and extends the methodology of **Pereira et al. (2018)**, exploring:
1. **Static embeddings**: GloVe vs Word2Vec for single-word decoding.
2. **Contextual embeddings**: BERT for sentence-level decoding.
3. **Brain encoders**: Predicting voxel activity from embeddings.
4. **Inter-subject generalization**: Transfer learning of decoders between participants.

---


---

## 📊 Dataset

The project uses the **Pereira et al. (2018)** fMRI dataset ([MIT EvLab link](https://web.mit.edu/evlab//sites/default/files/documents/index2.html)):

- **Participants**: 15 English-speaking adults (1 excluded from original 16 due to preprocessing format differences).
- **Stimuli**:
  - **Experiment 1**: 180 isolated concepts (nouns, verbs, adjectives) shown with sentence, image, and word cloud disambiguation.
  - **Experiment 2**: 384 sentences across 24 topics (Wikipedia-style).
  - **Experiment 3**: 243 sentences from 24 new topics (mix of factual & narrative).
- **fMRI Data**: Averaged across repetitions per stimulus.
- **Semantic Representations**:
  - **GloVe** (300-dim)
  - **Word2Vec** (300-dim)
  - **BERT** (768-dim, also PCA-reduced to 300-dim for comparison)

---

## 🛠 Preprocessing

### fMRI Preprocessing
- Averaged voxel responses over repetitions.
- PCA projection per participant to reduce dimensionality & standardize voxel space across subjects.
- Saved as `M##_avg_pca.csv` (one file per participant).

### Embeddings
- **Static**: Loaded pre-trained GloVe & Word2Vec.
- **Contextual**: Extracted sentence embeddings using BERT-base.
- Dimensionality reduction (PCA) applied to BERT embeddings for fair comparison to static embeddings.

---

## 🧪 Experiments

Note: Some parts of the notebooks are not included in the project report.
These were exploratory experiments to satisfy personal curiosity and should not be considered as part of the formal results.

### 1️⃣ Single-Word Decoding (Replication of Pereira et al. Exp. 1)
- **Goal**: Compare GloVe vs Word2Vec when decoding isolated concepts.
- **Method**: Ridge regression decoder trained on voxel → embedding mapping.
- **Evaluation**: Cross-fold mean rank accuracy (chance = 90 for 180 items).

### 2️⃣ Sentence Decoding (Replication of Exp. 2 & 3)
- **Goal**: Test generalization from single-word decoders to sentence-level stimuli.
- **Evaluation**:
  - Rank accuracy (sentence-level chance = 192 for Exp. 2, 121.5 for Exp. 3).
  - Topic-level analysis to detect domain-specific decoding strengths.

### 3️⃣ Direct Sentence-Level Decoding (Extension)
- **Goal**: Train decoder directly on sentence-level fMRI, compare GloVe vs BERT vs BERT-PCA.
- **Finding**: BERT-PCA outperformed both BERT and GloVe.

### 4️⃣ Brain Encoding Models
- **Goal**: Reverse mapping (embedding → voxel activation).
- **Analysis**: Voxelwise linear regression; significance via permutation testing.
- **Finding**: BERT encoders predict more voxels and with higher R².

### 5️⃣ Inter-Subject Generalization (Open-Ended Task)
- **Paradigms**:
  - **Full-data**: Train on all other participants’ data.
  - **Averaged-data**: Aggregate fMRI responses across participants (arithmetic, geometric, harmonic means).
  - **Pairwise transfer**: Train on participant i, test on participant j.
- **Finding**: No strong global generalization; certain pairs show strong transfer → potential subgroup-shared semantic representations.

---

## 💻 How to Reproduce

### 1. Clone Repository
```bash
git clone https://github.com/jeremyjrnt/LaCC-Project.git
cd LaCC-Project
