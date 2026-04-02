# 🎯 Lecture: Data Version Control (DVC) in MLOps

## 🧠 Agenda

We will cover:

1. **Why Data Versioning is needed (WHY)**
2. **What is DVC (WHAT)**
3. **How DVC works (HOW – Theory + Practical)**

---

# 1️⃣ WHY: Why Data Versioning?

## 🔁 Problem in ML Projects

A typical ML pipeline:

```
Data Ingestion → Data Preprocessing → Feature Engineering → 
Feature Selection → Model Training → Evaluation
```

Each stage produces **artifacts (outputs)**:

* Raw data
* Cleaned data
* Feature datasets
* Model files
* Metrics (JSON)

### 🚨 Key Issue

ML is **experimental**:

* Data changes
* Features change
* Models change
* Metrics change

👉 Example:

* Experiment 1 → Accuracy: 85%
* Experiment 2 → Accuracy: 90%
* Experiment 3 → Accuracy: 82%

Now question:

> ❓ How do you go back to Experiment 2?

---

## ⚠️ Without Data Versioning

* No history of datasets
* Cannot reproduce results
* Cannot rollback
* Team collaboration breaks

---

## ✅ Solution: Data Versioning

Just like:

* Git → versioning for **code**
* DVC → versioning for **data**

---

# 2️⃣ WHAT: What is DVC?

## 📦 Definition

**DVC (Data Version Control)** is a tool that:

* Tracks large datasets
* Stores them efficiently
* Links them with Git commits

---

## 🔄 Git vs DVC

| Feature             | Git             | DVC                        |
| ------------------- | --------------- | -------------------------- |
| Tracks              | Code            | Data & ML artifacts        |
| Handles large files | ❌ Not efficient | ✅ Designed for large data  |
| Storage             | Git repo        | External (S3, local, etc.) |
| Versioning          | Line-by-line    | File-level via hashes      |

---

## 💡 Key Idea

* Git stores **small pointer files**
* DVC stores **actual data separately**

---

# 3️⃣ HOW: How DVC Works (Theory)

## 🧩 Core Concept

Each dataset version gets:

* A **unique hash (MD5)**
* Stored in remote storage
* Tracked via `.dvc` file

---

## 🧠 Analogy (from transcript – simplified)

Think:

* Git = Counter 1 (code + small tokens)
* DVC = Counter 2 (actual data storage)

Process:

1. Store data → DVC gives **token (ID)**
2. Save token in Git
3. Later:

   * Git gives token
   * DVC fetches correct data

---

## 🔁 Flow

```
Code Version (Git)
        ↓
Pointer (.dvc file with hash)
        ↓
Actual Data (DVC remote storage)
```

---

# 4️⃣ Practical: Step-by-Step DVC Implementation

---

## 🧱 Step 1: Create Git Repo

```bash
git init
git remote add origin <repo_url>
```

---

## 📁 Step 2: Create Sample Data

Example Python file (`mycode.py`):

```python
import pandas as pd
import os

data = {
    "name": ["Alice", "Bob", "Charlie"],
    "age": [25, 30, 35],
    "city": ["A", "B", "C"]
}

df = pd.DataFrame(data)

os.makedirs("data", exist_ok=True)
df.to_csv("data/sample.csv", index=False)
```

Run:

```bash
python mycode.py
```

---

## 🧾 Step 3: Initial Git Commit

```bash
git add .
git commit -m "initial commit before DVC"
git push
```

---

## ⚙️ Step 4: Initialize DVC

```bash
dvc init
git add .
git commit -m "initialize DVC"
```

---

## ☁️ Step 5: Add Remote Storage

(Local simulation of S3)

```bash
mkdir s3
dvc remote add -d myremote s3
```

---

## 📊 Step 6: Track Data with DVC

```bash
dvc add data/
```

👉 This creates:

* `data.dvc`
* Updates `.gitignore`

---

## ⚠️ If error occurs:

```bash
git rm -r --cached data
git commit -m "stop tracking data with git"
```

Then again:

```bash
dvc add data/
```

---

## 💾 Step 7: Commit DVC Tracking

```bash
git add .
git commit -m "track data with DVC"
```

---

## 🚀 Step 8: Push Data to Storage

```bash
dvc push
```

👉 Data stored in `s3/` (or actual cloud in real use)

---

# 🔁 Step 9: Modify Data (New Version)

Update code:

```python
df.loc[len(df)] = ["David", 28, "D"]
```

Run again:

```bash
python mycode.py
```

---

## 🔍 Check Changes

```bash
dvc status
```

---

## 💾 Save New Version

```bash
dvc commit
dvc push
git add .
git commit -m "second version of data"
git push
```

---

# 🔄 Step 10: Rollback (Most Important 🔥)

## 🧭 Go to older version

```bash
git log --oneline
git checkout <commit_id>
```

---

## 🔄 Restore data

```bash
dvc checkout
```

👉 You now get **exact old dataset**

---

## 🔁 Return to latest version

```bash
git checkout main
dvc pull
```

---

# 🧠 Key Takeaways

### 🔑 Concepts

* Git → tracks code
* DVC → tracks data
* `.dvc` file → pointer to data
* `dvc push` → upload data
* `dvc pull` → download data
* `dvc checkout` → restore version

---

### ⚡ Important Commands Summary

| Command         | Purpose          |
| --------------- | ---------------- |
| `dvc init`      | Initialize DVC   |
| `dvc add data/` | Track data       |
| `dvc commit`    | Save new version |
| `dvc push`      | Upload data      |
| `dvc pull`      | Download data    |
| `dvc checkout`  | Restore version  |
| `dvc status`    | Check changes    |

---

# 🚀 Final Insight (Very Important)

> In ML, **code alone is useless without data versioning**.

DVC ensures:

* Reproducibility ✅
* Experiment tracking ✅
* Collaboration ✅
* Rollbacks ✅

---

# 🔮 What Next?

After mastering DVC:

* Build **ML pipelines**
* Integrate with:

  * MLflow (experiment tracking)
  * AWS S3 (real storage)
  * CI/CD pipelines

---
