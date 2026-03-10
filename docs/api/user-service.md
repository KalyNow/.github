# User Service API

Base URL: `/v1/users`

Authentication: JWT Bearer token (except registration and login endpoints)

---

## Endpoints

### Authentication

#### `POST /v1/auth/register`

Register a new user.

**Request Body**

```json
{
  "email": "user@example.com",
  "password": "strongpassword",
  "name": "Jane Doe"
}
```

**Response `201 Created`**

```json
{
  "id": "uuid",
  "email": "user@example.com",
  "name": "Jane Doe",
  "createdAt": "2024-01-01T12:00:00Z"
}
```

---

#### `POST /v1/auth/login`

Authenticate and receive a JWT token.

**Request Body**

```json
{
  "email": "user@example.com",
  "password": "strongpassword"
}
```

**Response `200 OK`**

```json
{
  "accessToken": "eyJ...",
  "expiresIn": 3600
}
```

---

### User Profile

#### `GET /v1/users/me`

Get the authenticated user's profile.

**Headers:** `Authorization: Bearer <token>`

**Response `200 OK`**

```json
{
  "id": "uuid",
  "email": "user@example.com",
  "name": "Jane Doe",
  "createdAt": "2024-01-01T12:00:00Z"
}
```

---

#### `PATCH /v1/users/me`

Update the authenticated user's profile.

**Headers:** `Authorization: Bearer <token>`

**Request Body** (all fields optional)

```json
{
  "name": "Jane Smith"
}
```

**Response `200 OK`**

```json
{
  "id": "uuid",
  "email": "user@example.com",
  "name": "Jane Smith",
  "updatedAt": "2024-01-02T09:00:00Z"
}
```

---

### Devices

#### `POST /v1/users/me/devices`

Register a device token for push notifications.

**Headers:** `Authorization: Bearer <token>`

**Request Body**

```json
{
  "token": "fcm-device-token",
  "platform": "android"
}
```

**Response `201 Created`**

```json
{
  "deviceId": "uuid"
}
```

---

## Error Responses

| Status | Code | Description |
|---|---|---|
| `400` | `VALIDATION_ERROR` | Invalid request body |
| `401` | `UNAUTHORIZED` | Missing or invalid token |
| `404` | `USER_NOT_FOUND` | User does not exist |
| `409` | `EMAIL_ALREADY_EXISTS` | Email already registered |
| `500` | `INTERNAL_ERROR` | Unexpected server error |

**Error Body**

```json
{
  "statusCode": 401,
  "code": "UNAUTHORIZED",
  "message": "Invalid or expired token"
}
```
