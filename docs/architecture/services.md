# Services

Detailed design documentation for each KalyNow microservice.

---

## User Service (`kalynow-user-service`)

| Attribute | Value |
|---|---|
| **Technology** | NestJS, TypeORM |
| **Database** | PostgreSQL |
| **Port (default)** | `3001` |
| **Repository** | `KalyNow/kalynow-user-service` |

### Responsibilities

- User registration and authentication (JWT)
- User profile management
- Device token registration (push notifications)
- Subscription management

### Domain Events

| Event | Topic | Trigger |
|---|---|---|
| `user.created` | `user-events` | New user registers |
| `user.updated` | `user-events` | Profile updated |
| `user.deleted` | `user-events` | Account deleted |

---

## Offer Service (`kalynow-offer-service`)

| Attribute | Value |
|---|---|
| **Technology** | NestJS, Mongoose |
| **Database** | MongoDB |
| **Cache** | Redis |
| **Storage** | MinIO |
| **Port (default)** | `3002` |
| **Repository** | `KalyNow/kalynow-offer-service` |

### Responsibilities

- Restaurant registration and management
- Offer creation, scheduling, and expiry
- Geo-based offer discovery
- Reservation and claim handling
- Image storage via MinIO

### Domain Events

| Event | Topic | Trigger |
|---|---|---|
| `offer.created` | `offer-events` | New offer published |
| `offer.updated` | `offer-events` | Offer updated |
| `offer.claimed` | `offer-events` | User claims an offer |
| `offer.expired` | `offer-events` | Offer passes expiry time |

### Events Consumed

| Event | Topic | Action |
|---|---|---|
| `user.created` | `user-events` | Initialise user preferences |

---

## Analytics Service (`kalynow-analytics`)

| Attribute | Value |
|---|---|
| **Technology** | FastAPI, Kafka Consumer |
| **Database** | ClickHouse |
| **Port (default)** | `8000` |
| **Repository** | `KalyNow/kalynow-analytics` |

### Responsibilities

- Consuming all platform domain events from Kafka
- Storing event data in ClickHouse for time-series analysis
- Exposing analytics endpoints for dashboards and reporting

### Events Consumed

| Topic | Events |
|---|---|
| `user-events` | `user.created`, `user.updated`, `user.deleted` |
| `offer-events` | `offer.created`, `offer.updated`, `offer.claimed`, `offer.expired` |
