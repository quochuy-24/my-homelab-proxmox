# Bastion Host: Secure Gateway & Automation Hub

## 1. Purpose and Design Philosophy

The Bastion Host (or Jump Server) is a multi-functional cornerstone of this homelab's architecture, serving two critical roles:

1.  **A Secure Gateway:** It is the **single, hardened, and monitored entry point** for all interactive SSH access into the internal network segments.
2.  **An Automation Hub:** It acts as the **Ansible Control Node**, from which all configuration management and application deployment playbooks are executed.

This dual-role design concentrates both security and automation efforts onto a single, dedicated machine. It adheres to the principle of **reducing the attack surface** while providing a centralized point for all infrastructure management tasks.

---

## 2. Technical Implementation Details

### a. Placement
- **Host Type:** Deployed as a lightweight Debian LXC Container for minimal resource usage.
- **Network:** Placed in the management network (`vmbr0`, `192.168.1.0/24`), making it directly accessible from the administrator's workstation and giving it the necessary network visibility to manage other servers.

### b. Role 1: Secure Gateway Configuration

Access to the Bastion Host itself is tightly controlled through several layers of security:

*   **Dedicated Non-Root User:** Direct root login via SSH is **disabled** (`PermitRootLogin no`). A dedicated, unprivileged user (`bastion-host`) is used for administrative access, with privileges escalated via `sudo`.
*   **SSH Key-Only Authentication:** Password-based authentication is completely **disabled** (`PasswordAuthentication no`), enforcing the use of strong SSH key pairs. See the hardened configuration in [`sshd_config.hardened`](./sshd_config.hardened).
*   **Automated Intrusion Prevention (Fail2ban):** The `fail2ban` service is installed to automatically ban IPs that exhibit brute-force attack patterns. See the custom configuration example in [`fail2ban-jail.local.example`](./fail2ban-jail.local.example).

### c. Role 2: Automation Hub (Ansible Control Node)

The Bastion Host is pre-configured to run and manage the entire infrastructure using Ansible.

*   **Software:** Ansible is installed within an isolated **Python Virtual Environment** (`venv`) to avoid system-wide dependency conflicts.
*   **Connectivity:**
    *   SSH keys have been generated on this host and distributed to all managed nodes (Docker hosts, Kubernetes cluster, etc.) to allow for passwordless automation.
    *   Static routes and specific firewall rules (on OPNsense) have been configured to allow the Bastion Host to reach and manage servers across different network segments (e.g., DMZ).

---

## 3. Administrative Workflows

This centralized design enables two primary workflows:

1.  **Interactive Management (The "Jump"):**
    *   **Admin -> Bastion Host:** An administrator SSHes from their trusted workstation to the Bastion Host using their private key.
    *   **Bastion Host -> Target Server:** From the Bastion Host, the admin can then "jump" to the final target server for manual troubleshooting or inspection.

2.  **Automated Management (The "Push"):**
    *   **Admin -> Bastion Host:** An administrator SSHes to the Bastion Host.
    *   **Bastion Host -> All Servers:** From here, the admin executes `ansible-playbook` commands. The Bastion Host then pushes out configuration changes and application deployments to the entire fleet of managed servers in a consistent and repeatable manner.

This ensures that all changes, whether manual or automated, originate from a single, controlled source.