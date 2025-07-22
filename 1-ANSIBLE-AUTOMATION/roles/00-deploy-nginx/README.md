# Role: Deploy Nginx

## Description

This role automates the installation and basic configuration of the Nginx web server directly on a Debian/Ubuntu host. It serves as a fundamental example of managing system packages, services, and configuration files with Ansible.

## Key Tasks
- Updates the `apt` cache.
- Installs the `nginx` package.
- Ensures the `nginx` service is started and enabled on boot.
- Deploys a basic Nginx site configuration from a Jinja2 template.
- Restarts the Nginx service only when the configuration file changes (using a handler).

## Requirements
- A Debian-based target host (e.g., Ubuntu, Debian).

## Example Playbook

    - hosts: webservers
      become: yes
      roles:
         - 00-deploy-nginx