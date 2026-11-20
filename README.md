# ansible-linux-automation

Production Ansible playbooks for Linux server automation — OS patching, SSH hardening, compliance remediation, health checks, and routine operations across RHEL, Ubuntu, and SUSE.

## Playbooks

| Playbook | Description |
|---|---|
| `patching.yml` | Full OS patch cycle: pre-check → patch → reboot → validate |
| `ssh-hardening.yml` | Harden SSH: disable root login, enforce key auth, set idle timeout |
| `health-check.yml` | Check CPU, memory, disk, service status — output summary report |
| `user-management.yml` | Create/remove users, manage groups and sudo permissions |
| `compliance-remediation.yml` | Apply CIS benchmark fixes across 200+ servers |
| `ssl-cert-rotation.yml` | Rotate SSL/TLS certificates with zero-downtime reload |
| `firewalld-setup.yml` | Configure firewalld zones, services, and ports |
| `cron-setup.yml` | Deploy and manage cron jobs across server estate |

## Directory Structure

```
ansible-linux-automation/
├── inventories/
│   ├── production/
│   │   ├── hosts.ini
│   │   └── group_vars/
│   └── staging/
│       └── hosts.ini
├── roles/
│   ├── common/
│   ├── patching/
│   ├── hardening/
│   └── monitoring/
├── playbooks/
│   ├── patching.yml
│   ├── ssh-hardening.yml
│   └── health-check.yml
└── ansible.cfg
```

## Example: Patching Playbook

```yaml
---
- name: OS Patching
  hosts: all
  become: true
  vars:
    reboot_required: false

  tasks:
    - name: Update all packages (RHEL)
      yum:
        name: "*"
        state: latest
      when: ansible_os_family == "RedHat"

    - name: Update all packages (Ubuntu)
      apt:
        upgrade: dist
        update_cache: yes
      when: ansible_os_family == "Debian"

    - name: Check if reboot required
      stat:
        path: /var/run/reboot-required
      register: reboot_file

    - name: Reboot if required
      reboot:
        reboot_timeout: 300
      when: reboot_file.stat.exists
```

## Impact

- Automated patching across **200+ servers** (RHEL, Ubuntu, SUSE)
- Saved **~18 engineer-hours/week** replacing manual operations
- Remediated **500+ servers** for security compliance at scale

## Stack

![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=flat-square&logo=ansible&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)
![RHEL](https://img.shields.io/badge/RHEL-EE0000?style=flat-square&logo=redhat&logoColor=white)
