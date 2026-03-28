## 🏗️ System Architecture

The pipeline follows a modern **Medallion-style Streaming Architecture**:

1.  **Data Ingestion (Producer):** A Python-based producer generates synthetic taxi ride events, simulating real-world scenarios including **Out-of-Order (Late)** events.
2.  **Message Broker:** **Redpanda** acts as the streaming backbone, providing high-throughput and low-latency message queuing.
3.  **Stream Processing (Flink):** An Apache Flink job (PyFlink) consumes the raw stream, handles event-time processing, manages watermarks, and performs windowed aggregations.
4.  **Data Sink:** Processed and aggregated data is stored in **PostgreSQL** for persistent storage and reporting.



---

## 🛠️ Tech Stack

* **Processing Engine:** Apache Flink (PyFlink Table API)
* **Message Broker:** Redpanda (Kafka API compatible)
* **Storage:** PostgreSQL
* **Language:** Python 3.12
* **Containerization:** Docker & Docker Compose
* **Environment:** GitHub Codespaces / Azure

---

## 🚀 Key Engineering Features

* **Event-Time Processing:** Accurate calculations based on when the ride happened, not when it was received.
* **Watermark Management:** Configured a 5-second strategy to handle late-arriving data without losing accuracy.
* **Tumbling Windows:** Aggregates total revenue and trip counts in fixed-size time intervals.
* **Medallion Architecture:** Demonstrates the flow from "Bronze" (Raw Kafka Events) to "Silver/Gold" (Aggregated Postgres Tables).

---

## 🚦 Getting Started

### 1. Start Infrastructure
Spin up the Docker containers (Redpanda, Flink, Postgres):
```bash
docker compose up -d

2. Submit the Flink Aggregation Job

Submit the Python script to the Flink JobManager:
Bash

docker compose exec jobmanager ./bin/flink run \
    -py /opt/src/job/aggregation_job.py \
    --pyFiles /opt/src -d

3. Start the Real-time Producer

Run the producer to start streaming events:
Bash

uv run python src/producers/producer_realtime.py

📊 Monitoring & Verification

    Flink Dashboard: Access at http://localhost:8081 to monitor job status and throughput.

    Postgres Query: Check the final results in the processed_events_aggregated table:
    SQL

    SELECT * FROM processed_events_aggregated ORDER BY window_start DESC;

📂 Project Structure
Plaintext

.
├── src/
│   ├── job/                # Flink Job scripts (Aggregation, Pass-through)
│   ├── producers/          # Python Kafka producers
│   └── models.py           # Data classes and schemas
├── docker-compose.yml      # Infrastructure orchestration
├── Dockerfile.flink        # Custom Flink image with dependencies
└── README.md

Developed by Mohamed Hesham Elnady 🚀

Data Engineering Zoomcamp 2026
