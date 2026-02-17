# 📘 Lecture 2: End-to-End MLOps Implementation (Real Project Walkthrough)

---

## 1️⃣ Introduction

In Lecture 1, we understood **why MLOps is necessary**.

Today, we move from theory to **practical implementation**.

This lecture covers a **real-world end-to-end MLOps system** built for:

> 🎯 Sentiment Analysis of YouTube Comments using a Chrome Plugin

The goal is to understand how modern ML systems move from experimentation to scalable production.

---

# 2️⃣ Problem Statement

### Business Use Case

Build a Chrome plugin that:

1. Fetches YouTube comments
2. Sends them to backend ML API
3. Classifies sentiment:

   * Positive
   * Neutral
   * Negative
4. Displays:

   * Pie chart distribution
   * Sentiment trends
   * Word clouds

---

# 3️⃣ Overall System Architecture

## High-Level Flow

![Image](https://editor.analyticsvidhya.com/uploads/96596Screenshot%20%2819%29.png)

![Image](https://miro.medium.com/1%2A1bmoE9B6U6qr1LXKVBxGVA.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2A1o9ptYkUif8Ro02CT4986w.jpeg)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2At0MLLv0UhYoETR8Y_ZaSmw.png)

### Components

* Frontend → HTML, CSS, JavaScript (Chrome Plugin)
* Backend → Flask API
* ML Model → Sentiment classifier
* CI/CD Pipeline
* Cloud Deployment Infrastructure

---

# 4️⃣ Model Development Phase

## Dataset

* Reddit dataset from Kaggle
* Labeled: Positive, Neutral, Negative
* Used instead of YouTube comments due to labeling challenges

---

## Data Challenges

### 1️⃣ Data Imbalance

Unequal class distribution.

Solution:

* Used **class weights** in LightGBM

---

## Feature Engineering Techniques Compared

| Method       | Description                    |
| ------------ | ------------------------------ |
| Bag of Words | Frequency-based representation |
| TF-IDF       | Weighted term importance       |
| Word2Vec     | Dense word embeddings          |

### Result:

TF-IDF performed best.

---

## Algorithms Tested

| Algorithm           | Performance                           |
| ------------------- | ------------------------------------- |
| LightGBM            | ~86% accuracy (best stable performer) |
| XGBoost             | ~90% in some experiments              |
| Random Forest       | Lower                                 |
| Logistic Regression | Tested                                |
| KNN                 | Tested                                |
| Naive Bayes         | Tested                                |
| SVM                 | Tested                                |

### Final Selection:

LightGBM with class weights.

---

# 5️⃣ Experiment Tracking

## Tool Used: MLflow

### Why MLflow?

* Track experiments
* Log parameters
* Compare metrics
* Store model artifacts
* Register model versions

---

### MLflow Architecture

![Image](https://mlflow.org/docs/latest/assets/images/tracking-setup-overview-3d8cfd511355d9379328d69573763331.png)

![Image](https://mlflow.org/docs/latest/assets/images/tracking-metrics-ui-temp-ffc0da57b388076730e20207dbd7f9c4.png)

![Image](https://mlflow.org/assets/images/classical_registry_2-9062d01dacf1bbf589d6d13bff9e5c54.png)

![Image](https://daxg39y63pxwu.cloudfront.net/images/blog/model-registry/Model_Registry_for_Machine_Learning_Projects-1.webp)

---

## Model Lifecycle Stages

* Development
* Staging
* Production
* Archived

This enables:

* Rollback
* Controlled promotion
* Governance

---

# 6️⃣ Pipeline Automation

## Tool Used: DVC

### Purpose

Automate:

* Data preprocessing
* Feature engineering
* Model training
* Evaluation

---

### DVC Pipeline Concept

![Image](https://mlops-guide.github.io/assets/dvc/complete_pipeline.png)

![Image](https://miro.medium.com/0%2ACKEc4j27kiRRJFJ-.jpg)

![Image](https://mlops-guide.github.io/assets/dvc/dvc_diagram.png)

![Image](https://croz.net/app/uploads/2023/06/slika1_project-versions.png)

---

### Key Feature

Pipeline accepts hyperparameters as input:

Example:

* Learning rate
* Number of estimators

Changing parameter → Full retraining automatically.

---

# 7️⃣ Model Testing & Promotion

Before deployment:

### Tests Performed

* Model loading test
* Performance threshold test (e.g., accuracy > X%)

If passed:

* Promote from Staging → Production

If failed:

* Reject and retrain

This prevents poor models from going live.

---

# 8️⃣ Containerization

## Tool: Docker

Why Docker?

* Environment consistency
* Dependency isolation
* Portable deployment
* Eliminates “works on my machine” problem

---

# 9️⃣ Cloud Deployment Architecture

### AWS Infrastructure Used:

* Amazon Elastic Container Registry
* Amazon EC2
* Elastic Load Balancing
* Amazon EC2 Auto Scaling
* AWS CodeDeploy

---

## Deployment Architecture

![Image](https://docs.aws.amazon.com/images/autoscaling/ec2/userguide/images/elb-tutorial-architecture-diagram.png)

![Image](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2021/05/13/Figure-2.-Pilot-light-DR-strategy-1024x538.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AY7PkPXSYi6XIy59Cei3WHw.jpeg)

![Image](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2021/05/07/codebuild-pipeline1-1.png)

---

### Flow

1. Build Docker image
2. Push to ECR
3. EC2 pulls image
4. ELB distributes traffic
5. Auto Scaling adjusts instance count

---

# 🔟 Deployment Strategies

## Problem: Single Server Risk

If server crashes → Entire system down.

## Solution: Multi-Instance Architecture

* Multiple EC2 instances
* Load balancer distributes traffic

---

## Rolling Updates

Instead of updating all servers simultaneously:

* Update one instance at a time
* Maintain availability
* Zero downtime deployment

Managed using:

* AWS CodeDeploy

---

# 1️⃣1️⃣ CI/CD Automation

## Tool Used: GitHub Actions

---

## CI/CD Pipeline Flow

![Image](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2022/03/27/1-ArchitectureDiagram.png)

![Image](https://blog.paperspace.com/content/images/2018/07/ml-cd2.png)

![Image](https://valohai.com/assets/img/cicd-for-ml-iot.png)

![Image](https://valohai.com/assets/img/cicd-abstract.png)

---

### Automated Steps

1. Install dependencies
2. Run DVC pipeline
3. Train model
4. Run tests
5. Register model in MLflow
6. Build Docker image
7. Push to ECR
8. Deploy via CodeDeploy
9. Auto Scaling updates instances

Triggered automatically by:

* Code change
* Parameter modification

---

# 1️⃣2️⃣ Future Improvements

Planned additions:

* Monitoring with Prometheus
* Visualization with Grafana
* Automated retraining loop
* Drift detection
* Alternative tools:

  * Airflow instead of DVC
  * Weights & Biases instead of MLflow

---

# 1️⃣3️⃣ End-to-End Workflow Summary

| Stage               | Tool           | Purpose                 |
| ------------------- | -------------- | ----------------------- |
| Data Versioning     | DVC            | Reproducibility         |
| Experiment Tracking | MLflow         | Model management        |
| Containerization    | Docker         | Environment consistency |
| Registry            | AWS ECR        | Image storage           |
| Compute             | EC2            | Hosting                 |
| Load Balancing      | ELB            | Traffic distribution    |
| Scaling             | Auto Scaling   | Dynamic scaling         |
| Deployment          | CodeDeploy     | Rolling updates         |
| CI/CD               | GitHub Actions | Automation              |

---

# 1️⃣4️⃣ Key Engineering Lessons

### 1️⃣ Notebook ≠ Production System

Training accuracy alone is not sufficient.

### 2️⃣ Automation Reduces Errors

Manual deployment leads to inconsistency.

### 3️⃣ Version Everything

* Code
* Data
* Model
* Parameters

### 4️⃣ Scalability Must Be Designed

Cloud-native design is essential.

### 5️⃣ Monitoring Is Mandatory

Without monitoring:

* You cannot detect drift
* You cannot maintain model quality

---

# 1️⃣5️⃣ Core Takeaway

MLOps integrates:

* Data Engineering
* Machine Learning
* DevOps
* Cloud Infrastructure
* Automation
* Governance

It transforms:

> A model in a notebook → Into a production-grade, scalable ML product.

---

# 📌 What You Should Understand After This Lecture

* How an ML model moves from experimentation to deployment
* Why experiment tracking is necessary
* How pipelines automate retraining
* How Docker ensures environment consistency
* Why cloud-native architectures improve reliability
* How CI/CD enables rapid iteration

---

That concludes Lecture 2: Practical MLOps Implementation.
