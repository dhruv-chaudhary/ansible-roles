# Docker Container Pi-hole Role

This Ansible role deploys and configures a Pi-hole DNS server using Docker. Pi-hole is a network-wide ad blocker that acts as a DNS sinkhole and optionally a DHCP server.

## Overview

The role creates a Docker container running Pi-hole with the following features:

- **DNS server** running on ports 53 (TCP/UDP)
- **Web interface** accessible on port 8080
- **Persistent storage** for Pi-hole configuration
- **Docker network integration** for container communication
- **Automatic restart** with unless-stopped policy
- **Timezone configuration** for Asia/Kolkata

## Requirements

### Ansible

- Ansible 2.9 or higher
- `community.docker` collection

### Target System

- Docker installed and running
- Docker network created (referenced by `docker_network` variable)
- Docker volume for Pi-hole data persistence

### Variables

The following variables must be defined:

| Variable          | Type   | Description                      | Required |
| ----------------- | ------ | -------------------------------- | -------- |
| `docker_network`  | string | Name of the Docker network       | Yes      |
| `pihole_web_pass` | string | Web interface password (vaulted) | Yes      |

## Role Variables

### Required Variables

- `docker_network`: The name of the Docker network to connect the Pi-hole container to
- `pihole_web_pass`: Password for the Pi-hole web interface (should be vaulted)

### Default Configuration

The role uses the following default configuration:

- **Image**: `pihole/pihole:2025.08.0`
- **Container name**: `pihole`
- **Ports**:
  - 53:53/tcp (DNS)
  - 53:53/udp (DNS)
- **Volume**: `pihole:/etc/pihole` (persistent storage)
- **Timezone**: Asia/Kolkata
- **Web interface port**: 8080
- **DNS listening mode**: all
- **Restart policy**: unless-stopped

## Usage

### Basic Usage

```yaml
# playbook.yml
- hosts: dns_servers
  roles:
    - role: docker_container_pihole
      vars:
        docker_network: "web"
        pihole_web_pass: "{{ vaulted_password }}"
```

### With Variable Files

```yaml
# group_vars/dns_servers.yml
docker_network: "web"
pihole_web_pass: "{{ vaulted_password }}"

# playbook.yml
- hosts: dns_servers
  roles:
    - docker_container_pihole
```

### Host Variables Example

```yaml
# host_vars/incipio.yaml
pihole_web_pass: !vault |
  $ANSIBLE_VAULT;1.1;AES256
  [encrypted_password_here]
```

## Container Features

### DNS Server

- Runs on standard DNS ports (53 TCP/UDP)
- Acts as a DNS sinkhole for ad domains
- Can be configured as primary or secondary DNS

### Web Interface

- Accessible on port 8080
- Provides dashboard for statistics and configuration
- Password-protected with configurable credentials

### Persistence

- Uses Docker volume `pihole` for configuration persistence
- Survives container restarts and updates

### Network Integration

- Connects to specified Docker network
- Enables communication with other containers

## Post-Deployment

After deployment, you can:

1. **Access the web interface** at `http://your-server:8080`
2. **Configure your router** to use the Pi-hole IP as DNS server
3. **Add custom blocklists** through the web interface
4. **Monitor statistics** and blocked queries

## Security Considerations

- The web interface password should be stored in Ansible Vault
- Consider restricting access to the web interface port
- Regularly update the Pi-hole image for security patches

## Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure port 53 is not used by other DNS services
2. **Network connectivity**: Verify the Docker network exists
3. **Volume permissions**: Ensure Docker has proper permissions for volume creation

### Logs

Check container logs:

```bash
docker logs pihole
```

### Container Status

Check container status:

```bash
docker ps | grep pihole
```

## License

Please refer to the main project license for usage terms.
