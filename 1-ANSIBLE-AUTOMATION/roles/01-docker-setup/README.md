# Role: Deploy WordPress with Docker Compose

## Description

This role automates the deployment of a complete WordPress application stack (WordPress, MySQL database, and phpMyAdmin) using Docker Compose.

## Key Tasks
- Creates a project directory on the target Docker host.
- Copies a templated `docker-compose.yml` file, injecting variables (like passwords from Ansible Vault).
- Uses the `docker_compose_v2` module to start the application stack.

## Dependencies
- The target host must have Docker and the Docker Compose plugin installed. It is recommended to run the `01-docker-setup` role first.

## Example Playbook

    - hosts: dockerhosts
      become: yes
      roles:
         - 01-docker-setup
         - 02-deploy-wordpress-docker