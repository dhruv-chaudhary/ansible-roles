# Docker Network Role

This Ansible role creates a simple bridge Docker network.

## Description

The `docker_network` role creates a Docker bridge network with a specified name. The network is created with default bridge driver settings.

## Requirements

- Ansible 2.9 or higher
- Docker installed on target hosts
- `community.docker` collection installed

## Required Variables

| Variable              | Description                |
| --------------------- | -------------------------- |
| `docker_network_name` | Name of the Docker network |

## Example Playbook

```yaml
- hosts: docker_hosts
  roles:
    - role: docker_network
      vars:
        docker_network_name: my_network
```

## License

MIT
