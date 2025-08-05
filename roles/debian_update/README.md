# Debian Update Role

This Ansible role handles the updating of Debian-based systems. It performs the following tasks:

1. Updates the apt cache
2. Upgrades all installed packages
3. Handles system reboots if required

## Requirements

- Ansible 2.9 or higher
- Target hosts must be Debian-based systems
- Target hosts must have sudo privileges configured

## Role Variables

The following variables can be configured in your playbook or inventory:

- `auto_reboot` (default: true): Whether to automatically reboot if required
- `reboot_timeout` (default: 300): Maximum time to wait for reboot in seconds
- `reboot_connect_timeout` (default: 5): Timeout for connecting to the host after reboot
- `reboot_pre_delay` (default: 0): Delay before reboot in seconds
- `reboot_post_delay` (default: 30): Delay after reboot in seconds

## Example Playbook

```yaml
- hosts: debian_servers
  roles:
    - role: debian_update
      vars:
        auto_reboot: true
        reboot_timeout: 600
```

## License

MIT
