# Infrastructure Overview

KalyNow uses a cloud-native infrastructure stack based on **HashiCorp Nomad** for service orchestration and **Traefik** as the API gateway and reverse proxy.

---

## Components

| Component | Technology | Purpose |
|---|---|---|
| Service Orchestration | HashiCorp Nomad | Deploy and manage containerised services |
| API Gateway | Traefik | Routing, TLS, load balancing |
| Relational Database | PostgreSQL | User service data |
| Document Database | MongoDB | Offer service data |
| Analytics Database | ClickHouse | Event data and analytics |
| Cache | Redis | Offer query caching |
| Message Broker | Apache Kafka | Event streaming |
| Object Storage | MinIO | Images and file assets |
| CI/CD | GitHub Actions | Build, test, deploy pipelines |

---

## Deployment Topology

All services run as Docker containers scheduled by Nomad.

```
┌──────────────────────────────────────────────────────────┐
│                     Nomad Cluster                         │
│                                                          │
│  ┌──────────────┐   ┌──────────────┐                    │
│  │    Traefik   │   │    Kafka     │                    │
│  │  (Gateway)   │   │  (Broker)    │                    │
│  └──────────────┘   └──────────────┘                    │
│                                                          │
│  ┌──────────────┐   ┌──────────────┐  ┌─────────────┐  │
│  │ User Service │   │Offer Service │  │  Analytics  │  │
│  └──────────────┘   └──────────────┘  └─────────────┘  │
│                                                          │
│  ┌──────────────┐   ┌──────────────┐  ┌─────────────┐  │
│  │  PostgreSQL  │   │   MongoDB    │  │ ClickHouse  │  │
│  └──────────────┘   └──────────────┘  └─────────────┘  │
│                                                          │
│  ┌──────────────┐   ┌──────────────┐                    │
│  │    Redis     │   │    MinIO     │                    │
│  └──────────────┘   └──────────────┘                    │
└──────────────────────────────────────────────────────────┘
```

---

## Networking

- All external traffic enters via **Traefik** on ports `80` (HTTP) and `443` (HTTPS)
- Traefik handles TLS termination and routes to internal services
- Internal services communicate on a private Nomad network
- Kafka, databases, and Redis are **not** exposed externally

---

## Secrets Management

- Secrets are provided to services via environment variables
- In production, Nomad's secret injection or a Vault integration is recommended
- Never hardcode secrets in job files or source code

---

## CI/CD

GitHub Actions workflows handle:

1. **Build** – Docker image build on every push
2. **Test** – Unit and integration tests
3. **Push** – Publish image to container registry
4. **Deploy** – Trigger Nomad job update via API

See [CI/CD Documentation](./cicd.md) for pipeline details.
