# AI-Based Self-Healing Network and Intrusion Detection System

An AI-powered Intrusion Detection System (IDS) that uses Machine Learning to classify network traffic as **Normal** or **Attack** and demonstrates a **self-healing mechanism** by simulating automated recovery actions after detecting malicious activity.

## Project Overview

Traditional Intrusion Detection Systems can identify malicious network activity, but they often require manual intervention after an attack is detected. This project extends a conventional IDS by integrating a **self-healing module** that automatically performs predefined recovery actions whenever an intrusion is identified.

The project is implemented using the **NSL-KDD dataset** and an optimized **Gradient Boosting Classifier** in Python.

## Features

- Binary classification of network traffic (Normal / Attack)
- Data preprocessing and feature encoding
- Gradient Boosting machine learning model (benchmark-optimized)
- Intrusion detection using network traffic features
- Performance evaluation with Accuracy, Precision, Recall, F1-Score, and Confusion Matrix
- Simulated self-healing actions
- Security event logging
- Model saving and loading using Joblib
- Developed using Jupyter Notebook

## Technologies Used

- Python
- Jupyter Notebook
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn
- Joblib

## Dataset

This project uses the **NSL-KDD** dataset, a widely used benchmark dataset for network intrusion detection research.

Dataset Files:

- `KDDTrain+.txt`
- `KDDTest+.txt`

Place both files inside the **dataset** folder before running the notebook.

## Project Workflow
                NSL-KDD Dataset
                        │
                        ▼
               Data Preprocessing
                        │
                        ▼
                Feature Encoding
                        │
                        ▼
        Gradient Boosting Classifier
                        │
                        ▼
             Intrusion Detection
                        │
                        ▼
          Self-Healing Simulation
      ├── Detect Attack
      ├── Block Malicious IP
      ├── Update Firewall Rules
      ├── Restart Network Service
      ├── Generate Alert
      └── Save Security Logs


## Machine Learning Model

**Algorithm Used**

- Gradient Boosting Classifier (Selected after comparative benchmarking against Random Forest, XGBoost, LightGBM, and Decision Trees)

**Classification Type**

- Normal Traffic → 0
- Attack Traffic → 1

## Model Performance

| Metric             |     Value |
| ------------------ | --------: |
| Accuracy           |  **85.91%** |
| Precision (Attack) |      0.97 |
| Recall (Attack)    |      0.78 |
| F1-Score (Attack)  |      0.86 |

### Confusion Matrix

|               | Predicted Normal | Predicted Attack |
| ------------- | ---------------: | ---------------: |
| Actual Normal |             9409 |          302     |
| Actual Attack |             2866 |         9967     |

The model demonstrates strong precision when identifying malicious traffic while achieving an overall optimized accuracy of **85.91%** on the NSL-KDD test dataset.

## Self-Healing Mechanism

When an intrusion is detected, the system simulates the following actions:

- Detect malicious activity
- Block malicious IP (Simulation)
- Update firewall rules (Simulation)
- Restart affected network service (Simulation)
- Generate security alert
- Log the incident for future analysis

> **Note:** The self-healing actions in this project are simulated for demonstration purposes and do not modify the host system.


## Project Structure

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
└── images/

## Installation
Clone the repository:
git clone https://github.com/vaishali0204/AI-Self-Healing-Network.git

Move into the project folder:
cd AI-Self-Healing-Network

Install the required libraries:
pip install -r requirements.txt

## How to Run

1. Download the NSL-KDD dataset.
2. Place `KDDTrain+.txt` and `KDDTest+.txt` inside the `dataset` folder.
3. Open the Jupyter Notebook.
4. Run all notebook cells from top to bottom.
5. View the evaluation metrics and self-healing simulation.

---

## Future Enhancements

- Real-time packet capture using Scapy or PyShark
- Deep Learning models (LSTM/CNN)
- Flask or Streamlit dashboard
- Integration with Software Defined Networking (SDN)
- Automated firewall updates
- Docker deployment
- Cloud deployment (AWS/Azure)

## Learning Outcomes

This project demonstrates practical knowledge of:

- Machine Learning & Model Benchmarking
- Network Security
- Intrusion Detection Systems (IDS)
- Cybersecurity Fundamentals
- Data Preprocessing
- Model Evaluation
- Security Automation
- Python Programming

## Author

**Vaishali**

If you have suggestions or would like to improve this project, feel free to fork the repository or open an issue.
