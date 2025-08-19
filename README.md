![Ansible](https://img.shields.io/badge/Ansible-Automation-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Arch](https://img.shields.io/badge/Arch-Mixed--ARM64--x86_64-orange)
![Kubernetes](https://img.shields.io/badge/Kubernetes-K3s-lightgrey)

# Raspberry Pi Kubernetes Automation Lab

This repository contains an Ansible-based setup for provisioning a fully functional Kubernetes cluster using Raspberry Pi and virtual machines. It is designed as a personal lab to explore infrastructure automation, container orchestration, and service deployment — all using modular and reusable code.

---

## Project Overview

The lab automates the setup of a multi-node K3s cluster using Ansible. It includes:

- System configuration and hostname setup
- K3s installation and cluster bootstrapping
- Workload deployment using templated manifests
- Complete monitoring and observability stack (Prometheus, Node Exporter, Alertmanager, Grafana)
- Role-based structure for easy expansion

> The cluster consists of a **Raspberry Pi 4 (ARM64)** as the master node and a **virtual machine (x86_64)** as the worker node, demonstrating a mixed-architecture setup supported by K3s.

---
## Prerequisites
- Ansible >= 2.16 (check with `ansible --version`)
- SSH access configured for all nodes in `hosts.ini`
- Python 3.x installed on controller host
---

## Directory Structure

```plaintext
iac-ansible-k3s-rpi-lab/
├── LICENSE
├── README.md
├── ansible.cfg
├── ca.crt
├── group_vars/              # Group-specific variables (all, master, worker, kubernetes)
│   ├── all.yml
│   ├── kubernetes.yml
│   ├── master.yml
│   └── worker.yml
├── hosts.ini                # Ansible inventory file
├── roles/
│   ├── common/              # Base system setup (packages, hostname, user, etc.)
│   │   ├── README.md
│   │   ├── handlers/        # Ansible handlers for common role
│   │   │   └── main.yml
│   │   └── tasks/           # Ansible tasks for common role
│   │       └── main.yml
│   ├── kubernetes/          # K3s cluster installation and configuration
│   │   ├── README.md
│   │   ├── defaults/        # Default variables for Kubernetes role
│   │   │   └── main.yml
│   │   ├── files/           # Static files (e.g. kube-state-metrics manifest)
│   │   │   └── kube-state-metrics.yaml
│   │   ├── images/          # Reference screenshots
│   │   │   ├── kubectl-get-nodes.png
│   │   │   ├── kubectl-get-pods.png
│   │   │   └── kubectl-get-services.png
│   │   ├── tasks/           # Playbook tasks for Kubernetes role
│   │   │   ├── main.yml
│   │   │   ├── master.yml
│   │   │   ├── worker.yml
│   │   │   └── workloads.yml
│   │   └── templates/       # Jinja2 templates for Kubernetes manifests
│   │       ├── deployment.yml.j2
│   │       └── service.yml.j2
│   └── monitoring/          # Monitoring and observability stack
│       ├── README.md
│       ├── defaults/        # Default variables for monitoring role
│       │   └── main.yml
│       ├── files/           # Static files for monitoring
│       │   └── dashboards/  # Grafana dashboards in JSON format
│       │       ├── k8s-cluster-monitoring.json
│       │       └── node-exporter-full.json
│       ├── handlers/        # Handlers for monitoring role
│       │   └── main.yml
│       ├── images/          # Monitoring screenshots
│       │   ├── alertmanager.png
│       │   ├── grafana-k8s-cluster.png
│       │   ├── node-exporter.png
│       │   └── prometheus-targets.png
│       ├── tasks/           # Playbook tasks for monitoring role
│       │   ├── alertmanager.yml
│       │   ├── grafana.yml
│       │   ├── main.yml
│       │   ├── node_exporter.yml
│       │   └── prometheus.yml
│       ├── templates/       # Jinja2 templates for monitoring configs
│       │   ├── alertmanager.yml.j2
│       │   ├── grafana-datasource.yml.j2
│       │   ├── k8s-prometheus-scrape-rbac.yml.j2
│       │   ├── prometheus.yml.j2
│       │   └── rules/       # Prometheus rule files (templates)
│       │       └── teste.yml.j2
│       └── vars/            # Variable files for monitoring role
│           ├── main.yml
│           └── vault.yml
└── setup.yml                # Main playbook entrypoint
```
---

## Roadmap

### ✅ Phase 1: Basic Raspberry Pi Setup
- [x] Update and upgrade system packages
- [x] Install essential tools (htop, curl, vim)
- [x] Create a user with sudo privileges
- [x] Set custom hostname and update /etc/hosts
- [x] Refactor playbook into roles and variables


### ✅ Phase 2: Kubernetes Cluster
- [x] Install K3s on master and worker nodes
- [x] Configure cluster join and token exchange
- [x] Deploy sample workloads and services

### ✅ Phase 3: Monitoring & Observability
- [x] Install Prometheus and Node Exporter
- [x] Install Grafana with custom dashboards
- [x] Configure alerting with Alertmanager

### 🔜 Phase 4: Application Deployment
- [ ] Deploy custom apps (Flask, PostgreSQL, MQTT, etc.)
- [ ] Use Helm charts or raw manifests
- [ ] Setup CI/CD pipeline (GitHub Actions, Drone, etc.)

### 🔜 Phase 5: Documentation & Automation
- [ ] Add architecture diagrams and flowcharts
- [ ] Create bootstrap scripts for quick setup
- [ ] Add Makefile or CLI wrapper for common tasks

---

## Getting Started

To run full setup:

```bash
ansible-playbook -i hosts.ini setup.yml
```
To run specific parts::

```bash
ansible-playbook -i hosts.ini setup.yml --tags workloads
```

Available tags:

- `k3s` - installs K3s on master and workers
- `workloads` - deploys sample pods and services
- `master` - tasks specific to the master node
- `workers` - tasks specific to worker nodes
---

## Notes
* This lab uses a **mixed-architecture cluster**:
    * Master node: Raspberry Pi 4 (ARM64)
    * Worker node: Virtual Machine (x86_64)
* Ensure container images used in workloads are **multi-arch compatible** (e.g. nginx, busybox, etc.)
* SSH keys and Python interpreters must be correctly set in hosts.ini.
* The K3s token is defined in group_vars/kubernetes.yml — consider externalizing it for security.
> ℹ️ **Tip:** Each role/module has its own README.md with specific instructions and variables.

---
## Author

**Daniel Gil**

---

## License

This project is licensed under the [MIT License](LICENSE).