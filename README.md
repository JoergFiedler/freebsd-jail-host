Role Name
=========

This role is used to create a FreeBSD system which in turn may be used to host one or more jails.
There are roles for jails which may be used as a www, db, or mail server. You may combine those jails as you wish to create a server that may host a bunch of wordpress installations,a single mail server, or both.

See [my other project](https://github.com/JoergFiedler/freebsd-ansible-demo) for further information and how to use it.

Requirements
------------

This role is intent to be used with a fresh FreeBSD 10.2 install with some minor modifications. There is a Vagrant Box with providers for VirtualBox and EC2 you may use.

You will find a sample project which uses this role [here](https://github.com/JoergFiedler/freebsd-ansible-demo).

Role Variables
--------------

### jh_net_int_if

The internal interface to which the jail's ip addresses will be added. Default: `lo0`.

### jh_net_int_ip

The hosts internal ip address. This address is added to the internal interface as well. Default: `10.1.0.1`.

### jh_net_ext_if

The servers external interface. Default: `vtnet0`.

### jh_net_ext_ip

The servers external ip address: Default: `10.0.2.15`.

### jh_ssh_user

The user name allowed to access this server via ssh. Default: `vagrant`.

### jh_ssh_port

The port sshd listens on. Default: `22`.

### jh_ioc_zpool_name

The name of the ZFS pool that should be used by iocage. Default: `tank`.

### jh_ioc_zpool_devices

If the ZFS pool is to be created, this specifies a space sparated list of devices to use for the pool. There is no valid default. You have to specify, if the ZFS pool does not exist already. Default: ``.

### jh_ioc_dir

The base directory for iocage. Default: `/ioc`.

### jh_ioc_releases_dir

The directory for FreeBSD releases used by iocage: Default: `{{ ioc_dir }}/releases`.

### jh_ioc_jails_dir

The directory the jails will resist in. Default: `{{ ioc_dir }}/jails`.

### jh_ioc_release_version

The FreeBSD version fetched/used by iocage. Default: `10.2-RELEASE`.:w!


A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles::
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue on Github. Thanks.
