# MLOPs-DVC-data-version-control
this repo implement the data versioning using DVC pipeline

readme_content = """# Comprehensive Guide to Data Version Control (DVC) in MLOps Pipelines

[![DVC](https://img.shields.io/badge/Data%20Version%20Control-DVC-brightgreen.svg)](https://dvc.org/)
[![Git](https://img.shields.io/badge/Version%20Control-Git-orange.svg)](https://git-scm.com/)
[![Python](https://img.shields.io/badge/Language-Python-blue.svg)](https://www.python.org/)
[![MLOps](https://img.shields.io/badge/Field-MLOps-violet.svg)](https://mlops.org/)

## 📌 Project Overview
In production Machine Learning workflows, code and data evolve along entirely separate trajectories. While Git is the industry standard for tracking code changes, it is fundamentally unsuited for handling large datasets, binary files, and model weights. 

This repository demonstrates a production-grade **Data Version Control (DVC)** workflow integrated seamlessly with **Git**. By treating data-as-code, this pipeline ensures absolute reproducibility, eliminates data bloating in your Git history, and establishes an untangled data lineage across multiple operational versions (V1, V2, V3).

---

## 🏗️ Dual-Layer Architecture
# Data Version Control (DVC) Workflow with Git & S3

## Overview

This project demonstrates an end-to-end **Data Version Control (DVC)** workflow integrated with:

* Git for source code versioning
* DVC for dataset tracking
* Amazon Web Services S3 for remote data storage

The objective is to build a reproducible and production-style ML/Data Engineering workflow where:

* Code versions are tracked with Git
* Dataset versions are tracked with DVC
* Large files are stored efficiently in S3
* Every data modification becomes reproducible and auditable

This repository simulates a real-world industry workflow followed in ML Engineering, MLOps, and Data Science teams.

---

# Architecture Workflow

```text
Developer Machine
       │
       │  Git Commit
       ▼
    Git Repository
       │
       │  DVC Add
       ▼
    DVC Tracking
       │
       │  DVC Push
       ▼
   AWS S3 Remote Storage
```

---

# Tech Stack

| Tool   | Purpose                        |
| ------ | ------------------------------ |
| Python | Data generation & manipulation |
| Git    | Source code versioning         |
| DVC    | Data versioning                |
| AWS S3 | Remote storage backend         |
| CSV    | Dataset format                 |

---

# Project Structure

```bash
project/
│
├── data/
│   └── dataset.csv
│
├── mycode.py
│
├── .dvc/
├── .dvcignore
├── data.dvc
│
├── requirements.txt
└── README.md
```

---

# Step-by-Step Workflow

---

# 1. Create Git Repository

Initialize a Git repository and clone it locally.

```bash
git init
git clone <repository-url>
```

---

# 2. Create Python Script

Create a Python file that generates or modifies data.

Example:

```python
import pandas as pd
import os

os.makedirs("data", exist_ok=True)

df = pd.DataFrame({
    "id": [1, 2, 3],
    "name": ["A", "B", "C"]
})

df.to_csv("data/dataset.csv", index=False)

print("CSV file created successfully.")
```

Save as:

```bash
mycode.py
```

Run:

```bash
python mycode.py
```

---

# 3. Initial Git Commit

Before initializing DVC, commit the current codebase.

```bash
git add .
git commit -m "Initial project setup"
```

---

# 4. Initialize DVC

Install and initialize DVC.

```bash
pip install dvc
dvc init
```

This creates:

```bash
.dvc/
.dvcignore
```

Commit DVC initialization:

```bash
git add .
git commit -m "Initialize DVC"
```

---

# 5. Create S3 Remote Directory

Create a directory reference for remote storage.

```bash
mkdir S3
```

---

# 6. Configure DVC Remote

Connect DVC to remote storage.

```bash
dvc remote add -d myremote S3
```

For AWS S3 in production:

```bash
dvc remote add -d myremote s3://your-bucket-name/path
```

Verify:

```bash
dvc remote list
```

---

# 7. Start Tracking Data with DVC

Track the data directory.

```bash
dvc add data/
```

This creates:

```bash
data.dvc
```

---

## If Git Already Tracks the Data Folder

Remove data tracking from Git first:

```bash
git rm -r --cached data
git commit -m "Stop tracking data with Git"
```

Now re-add using DVC:

```bash
dvc add data/
```

Track metadata files:

```bash
git add data.dvc .gitignore
```

Check status:

```bash
dvc status
git status
```

---

# 8. Push Data to Remote Storage

Commit DVC metadata:

```bash
dvc commit
```

Push actual data to remote storage:

```bash
dvc push
```

At this point:

* Git stores metadata
* DVC tracks versions
* S3 stores actual datasets

---

# 9. Save Version 1 (V1)

Commit the first official dataset version.

```bash
git add .
git commit -m "Version 1 of dataset"
git push
```

---

# 10. Modify Dataset

Update `mycode.py` to append new rows.

Example:

```python
new_row = pd.DataFrame({
    "id": [4],
    "name": ["D"]
})

df = pd.read_csv("data/dataset.csv")
df = pd.concat([df, new_row], ignore_index=True)

df.to_csv("data/dataset.csv", index=False)
```

Run again:

```bash
python mycode.py
```

Check changes:

```bash
dvc status
```

---

# 11. Track New Data Version

Commit and push updated dataset.

```bash
dvc commit
dvc push
```

---

# 12. Save Version 2 (V2)

Save metadata changes with Git.

```bash
git add .
git commit -m "Version 2 of dataset"
git push
```

---

# 13. Verify Everything

Ensure both systems are synchronized.

```bash
git status
dvc status
```

Expected:

```bash
nothing to commit, working tree clean
Data and pipelines are up to date.
```

---

# 14. Repeat for Future Versions

Continue modifying data and repeating:

```text
Modify Data
→ dvc status
→ dvc commit
→ dvc push
→ git add
→ git commit
→ git push
```

This creates:

* V3
* V4
* V5
* and so on...

---

# Key Benefits of This Workflow

## Reproducibility

Every dataset version can be restored exactly.

```bash
git checkout <commit>
dvc pull
```

---

## Collaboration Friendly

Teams can:

* Share datasets efficiently
* Avoid pushing huge files into Git
* Reproduce experiments reliably

---

## Scalable Architecture

This workflow is production-ready for:

* ML pipelines
* Data Engineering
* MLOps systems
* Experiment tracking

---

# Useful DVC Commands

| Command         | Purpose                 |
| --------------- | ----------------------- |
| `dvc init`      | Initialize DVC          |
| `dvc add data/` | Track dataset           |
| `dvc status`    | Check data changes      |
| `dvc commit`    | Save dataset state      |
| `dvc push`      | Upload data to remote   |
| `dvc pull`      | Download data           |
| `dvc checkout`  | Restore dataset version |

---

# Example Workflow Timeline

| Version | Action                       |
| ------- | ---------------------------- |
| V1      | Initial dataset created      |
| V2      | New row appended             |
| V3      | Additional updates           |
| V4      | Cleaned and transformed data |

---

# Real Industry Use Cases

This workflow is commonly used in:

* Recommendation Systems
* Fraud Detection Pipelines
* NLP Training Systems
* Computer Vision Datasets
* Enterprise MLOps Platforms

Companies using similar workflows include:

* Netflix
* Uber
* Airbnb
* Spotify

---

# Future Improvements

Possible enhancements:

* CI/CD integration
* Automated DVC pipelines
* ML experiment tracking
* Docker containerization
* GitHub Actions automation
* Model registry integration
* Terraform infrastructure setup

---

# Author

**Shivam Kumar**
Data Science | Machine Learning | MLOps | AI Engineering

---

# Final Notes

This project demonstrates:

* Strong understanding of version control
* Real-world MLOps workflow implementation
* Cloud-integrated data engineering practices
* Reproducible machine learning infrastructure

A recruiter or engineering team reviewing this project can immediately recognize familiarity with modern ML infrastructure and collaborative data workflows.

