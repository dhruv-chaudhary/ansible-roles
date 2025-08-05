# Docker Installation Role

This Ansible role installs Docker Engine on Debian-based systems.

## Requirements

- Ansible 2.9 or higher
- Debian-based system (Debian, Ubuntu)
- Root or sudo privileges

## Role Variables

No variables are required for this role.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: servers
  roles:
    - debian_install_docker
```

## What This Role Does

1. Installs required system packages
2. Adds Docker's official GPG key
3. Adds Docker repository
4. Installs Docker Engine (docker-ce, docker-ce-cli, containerd.io)
5. Ensures Docker service is enabled and started

## License

MIT

## Author Information

OASYS Infrastructure Team
