# 🍲 KalyNow Platform

![Architecture](https://img.shields.io/badge/architecture-microservices-blue)
![Backend](https://img.shields.io/badge/backend-NestJS-red)
![Analytics](https://img.shields.io/badge/python-FastAPI-green)
![Mobile](https://img.shields.io/badge/mobile-Flutter-blue)
![Web](https://img.shields.io/badge/web-React%20%2B%20TypeScript-61dafb)
![Infrastructure](https://img.shields.io/badge/infrastructure-Nomad%20%7C%20Traefik-orange)
![License](https://img.shields.io/badge/purpose-educational-lightgrey)

## Overview

**KalyNow** is an experimental platform designed to explore modern **microservices architecture and cloud-native technologies**.

The platform simulates a food rescue system where restaurants can publish **end-of-day meals or discounted offers**, and users can discover nearby offers in real time.

⚠️ This project is developed **for educational purposes only** to practice system design, distributed architecture, and DevOps practices.

---

# 🏗 Architecture

The platform is built using a **microservices architecture** with separate services for users, offers, analytics, and infrastructure.

Key architecture principles:

* Microservices architecture
* Event-driven communication
* Clean Architecture & SOLID principles
* Cloud-native infrastructure

Core technologies:

* **NestJS** – backend services
* **FastAPI** – analytics and data processing
* **React + TypeScript** – web interface
* **Flutter** – mobile application
* **PostgreSQL / MongoDB** – main databases
* **ClickHouse** – analytics database
* **Kafka** – event streaming
* **Redis** – caching
* **MinIO** – object storage
* **Traefik** – API Gateway
* **Nomad** – service orchestration
* **GitHub Actions** – CI/CD

---

# System Architecture

```
          Web Front ( React )
                 |
                 |
          Mobile App (Flutter)
                 │
                 │
           API Gateway
             (Traefik)
                 │
     ┌───────────┼───────────┐
     │           │           │
 User Service  Offer Service  Analytics
   (NestJS)      (NestJS)      (FastAPI)
     │              │              │
 PostgreSQL       MongoDB       ClickHouse
                     │
                   Kafka
                     │
                   Redis
                     │
                    MinIO
```

---

# Repositories

| Repository                | Role                                                   | Technology           |
| ------------------------- | ------------------------------------------------------ | -------------------- |
| **kalynow-mobile**        | Mobile application for users to discover nearby offers | Flutter              |
| **kalynow-web**           | Web interface for restaurants and partners             | React + TypeScript   |
| **kalynow-user-service**  | Manages users, subscriptions and devices               | NestJS + PostgreSQL  |
| **kalynow-offer-service** | Handles restaurants, offers and reservations           | NestJS + MongoDB     |
| **kalynow-analytics**     | Processes events and generates analytics               | FastAPI + ClickHouse |
| **kalynow-infra**         | Infrastructure configuration and deployment            | Nomad + Traefik      |
| **kalynow-contracts**     | Shared schemas, events and API contracts               | JSON Schema / Types  |

---

# Project Goal

KalyNow is a **learning platform** used to explore:

* distributed system design
* microservices architecture
* event-driven systems
* cloud-native infrastructure
* DevOps and CI/CD workflows

The goal is to simulate a **real production architecture** while improving engineering skills.

---

# Learning Focus

This project explores topics such as:

* Clean Architecture
* SOLID design principles
* Domain-driven service separation
* Event streaming with Kafka
* Infrastructure orchestration
* Observability and monitoring

---

# Status

Early stage educational project used to experiment with architecture and infrastructure patterns.
