# linux_initial_setup

This repository contains an Ansible playbook configured for the initial setup and provisioning of Linux machines (primarily targeting Ubuntu/Debian-based systems).

## High-Level Summary of Configurations

When run, the playbook performs the following key setup tasks:

1. **System Preparation**: Updates package caches and removes conflicting/legacy Docker packages to ensure a clean state.
2. **Repository Configuration**: Adds the official repositories and GPG keys for **Docker** and **Zabbix**.
3. **Software Installation**: Installs a core set of system packages and tools, including:
   - Base utilities: `tree`, `vim`, `curl`, `ca-certificates`, `neofetch`
   - Containerization: `docker-ce`, `docker-ce-cli`, `containerd.io`, `docker-buildx-plugin`, `docker-compose-plugin`
   - Monitoring & Virtualization: `zabbix-agent2`, `qemu-guest-agent`
4. **User & Access Management**:
   - Creates/configures a user named `jose` and adds them to the `docker` group.
   - Sets up custom sudoers rules (`/etc/sudoers.d/jose`).
5. **System Customization**: Configures a custom Message of the Day (MOTD) for when users log in.
6. **Monitoring Configuration**: Configures the Zabbix agent (`zabbix_agent2.conf`) to connect to specific Zabbix servers (`192.168.15.16`, `192.168.15.19`) and registers the host.
7. **Service Management**: Ensures that both the Zabbix agent and QEMU guest agent services are started, enabled, and restarted to apply configurations.

## Structure

- `start_deploy.yml`: The main entrypoint playbook that gathers facts, loads vaulted variables (`pwd_jose.yml`), and triggers the `linux_setup` role.
- `linux_setup/`: The Ansible role containing the specific setup tasks, templates, and files.
- `pwd_jose.yml`: An Ansible vault file containing encrypted secrets/variables.
