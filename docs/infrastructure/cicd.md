# CI/CD Pipelines

KalyNow uses **GitHub Actions** for automated build, test, and deployment workflows.

---

## Pipeline Overview

Each service repository contains its own GitHub Actions workflow.

### Standard Pipeline Stages

```
Push / PR
  └──▶ 1. Lint & Type Check
  └──▶ 2. Unit Tests
  └──▶ 3. Integration Tests
  └──▶ 4. Build Docker Image
  └──▶ 5. Push to Container Registry (on main branch)
  └──▶ 6. Deploy to Nomad (on main branch)
```

---

## Workflow Example

```yaml
name: CI/CD

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run lint
      - run: npm run test

  build-and-push:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: ghcr.io/kalynow/${{ github.event.repository.name }}:latest

  deploy:
    needs: build-and-push
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Nomad
        run: |
          nomad job run jobs/${{ github.event.repository.name }}.hcl
```

---

## Container Registry

Docker images are published to the **GitHub Container Registry (GHCR)**:

```
ghcr.io/kalynow/<service-name>:latest
ghcr.io/kalynow/<service-name>:<git-sha>
```

---

## Secrets

CI/CD secrets are stored as **GitHub Actions secrets** at the organisation level:

| Secret | Description |
|---|---|
| `NOMAD_ADDR` | Nomad cluster address |
| `NOMAD_TOKEN` | Nomad ACL token for deployment |
| `GHCR_TOKEN` | GitHub token for container registry |
