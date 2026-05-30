# Network Anomaly Detection using Random Forests

**An ensemble-learning approach to identifying zero-day network intrusions by establishing a baseline of normal traffic behavior.**

---

## Overview

Traditional signature-based intrusion detection systems frequently fail to identify novel, zero-day attacks. This repository contains a machine learning-based defense mechanism utilizing a Random Forest ensemble model. By training the algorithm exclusively on benign, normal network traffic, it establishes a statistical baseline of the network environment. Any deviation from this baseline—measured via high prediction variance or low confidence scores across the decision trees—is flagged as a potential anomaly or malicious intrusion.

**Note on Documentation:** This README serves as a high-level technical summary of the repository's purpose and usage. Comprehensive project documentation, including mathematical foundations, in-depth pipeline explanations, exploratory data analysis, and rigorous evaluation metrics, has been fully documented externally. Please refer to the `docs/` directory for the complete GitBook documentation.

---

## Core Architecture

This pipeline leverages the Random Forest algorithm to process complex, high-dimensional network telemetry without relying on extensive dimensionality reduction. The architecture relies on three primary mechanisms:

1. **Bootstrapping for Data Diversity:** The model generates multiple subsets of normal network traffic using random sampling with replacement. Each subset trains a distinct decision tree, allowing the ensemble to capture a wide variance of benign behavioral patterns.
2. **Feature Subsets in Tree Construction:** At each node split, the algorithm evaluates only a random subset of network features. This prevents the model from overfitting to dominant features and enforces a deeper contextual understanding of the traffic.
3. **Aggregation and Confidence Scoring:** During inference, a network event is processed by the entire forest. The final prediction confidence is the proportion of trees classifying the event as normal. If this confidence score falls below a dynamically defined threshold, the event is flagged as an anomaly.

---

## Dataset

The model is trained and validated using a modified version of the KDD benchmark dataset (NSL-KDD). This dataset provides labeled instances of normal traffic alongside specific attack categories, allowing for robust multi-class classification and threshold testing.

---

## Quick Start & Installation

**Prerequisites:** Python 3.9+

Clone the repository and navigate to the project directory:
   ```bash
   git clone [https://github.com/aashraymt2/network-anomaly-detection.git](https://github.com/aashraymt2/network-anomaly-detection.git)
   cd network-anomaly-detection
   ```
Establish a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # Windows environments: venv\Scripts\activate
```

Install pipeline dependencies:
```bash
pip install -r requirements.txt
```

---

## Usage Pipeline

The codebase is modularized to separate data processing, training, and inference.

**Data Ingestion:**
Downloads and maps the KDD dataset features to a structured format.
```bash
python src/data_loader.py
```

**Model Training:**
Trains the multi-class Random Forest model and exports the serialized `.joblib` binary.
```bash
python src/train.py
```

**Local Inference:**
Executes the trained model against a test set or live packet capture to generate classification reports.
```bash
python src/detect.py --model models/network_anomaly_detection_model.joblib
```

---

## Remote Evaluation

The final verification of this architecture requires remote validation. The model must be submitted to an external evaluation portal running on a designated virtual machine. The included evaluation script automates the API submission of the serialized model.

```bash
python src/htb_evaluate.py
```

If the model successfully meets the required accuracy thresholds against the server's hidden holdout dataset, the portal returns a cryptographic token verifying the deployment's success.

---

## Comprehensive Documentation

For full project details, please navigate to the GitBook documentation [](https://aashraymt.gitbook.io/docs/sms-spam-detector/network-anomaly-detector). The extensive documentation covers:

* Theoretical concepts of anomaly detection
* Feature selection and mapping tables
* Training logic and validation matrices
* Advanced deployment instructions
