Delphix Target Host
===================

[![Build Status](https://travis-ci.com/delphix/ansible-target-host.svg?branch=master)](https://travis-ci.com/delphix/ansible-target-host)

#### Table of Contents
1.  [Overview](#overview)
    *   [Role Variables](#variables)
    *   [Dependencies](#dependencies)
    *   [Example Playbook](#playbook)
2.  [Contribute](#contribute)
    *   [Code of conduct](#code-of-conduct)
    *   [Community Guidelines](#community-guidelines)
    *   [Contributor Agreement](#contributor-agreement)
3.  [Reporting Issues](#reporting-issues)
4.  [Statement of Support](#statement-of-support)
5.  [License](#license)

## <a id="overview"></a>Overview

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

This role is automatically tested on a weekly basis for the following OS:
* CentOS6
* CentOS7
* Ubuntu 18.04
* Ubuntu 16.04
* Debian 9
* Debian 8

## <a id="variables"></a>Role Variables

Available variables are listed below, along with default values (see defaults/main.yml).
There is also a sample group_vars directory with common settings for specific target types, like oracletargets.

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
The list of packages to be installed. This defaults to a set of platform-specific packages for RedHat or Debian-based
systems (see `vars/RedHat.yml` and `vars/Debian.yml` for the default values).

    install_oracle: false
Set to "true" to perform additional tasks which allow the Delphix Target to provision Oracle VDBs

## <a id="dependencies"></a>Dependencies

None

## <a id="playbook"></a>Example Playbook

    - hosts: servers
      roles:
         - { role: delphix.target-host, delphix_toolkit: /toolkit }

## <a id="contribute"></a>Contribute

1.  Fork the project.
2.  Make your bug fix or new feature.
3.  Add tests for your code.
4.  Send a pull request.

Contributions must be signed as `User Name <user@email.com>`. Make sure to [set up Git with user name and email
address](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup). Bug fixes should branch from the current
stable branch. New features should be based on the `master` branch.

#### <a id="code-of-conduct"></a>Code of Conduct

This project operates under the [Delphix Code of Conduct](https://delphix.github.io/code-of-conduct.html). By
participating in this project you agree to abide by its terms.

#### <a id="contributor-agreement"></a>Contributor Agreement

All contributors are required to sign the Delphix Contributor agreement prior to contributing code to an open source
repository. This process is handled automatically by [cla-assistant](https://cla-assistant.io/). Simply open a pull
request and a bot will automatically check to see if you have signed the latest agreement. If not, you will be prompted
to do so as part of the pull request process.


## <a id="reporting_issues"></a>Reporting Issues

Issues should be reported [here](https://github.com/delphix/ansible-target-host/issues).

## <a id="statement-of-support"></a>Statement of Support

This software is provided as-is, without warranty of any kind or commercial support through Delphix. See the associated
license for additional details. Questions, issues, feature requests, and contributions should be directed to the
community as outlined in the [Delphix Community Guidelines](https://delphix.github.io/community-guidelines.html).

## <a id="license"></a>License

This is code is licensed under the Apache License 2.0. Full license is available [here](./LICENSE).

