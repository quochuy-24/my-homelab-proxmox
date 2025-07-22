# Role: Deploy Node Exporter

## Description

This role deploys the Prometheus Node Exporter as a Docker container on target hosts. The Node Exporter exposes a wide range of hardware and OS metrics for Prometheus to scrape.

## Key Tasks
- Creates a project directory for the Node Exporter configuration.
- Copies a `docker-compose.yml` file to the target host.
- Runs the Node Exporter container, mounting the host's root filesystem in read-only mode to collect metrics.

## Dependencies
- The target host must have Docker and the Docker Compose plugin installed (use the `01-docker-setup` role).

## Example Playbook

    - hosts: all_linux_servers
      become: yes
      roles:
         - 04-deploy-node-exporter