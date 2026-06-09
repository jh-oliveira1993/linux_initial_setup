# Role Name: linux_setup

An Ansible role designed for the initial setup and provisioning of Linux machines. It supports dynamic operating system detection for both Debian-based (Ubuntu, Debian) and RedHat-based (CentOS, RHEL, Oracle Linux) families.

## Requirements

- Ansible version 2.1 or newer.
- For Debian-based systems, an `apt` package manager environment.
- For RedHat-based systems, a `dnf`/`yum` package manager environment.

## Role Variables

The following variables can be configured in `defaults/main.yml` to customize the installation:

| Variable | Default Value | Description |
|---|---|---|
| `zbx_maj_ver` | `7` | The major version of the Zabbix official repository to install. |
| `zbx_min_ver` | `0` | The minor version of the Zabbix official repository. |
| `zbx_srv_ip` | `192.168.15.0/24` | IP configuration for the `Server` parameter in the Zabbix Agent configuration. |
| `zbx_srv_act_ip` | `192.168.15.13;192.168.15.14;192.168.15.93;192.168.15.90` | IP configuration for the `ServerActive` parameter in the Zabbix Agent configuration. Use semicolons to define a cluster/HA setup. |

*Note: There are other files templated by this role such as `/etc/sudoers.d/jose` and `/etc/update-motd.d/01-welcome`. Consider overriding these static templates in the role's `files` directory if your user or MOTD needs differ.*

## Dependencies

None.

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: servers
  become: true
  roles:
    - role: linux_setup
      vars:
        zbx_maj_ver: 7
        zbx_min_ver: 0
        zbx_srv_ip: 10.0.0.0/24
        zbx_srv_act_ip: 10.0.0.50
```

## License

MIT

## Author Information

José Henrique de Oliveira
