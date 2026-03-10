# 🛠 Infrastructure

This section documents the KalyNow platform infrastructure, including deployment, networking, service orchestration, and operations.

## Contents

| Document | Description |
|---|---|
| [Overview](./overview.md) | Infrastructure overview and key components |
| [Nomad](./nomad.md) | Service orchestration with HashiCorp Nomad |
| [Traefik](./traefik.md) | API Gateway and reverse proxy configuration |
| [Databases](./databases.md) | Database setup and configuration |
| [Messaging](./messaging.md) | Kafka event streaming setup |
| [Monitoring](./monitoring.md) | Observability, logging, and alerting |
| [CI/CD](./cicd.md) | GitHub Actions pipelines and deployment workflows |

## Stack Summary

| Component | Technology | Purpose |
|---|---|---|
| Orchestration | HashiCorp Nomad | Service scheduling and deployment |
| Gateway | Traefik | API routing, TLS, load balancing |
| Relational DB | PostgreSQL | User service data |
| Document DB | MongoDB | Offer service data |
| Analytics DB | ClickHouse | Event storage and querying |
| Cache | Redis | Session and result caching |
| Message Broker | Apache Kafka | Event streaming between services |
| Object Storage | MinIO | File and image storage |
| CI/CD | GitHub Actions | Automated build, test, and deploy |
