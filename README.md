# Ansible Roles Collection

This project contains a collection of Ansible roles that perform commonly used tasks for server management and automation. These roles are designed to be reusable, well-documented, and follow Ansible best practices.

## Overview

This repository provides a set of modular Ansible roles that can be used individually or combined to automate various server management tasks. Each role is self-contained and follows the standard Ansible role structure.

## Available Roles

### Docker Management

- **install-docker**: Installs Docker and Docker Compose

### System Administration

- **update-server**: Handles system updates, package management, and security patches

## Prerequisites

- Ansible 2.9 or higher
- Target servers running supported Linux distributions (Ubuntu, Debian, Raspbery Pi)
- SSH access to target servers
- Python 3.6+ on target servers

## Installation

1. Clone this repository:

```bash
git clone https://github.com/dhruv-chaudhary/ansible-roles.git
cd ansible-roles
```

## Usage

### Basic Usage

Each role can be used independently in your playbooks:

```yaml
---
- hosts: servers
  become: yes
  roles:
    - update-server
    - install-docker
```

### Example Playbooks

#### Complete Server Setup

```yaml
---
- name: Complete server setup
  hosts: webservers
  become: yes
  roles:
    - update-server
    - install-docker
```

## Role Configuration

Each role includes default variables that can be overridden in your playbook or inventory.

## Directory Structure

```
ansible-roles/
├── roles/
│   ├── install-docker/
│   │   ├── defaults/
│   │   ├── handlers/
│   │   ├── tasks/
│   │   └── vars/
│   ├── update-server/
│   └── ...
└── README.md
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow Ansible best practices and style guidelines
- Include proper documentation for each role
- Use semantic versioning for releases
- Ensure roles are idempotent

## Testing

Test your roles using:

```bash
# Test with molecule (if configured)
molecule test

# Test with ansible-playbook
ansible-playbook -i inventory/test playbooks/test.yml --check
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

- Create an issue for bug reports or feature requests
- Check the documentation in each role's README

## Roadmap

- [ ] Copy SSL certificates to target server
- [ ] Wireguard installation and configuration
- [ ] Mount SMB network shares

## Acknowledgments

- Ansible community for best practices and examples
