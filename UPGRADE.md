# Upgrade to next major version with this Ansible role

* Backup the system.
* Create a file **upgrade.yml** in Ansible inventory to override current release for the desired target(s).


    ---
    apt_codename: 'buster'
    uu_minimal_steps: false
    uu_mailonlyonerror: false
    ...

or

    ---
    apt_codename: 'stable'
    uu_minimal_steps: false
    uu_mailonlyonerror: false
    ...
* Apply roles **apt** (<a href="https://github.com/mbocquet/apt" target="new">https://github.com/mbocquet/apt</a>) AND **unattended-upgrades** (this current role).
* Wait for upgrades
* If you're impatient, force them via running `unattended-upgrades` on the target host.

When upgrade is successful :

* In the Ansible inventory, move host from distro-codename group to distro-codename+1 group.
  for stretch to buster :


    ...
    [stretch]
    server0     <-- before
    server1
    server2

    [buster]
    server0     <-- after
    ...

  for stretch to stable :


    ...
    [stretch]
    server0     <-- before
    server1
    server2

    [stable]
    server0     <-- after
    ...
* Remove the **upgrade.yml** file.
* Apply roles **apt** (<a href="https://github.com/mbocquet/apt" target="new">https://github.com/mbocquet/apt</a>) AND **unattended-upgrades** (this current role).

* Merge new config files if needed (<a href="https://wiki.sekoya.org/#!apt.md#Configuration_files_handling" target="new">https://wiki.sekoya.org/#!apt.md#Configuration_files_handling</a>)

You're done !
