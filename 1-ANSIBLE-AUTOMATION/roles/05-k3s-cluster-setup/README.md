# Role: K3s Cluster Setup

## Description

This is an advanced role that fully automates the creation of a lightweight Kubernetes cluster using K3s. It handles the installation of the master node (control plane) and the subsequent joining of worker nodes.

## Key Tasks
- **On Master Node:**
    - Installs the K3s server.
    - Waits for the server to be ready.
- **On Worker Nodes:**
    - Retrieves the master node's IP and join token using `delegate_to` and `hostvars`.
    - Uses the retrieved information to install the K3s agent and join the cluster.

## Requirements
- An inventory file with two groups: `[k3s_master]` (with exactly one host) and `[k3s_workers]`.

## Example Playbook

    - hosts: k3s_cluster # A parent group containing master and workers
      become: yes
      roles:
         - 05-k3s-cluster-setup