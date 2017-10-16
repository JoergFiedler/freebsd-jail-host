[![Build Status](https://travis-ci.org/JoergFiedler/freebsd-jail-host.svg?branch=master)](https://travis-ci.org/JoergFiedler/freebsd-jail-host)

freebsd-jail-host
=================

This role is used to create a FreeBSD system which in turn may be used to host
one or more jails. There are roles for jails which may be used in combination
with this one to create a jailed www, db, or mail server. You may combine those
jails as you wish to create a server that may host a bunch of WordPress
installations,a single mail server, both, or anything else you want to run
inside a jail.

Requirements 
------------
 
This role is intent to be used with a fresh FreeBSD 11.1 installation. There is
a [Vagrant Box](https://app.vagrantup.com/JoergFiedler/boxes/freebsd-11.1/) with
providers for VirtualBox and AWS.

HowTo
=====

This project contains a `Vagrantfile`. Type

    vagrant up
    
and you will have a clean FreeBSD machine up and running. You may now create
jails manually or use one of the other roles I created.

Role Variables
--------------

### Network

##### host_net_ext_if

The servers external interface. Default: `'{{ ansible_default_ipv4.interface }}'`.

##### host_net_ext_ip

The servers external ip address: Default: `{{ ansible_default_ipv4.address }}'`.

##### host_net_int_if

The internal interface to which the jail's ip addresses will be added. Default: `lo0`.

##### host_net_int_ip

The servers internal ip address. This address is added to the internal interface
as well. Default: `10.1.0.1`.

##### host_net_int_net

The netmask for the jail's internal network. Used allow UDP pass pf in order to
reach syslogd. Default: `'10.1.0.1/24'`.

### Disk/ZFS/iocage

##### host_home_zpool_name

ZPool that should be used for `/home`. Default: `'tank'`.

##### host_ioc_release_version

The FreeBSD version fetched/used by iocage. Default: `11.1-RELEASE`.

##### host_ioc_zpool_name

The name of the ZFS pool that should be used by iocage. Default: `tank`.

##### host_ioc_zpool_devices

If the ZFS pool used for iocage (jails home) is to be created, this specifies a
space separated list of devices to use for the pool. There is no valid default.
You have to specify, if the ZFS pool does not exist already. Default: None.

##### host_srv_zpool_name

The name of the ZFS pool that should be used by `/srv` folder. Default: `tank`.

##### host_srv_zpool_devices

If the ZFS pool used for `/srv` folder is to be created, this specifies a
space separated list of devices to use for the pool. There is no valid default.
You have to specify, if the ZFS pool does not exist already. Default: None.

### SSH

##### host_sshd_authorized_keys_file

The file that contains the public keys used to authenticate the sshd user.
Defaults to vagrant insecure public key: `'vagrant_pub_key'`

##### host_sshd_port

The port sshd listens on. Default: `22`.

##### host_sshd_user

The user name allowed to access this server via ssh. Default: `vagrant`.

### SSMTP

This feature is only active, if the variable `use_ssmtp` is set.

##### ssmtp_auth_pass

The password which is used to perform SMTP AUTH. No authentication if blank.
Default: `''`.

##### ssmtp_auth_user

The user name which is used to authenticate against the SMTP server. No SMTP
AUTH if blank. Default: `''`.

##### ssmtp_mailhub

System mails are forwarded to this mail host. See [ssmtp man
page](https://www.freebsd.org/cgi/man.cgi?query=ssmtp&apropos=0&sektion=0&manpath=FreeBSD+10.2-RELEASE+and+Ports&arch=default&format=html)
for further information.

Default: `'mail.maildrop.cc'`.

##### ssmtp_rewrite_domain

The domain part of mails sent by ssmtp is rewritten using this variable. See
[ssmtp man
page](https://www.freebsd.org/cgi/man.cgi?query=ssmtp&apropos=0&sektion=0&manpath=FreeBSD+10.2-RELEASE+and+Ports&arch=default&format=html)
for further information.

Default: `'maildrop.cc'`.

##### ssmtp_root

System mails are forwarded to this account. See [ssmtp man
page](https://www.freebsd.org/cgi/man.cgi?query=ssmtp&apropos=0&sektion=0&manpath=FreeBSD+10.2-RELEASE+and+Ports&arch=default&format=html)
for further information.

Default: `'freebsd-jail-host'`.

##### ssmtp_use_starttls

Use STARTTLS before starting SSL negotiation. Default: `'no'`.

##### ssmtp_use_tls

Uses TLS when talking to SMTP server. Default: `'no'`.

### Tarsnap

##### tarsnap_enabled

Set this to true to use tarsnap for backup. Default: `false`.

##### tarsnap_keyfile

The keyfile to use to backup using tarsnap. See tarsnap documentation how to
create one. Default: None.


### Package Repository

##### host_build_server_enabled

Create an additional repository in `/usr/local/etc/pkg/repos/` using the URL and
public key provided by the following two variables. Default: `false`.

##### host_build_server_pubkey

The additional repositories public key used to verify downloaded packages.
Default: None.

##### host_build_server_url

The additional repositories URL. Default: None.


### Misc

##### host_timezone

The timezone the server is located. Default: `'Europe/Berlin'`.

Dependencies
------------

None.

Example Playbook
----------------

Playbook example with overridden defaults to use this role to setup a EC2 instance.

    ---
    - hosts: all
      become: true

      vars:
        host_build_server_url: 'http://vastland.moumantai.de/FreeBSD/packages/freebsd-11_1_x64-HEAD'

        ssmtp_auth_pass: 'mail.maildrop.cc'
        ssmtp_auth_user: 'mail.maildrop.cc'
        ssmtp_mailhub: 'mail.maildrop.cc'
        ssmtp_rewrite_domain: 'maildrop.cc'
        ssmtp_root: 'freebsd-ansible-demo'

        ansible_python_interpreter: '/usr/local/bin/python2.7'

      roles:
        - role: 'JoergFiedler.freebsd-jail-host'

Author Information
------------------

If you like it or do have ideas to improve this project, please open an issue on Github. Thanks.
