# 📡 API Documentation

This section documents the public and internal APIs exposed by each KalyNow microservice.

## Contents

| Document | Service | Description |
|---|---|---|
| [User Service API](./user-service.md) | `kalynow-user-service` | Users, auth, subscriptions, devices |
| [Offer Service API](./offer-service.md) | `kalynow-offer-service` | Restaurants, offers, reservations |
| [Analytics API](./analytics-service.md) | `kalynow-analytics` | Metrics, events, dashboards |

## API Conventions

- All REST APIs use JSON payloads
- Authentication via **JWT Bearer tokens**
- API versioning via URL prefix: `/v1/`
- Standard HTTP status codes
- Pagination via `page` and `limit` query parameters
- ISO 8601 date/time format

## Gateway

All external requests pass through the **Traefik API Gateway**, which handles:

- Routing to backend services
- TLS termination
- Rate limiting
- Request logging

Internal service-to-service communication uses private network routes without going through the gateway.
