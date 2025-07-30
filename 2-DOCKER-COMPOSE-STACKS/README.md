# Dockerized Services

This directory contains the Docker Compose configurations for various services running in the homelab. Using Docker Compose allows for easy definition and management of multi-container applications.

## Stacks

### 1. `private-registry/`
- **Purpose:** Deploys a private Docker Registry to store custom-built container images.
- **Components:**
  - `registry:3` (Backend): The core registry service that stores image layers.
  - `joxit/docker-registry-ui` (Frontend): A web-based user interface for browsing and managing images in the registry.
- **Usage:**
  ```bash
  cd private-registry
  docker compose up -d
  ```
  The registry will be available at `HOST_IP:5000` and the UI at `HOST_IP:8080`.

### 2. `cicd-stack/`
- **Purpose:** Deploys a complete CI/CD (Continuous Integration/Continuous Deployment) environment.
- **Components:**
  - `gitea/gitea`: A lightweight, self-hosted Git service.
  - `drone/drone`: The CI/CD server that listens for webhooks from Gitea.
  - `drone/drone-runner-docker`: The runner agent that executes pipeline steps, with access to the host's Docker socket to build images.
- **Usage:**
  ```bash
  cd cicd-stack
  # Update environment variables in docker-compose.yml first
  docker compose up -d
  ```

### 3. `monitoring-stack/` (Example)
- **Purpose:** Deploys a monitoring stack based on Prometheus and Grafana.
- **Components:**
  - `prom/prometheus`: The time-series database for collecting metrics.
  - `grafana/grafana`: The visualization platform for creating dashboards.
  - `prom/node-exporter`: The agent to be deployed on target machines to export node-level metrics.
- **Usage:**
  ```bash
  cd monitoring-stack
  docker compose up -d
  ```
