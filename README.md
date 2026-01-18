# 🎧 Spotify Modern Data Stack Project

![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?logo=snowflake&logoColor=white)
![DBT](https://img.shields.io/badge/dbt-FF694B?logo=dbt&logoColor=white)
![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?logo=apacheairflow&logoColor=white)
![Apache Kafka](https://img.shields.io/badge/Apache%20Kafka-231F20?logo=apachekafka&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Modern Data Stack](https://img.shields.io/badge/Modern%20Data%20Stack-00C7B7?logo=databricks&logoColor=white)

---



# 🎧 Youtube Music Modern Data Stack Project

##  Project Overview

This project demonstrates an **end-to-end real-time data engineering pipeline** for Youtube music analytics. By simulating high-velocity streaming data (song plays, listeners, and device metadata), this project showcases a professional-grade **Modern Data Stack (MDS)** implementation.

**The Workflow:** Data is generated in real-time → Streamed via **Kafka** → Buffered in **MinIO** → Orchestrated by **Airflow** → Loaded into **Snowflake** → Transformed via **dbt** ready to use or analyze

---

## Architecture

*(Replace the link below with the image I generated for you or your existing GitHub asset link)*

### The Pipeline Flow:

1. **Data Generation:** Python/Faker simulates a live stream of Spotify events.
2. **Ingestion:** Kafka Producer sends events to topics; Consumer writes raw JSON to MinIO (S3-compatible).
3. **Orchestration:** Airflow manages the "Extract & Load" (MinIO to Snowflake) and triggers the "Transform" (dbt).
4. **Warehouse:** Snowflake hosts the **Medallion Architecture**:
* **Bronze:** Raw JSON landing zone.
* **Silver:** Cleaned, deduplicated, and casted data.
* **Gold:** Aggregated business-level marts (Top Artists, Regional Trends).

---

## Technical Challenges & Lessons Learned

*As we discussed, this project wasn't without its hurdles. Here is how I overcame them:*

* **Handling Semi-Structured Data:** The raw Spotify JSON was deeply nested. I utilized Snowflake’s `FLATTEN` functions and dbt's modularity to transform complex variants into clean relational tables.
* **Schema Evolution:** Dealing with streaming data meant ensuring the Silver layer could handle potential schema changes without breaking the downstream Power BI dashboards.
* **Orchestration Logic:** I implemented **Airflow Sensors** to ensure that dbt transformations only run once the data has successfully landed in the Bronze layer, preventing "empty" runs.
* **Container Networking:** Managing the communication between Kafka, Zookeeper, and Airflow inside Docker required a custom bridge network and careful health-check configurations.

* **Networking in Docker for Mac:** Using "localhost" instead of host.docker.internal was essential because the Python script was trying to talk to a Kafka broker, but it can’t find any running or reachable address but in the code provided i have changed it to internal for common use.

---

## Repository Structure

```text
spotify-mds-pipeline/
├── docker/                 # Infrastructure configuration
│   ├── docker-compose.yml  # Services: Kafka, Airflow, MinIO
│   └── dags/               # Airflow DAGs for E, L, and T
├── spotify_dbt/            # dbt Project
│   └── models/             # Medallion layers (Bronze, Silver, Gold)
├── simulator/              # Python Kafka Producer (Faker)
├── consumer/               # Kafka to MinIO ingestion logic
└── README.md

```
---

## 📣 Author

**Sai Kishan Kumar** [LinkedIn](https://www.linkedin.com/in/sai-kishan-385713173/) | saik77556@gmail.com