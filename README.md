# Kafka Developer Workshop Collection

Ansible collection for deploying the Kafka developer workshop on OpenShift.

## Description

This collection provides automation to deploy and configure a Kafka developer workshop environment on OpenShift, including:
- OpenShift GitOps operator installation and configuration
- Workshop application deployment via ArgoCD
- User workload monitoring setup
- Showroom integration

## Installation

### From Git Repository

```bash
ansible-galaxy collection install git+https://github.com/yourusername/kafka-developer-workshop.git
```

### From Local Source

```bash
ansible-galaxy collection install /path/to/kafka-developer-workshop
```

## Included Content

### Roles

- `kafka.workshop.ocp4_workload_kafka_workshop` - Main role for deploying the Kafka workshop

## Usage

### Basic Example

```yaml
---
- name: Deploy Kafka Workshop
  hosts: localhost
  connection: local
  tasks:
    - name: Include Kafka workshop role
      ansible.builtin.include_role:
        name: kafka.workshop.ocp4_workload_kafka_workshop
      vars:
        ACTION: provision
        ocp4_workload_kafka_workshop_user_count: "30"
        ocp4_workload_kafka_workshop_user_prefix: "user"
```

### Remove Workshop

```yaml
---
- name: Remove Kafka Workshop
  hosts: localhost
  connection: local
  tasks:
    - name: Include Kafka workshop role
      ansible.builtin.include_role:
        name: kafka.workshop.ocp4_workload_kafka_workshop
      vars:
        ACTION: destroy
```

## Role Variables

See [roles/ocp4_workload_kafka_workshop/defaults/main.yml](roles/ocp4_workload_kafka_workshop/defaults/main.yml) for all available variables.

### Key Variables

- `ACTION`: Either `provision` or `destroy` to deploy or remove the workshop
- `ocp4_workload_kafka_workshop_user_count`: Number of workshop users
- `ocp4_workload_kafka_workshop_user_prefix`: Prefix for user accounts
- `ocp4_workload_kafka_workshop_workshop_gitops_repo`: Git repository for workshop content
- `ocp4_workload_kafka_workshop_openshift_gitops_operator_channel`: OpenShift GitOps operator channel

## Requirements

- OpenShift cluster with cluster-admin access
- Ansible 2.9 or higher
- kubernetes.core collection
- community.okd collection (for OpenShift modules)

## License

GPL-2.0-or-later

## Author Information

