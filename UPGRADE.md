# Upgrade to a next major version with this Ansible role

Targets can be upgraded from Debian N to Debian N+n major version. n can be 1, 2, 3...
Upgrading Debian from version 8 and later to Debian latest release is possible.
Upgrading from lower versions is untested but should also work.

Note: Always use backups !

For each Debian version, use codename groups and related variables in Ansible inventory.

**inventory/group_vars/bookworm/apt.yml**

    ---
    apt_codename: bookworm

**inventory/group_vars/trixie/apt.yml**

    ---
    apt_codename: trixie

**inventory/hosts**

    [bookworm]
    node0
    node1
    node2

    [trixie]
    node3

In the Ansible inventory, move host from distro-codename group to distro-codename+1 group.
The inventory reflects the "desired" state.

Example to upgrade node0 from bookworm to trixie :

**inventory/hosts**

```diff
    [bookworm]
-   node0
    node1
    node2

    [trixie]
+   node0
    node3
```

Apply roles **apt** (<a href="https://github.com/mbocquet/apt" target="new">https://github.com/mbocquet/apt</a>) AND **unattended-upgrades** (this current role).
```sh
./playbooks/unattended-upgrades.yml [-l node0]
```

`Unattended-Upgrade::MinimalSteps "true";` is a great way of upgrading few
packages and minor releases. If you use it in unattended-upgrades config, you
should disable it for a major upgrade.
With `Unattended-Upgrade::MinimalSteps "true";` a major upgrade can take days
to finish !

```sh
./playbooks/unattended-upgrades.yml -e "uu_minimal_steps=false" [-l node0]
```
Adapt third party repositories on targets (`/etc/apt/sources.list.d/*.list`)
to reflect new codename or release.

Wait for upgrades. If you're impatient, force them by running
`unattended-upgrades` on the target host or modify APT timers frequency.

After successful upgrade :

Merge new config files if needed.
(search `/etc` for `*.dpkg-new` or `*.dpkg-dist` files).

Revert to your default MinimalSteps value if applicable by running the
playbook again without extra var "uu_minimal_steps".

```sh
./playbooks/unattended-upgrades.yml [-l node0]
```

You're done !
