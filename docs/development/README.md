# 💻 Development

This section contains guides for developers working on the KalyNow platform.

## Contents

| Document | Description |
|---|---|
| [Getting Started](./getting-started.md) | Local environment setup and running services |
| [Contributing](./contributing.md) | Contribution guidelines and workflow |
| [Code Style](./code-style.md) | Coding standards and conventions |
| [Testing](./testing.md) | Testing strategy and running tests |
| [Environment Variables](./environment-variables.md) | Configuration reference |

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and Docker Compose
- [Node.js](https://nodejs.org/) v20+
- [Python](https://www.python.org/) 3.11+
- [Flutter](https://flutter.dev/docs/get-started/install) SDK
- [Git](https://git-scm.com/)

## Repository Structure

Each service lives in its own repository under the [KalyNow](https://github.com/KalyNow) GitHub organisation:

```
KalyNow/
├── .github           # Org-level docs and workflows
├── kalynow-user-service
├── kalynow-offer-service
├── kalynow-analytics
├── kalynow-web
├── kalynow-mobile
├── kalynow-infra
└── kalynow-contracts
```
