# Data Architecture

Each KalyNow service owns its own data store. Direct cross-service database access is not permitted.

---

## Data Ownership

| Service | Database | Type | Rationale |
|---|---|---|---|
| User Service | PostgreSQL | Relational | User data requires strong consistency and ACID transactions |
| Offer Service | MongoDB | Document | Flexible schema for geo-data, images, and varying offer structures |
| Analytics Service | ClickHouse | Column-store | Optimised for high-throughput inserts and analytical queries |

---

## Supporting Storage

| Service | Storage | Purpose |
|---|---|---|
| Offer Service | Redis | Cache geo-based offer queries |
| Offer Service | MinIO | Store restaurant and offer images |

---

## Data Flow Summary

```
User registers
  └──▶ UserService writes to PostgreSQL
  └──▶ Publishes user.created to Kafka
         └──▶ Analytics consumes and writes to ClickHouse

User claims offer
  └──▶ OfferService updates MongoDB
  └──▶ Publishes offer.claimed to Kafka
         └──▶ Analytics consumes and writes to ClickHouse
```

---

## Migrations and Schema Management

| Service | Tool | Notes |
|---|---|---|
| User Service | TypeORM Migrations | Run on deployment |
| Offer Service | Mongoose Schemas | Schema-less; validation via DTOs |
| Analytics | ClickHouse DDL | Tables created via migration scripts in `kalynow-infra` |
