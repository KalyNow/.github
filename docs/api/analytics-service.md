# Analytics Service API

Base URL: `/v1/analytics`

Authentication: JWT Bearer token (internal service use)

The Analytics Service exposes read-only endpoints for querying aggregated event data stored in ClickHouse.

---

## Endpoints

### Offers Analytics

#### `GET /v1/analytics/offers/summary`

Get a summary of offer activity over a time range.

**Query Parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `from` | `string` (ISO 8601) | ✅ | Start of period |
| `to` | `string` (ISO 8601) | ✅ | End of period |
| `restaurantId` | `string` | ❌ | Filter by restaurant |

**Response `200 OK`**

```json
{
  "period": {
    "from": "2024-01-01T00:00:00Z",
    "to": "2024-01-31T23:59:59Z"
  },
  "totalOffersCreated": 120,
  "totalOffersClaimed": 95,
  "totalOffersExpired": 18,
  "claimRate": 0.79
}
```

---

#### `GET /v1/analytics/offers/timeseries`

Get time-series data for offer events.

**Query Parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `from` | `string` (ISO 8601) | ✅ | Start of period |
| `to` | `string` (ISO 8601) | ✅ | End of period |
| `interval` | `string` | ❌ | Bucket size: `hour`, `day`, `week` (default: `day`) |

**Response `200 OK`**

```json
{
  "interval": "day",
  "data": [
    {
      "timestamp": "2024-01-01T00:00:00Z",
      "created": 10,
      "claimed": 8,
      "expired": 1
    }
  ]
}
```

---

### User Analytics

#### `GET /v1/analytics/users/summary`

Get user registration and activity summary.

**Query Parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `from` | `string` (ISO 8601) | ✅ | Start of period |
| `to` | `string` (ISO 8601) | ✅ | End of period |

**Response `200 OK`**

```json
{
  "period": {
    "from": "2024-01-01T00:00:00Z",
    "to": "2024-01-31T23:59:59Z"
  },
  "totalNewUsers": 340,
  "totalActiveUsers": 210
}
```

---

### Health

#### `GET /health`

Service health check.

**Response `200 OK`**

```json
{
  "status": "ok",
  "clickhouse": "connected",
  "kafka": "connected"
}
```

---

## Error Responses

| Status | Code | Description |
|---|---|---|
| `400` | `VALIDATION_ERROR` | Invalid query parameters |
| `401` | `UNAUTHORIZED` | Missing or invalid token |
| `500` | `INTERNAL_ERROR` | Unexpected server error |
