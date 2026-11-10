# AI-Based Self-Healing Network and Intrusion Detection System

A machine learning based Intrusion Detection System (IDS) that classifies network traffic as **Normal** or **Attack** and demonstrates a **self-healing mechanism** by simulating automated recovery actions after malicious activity is detected.

> **Scope:** This is a prototype. The ML model is trained and evaluated on a real benchmark dataset (NSL-KDD). Traffic is simulated by replaying recorded connections, and the self-healing actions are workflow templates that print and log steps. They do not modify the host system or connect to real firewalls or services.

## Project Overview

Traditional Intrusion Detection Systems can identify malicious network activity, but they usually need manual intervention afterwards. This project extends a conventional IDS with a **self-healing module** that runs predefined recovery steps whenever an intrusion is detected.

It is implemented in Python using the **NSL-KDD dataset** and a **LightGBM classifier**, selected through cross-validation.

## Features

- Binary classification of network traffic (Normal / Attack)
- Data preprocessing and categorical feature encoding
- Model comparison using 3-fold stratified cross-validation
- Evaluation with Accuracy, Precision, Recall, F1-Score and Confusion Matrix
- Simulated self-healing actions
- Security event logging to CSV
- Model saving and loading with Joblib
- Developed in Jupyter Notebook

## Technologies Used

- Python
- Jupyter Notebook
- Pandas, NumPy
- Scikit-learn, XGBoost, LightGBM
- Matplotlib, Seaborn
- Joblib

## Dataset

This project uses **NSL-KDD**, a widely used benchmark for network intrusion detection research. It is an improved version of KDD Cup 1999 with duplicate records removed.

| Item | Value |
|---|---|
| Training records | 125,973 |
| Test records | 22,544 |
| Features | 41 (3 categorical, the rest numeric) |
| Classes used | Normal (0), Attack (1) |
| Attack families | DoS, Probe, R2L, U2R |

Each record is one network connection. Features cover basic connection details, content indicators, 2-second time-window traffic statistics, and host-based statistics over the last 100 connections.

The test set contains attack types that **do not appear in the training set**, so test results measure how well the model generalizes to unseen attacks.

Dataset files:

- `KDDTrain+.txt`
- `KDDTest+.txt`

Place both files inside the **datasets** folder before running the notebook.

## Project Workflow

```
NSL-KDD Dataset
      │
      ▼
Data Preprocessing + Feature Encoding
      │
      ▼
Model Comparison (cross-validation on training data)
      │
      ▼
LightGBM Classifier  →  evaluated once on the test set
      │
      ▼
Intrusion Detection (Normal / Attack)
      │
      ▼
Self-Healing Simulation
  ├── Detect Attack
  ├── Block Malicious IP (simulated)
  ├── Update Firewall Rules (simulated)
  ├── Restart Network Service (simulated)
  ├── Generate Alert
  └── Save Security Logs
```

## Machine Learning Model

**Model selection**

Five tree-based models were compared using 3-fold stratified cross-validation (F1 score) on the training data only:

| Model | CV F1 |
|---|---|
| Decision Tree | 0.9976 |
| Random Forest | 0.9986 |
| Gradient Boosting | 0.9955 |
| XGBoost | 0.9986 |
| **LightGBM** | **0.9988** |

The top models are within about 0.0003 of each other, so the differences between them are small. LightGBM was selected for the best score and fast training. The test set was used only once, after selection.

**Classification type**

- Normal Traffic → 0
- Attack Traffic → 1

## Model Performance

Final model: **LightGBM**, evaluated on the NSL-KDD test set (22,544 records).

| Class | Precision | Recall | F1-Score | Support |
|---|---:|---:|---:|---:|
| Normal (0) | 0.69 | 0.97 | 0.81 | 9,711 |
| Attack (1) | 0.97 | 0.67 | 0.79 | 12,833 |

**Overall test accuracy: about 80%** (macro F1: 0.80)

### How to read these results

- **Precision (Attack) 0.97:** when the model flags an attack, it is correct 97% of the time, so false alarms are rare.
- **Recall (Attack) 0.67:** the model catches about two thirds of real attacks, so roughly one in three attacks is missed. In security, recall is the more important number.
- **CV vs test gap:** cross-validation F1 is about 0.999, while test accuracy is about 80%. This is expected because the test set includes attack types never seen during training. The model recognizes known attacks very well but is weaker on unseen ones.

### Fixes made during development

An earlier version of this notebook reported about 86% accuracy. Reviewing the evaluation revealed three problems, which were fixed:

| Issue found | Fix |
|---|---|
| `LabelEncoder` was fitted separately on train and test data, which can map the same category to different numbers | Fit `OrdinalEncoder` on training data only, then apply it to test data |
| The `difficulty` column (a dataset annotation that would not exist in real traffic) was used as a feature | Removed from the feature set |
| The model was chosen using test-set accuracy | Switched to cross-validation on training data; test set used once |

After these fixes, test accuracy is about 80%. The lower number is the more trustworthy one.

## Self-Healing Mechanism

When an intrusion is detected, the system runs a predefined recovery template:

- Detect malicious activity
- Block malicious IP (simulation)
- Update firewall rules (simulation)
- Restart affected network service (simulation)
- Generate security alert
- Log the incident for future analysis

> **Note:** The self-healing actions are simulated for demonstration purposes. They print and log steps and do not modify the host system or any real network device.

## Limitations

- **Simulated environment:** the project replays recorded benchmark data instead of capturing live traffic. Real traffic would need a feature-extraction step to produce the same 41 features.
- **Template-based response:** recovery steps are not connected to real firewalls or service managers.
- **Attack recall is about 67%:** roughly one in three attacks is missed, especially attack types unseen in training.
- **Binary classification:** the response does not yet adapt to the attack family (DoS, Probe, R2L, U2R).
- **Old dataset:** NSL-KDD derives from 1990s data and does not reflect modern or encrypted traffic.
- **Traffic mix:** the test set is roughly half attacks, while real networks are mostly normal traffic, so precision would be lower in a real deployment.

## Project Structure

```
Self-Healing-Intrusion-Detection/
│
├── dataset/
│   ├── KDDTrain+.txt
│   └── KDDTest+.txt
│
├── AI_Self_Healing_Intrusion_Detection.ipynb
├── ids_model.pkl
├── network_logs.csv
├── requirements.txt
├── README.md
```

## Installation

Clone the repository:

```bash
git clone https://github.com/vaishali0204/AI-Self-Healing-Network.git
```

Move into the project folder:

```bash
cd AI-Self-Healing-Network
```

Install the required libraries:

```bash
pip install -r requirements.txt
```

## How to Run

1. Download the NSL-KDD dataset.
2. Place `KDDTrain+.txt` and `KDDTest+.txt` inside the `datasets` folder.
3. Open the Jupyter Notebook.
4. Use **Restart & Run All** so every cell runs from top to bottom.
5. View the model comparison, evaluation metrics and self-healing simulation.

---

## Future Enhancements

- Multi-class detection (DoS, Probe, R2L, U2R) so the response matches the attack type
- Threshold tuning and per-attack-family analysis to improve recall
- Real-time packet capture using Scapy or PyShark, with feature extraction
- Deep Learning models (LSTM/CNN)
- Flask or Streamlit dashboard
- Integration with Software Defined Networking (SDN)
- Real firewall integration with confidence thresholds and human approval
- Load-aware traffic redirection and automatic service restarts
- Retraining on modern datasets such as CIC-IDS2017
- Docker and cloud deployment (AWS/Azure)

## Learning Outcomes

This project demonstrates practical knowledge of:

- Machine Learning and model comparison with cross-validation
- Detecting and fixing evaluation mistakes (encoding and feature leakage)
- Network Security and Intrusion Detection Systems
- Data Preprocessing
- Model Evaluation (precision, recall, F1, confusion matrix)
- Security Automation concepts
- Python Programming

## Reference

M. Tavallaee, E. Bagheri, W. Lu, A. Ghorbani. *A Detailed Analysis of the KDD CUP 99 Data Set.* IEEE CISDA, 2009.

## Author

**Vaishali**

If you have suggestions or would like to improve this project, feel free to fork the repository or open an issue.
