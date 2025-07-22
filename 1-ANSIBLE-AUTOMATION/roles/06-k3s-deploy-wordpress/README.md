# Role: Deploy WordPress to Kubernetes

## Description

This role automates the deployment of a complete WordPress application onto an existing Kubernetes cluster. It uses Ansible to manage and apply a full set of templated Kubernetes manifest files.

## Key Tasks
- Loads sensitive data (passwords) from an Ansible Vault file.
- Creates a dedicated directory on the K8s master node to store the generated manifests.
- Uses the `template` module to generate all necessary K8s manifests (Namespace, Secret, PVC, Deployment, Service, Ingress) from `.j2` templates.
- Uses the `command` module to run `kubectl apply` on the master node, deploying the entire application stack.

## Dependencies
- A running Kubernetes cluster. This role is intended to be run after `05-k3s-cluster-setup`.
- `kubectl` must be configured and available on the master node.

## Example Playbook

    - hosts: k3s_master # This role runs its tasks on the master node
      become: yes
      roles:
         - 06-k3s-deploy-wordpress