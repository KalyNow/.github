# Environment Variables

This document lists the environment variables required by each KalyNow service.

---

## User Service (`kalynow-user-service`)

| Variable | Required | Example | Description |
|---|---|---|---|
| `PORT` | ❌ | `3001` | HTTP port (default: 3001) |
| `DATABASE_URL` | ✅ | `postgresql://user:pass@localhost:5432/kalynow_users` | PostgreSQL connection string |
| `JWT_SECRET` | ✅ | `supersecretkey` | JWT signing secret |
| `JWT_EXPIRES_IN` | ❌ | `3600` | JWT expiry in seconds (default: 3600) |
| `KAFKA_BROKERS` | ✅ | `localhost:9092` | Kafka broker address(es) |

---

## Offer Service (`kalynow-offer-service`)

| Variable | Required | Example | Description |
|---|---|---|---|
| `PORT` | ❌ | `3002` | HTTP port (default: 3002) |
| `MONGODB_URI` | ✅ | `mongodb://localhost:27017/kalynow_offers` | MongoDB connection string |
| `REDIS_URL` | ✅ | `redis://localhost:6379` | Redis connection URL |
| `MINIO_ENDPOINT` | ✅ | `localhost:9000` | MinIO server endpoint |
| `MINIO_ACCESS_KEY` | ✅ | `minioadmin` | MinIO access key |
| `MINIO_SECRET_KEY` | ✅ | `minioadmin` | MinIO secret key |
| `KAFKA_BROKERS` | ✅ | `localhost:9092` | Kafka broker address(es) |
| `JWT_SECRET` | ✅ | `supersecretkey` | JWT signing secret (for token validation) |

---

## Analytics Service (`kalynow-analytics`)

| Variable | Required | Example | Description |
|---|---|---|---|
| `PORT` | ❌ | `8000` | HTTP port (default: 8000) |
| `CLICKHOUSE_URL` | ✅ | `http://localhost:8123` | ClickHouse HTTP interface URL |
| `CLICKHOUSE_DB` | ✅ | `kalynow_analytics` | ClickHouse database name |
| `CLICKHOUSE_USER` | ✅ | `default` | ClickHouse username |
| `CLICKHOUSE_PASSWORD` | ✅ | `` | ClickHouse password |
| `KAFKA_BROKERS` | ✅ | `localhost:9092` | Kafka broker address(es) |
| `KAFKA_GROUP_ID` | ❌ | `analytics-consumer` | Kafka consumer group ID |

---

## Notes

- Never commit `.env` files to source control
- Use `.env.example` as a template for required variables
- In production, secrets are injected via Nomad or a secrets manager
