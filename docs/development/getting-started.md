# Getting Started

This guide walks you through setting up the KalyNow platform locally for development.

---

## Prerequisites

Ensure you have the following installed:

| Tool | Version | Purpose |
|---|---|---|
| [Docker](https://docs.docker.com/get-docker/) | 24+ | Run services and dependencies |
| [Docker Compose](https://docs.docker.com/compose/) | v2+ | Local orchestration |
| [Node.js](https://nodejs.org/) | 20+ | NestJS services |
| [Python](https://www.python.org/) | 3.11+ | Analytics service |
| [Flutter](https://flutter.dev/docs/get-started/install) | 3+ | Mobile app |
| [Git](https://git-scm.com/) | any | Version control |

---

## Step 1 – Clone the Repositories

Clone the repositories you want to work on from the [KalyNow GitHub organisation](https://github.com/KalyNow):

```bash
git clone https://github.com/KalyNow/kalynow-user-service
git clone https://github.com/KalyNow/kalynow-offer-service
git clone https://github.com/KalyNow/kalynow-analytics
git clone https://github.com/KalyNow/kalynow-infra
```

---

## Step 2 – Start Infrastructure Dependencies

Use Docker Compose from the `kalynow-infra` repository to start shared dependencies:

```bash
cd kalynow-infra
docker compose up -d
```

This starts:
- PostgreSQL (port `5432`)
- MongoDB (port `27017`)
- ClickHouse (port `8123`)
- Redis (port `6379`)
- Kafka + Zookeeper (port `9092`)
- MinIO (port `9000`)

---

## Step 3 – Configure Environment Variables

Each service has a `.env.example` file. Copy it and fill in values:

```bash
cp .env.example .env
```

Refer to [Environment Variables](./environment-variables.md) for a complete reference.

---

## Step 4 – Start a Service

### NestJS Services (User / Offer)

```bash
cd kalynow-user-service
npm install
npm run start:dev
```

### Analytics Service (FastAPI)

```bash
cd kalynow-analytics
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload
```

---

## Step 5 – Verify

- User Service: [http://localhost:3001/health](http://localhost:3001/health)
- Offer Service: [http://localhost:3002/health](http://localhost:3002/health)
- Analytics Service: [http://localhost:8000/health](http://localhost:8000/health)

---

## Next Steps

- [Contributing](./contributing.md)
- [Code Style](./code-style.md)
- [Testing](./testing.md)
