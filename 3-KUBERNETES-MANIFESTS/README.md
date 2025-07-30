# Kubernetes Manifests

This directory contains the Kubernetes manifest files (in YAML format) used to define and deploy applications on the K3s cluster. These files are typically managed and deployed automatically via the `deploy-k8s-app` Ansible role.

The manifests are structured as templates (`.j2` extension) to allow for dynamic variable injection from Ansible.

## Application: WordPress

The primary application deployed is a full WordPress stack, which includes:

- **`01-namespace.yml.j2`**: Creates a dedicated namespace (`wp-namespace`) to isolate all application resources.

- **`02-mysql-secret.yml.j2`**: Creates a Kubernetes Secret to securely store the MySQL database credentials. The data is managed by Ansible Vault.

- **`04-pvc.yml.j2`**: Defines two `PersistentVolumeClaim` resources:
  - `mysql-pvc`: Requests persistent storage for the MySQL database data.
  - `wordpress-pvc`: Requests persistent storage for WordPress web content (themes, plugins, uploads).

- **`05-mysql-deployment.yml.j2`**:
  - Defines a `Service` (`ClusterIP`) to provide a stable internal endpoint for the MySQL database.
  - Defines a `Deployment` to manage the MySQL server Pod, mounting the `mysql-pvc` and sourcing credentials from the `mysql-secret`.

- **`06-wordpress-deployment.yml.j2`**:
  - Defines a `Service` (`ClusterIP`) for the WordPress application Pods.
  - Defines a `Deployment` for WordPress, which connects to the database via the `mysql-service` and mounts the `wordpress-pvc`.

- **`07-ingress.yml.j2`**:
  - Defines an `Ingress` resource to expose the `wordpress-service` to the outside world via the Traefik Ingress Controller. It handles routing external HTTP traffic to the WordPress application.

## Deployment

These manifests are not intended to be applied manually with `kubectl apply`. Instead, they are orchestrated by the **`deploy-k8s-app` Ansible Role**, which templates them with the correct variables and applies them to the cluster in the correct order.
