# Upgrade to next major version with this Ansible role

Targets can be upgraded from Debian N to Debian N+n major version. n can be 1 but also 2, 3 etc.

Upgrading Debian 8 or 9 directly to Debian latest release is possible.

Upgrading from lower versions is untested but should also work.

Note: Always use backups !

* For each Debian version, use codename groups and related variables in Ansible inventory

**inventory/group_vars/buster/apt.yml**

    ---
    apt_codename: buster

**inventory/group_vars/bullseye/apt.yml**

    ---
    apt_codename: bullseye

**inventory/hosts**

    ...
    [buster]
    node0
    node1
    node2
    ...
    [bullseye]
    node3
    ...

* In the Ansible inventory, move host from distro-codename group to distro-codename+1 group.

The inventory reflects the "desired" state

Example to upgrade node0 from buster to bullseye :

**inventory/hosts**

Before :

    ...
    [buster]
    node0
    node1
    node2
    ...

After :

    ...
    [buster]
    node1
    node2
    ...
    [bullseye]
    node0
    node3
    ...

* Apply roles **apt** (<a href="https://github.com/mbocquet/apt" target="new">https://github.com/mbocquet/apt</a>) AND **unattended-upgrades** (this current role).
`./playbooks/unattended-upgrades.yml`
`Unattended-Upgrade::MinimalSteps "true";` is a great way of upgrading few packages and minor releases. If you use it in unattended-upgrades config, you should disable it. With `Unattended-Upgrade::MinimalSteps "true";` a major upgrade can take days to finish !
`./playbooks/unattended-upgrades.yml -e "uu_minimal_steps=false"`
* Adapt third party repositories on targets (**/etc/apt/sources.list.d/\*.list**) to reflect new codename or release.
* Wait for upgrades. If you're impatient, force them via running `unattended-upgrades` on the target host or modify APT timers frequency.

When upgrade is successful :

* Merge new config files if needed (<a href="https://wiki.sekoya.org/#!apt.md#Configuration_files_handling" target="new">https://wiki.sekoya.org/#!apt.md#Configuration_files_handling</a>)
* Revert to your default MinimalSteps value if applicable by running the playbook again without extra var "uu_minimal_steps".

`./playbooks/unattended-upgrades.yml`

You're done !
