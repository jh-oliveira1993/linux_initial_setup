# linux_initial_setup

This repository contains an Ansible playbook configured for the initial setup and provisioning of Linux machines. It features a modular design that supports both **Debian-based** (Ubuntu, Debian) and **RedHat-based** (CentOS, Oracle Linux, RHEL) operating systems.

## High-Level Summary of Configurations

When run, the playbook dynamically detects the target OS family and executes the corresponding OS-specific setup steps. The core tasks include:

1. **OS-Specific System Preparation**:
   - **Debian Family**: Updates `apt` caches and removes conflicting/legacy Docker packages.
   - **RedHat Family**: Removes conflicting Docker/Podman packages via `dnf`.

2. **Repository Configuration**:
   - Dynamically adds the official repositories and GPG keys for **Docker** and **Zabbix** tailored to the specific OS distribution and release version.
   - For RedHat-based systems, it automatically installs the EPEL repository (with specific handling for Oracle Linux).

3. **Software Installation**: Installs a core set of system packages and tools, utilizing the appropriate package manager (`apt` or `dnf`):
   - Base utilities: `tree`, `vim`, `curl`, `ca-certificates`, `neofetch`
   - Containerization: `docker-ce`, `docker-ce-cli`, `containerd.io`, `docker-buildx-plugin`, `docker-compose-plugin`
   - Monitoring & Virtualization: `zabbix-agent2`, `qemu-guest-agent`

4. **User & Access Management**:
   - Creates/configures a user named `jose` and adds them to the `docker` group.
   - Sets up custom sudoers rules (`/etc/sudoers.d/jose`).

5. **System Customization**: Configures a custom Message of the Day (MOTD) for when users log in.

6. **Monitoring Configuration**:
   - Configures the Zabbix agent (`zabbix_agent2.conf`) to connect to designated Zabbix servers.
   - Server IP configurations are parameterized using concise variables (`zbx_srv_ip` and `zbx_srv_act_ip`) found in `defaults/main.yml`.

7. **Service Management**: Ensures that both the Zabbix agent and QEMU guest agent services are started, enabled, and restarted to apply configurations.

## Structure

- `start_deploy.yml`: The main entrypoint playbook that gathers facts, loads vaulted variables (`pwd_jose.yml`), and triggers the `linux_setup` role.
- `linux_setup/`: The Ansible role containing:
  - `tasks/main.yml`: The primary task list that includes OS-specific tasks and handles generic configurations (users, Zabbix configs, services).
  - `tasks/Debian.yml` & `tasks/RedHat.yml`: OS-family specific modules for package management and repository setups.
  - `defaults/main.yml`: Default parameterized variables (like Zabbix repo versions and Server IPs).
- `pwd_jose.yml`: An Ansible vault file containing encrypted secrets/variables.
