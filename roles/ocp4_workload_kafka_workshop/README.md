# ocp4_workload_kafka_workshop

Ansible role to deploy a Kafka developer workshop on OpenShift 4.

## Description

This role automates the deployment of a complete Kafka developer workshop environment, including:

- **OpenShift GitOps**: Installs and configures the OpenShift GitOps operator with custom resource settings
- **Workshop Application**: Deploys the workshop content via ArgoCD/GitOps
- **User Workload Monitoring**: Configures monitoring for workshop users
- **Showroom Integration**: Sets up the Showroom environment for workshop delivery

## Requirements

- OpenShift 4.x cluster with cluster-admin privileges
- Ansible 2.9 or higher
- `kubernetes.core` collection
- `community.okd` collection

## Role Variables

All variables are defined in [defaults/main.yml](defaults/main.yml).

### Core Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `ACTION` | N/A | **Required**. Either `provision` or `destroy` |
| `ocp4_workload_kafka_workshop_user_count` | `{{ ocp4_workload_authentication_htpasswd_user_count }}` | Number of workshop users |
| `ocp4_workload_kafka_workshop_user_prefix` | `{{ ocp4_workload_authentication_htpasswd_user_base }}` | Prefix for user accounts |
| `become_override` | `false` | Override become behavior |
| `ocp_username` | `opentlc-mgr` | OpenShift username for operations |
| `silent` | `false` | Suppress verbose output |

### OpenShift GitOps Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `ocp4_workload_kafka_workshop_openshift_gitops_operator_channel` | `latest` | Operator subscription channel |
| `ocp4_workload_kafka_workshop_openshift_gitops_operator_automatic_install_plan_approval` | `true` | Auto-approve install plans |
| `ocp4_workload_kafka_workshop_openshift_gitops_setup_cluster_admin` | `true` | Grant cluster-admin to GitOps |
| `ocp4_workload_kafka_workshop_openshift_gitops_controller_app_sync` | `3m` | ArgoCD app sync timeout |

### Workshop Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `ocp4_workload_kafka_workshop_workshop_application_namespace` | `openshift-gitops` | Namespace for workshop app |
| `ocp4_workload_kafka_workshop_workshop_application_name` | `kafka-workshop` | Name of workshop application |
| `ocp4_workload_kafka_workshop_workshop_gitops_repo` | `https://github.com/streams-kafka-workshop/helm.git` | Git repo with workshop content |
| `ocp4_workload_kafka_workshop_workshop_gitops_repo_tag` | `main` | Git branch/tag to use |
| `ocp4_workload_kafka_workshop_workshop_gitops_repo_path` | `kafka-workshop` | Path in repo to workshop helm chart |

See [defaults/main.yml](defaults/main.yml) for complete list of resource limits and additional configuration options.

## Dependencies

None

## Example Playbook

### Deploy Workshop

```yaml
---
- name: Deploy Kafka Developer Workshop
  hosts: localhost
  connection: local
  gather_facts: false
  tasks:
    - name: Deploy workshop
      ansible.builtin.include_role:
        name: kafka.workshop.ocp4_workload_kafka_workshop
      vars:
        ACTION: provision
        ocp4_workload_kafka_workshop_user_count: "30"
        ocp4_workload_kafka_workshop_user_prefix: "user"
        ocp4_workload_kafka_workshop_workshop_gitops_repo: "https://github.com/streams-kafka-workshop/helm.git"
        ocp4_workload_kafka_workshop_workshop_gitops_repo_tag: "main"
```

### Remove Workshop

```yaml
---
- name: Remove Kafka Developer Workshop
  hosts: localhost
  connection: local
  gather_facts: false
  tasks:
    - name: Remove workshop
      ansible.builtin.include_role:
        name: kafka.workshop.ocp4_workload_kafka_workshop
      vars:
        ACTION: destroy
```

## Task Files

- `main.yml` - Entry point that routes to provision or destroy tasks
- `workload.yml` - Main provisioning tasks
- `openshift_gitops.yml` - OpenShift GitOps operator setup
- `workshop.yml` - Workshop application deployment
- `user_workload_monitoring.yml` - User monitoring configuration
- `showroom.yml` - Showroom environment setup
- `remove_workload.yml` - Cleanup and removal tasks (referenced in main.yml)

## License

GPL-2.0-or-later

## Author Information

Created for the Kafka Developer Workshop
