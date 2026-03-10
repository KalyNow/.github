# Architecture Overview

## Introduction

KalyNow is built on a **microservices architecture** where each bounded domain is handled by an independently deployable service. Services communicate asynchronously via **Apache Kafka** for event-driven workflows and synchronously via HTTP for real-time queries.

---

## Key Principles

| Principle | Description |
|---|---|
| **Microservices** | Each service owns its domain, data, and deployment lifecycle |
| **Event-Driven** | Services publish and consume domain events via Kafka |
| **Clean Architecture** | Business logic is decoupled from frameworks and infrastructure |
| **SOLID** | Design principles applied at service and module level |
| **Cloud-Native** | Services are containerised and orchestrated with Nomad |

---

## System Overview

```
          Web Frontend (React + TypeScript)
                     │
          Mobile App (Flutter)
                     │
               API Gateway
                (Traefik)
                     │
       ┌─────────────┼─────────────┐
       │             │             │
 User Service   Offer Service   Analytics
  (NestJS)        (NestJS)      (FastAPI)
       │               │             │
  PostgreSQL        MongoDB      ClickHouse
                       │
                     Kafka
                  ┌────┴────┐
                Redis      MinIO
```

---

## Services

### User Service (`kalynow-user-service`)

- **Technology:** NestJS, TypeORM, PostgreSQL
- **Responsibilities:** User registration, authentication, profile management, device registration, subscription management
- **Events Published:** `user.created`, `user.updated`, `user.deleted`

### Offer Service (`kalynow-offer-service`)

- **Technology:** NestJS, Mongoose, MongoDB
- **Responsibilities:** Restaurant management, offer creation, real-time offer discovery, reservation handling
- **Events Published:** `offer.created`, `offer.updated`, `offer.claimed`, `offer.expired`
- **Events Consumed:** `user.created`

### Analytics Service (`kalynow-analytics`)

- **Technology:** FastAPI, ClickHouse, Kafka Consumer
- **Responsibilities:** Consuming domain events, storing time-series data, providing analytics dashboards and metrics endpoints
- **Events Consumed:** All platform events

---

## Communication Patterns

### Synchronous (REST)

Used for real-time client requests routed through Traefik:

```
Client → Traefik (Gateway) → Service → Response
```

### Asynchronous (Kafka)

Used for inter-service events and background processing:

```
Producer Service → Kafka Topic → Consumer Service
```

---

## Data Ownership

Each service owns its own database. No direct cross-database queries are allowed.

| Service | Database | Rationale |
|---|---|---|
| User Service | PostgreSQL | Relational user data with strong consistency |
| Offer Service | MongoDB | Flexible document model for offer/location data |
| Analytics | ClickHouse | Column-oriented store optimised for analytics queries |

---

## Security

- All external traffic is routed through Traefik with TLS termination
- Services authenticate requests using JWT tokens issued by the User Service
- Internal service-to-service communication uses private network routes
- Secrets are managed via environment variables (Nomad secrets in production)
