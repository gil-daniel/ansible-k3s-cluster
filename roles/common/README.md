# Common Role

This Ansible role prepares Raspberry Pi or VM nodes with essential system updates, user creation, and hostname configuration.

## Features

- Updates APT package index and upgrades installed packages
- Installs useful packages defined by the user
- Creates a user with sudo access
- Sets system hostname and updates `/etc/hosts`
- Reboots the device if hostname changes

## Requirements

- Debian-based system (Raspberry Pi OS or Ubuntu)
- SSH access to target node
- Ansible installed locally or in CI/CD

## Variables

Defined in `group_vars/rpi.yml` or similar:

```yaml
packages_to_install:
  - curl
  - git
  - vim
  - htop

user_name: daniel
new_hostname: rpi-node-1
```

## Usage

```bash
ansible-playbook setup.yml --tags common
```
Or include in your main playbook:
```yaml
- name: Prepare system
  hosts: all
  roles:
    - role: common
```

## Notes
* This role is intended to run before container or Kubernetes setup.
* Reboot is triggered automatically if hostname is changed.