# Traefik – API Gateway

KalyNow uses **Traefik** as the API Gateway and reverse proxy for all external traffic.

---

## Responsibilities

- Route incoming HTTP/HTTPS requests to backend services
- TLS certificate management (Let's Encrypt)
- Load balancing across service instances
- Request logging and metrics

---

## Routing

Traefik discovers services registered in Nomad via its native Nomad provider. Services opt-in to Traefik routing with tags:

```hcl
tags = [
  "traefik.enable=true",
  "traefik.http.routers.user-service.rule=PathPrefix(`/v1/users`) || PathPrefix(`/v1/auth`)",
  "traefik.http.routers.user-service.entrypoints=websecure",
  "traefik.http.routers.user-service.tls.certresolver=letsencrypt"
]
```

### Routing Rules

| Path Prefix | Service |
|---|---|
| `/v1/auth` | User Service |
| `/v1/users` | User Service |
| `/v1/offers` | Offer Service |
| `/v1/restaurants` | Offer Service |
| `/v1/analytics` | Analytics Service |

---

## TLS

Traefik handles TLS termination using Let's Encrypt certificates (ACME HTTP-01 challenge). All external traffic is served over HTTPS.

---

## Dashboard

The Traefik dashboard (if enabled) is accessible at `/traefik` and provides a live view of routers, services, and middleware.

> ⚠️ Restrict dashboard access to internal networks only in production.
