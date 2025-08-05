# OASYS Ansible Roles

A collection of Ansible roles for automating infrastructure deployment and management on Debian-based systems with Docker containerization.

## Overview

This repository contains a set of modular Ansible roles designed to automate common infrastructure tasks including system updates, Docker installation, network configuration, and container deployment.

## Roles

### System Management

- **[debian_update](roles/debian_update/README.md)** - Updates Debian-based systems with package upgrades and optional reboots
- **[debian_validate_network](roles/debian_validate_network/README.md)** - Validates internet connectivity on Debian servers
- **[debian_install_docker](roles/debian_install_docker/README.md)** - Installs Docker Engine on Debian-based systems

### Docker Infrastructure

- **[docker_network_create](roles/docker_network_create/README.md)** - Creates Docker bridge networks for container communication
- **[docker_container_nginx](roles/docker_container_nginx/README.md)** - Deploys Nginx reverse proxy with SSL termination
- **[docker_container_pihole](roles/docker_container_pihole/README.md)** - Deploys Pi-hole DNS server for network-wide ad blocking

## Requirements

- Ansible 2.9 or higher
- `community.docker` collection
- Debian-based systems with root/sudo privileges

## Quick Start

```bash
# Install dependencies
ansible-galaxy collection install community.docker
```

```yaml
# Basic setup playbook
- hosts: servers
  become: yes
  roles:
    - debian_update
    - debian_validate_network
    - debian_install_docker
    - role: docker_network_create
      vars:
        docker_network_name: web
```

## Complete Example

```yaml
# site.yml
- hosts: all
  become: yes
  roles:
    - debian_update
    - debian_validate_network
    - debian_install_docker
    - role: docker_network_create
      vars:
        docker_network_name: web
    - role: docker_container_pihole
      vars:
        docker_network: "web"
        pihole_web_pass: "{{ vaulted_password }}"
    - role: docker_container_nginx
      vars:
        nginx_sites:
          - server_name: "app.example.com"
            port: 8080
        docker_network: "web"
```

## Security

- Use Ansible Vault for sensitive data (passwords, SSL certificates)
- Regularly update system packages and Docker images
- Consider restricting access to management interfaces

## License

MIT License
