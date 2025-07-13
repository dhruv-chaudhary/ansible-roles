# Update server

This Ansible role handles the updating of Debian-based systems. It performs the following tasks:

1. Updates the apt cache
2. Upgrades all installed packages
3. Handles system reboots if required

## Requirements

- Ansible 2.9 or higher
- Target hosts must be Debian-based systems
- Target hosts must have sudo privileges configured

## Role Variables

No variables are required for this role.

## Example Playbook

```yaml
- hosts: debian_servers
  roles:
    - role: debian_update
```
