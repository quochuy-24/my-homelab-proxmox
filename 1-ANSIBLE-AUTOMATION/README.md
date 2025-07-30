# Ansible Automation for Homelab

This directory contains all Ansible configurations used to automate the setup and management of the homelab infrastructure. The automation is structured using Ansible Roles for modularity and reusability.

## Roles

Below is a summary of the available roles and their purposes:

### 1. `k3s-cluster`
- **Purpose:** Automates the complete installation of a K3s Kubernetes cluster.
- **Features:**
  - Installs the K3s server (control plane) on master nodes.
  - Retrieves the join token from the master.
  - Joins worker nodes to the cluster using the fetched token.
  - The process is idempotent; it will not re-install if K3s is already present.

### 2. `deploy-k8s-app`
- **Purpose:** Deploys a complete application stack onto the Kubernetes cluster.
- **Features:**
  - Uses Ansible Templates (`.j2`) to manage Kubernetes manifest files, allowing for dynamic configuration.
  - Manages sensitive data (like database passwords) securely using Ansible Vault.
  - Deploys all necessary Kubernetes resources, including Namespaces, Secrets, PersistentVolumeClaims, Deployments, Services, and Ingress.
  - This role is designed to run from the control node and executes `kubectl` commands on the K3s master node.

### 3. `configure_docker` (Example)
- **Purpose:** Ensures Docker daemon on specified hosts is correctly configured.
- **Features:**
  - Configures the Docker daemon to trust the private, insecure Docker registry.
  - Restarts the Docker service to apply changes, using a handler to avoid unnecessary restarts.

## Usage

To run a deployment, create a main playbook (e.g., `deploy_all.yml`) that includes these roles and target the appropriate hosts from the `inventory.ini` file.

Example:
```yaml
---
- name: Setup K3s Cluster and Deploy WordPress
  hosts: k3s_cluster
  become: yes
  roles:
    - k3s-cluster
    - deploy-k8s-app
```
