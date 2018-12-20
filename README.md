Delphix Target Host
===================

This role will configure a Linux system for use as a target host in the Delphix
platform. This includes installing all required packages, and creating a
`delphix` user with sufficient sudo privileges support all platform operations,
most notably managing NFS mounts.  The resulting host can be used with a
standard username, directories, and SSH key access.

The role provides a mechanism for configuring the `delphix` user with a single
engine public SSH key in `/home/delphix/.ssh/authorized_keys`. In the event
that you are building a cloud image and want to configure the SSH key at
runtime, you can use cloud init (as described for AWS
[here](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)) to
append one or more SSH keys to the `authorized_keys` file on first boot. To get
the public SSH key of an engine, use the `system get sshPublicKey` CLI command.

This role has been manually tested against latest Ubuntu and CentOS AMIs, but
there is no reason it should not work with any RedHat or Debian variant.

Role Variables
--------------

Available variables are listed below, along with default values (see defaults/main.yml):

    delphix_user: delphix
The user for the Delphix DDP to login to the system

    user_comment: "Delphix Automation"
Comment for the user

    delphix_group: delphix
The group to which delphix_user should belong

    delphix_home: "/home/{{ delphix_user }}"
The home of the delphix_user

    delphix_mount: /mnt/delphix
The directory where the Delphix DDP should mount the VDBs

    delphix_toolkit: /home/delphix/toolkit
The directory where the Delphix DDP will store the toolkit files

    delphix_ssh_keys: {}
Optional: The SSH key of the Delphix DDP

Here's an example using delphix_ssh_keys to add a Delphix DDP public key to a host:

    delphix_ssh_keys:
      - name: DE1
        key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDYCptAFSoJwnHVgCX5xC2c5W9unRbFen5PvUasQ8tnlouNRte5hnKe10bgkMFN4LSUbXggXk1C+9eMMKvpKxifFFLVTNCtKGyBsrUpmKLWAoQU5ycpXFfGmgtASJn+VZh5VYbCKTcvVsadR8UA7PyOU6V5gayELKZBczw7gvpaGyM4cF4/1VqK7UzbnEXe0Rt3xozYMC+xyRSk+RZTh+grHYHb1Q/0cSsQmaw1sTlIFl8BAjXNMEmPoCISfSpO4F3yWmJUIEPtkWHWiKOUzgoi20vnvZnLHd54NAE8F+aJn4eNarAn0x2XWUdmgLuAM/7KzF5kn0+k9Pm3iJ1jE14J root@ip-10-0-1-10"
Define each ssh_key using a name:key value pair

    allowed_sudo_commands: 
      - /bin/mount
      - /bin/umount
      - /bin/ps
      - /bin/mkdir
      - /bin/rmdir
Allowed sudo commands for the delphix_user

    packages:
      - [platform-specific]
The list of packages to be installed. This defaults to a set of platform-specific packages for RedHat or Debian-based systems (see vars/RedHat.yml and vars/Debian.yml for the default values).

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: delphix.target-host, delphix_toolkit: /toolkit }

License
-------

Apache 2.0

