# Inter-Service Communication

KalyNow services communicate using two patterns depending on the use case.

---

## 1. Synchronous Communication (REST / HTTP)

Used for real-time client-facing requests and internal queries where an immediate response is required.

All external requests pass through the **Traefik API Gateway**.

```
Client ──▶ Traefik ──▶ Target Service ──▶ Response
```

Internal service-to-service HTTP calls (if any) use private network routes, bypassing the gateway.

---

## 2. Asynchronous Communication (Apache Kafka)

Used for domain events and background workflows where decoupling and resilience are priorities.

```
Producer Service
      │
      └──▶ Kafka Topic ──▶ Consumer Service(s)
```

### Topics

| Topic | Producers | Consumers |
|---|---|---|
| `user-events` | User Service | Analytics Service |
| `offer-events` | Offer Service | Analytics Service |

### Event Schema

All events follow a standard envelope:

```json
{
  "eventType": "offer.created",
  "version": "1.0",
  "timestamp": "2024-01-01T12:00:00Z",
  "serviceSource": "offer-service",
  "payload": { }
}
```

Shared event schemas are maintained in the `kalynow-contracts` repository.

---

## 3. Caching (Redis)

The **Offer Service** uses Redis to cache frequently accessed offer listings, reducing database load for geo-queries.

Cache invalidation occurs on `offer.updated` and `offer.expired` events.

---

## Communication Rules

- Services **must not** directly query another service's database
- All inter-service contracts are defined in `kalynow-contracts`
- HTTP APIs are versioned (`/v1/`)
- Kafka topics use a `{domain}-events` naming convention
