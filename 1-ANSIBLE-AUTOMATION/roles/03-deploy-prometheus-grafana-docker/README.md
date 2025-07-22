# Role: Deploy Monitoring Stack (Prometheus + Grafana)

## Description

This role deploys a full-featured monitoring stack using Docker Compose. It sets up Prometheus for metrics collection and Grafana for data visualization.

## Key Tasks
- Creates project and configuration directories on the target host.
- Deploys a `prometheus.yml` configuration file from a template. This file defines the scrape targets.
- Deploys a `docker-compose.yml` file to define and run the `prometheus` and `grafana` services.
- Creates a persistent Docker Volume for Grafana's data.

## Dependencies
- The target host must have Docker and the Docker Compose plugin installed (use the `01-docker-setup` role).

## Example Playbook

    - hosts: monitoring_server
      become: yes
      roles:
         - 01-docker-setup
         - 03-deploy-prometheus-grafana-docker