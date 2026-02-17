# 📘 Lecture 3: MLOps Foundations – Data Management & ETL Systems

---

## 1️⃣ Introduction

In previous lectures, we understood:

* Why MLOps is necessary
* How end-to-end MLOps works in a real project

Today we move to the **first foundational pillar of MLOps**:

# 🧠 Data Management

Remember:

> Machine Learning systems are **data-driven systems**.
> Without reliable data management, MLOps collapses.

---

# 2️⃣ What is MLOps? (Quick Revision)

MLOps is:

> A set of practices and tools that help productionize machine learning systems.

Unlike traditional software systems, ML systems involve:

* Code
* Data
* Models

This makes them significantly more complex.

---

## Chef Analogy

Being a great chef (building a good ML model)
≠
Running a multimillion-dollar restaurant chain (deploying & managing ML in production)

MLOps bridges that gap.

---

# 3️⃣ The 8 Core Aspects of MLOps

While there are multiple components, the playlist focuses on 8 key principles.

The **first and most foundational aspect**:

# 📦 Data Management

---

# 4️⃣ Why Data Management is Critical

Machine learning systems depend entirely on data.

Poor data management leads to:

* Incorrect predictions
* Data leakage
* Legal violations
* Model instability
* Reproducibility failures

---

# 5️⃣ Components of Data Management

Data management includes multiple sub-aspects:

* Data versioning
* Feature stores
* Data pipelines
* Data annotation
* Data validation
* Data governance
* Security

⚠ Important:
Not every company needs all components. Implementation depends on:

* Use case
* Scale
* Industry
* Regulatory requirements

---

# 6️⃣ Real-World Example: Data Annotation

Consider self-driving cars.

Raw camera feeds must be labeled with:

* Vehicles
* Pedestrians
* Traffic signs
* Road boundaries

Without annotation, models cannot learn.

This is critical in industries like autonomous driving, medical imaging, and NLP.

However, not every company needs heavy annotation workflows.

---

# 7️⃣ Data Governance Importance

Data governance ensures:

* Legal compliance
* Privacy protection
* Access control
* Audit trails

Companies ignoring governance face lawsuits and penalties.

For example, concerns were raised around data usage practices of
ChatGPT during early adoption phases.

This highlights the need for strict governance policies.

---

# 8️⃣ The First Three Core Data Management Stages

We now focus on:

1. Data Ingestion
2. Data Transformation
3. Data Storage

These form the backbone of ML systems.

---

# 9️⃣ 1. Data Ingestion (Extract Phase)

### Definition:

Collecting data from multiple sources into a centralized system.

---

## Example Scenario

Suppose a retail company receives data from:

* Official website
* Amazon marketplace
* Offline retail stores

All these sources must be ingested.

---

## Tools for Data Ingestion

| Source Type       | Tools                     |
| ----------------- | ------------------------- |
| Databases         | SQL queries               |
| Real-time streams | Apache Kafka              |
| Cloud streaming   | Amazon Kinesis            |
| APIs              | Python `requests` library |

---

## Conceptual Architecture

![Image](https://www.databricks.com/sites/default/files/2025-06/data-ingestion-reference-architecture-2x.png?v=1748941441)

![Image](https://daxg39y63pxwu.cloudfront.net/images/blog/apache-kafka-architecture-/image_589142173211625734253276.png)

![Image](https://docs.aws.amazon.com/images/streams/latest/dev/images/architecture.png)

![Image](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2023/11/20/bdb3757-003.png)

---

# 🔟 2. Data Transformation (Transform Phase)

Raw data is rarely usable.

It requires:

* Cleaning
* Joining
* Normalizing
* Aggregating

---

## Common Transformations

* Denormalization (joining multiple tables)
* Concatenation of data sources
* Currency conversion (INR → USD)
* Aggregation (GROUP BY in SQL)
* Feature engineering

---

## Tools for Transformation

| Tool         | Use Case                     |
| ------------ | ---------------------------- |
| Pandas       | Small-scale processing       |
| Apache Spark | Large-scale distributed data |
| PySpark      | Python interface for Spark   |

---

## Architecture Illustration

![Image](https://www.altexsoft.com/media/2020/03/etl_pipeline.png)

![Image](https://spark.apache.org/docs/latest/img/cluster-overview.png)

![Image](https://www.informatica.com/content/dam/informatica-com/en/images/misc/etl-process-explained-diagram.png)

![Image](https://www.getdbt.com/_next/image?q=75\&url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2Fwl0ndo6t%2Fmain%2F8ebc1795380b891973bec53f9410998e4e21e006-1216x698.webp%3Ffit%3Dmax%26auto%3Dformat\&w=3840)

---

# 1️⃣1️⃣ 3. Data Storage (Load Phase)

After transformation, data must be stored for:

* Data scientists
* Analysts
* ML pipelines
* Reporting teams

---

## Storage Options

### 1️⃣ Relational Databases

* MySQL
* Oracle
* MS Access

### 2️⃣ NoSQL Databases

* MongoDB

### 3️⃣ Data Warehouses (Structured Analytics)

* Snowflake
* Amazon Redshift
* Google BigQuery

### 4️⃣ Data Lakes (Unstructured Data)

* Amazon S3

---

## Storage Architecture

![Image](https://scaler.com/topics/images/architecture-of-data-warehoue-image-data-warehouse.webp)

![Image](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2021/07/22/Figure-2.-High-level-design-for-an-AWS-lake-house-implementation.png)

![Image](https://docs.snowflake.com/en/_images/architecture-overview.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2ARqEeNuMrfduB5C4CYzLIAQ.png)

---

# 1️⃣2️⃣ ETL Pipeline – The Backbone

The combination of:

* Extract → Data Ingestion
* Transform → Data Cleaning & Processing
* Load → Data Storage

Forms the:

# 🔄 ETL Pipeline

---

## Responsibility

ETL pipelines are primarily designed and maintained by:

> Data Engineers

They ensure:

* Data reliability
* Data consistency
* Data availability
* Reproducibility

---

# 1️⃣3️⃣ Why ETL is Critical in MLOps

Without ETL:

* Training data becomes inconsistent
* Features change unexpectedly
* Reproducibility breaks
* Models degrade silently

ETL ensures stable inputs to ML pipelines.

---

# 1️⃣4️⃣ Tool Selection Depends on Company Maturity

Small startup:

* CSV files
* Pandas
* Simple SQL

Large enterprise:

* Kafka streams
* Spark clusters
* Snowflake warehouses
* Real-time ingestion systems

MLOps is not about using all tools.

It is about using appropriate tools.

---

# 1️⃣5️⃣ Key Engineering Insights

1️⃣ ML systems fail without strong data foundations
2️⃣ ETL pipelines enable reproducibility
3️⃣ Governance prevents legal risks
4️⃣ Tool complexity reduces with familiarity
5️⃣ Data management is the first pillar of MLOps maturity

---

# 📌 What You Should Understand After This Lecture

* Why data management is foundational
* The role of ETL in ML systems
* Differences between ingestion, transformation, and storage
* Why governance and annotation matter
* How tool selection depends on scale

---

### Final Takeaway

> MLOps begins with data discipline.
> Without structured data management, production ML systems cannot survive.

---

That concludes Lecture 3: Data Management & ETL Foundations.
