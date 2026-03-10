# Nomad – Service Orchestration

KalyNow uses **HashiCorp Nomad** to schedule and manage all containerised services.

---

## Why Nomad?

- Lightweight alternative to Kubernetes
- Supports Docker workloads natively
- Simple job specification (HCL)
- Supports rolling deployments and health checks

---

## Job Structure

Each service has a Nomad job file in the `kalynow-infra` repository.

### Example Job Skeleton

```hcl
job "user-service" {
  datacenters = ["dc1"]
  type        = "service"

  group "user-service" {
    count = 2

    network {
      port "http" { to = 3001 }
    }

    task "user-service" {
      driver = "docker"

      config {
        image = "ghcr.io/kalynow/kalynow-user-service:latest"
        ports = ["http"]
      }

      env {
        DATABASE_URL = "..."
        JWT_SECRET   = "..."
      }

      resources {
        cpu    = 256
        memory = 256
      }

      service {
        name = "user-service"
        port = "http"
        tags = ["traefik.enable=true"]

        check {
          type     = "http"
          path     = "/health"
          interval = "10s"
          timeout  = "2s"
        }
      }
    }
  }
}
```

---

## Deployment

To deploy or update a service:

```bash
nomad job run jobs/user-service.hcl
```

To check job status:

```bash
nomad job status user-service
```

To view allocations:

```bash
nomad alloc logs <alloc-id>
```

---

## Rolling Updates

Nomad performs rolling updates by default. Configure in the `update` stanza:

```hcl
update {
  max_parallel     = 1
  min_healthy_time = "10s"
  healthy_deadline = "3m"
  auto_revert      = true
}
```
