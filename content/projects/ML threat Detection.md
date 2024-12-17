
+++
title = "Building a Machine Learning-Based Threat Detection System"
description = "An end-to-end cybersecurity project to detect threats using machine learning and log data."
type = ["projects", "post"]
tags = ["machine learning", "cybersecurity", "python", "threat detection"]
date = "2024-12-17"
categories = ["Cybersecurity", "Machine Learning"]
series = ["Cybersecurity Projects"]
[author]
name = "Francesco Citti"
+++

## 🚀 Overview

In this project, I built a **threat detection system** using machine learning and log data.

---

## 1. **Data Preprocessing**

Here’s an example of raw logs and how I processed them:

**Raw Logs**:
```
Dec 15 00:12:10 server sshd[12345]: Failed password for invalid user admin from 192.168.1.2 port 22
```

**Python Code for Preprocessing**:
```python
import pandas as pd

# Load log file
logs = pd.read_csv("auth.log")

# Extract IP and Status
logs["IP"] = logs["raw"].str.extract(r'from (\d+\.\d+\.\d+\.\d+)')
logs["Status"] = logs["raw"].str.contains("Failed").replace({True: "Failed", False: "Success"})

print(logs.head())
```

---

## 2. **Building the Isolation Forest Model**

```python
from sklearn.ensemble import IsolationForest

# Feature engineering
features = logs[["IP", "Timestamp"]]
features["IP"] = features["IP"].astype("category").cat.codes

# Train Isolation Forest
model = IsolationForest(contamination=0.01)
logs["Anomaly"] = model.fit_predict(features)

# View anomalies
print(logs[logs["Anomaly"] == -1])
```

---

## Results

| Timestamp       | IP             | Status   | Anomaly |
|-----------------|----------------|----------|---------|
| 1702591930     | 192.168.1.2    | Failed   | -1      |

---

## 🔗 GitHub Repository

[Check the full code here](https://github.com/FrancescoCitti/ml-threat-detection).
