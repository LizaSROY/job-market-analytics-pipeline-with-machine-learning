# Job Market Analytics & Salary Prediction System

> End-to-end data engineering and machine learning pipeline for processing large-scale job market data, generating analytical insights, and predicting salaries.

---

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Dataset Setup](#dataset-setup)
  - [Running the Project](#running-the-project)
- [Applications](#applications)
- [Machine Learning Models](#machine-learning-models)
- [Docker Reference](#docker-reference)

---

## Overview

This project implements a production-grade ETL pipeline capable of processing 500,000+ job market records. It combines distributed data transformation, relational storage, and machine learning to deliver an interactive analytics dashboard and a real-time salary prediction application — all orchestrated through Apache Airflow and containerized with Docker.

**Key capabilities:**

- Scalable ETL pipeline using PySpark for distributed transformation
- Feature engineering and data cleaning on large datasets
- Salary prediction via multiple machine learning models
- Interactive analytics dashboard with Chart.js visualizations
- Fully Dockerized environment for reproducible deployment
- Automated workflow orchestration with Apache Airflow

---

## Architecture

```
Dataset (job_market_dataset.csv)
        │
        ▼
  Extract (Python)          — Raw ingestion from CSV source
        │
        ▼
Transform (PySpark)         — Cleaning, normalization, feature engineering
        │
        ▼
   Load (MySQL)             — Structured storage for analytics and ML
        │
        ▼
Analytics + ML              — Insight generation, model training, prediction
        │
        ▼
Dashboard + Prediction App  — Flask web applications with Chart.js
        │
        ▼
Airflow Orchestration       — Scheduled DAGs managing the full pipeline
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3.x |
| Distributed Processing | Apache Spark (PySpark) |
| Orchestration | Apache Airflow |
| Database | MySQL |
| Web Framework | Flask |
| Machine Learning | Scikit-learn |
| Visualization | Chart.js |
| Containerization | Docker & Docker Compose |

---

## Project Structure

```
job_market/
├── analytics/              # Analytical query modules and aggregations
├── dashboard/              # Flask dashboard application (port 5001)
├── prediction_app/         # Flask salary prediction application (port 5002)
├── extraction/             # Data ingestion scripts
├── transformation/         # PySpark transformation logic
├── loading/                # MySQL loading utilities
├── ml/
│   ├── preprocessing.py    # Feature engineering and data preparation
│   └── train.py            # Model training for all ML algorithms
├── dags/                   # Airflow DAG definitions
├── data/
│   ├── raw/                # Source data (not tracked in version control)
│   └── processed/          # Pipeline output files
├── docker/                 # Docker Compose and service configuration
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Getting Started

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed
- At least 8 GB RAM recommended for PySpark and Airflow services

### Dataset Setup

Dataset files are **not included** in this repository.

1. Obtain the job market dataset(https://www.kaggle.com/datasets/kundanbedmutha/job-market-dataset-global) (CSV format)
2. Place it at the following path before starting services:

```
data/raw/job_market_dataset.csv
```

### Running the Project

**1. Clone the repository**

```bash
git clone https://github.com/LizaSROY/job-market-analytics-pipeline-with-machine-learning.git
cd job-market-analytics-pipeline-with-machine-learning/docker
```

**2. Build and start all services**

```bash
docker compose up -d --build
```

> This starts MySQL, Airflow (webserver + scheduler), the dashboard, and the prediction app. Wait ~60 seconds for all services to initialize before proceeding.

**3. Generate processed data files**

```bash
docker exec -it docker-airflow-webserver-1 \
  python ml/preprocessing.py
```

**4. Train machine learning models**

```bash
docker exec -it docker-airflow-webserver-1 \
  python ml/train.py
```

---

## Applications

| Application | URL | Credentials |
|---|---|---|
| Airflow UI | http://localhost:8080 | `admin` / `admin` |
| Analytics Dashboard | http://localhost:5001 | — |
| Salary Prediction App | http://localhost:5002 | — |

---

## Machine Learning Models

Three regression models are trained and evaluated for salary prediction:

| Model | Description |
|---|---|
| **Linear Regression** | Baseline model; fast and interpretable |
| **Random Forest** | Ensemble of decision trees; handles non-linear relationships and feature interactions |
| **Gradient Boosting** | Sequential boosting approach; typically achieves highest predictive accuracy |

Model artifacts are saved to `data/processed/` after training and loaded at inference time by the prediction application.

---

## Docker Reference

| Command | Description |
|---|---|
| `docker compose up -d --build` | Build images and start all containers in the background |
| `docker compose start` | Start existing (already built) containers |
| `docker compose stop` | Stop running containers without removing them |
| `docker compose down` | Stop and remove containers (data volumes are preserved) |
| `docker compose logs -f` | Stream logs from all services |

---

## License

This project is for educational and portfolio purposes.
