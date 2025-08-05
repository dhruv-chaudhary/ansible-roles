# Debian Internet Check Role

This Ansible role checks internet connectivity on Debian servers by performing two tests:

1. DNS resolution test using ping to google.com
2. HTTP connectivity test by accessing debian.org

## Requirements

- Ansible 2.9 or higher
- Target hosts must be Debian-based systems

## Role Variables

No variables are required for this role.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: debian_servers
  roles:
    - debian_internet_check
```

## Output

The role will output the status of both connectivity tests:

- DNS Resolution status (Success/Failed)
- HTTP Access status (Success/Failed)

## License

MIT
