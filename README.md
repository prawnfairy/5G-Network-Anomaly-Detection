# Deep Learning Model Analysis for Network Anomaly Detection

Final Year Project (FYP) — Bachelor of Computer Science (Hons) Artificial Intelligence, Faculty of Information Science & Technology, Multimedia University (July 2026).

**Author:** Sofia Batrisyia Binti Mohamad Faris<br>
**Supervisor:** Assoc. Prof. Ts. Dr. Pang Ying Han

## Project Overview

A lightweight deep learning-based network anomaly detection system designed for resource-constrained 5G environments. The model combines a **Conv1D** feature extractor, a **Minimal Gated Unit (MGU)** for temporal modelling, and an **Additive Attention** mechanism, further enhanced with a hybrid **Continual Learning (CL)** framework, comprising **Experience Replay (ER) + Elastic Weight Consolidation (EWC)**, to mitigate catastrophic forgetting as new attack types are learned in sequence.

## Project Objectives

1. Design an efficient, lightweight recurrent architecture for network anomaly detection suitable for resource-constrained 5G environments.
2. Develop and evaluate a continual learning mechanism (ER + EWC), allowing the model to learn new attack types sequentially without forgetting previously learned ones.
3. Assess the trade-offs between model complexity, detection accuracy, and computational efficiency (model size, training time, inference time) against baseline models.

## Tech Stack

**Modeling**
- Python 3, Jupyter Notebook
- PyTorch/TensorFlow
- NumPy, Pandas, scikit-learn
- Matplotlib/Seaborn for visualization

**Dataset (Public)**
- [5G-NIDD](https://www.kaggle.com/datasets/humera11/5g-nidd-dataset/data) — 5G Network Intrusion Detection Dataset (Kaggle)

## Model Architecture

| Component | Description |
|---|---|
| **Conv1D** | Local feature extraction over input sequences — 64 channels, kernel size 3, batch normalization, ReLU |
| **MGU (Minimal Gated Unit)** | Lightweight alternative to GRU/LSTM using only 2 gates (forget gate + candidate state), hidden size 64 |
| **Additive Attention** | Softmax-weighted attention over the sequence dimension via a learnable context vector, to focus on the most informative time steps |
| **Classification Head** | Dropout (0.3) + fully connected linear layer mapping the context vector to class logits |

**Key training configuration:** 
- Sequence length 20 time steps
- Batch size 256
- 70 epochs
- Adam optimizer (lr = 0.001, weight decay 1e-5)
- Focal Loss (γ = 1.0) for class imbalance
- CosineAnnealingLR schedule (down to 1e-6)
- Early stopping (patience 10).

**Continual learning (ER + EWC):**
- The dataset is split into 5 sequential tasks (each introducing 1–2 new attack classes).
- Each training batch mixes 75% new-task samples with 25% samples replayed from a per-class memory buffer (capacity 500 samples/class, reservoir sampling).
- EWC computes a Fisher Information Matrix after each task (λ = 1e-4) to penalize large updates to parameters important for previously learned tasks.

## Dataset — 5G-NIDD

The **5G Network Intrusion Detection Dataset (5G-NIDD)** was captured from an operational 5G testbed emulating real 5G core/RAN traffic, and is used as the primary dataset for this project.

- ~2–3 million labelled flow records (≈55–60% benign, ≈40–45% malicious)
- Attack categories include DDoS, SYN flood, Slow-rate DoS, and other protocol-level/flooding attacks
- Provided as CSV, with features extracted from raw PCAP captures (protocol attributes, packet statistics, timing information)
- Supports both **binary** (benign vs. attack) and **multiclass** (specific attack type) classification
- Dataset page: https://www.kaggle.com/datasets/humera11/5g-nidd-dataset/data

**Preprocessing pipeline:**
Missing-value handling (median/mode imputation) → removal of redundant/uninformative features → `StandardScaler` normalization of numerical features → one-hot encoding of categorical features → train/validation/test split.

## Key Results

An ablation study compared four variants — **MGU**, **MGU + CL**, **MGU + Additive Attention**, and **MGU + Additive Attention + CL** (the proposed model) — under clean (0% noise) and noisy (30% noise) conditions:

| Task | Condition | Best variant | Accuracy | Macro F1 |
|---|---|---|---|---|
| Binary | Noise 0% | All variants comparable | ~99.8–99.9% | ~0.998 |
| Binary | Noise 30% | MGU + Additive Attention + CL | 67.19% | 0.6147 |
| Multiclass | Noise 0% | MGU + Additive Attention + CL | ~99.79% | 0.8701 |
| Multiclass | Noise 30% | MGU + Additive Attention + CL | 66.85% | 0.3454 |

The proposed **MGU + Additive Attention + CL** model was also markedly more efficient to train — roughly **78–84% faster** than the baseline MGU (e.g., ~2,942s vs. ~18,514s on multiclass) — while adding only a small increase in model size (~80 KB → ~91–93 KB). Full per-class metrics, confusion matrices, and comparative analysis against existing works are available in the project report.

## Project Structure

```
5G-Network-Anomaly-Detection/
├── Dataset/                                                        # not included in repo — download separately (see below)
├── Lightweight GRU (MGU + Additive Attention + CL) TimeSeries_Binary Model.ipynb
├── Lightweight GRU (MGU + Additive Attention + CL) TimeSeries_Multiclass Model.ipynb
└── README.md
```

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/prawnfairy/5G-Network-Anomaly-Detection.git
cd 5G-Network-Anomaly-Detection
```

### 2. Set up the environment

```bash
python -m venv venv
source venv/bin/activate       # On Windows: venv\Scripts\activate
pip install numpy pandas scikit-learn tensorflow matplotlib seaborn jupyter kaggle
```

> Adjust the package list to match the exact imports used in the notebooks (e.g., swap `tensorflow` for `torch` if the model is implemented in PyTorch).

### 3. Download the dataset

The dataset (`5G_NIDD_Full.csv`) is **not included in this repository** as it exceeds GitHub's file size limits. Instead, this README links to the source so you can download it yourself:
https://www.kaggle.com/datasets/humera11/5g-nidd-dataset/data

**Manual download**
1. Sign in to Kaggle and open the dataset page above.
2. Click **Download** and extract the archive.
3. Move/rename the extracted CSV to `Dataset/5G_NIDD_Full.csv`.

### 4. Run the notebooks

```bash
jupyter notebook
```

Both notebooks read the dataset from `Dataset/5G_NIDD_Full.csv`, so make sure the extracted file is named exactly according to the path directory. Then open either notebook and run all cells:

- `Lightweight GRU (MGU + Additive Attention + CL) TimeSeries_Binary Model.ipynb` — binary classification (benign vs. attack)
- `Lightweight GRU (MGU + Additive Attention + CL) TimeSeries_Multiclass Model.ipynb` — multiclass classification (specific attack type)

## Notes

The dataset is excluded from version control (see `.gitignore`) due to its size. It must be downloaded locally following the steps above before running the notebooks. This project was developed as part of an academic Final Year Project; core logic, architecture, and implementation decisions are original work. If you use the 5G-NIDD dataset, please cite it per the guidance on its Kaggle page.

> **Disclaimer:** This repository was developed solely for academic purposes as a Final Year Project (FYP) submitted in partial fulfilment of the Bachelor of Computer Science (Hons) Artificial Intelligence at the Faculty of Information Science & Technology, Multimedia University. It is not intended for production or commercial use, and no warranty is provided regarding its accuracy, performance, or suitability for real-world deployment.
