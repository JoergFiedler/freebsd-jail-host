freebsd-jail-host
=========

This role is used to create a FreeBSD system which in turn may be used to host one or more jails.
There are roles for jails which may be used in combination with this one to create a jailed www, db, or mail server. You may combine those jails as you wish to create a server that may host a bunch of wordpress installations,a single mail server, or both.

See [my other project](https://github.com/JoergFiedler/freebsd-ansible-demo) for further information and how to use it.

Requirements
------------

This role is intent to be used with a fresh FreeBSD 10.2 install with some minor modifications. There is a Vagrant Box with providers for VirtualBox and EC2 you may use.

You will find a sample project which uses this role [here](https://github.com/JoergFiedler/freebsd-ansible-demo).

Role Variables
--------------

##### host_net_int_if

The internal interface to which the jail's ip addresses will be added. Default: `lo0`.

##### host_net_int_ip

The servers internal ip address. This address is added to the internal interface as well. Default: `10.1.0.1`.

##### host_net_int_net

The mask for the jail's internal network. Used allow UDP pass pf in order to reach syslogd. Default: `'10.1.0.1/24'`.

##### host_net_ext_if

The servers external interface. Default: `'{{ ansible_default_ipv4.interface }}'`.

##### host_net_ext_ip

The servers external ip address: Default: `{{ ansible_default_ipv4.address }}'`.

##### host_sshd_user

The user name allowed to access this server via ssh. Default: `vagrant`.

##### host_sshd_port

The port sshd listens on. Default: `22`.

##### host_sshd_pub_key

The user's public key used for ssh authentication. Defaults to vagrant insecure public key: `'ssh-rsa AAAAB3NzaC1yc2E…'`

##### host_ioc_zpool_name

The name of the ZFS pool that should be used by iocage. Default: `tank`.

##### host_ioc_zpool_devices

If the ZFS pool is to be created, this specifies a space sparated list of devices to use for the pool. There is no valid default. You have to specify, if the ZFS pool does not exist already. No default value.

##### host_ioc_release_version

The FreeBSD version fetched/used by iocage. Default: `10.2-RELEASE`.

##### ssmtp_forward_address

System mails are forwarded to this address. See [ssmtp man page](https://www.freebsd.org/cgi/man.cgi?query=ssmtp&apropos=0&sektion=0&manpath=FreeBSD+10.2-RELEASE+and+Ports&arch=default&format=html) for further information.

Default: `'freebsd-ansible-demo@maildrop.cc'`.

This feature is only active, if the variable `use_ssmtp` is set to any value.

##### ssmtp_forward_mailhub

System mails are forwarded using this mail relay. See [ssmtp man page](https://www.freebsd.org/cgi/man.cgi?query=ssmtp&apropos=0&sektion=0&manpath=FreeBSD+10.2-RELEASE+and+Ports&arch=default&format=html) for further information.

Default: `'mail.maildrop.cc'`.

This feature is only active, if the variable `use_ssmtp` is set to any value.

##### ssmtp_auth_user
The user name which is used to authenticate against the SMTP server. No SMTP AUTH if blank. Default: `''`.

This feature is only active, if the variable `use_ssmtp` is set to any value.
##### ssmtp_auth_pass
The password which is used to perform SMTP AUTH. No authentication if blank. Default: `''`.
This feature is only active, if the variable `use_ssmtp` is set to any value.
##### ssmtp_use_starttls
Use STARTTLS before starting SSL negotiation. Default: `'no'`.

This feature is only active, if the variable `use_ssmtp` is set to any value.
##### ssmtp_use_tls
Uses TLS when talking to SMTP server. Default: `'no'`.

This feature is only active, if the variable `use_ssmtp` is set to any value.

Dependencies
------------

None.

Example Playbook
----------------

Playbook example with overriden defaults to use this role to setup a EC2 instance.

    - hosts: servers
      vars:
      - host_ssh_user: 'ec2-user'
      - host_ioc_devices: 'xbd5'

      roles:
      - { role: 'JoergFiedler.freebsd-jail-host' }

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue on Github. Thanks.
