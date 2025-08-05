# Docker Container Nginx Role

This Ansible role deploys and configures an Nginx reverse proxy container using Docker. It sets up SSL termination, HTTP-to-HTTPS redirects, and virtual host configurations for multiple domains.

## Overview

The role creates a Docker container running Nginx that acts as a reverse proxy with the following features:

- **SSL/TLS termination** with automatic certificate management
- **HTTP-to-HTTPS redirects** for all configured domains
- **Virtual host support** for multiple domains
- **Proxy forwarding** to backend services
- **Docker network integration** for container communication

## Requirements

### Ansible

- Ansible 2.9 or higher
- `community.docker` collection

### Target System

- Docker installed and running
- Docker network created (referenced by `docker_network` variable)

### SSL Certificates

- SSL certificate files in vault format:
  - `ssl/fullchain.pem.vault`
  - `ssl/privkey.pem.vault`

## Role Variables

### Required Variables

| Variable         | Type   | Description                 | Example           |
| ---------------- | ------ | --------------------------- | ----------------- |
| `nginx_sites`    | list   | List of site configurations | See example below |
| `docker_network` | string | Name of the Docker network  | `"web"`           |

### Site Configuration Structure

Each site in the `nginx_sites` list should have the following structure:

```yaml
nginx_sites:
  - server_name: "example.com"
    port: 8080
  - server_name: "api.example.com"
    port: 3000
```

## Usage

### Basic Usage

```yaml
# playbook.yml
- hosts: webservers
  roles:
    - role: docker_container_nginx
      vars:
        nginx_sites:
          - server_name: "app.example.com"
            port: 8080
          - server_name: "api.example.com"
            port: 3000
        docker_network: "web"
```

### With Custom Variables

```yaml
# playbook.yml
- hosts: webservers
  roles:
    - role: docker_container_nginx
      vars:
        nginx_sites:
          - server_name: "myapp.com"
            port: 8080
          - server_name: "admin.myapp.com"
            port: 9000
        docker_network: "production"
```

## Directory Structure

```
roles/docker_container_nginx/
├── files/
│   ├── nginx.conf              # Main Nginx configuration
│   └── ssl/
│       ├── fullchain.pem.vault # SSL certificate (encrypted)
│       └── privkey.pem.vault   # SSL private key (encrypted)
├── handlers/
│   └── main.yaml               # Nginx reload handler
├── tasks/
│   └── main.yaml               # Main role tasks
├── templates/
│   ├── nginx-redirect.conf.j2  # HTTP to HTTPS redirect template
│   └── nginx-vhost.conf.j2     # Virtual host configuration template
└── README.md                   # This file
```

## What the Role Does

1. **Creates directories** for Nginx configuration and SSL certificates
2. **Deploys SSL certificates** from vault-encrypted files
3. **Copies main Nginx configuration** with optimized settings
4. **Generates virtual host configurations** for each site in `nginx_sites`
5. **Creates HTTP-to-HTTPS redirect** for all configured domains
6. **Deploys Docker container** with proper volume mounts and network configuration

## Container Configuration

The Nginx container is configured with:

- **Ports**: 80 (HTTP) and 443 (HTTPS)
- **Restart policy**: `unless-stopped`
- **Volumes**:
  - `/opt/nginx/nginx.conf` → `/etc/nginx/nginx.conf`
  - `/opt/nginx/conf.d` → `/etc/nginx/conf.d`
  - `/opt/nginx/certs` → `/etc/nginx/certs`
- **Network**: Connected to specified Docker network

## SSL/TLS Configuration

The role automatically configures SSL termination with:

- Modern SSL settings with HTTP/2 support
- Certificate and private key management
- Automatic HTTP-to-HTTPS redirects

## Proxy Configuration

Each virtual host is configured to proxy requests to backend services:

- **Host header preservation**
- **Real IP forwarding**
- **X-Forwarded headers** for proper proxy behavior
- **Backend service discovery** via Docker network

## Security Considerations

- SSL private keys are stored with `0600` permissions
- Certificates are stored with `0644` permissions
- All configuration files are read-only in the container
- Container runs as non-root user (nginx)

## Dependencies

This role depends on:

- Docker being installed and running
- A Docker network being created (typically by `docker_network_create` role)
- SSL certificates being available in vault format

## Example Playbook

```yaml
# site.yml
- hosts: webservers
  become: yes
  roles:
    - role: debian_update
    - role: debian_install_docker
    - role: docker_network_create
      vars:
        docker_network: "web"
    - role: docker_container_nginx
      vars:
        nginx_sites:
          - server_name: "app.example.com"
            port: 8080
          - server_name: "api.example.com"
            port: 3000
        docker_network: "web"
```

## Troubleshooting

### Common Issues

1. **SSL certificate errors**: Ensure vault files are properly encrypted and accessible
2. **Container won't start**: Check Docker network exists and is accessible
3. **Backend services unreachable**: Verify services are running and accessible on the Docker network

### Debugging

To debug configuration issues:

```bash
# Check container logs
docker logs nginx

# Inspect container configuration
docker inspect nginx

# Test Nginx configuration
docker exec nginx nginx -t
```

## License

Please refer to the main project license for usage terms.
