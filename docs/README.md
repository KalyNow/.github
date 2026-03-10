# 📚 KalyNow Platform – Documentation

Welcome to the KalyNow platform documentation. This directory contains all technical and architectural documentation for the KalyNow microservices platform.

## Overview

**KalyNow** is an experimental food-rescue platform built on a modern microservices architecture. Restaurants can publish end-of-day meals and discounted offers, and users can discover nearby offers in real time.

> ⚠️ This project is developed **for educational purposes only** to practice system design, distributed architecture, and DevOps practices.

---

## Documentation Structure

| Folder | Contents |
|---|---|
| [`architecture/`](./architecture/) | System architecture, service design, and diagram placeholders |
| [`api/`](./api/) | API documentation for each microservice |
| [`infrastructure/`](./infrastructure/) | Infrastructure setup, deployment, and operations |
| [`development/`](./development/) | Developer guides, local setup, and contribution guidelines |

---

## Quick Links

- [Architecture Overview](./architecture/overview.md)
- [System Diagrams](./architecture/diagrams.md)
- [API Reference](./api/README.md)
- [Infrastructure Guide](./infrastructure/README.md)
- [Development Setup](./development/getting-started.md)

---

## Platform at a Glance

### Core Services

| Service | Technology | Responsibility |
|---|---|---|
| `kalynow-user-service` | NestJS + PostgreSQL | Users, subscriptions, devices |
| `kalynow-offer-service` | NestJS + MongoDB | Restaurants, offers, reservations |
| `kalynow-analytics` | FastAPI + ClickHouse | Event processing, analytics |
| `kalynow-web` | React + TypeScript | Web interface |
| `kalynow-mobile` | Flutter | Mobile application |
| `kalynow-infra` | Nomad + Traefik | Infrastructure & deployment |
| `kalynow-contracts` | JSON Schema / Types | Shared schemas and API contracts |

### Key Principles

- Microservices architecture
- Event-driven communication via Kafka
- Clean Architecture & SOLID principles
- Cloud-native infrastructure
- CI/CD with GitHub Actions
