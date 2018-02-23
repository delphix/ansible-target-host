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

The following role variables can be configured:

    delphix_user: delphix
    delphix_group: delphix
    delphix_mount: /mnt/delphix
    delphix_toolkit: /home/delphix/toolkit
    delphix_ssh_key:

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

