# System Diagrams

This page contains placeholder diagrams for the KalyNow platform. Detailed diagrams will be added as the platform evolves.

> 💡 Diagrams can be authored using [Mermaid](https://mermaid.js.org/), [draw.io](https://draw.io), or [PlantUML](https://plantuml.com/). Replace the placeholders below with actual diagrams.

---

## 1. High-Level Component Diagram

> **Placeholder** – Overall platform component layout

```
┌────────────────────────────────────────────────────────────────────────┐
│                          KalyNow Platform                              │
│                                                                        │
│   ┌──────────────┐     ┌──────────────┐                               │
│   │  Web App     │     │  Mobile App  │                               │
│   │ (React / TS) │     │  (Flutter)   │                               │
│   └──────┬───────┘     └──────┬───────┘                               │
│          │                   │                                        │
│          └─────────┬─────────┘                                        │
│                    ▼                                                   │
│          ┌─────────────────┐                                          │
│          │   API Gateway   │  (Traefik)                               │
│          └────────┬────────┘                                          │
│                   │                                                   │
│     ┌─────────────┼──────────────┐                                   │
│     ▼             ▼              ▼                                    │
│ ┌──────────┐ ┌──────────┐ ┌──────────────┐                          │
│ │  User    │ │  Offer   │ │  Analytics   │                          │
│ │ Service  │ │ Service  │ │   Service    │                          │
│ │ (NestJS) │ │ (NestJS) │ │  (FastAPI)   │                          │
│ └────┬─────┘ └────┬─────┘ └──────┬───────┘                          │
│      │             │              │                                   │
│ ┌────▼──────┐ ┌────▼──────┐ ┌────▼──────┐                          │
│ │PostgreSQL │ │  MongoDB  │ │ClickHouse │                           │
│ └───────────┘ └─────┬─────┘ └───────────┘                           │
│                     │                                                 │
│               ┌─────▼─────┐                                          │
│               │   Kafka   │                                          │
│               └─────┬─────┘                                          │
│                     │                                                 │
│            ┌────────┴────────┐                                        │
│            ▼                 ▼                                        │
│         ┌───────┐       ┌────────┐                                   │
│         │ Redis │       │ MinIO  │                                   │
│         └───────┘       └────────┘                                   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Request Flow – Offer Discovery

> **Placeholder** – Sequence diagram for a user discovering nearby offers

```
Client          Traefik         OfferService        MongoDB
  │                │                  │                 │
  │─ GET /offers ─▶│                  │                 │
  │                │─ route request ─▶│                 │
  │                │                  │─ find offers ──▶│
  │                │                  │◀─ offer list ───│
  │                │◀─ 200 response ──│                 │
  │◀─ offer list ──│                  │                 │
```

---

## 3. Event Flow – Offer Claimed

> **Placeholder** – Event-driven flow when a user claims an offer

```
UserApp        OfferService         Kafka            AnalyticsService
   │                │                  │                    │
   │─ POST /claim ─▶│                  │                    │
   │                │─ publish event ─▶│                    │
   │                │  offer.claimed   │                    │
   │◀─ 201 OK ──────│                  │                    │
   │                │                  │─ consume event ───▶│
   │                │                  │   offer.claimed    │
   │                │                  │                    │─ store in ClickHouse
```

---

## 4. Infrastructure Diagram

> **Placeholder** – Deployment topology on Nomad

```
┌──────────────────────────────────────────────────────────┐
│                        Nomad Cluster                      │
│                                                          │
│  ┌───────────┐  ┌───────────┐  ┌───────────────────┐   │
│  │  Traefik  │  │  Kafka    │  │  Databases        │   │
│  │ (Gateway) │  │ (Broker)  │  │  PG / Mongo / CH  │   │
│  └───────────┘  └───────────┘  └───────────────────┘   │
│                                                          │
│  ┌───────────┐  ┌───────────┐  ┌───────────────────┐   │
│  │   User    │  │   Offer   │  │    Analytics      │   │
│  │  Service  │  │  Service  │  │     Service       │   │
│  └───────────┘  └───────────┘  └───────────────────┘   │
│                                                          │
│  ┌───────────┐  ┌───────────┐                           │
│  │   Redis   │  │   MinIO   │                           │
│  └───────────┘  └───────────┘                           │
└──────────────────────────────────────────────────────────┘
```

---

## 5. Data Flow Diagram

> **Placeholder** – Data flow between services and storage systems

```
[ Clients ]
     │
     ▼
[ Traefik Gateway ]
     │
     ├──▶ [ User Service ]  ──▶ PostgreSQL
     │            │
     │            └──▶ Kafka: user.created
     │
     ├──▶ [ Offer Service ] ──▶ MongoDB
     │            │              Redis (cache)
     │            │              MinIO (images)
     │            └──▶ Kafka: offer.created / offer.claimed
     │
     └──▶ [ Analytics ]    ──▶ ClickHouse
                  ▲
                  └── Kafka (consumes all events)
```

---

## Adding New Diagrams

To add a diagram:

1. Create or update a file in this `architecture/` folder
2. Use [Mermaid syntax](https://mermaid.js.org/syntax/flowchart.html) for diagrams rendered in GitHub Markdown, or attach exported image files (`.png`, `.svg`)
3. Reference the diagram file from this page or from `overview.md`
