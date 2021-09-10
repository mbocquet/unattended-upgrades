# unattended-upgrades

Ansible role to configure unattended-upgrades.

## Requirements

None.

## Role Variables

Many. See defaults/main.yml

## Dependencies

None.

## Install this role as submodule in a git repository

`git submodule add https://github.com/mbocquet/unattended-upgrades.git roles/unattended-upgrades`

## Example Playbook

    - hosts: servers
      roles:
         - unattended-upgrades

or

    - hosts: servers
      roles:
         - { role: unattended-upgrades, x: 42 }

if any variables comes in the future fot this role.

See [UPGRADE.md](UPGRADE.md) to upgrade from Debian N to Debian N+n with this Ansible role.

## License

GPLv3

## Author Information

http://www.sekoya.org
