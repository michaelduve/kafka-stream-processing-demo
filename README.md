# Kafka Stream Processing Demo

A production-style, event-driven data engineering project that ingests retail orders, validates and enriches events, calculates real-time revenue aggregates, and routes invalid records to a dead-letter topic.

## Architecture

`Order producer -> orders.raw -> stream processor -> orders.validated / orders.dlq / sales.metrics -> consumers`

The local platform uses a three-broker Kafka-compatible Redpanda cluster, PostgreSQL, Prometheus, Grafana, and Redpanda Console.

## Engineering features

- Idempotent, keyed event production
- Consumer groups, manual offset commits, and dead-letter handling
- Event-time windows and branch/product revenue metrics
- PostgreSQL analytics serving schema
- Prometheus application and broker metrics
- Docker health checks, tests, documentation, and CI

## Quick start

Requirements: Docker Desktop with Compose and Python 3.11+.

```bash
cp .env.example .env
docker compose up -d
python -m pip install -r requirements.txt
python scripts/create_topics.py
python -m src.processor
```

In another terminal, run `python -m src.producer`.

Open Redpanda Console at `http://localhost:8080`, Grafana at `http://localhost:3000`, and Prometheus at `http://localhost:9090`.

## Test

```bash
python -m unittest discover -s tests -v
```

## Production considerations

Enable TLS/SASL, use managed secrets, enforce schema compatibility, deploy processors on Kubernetes, configure cross-zone replication, archive raw events, and define SLO-based alerts.

MIT licensed.

