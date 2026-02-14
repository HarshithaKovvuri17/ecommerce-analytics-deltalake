# E-Commerce Analytics Lakehouse (Delta Lake + Spark + MinIO)

A fully containerized lakehouse data engineering project using **Apache Spark**, **Delta Lake**, and **MinIO**. This pipeline demonstrates batch ingestion, streaming ingestion, Delta table operations, and automated optimization within a modern data lakehouse architecture.

---

## 🚀 Project Overview

This repository implements a complete data lakehouse pipeline:

* **Batch Ingestion**: CSV data ingestion into Delta tables.
* **Delta Lake Storage**: ACID-compliant tables stored on MinIO (S3-compatible).
* **MERGE Operations**: Upserts for incremental updates.
* **Constraint Enforcement**: Data validation at table level.
* **Optimization**: File compaction using Delta OPTIMIZE.
* **Time Travel**: Historical data versioning.
* **Streaming**: Continuous ingestion pipeline.

The system is fully automated and runs end-to-end using Docker Compose.

---

## 🛠️ Tech Stack

* **Processing**: Apache Spark 3.5
* **Storage Format**: Delta Lake 3.1
* **Object Storage**: MinIO (S3-compatible)
* **Containerization**: Docker & Docker Compose

---

## 🏗️ Architecture
```

CSV Data → Spark → Delta Lake → MinIO (S3A)

````
Components:

* `spark-master`
* `spark-worker`
* `spark-app` (pipeline runner)
* `minio` (object storage)

---

## 📂 Project Structure

```text
.
├── docker-compose.yml
├── Dockerfile
├── .env.example
├── requirements.txt
├── data/
│   ├── customers.csv
│   ├── updates.csv
│   ├── products.csv
│   └── sales.csv
└── app/
    ├── main.py
    ├── spark_session.py
    ├── merge.py
    ├── optimize.py
    ├── streaming.py
    ├── batch.py
    └── utils.py
```
---

## 📋 Prerequisites

* Docker installed
* Docker Compose installed
* Minimum 4GB RAM recommended for containers

---

## ⚙️ Setup & Installation

### 1. Clone Repository

```bash
git clone <repo-url>
cd <repo-name>
```

---

### 2. Create Environment File

Copy example file:

```bash
cp .env.example .env
```

Modify values if needed.

---

### 3. Start Pipeline

```bash
docker-compose up --build
```

This will:

* Start MinIO
* Start Spark cluster
* Upload CSV data
* Create Delta tables
* Perform MERGE operations
* Run OPTIMIZE compaction
* Execute streaming ingestion
* Complete pipeline automatically

No manual intervention required.

---

## 🔧 Environment Variables

Defined in `.env.example`:

```env
MINIO_ACCESS_KEY=minioadmin
MINIO_SECRET_KEY=minioadmin

SPARK_MASTER_URL=spark://spark-master:7077

MINIO_ENDPOINT=http://minio:9000

AWS_ACCESS_KEY_ID=minioadmin
AWS_SECRET_ACCESS_KEY=minioadmin
```

---

## 📊 Pipeline Steps

### 1. Data Upload

CSV files uploaded to:

```
s3a://data/raw/
```

---

### 2. Delta Table Creation

Delta tables stored at:

```
s3a://data/warehouse/
```

Tables created:

* `products`
* `customers`

---

### 3. Constraint Enforcement

Products table includes validation:

```
price > 0
```

Verified via Delta transaction logs.

---

### 4. MERGE (Upsert) Operation

Customers table performs:

* UPDATE when matched
* INSERT when not matched

Time travel confirms version history.

---

### 5. OPTIMIZE (Compaction)

Delta OPTIMIZE reduces small files.

Verification:

* `_delta_log` shows removed and added files.

---

### 6. Time Travel

Example:

* Version 0 → original dataset
* Latest version → updated records

---

### 7. Streaming Ingestion

Streaming job executes automatically as part of pipeline workflow.

---

## ✅ Verification

### Check MinIO Structure

```
data/
├── raw/
└── warehouse/
```

---

### Check Delta Logs

```
_delta_log/
```

Contains:

* ADD CONSTRAINT operations
* MERGE history
* OPTIMIZE actions

---

### Successful Completion

Pipeline logs end with:

```
Pipeline finished successfully
```

---

## 📝 Notes

* Delta tables stored on MinIO via S3A protocol.
* Fully automated pipeline execution.
* Designed for reproducible deployment and evaluation.

## 📞Contact