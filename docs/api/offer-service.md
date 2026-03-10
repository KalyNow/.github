# Offer Service API

Base URL: `/v1`

Authentication: JWT Bearer token (required for write operations)

---

## Endpoints

### Restaurants

#### `POST /v1/restaurants`

Register a new restaurant.

**Headers:** `Authorization: Bearer <token>`

**Request Body**

```json
{
  "name": "Green Garden",
  "address": "123 Main St",
  "location": {
    "lat": 48.8566,
    "lng": 2.3522
  },
  "category": "vegetarian"
}
```

**Response `201 Created`**

```json
{
  "id": "uuid",
  "name": "Green Garden",
  "address": "123 Main St",
  "location": { "lat": 48.8566, "lng": 2.3522 },
  "category": "vegetarian",
  "createdAt": "2024-01-01T12:00:00Z"
}
```

---

#### `GET /v1/restaurants/:id`

Get restaurant details.

**Response `200 OK`**

```json
{
  "id": "uuid",
  "name": "Green Garden",
  "address": "123 Main St",
  "location": { "lat": 48.8566, "lng": 2.3522 },
  "category": "vegetarian"
}
```

---

### Offers

#### `POST /v1/restaurants/:restaurantId/offers`

Create a new offer for a restaurant.

**Headers:** `Authorization: Bearer <token>`

**Request Body**

```json
{
  "title": "End-of-day pasta deal",
  "description": "Mixed pasta dish, freshly made.",
  "originalPrice": 12.00,
  "discountedPrice": 4.50,
  "quantity": 10,
  "expiresAt": "2024-01-01T22:00:00Z"
}
```

**Response `201 Created`**

```json
{
  "id": "uuid",
  "title": "End-of-day pasta deal",
  "discountedPrice": 4.50,
  "quantity": 10,
  "expiresAt": "2024-01-01T22:00:00Z",
  "status": "active"
}
```

---

#### `GET /v1/offers`

Discover offers near a location.

**Query Parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `lat` | `number` | ✅ | Latitude |
| `lng` | `number` | ✅ | Longitude |
| `radius` | `number` | ❌ | Search radius in km (default: 5) |
| `page` | `number` | ❌ | Page number (default: 1) |
| `limit` | `number` | ❌ | Results per page (default: 20) |

**Response `200 OK`**

```json
{
  "data": [
    {
      "id": "uuid",
      "title": "End-of-day pasta deal",
      "discountedPrice": 4.50,
      "restaurant": { "id": "uuid", "name": "Green Garden" },
      "distance": 1.2,
      "expiresAt": "2024-01-01T22:00:00Z"
    }
  ],
  "total": 1,
  "page": 1,
  "limit": 20
}
```

---

#### `POST /v1/offers/:offerId/claim`

Claim an offer.

**Headers:** `Authorization: Bearer <token>`

**Response `201 Created`**

```json
{
  "reservationId": "uuid",
  "offerId": "uuid",
  "userId": "uuid",
  "claimedAt": "2024-01-01T18:30:00Z"
}
```

---

## Error Responses

| Status | Code | Description |
|---|---|---|
| `400` | `VALIDATION_ERROR` | Invalid request body |
| `401` | `UNAUTHORIZED` | Missing or invalid token |
| `404` | `OFFER_NOT_FOUND` | Offer does not exist |
| `409` | `OFFER_ALREADY_CLAIMED` | Offer already claimed by user |
| `410` | `OFFER_EXPIRED` | Offer has expired |
| `500` | `INTERNAL_ERROR` | Unexpected server error |
