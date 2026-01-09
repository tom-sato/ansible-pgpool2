Ansible Playbook for Pgpool-II
==============================

Ansible playbook that set up a PostgreSQL cluster with Pgpool-II.

Requirements
------------

The playbook is tested with the following software versions:

* VirtualBox 7.1.x
* Vagrant 2.4.x
* Vagrant box
  * [centos/7](https://app.vagrantup.com/centos/boxes/7)
  * [rockylinux/8](https://app.vagrantup.com/rockylinux/boxes/8)
  * [rockylinux/9](https://app.vagrantup.com/rockylinux/boxes/9)
* Ansible 2.16.x
* PostgreSQL 9.2.x - 18.x
* Pgpool-II 3.3.x - 4.7.x

Usage
-----

To run the playbook:

```ShellSession
$ git clone https://github.com/tom-sato/ansible-pgpool2.git
$ cd ansible-pgpool2
$ vagrant up --provision
(snip)
PLAY RECAP *********************************************************************
node-1                     : ok=110  changed=48   unreachable=0    failed=0    skipped=26   rescued=0    ignored=0
node-2                     : ok=75   changed=38   unreachable=0    failed=0    skipped=26   rescued=0    ignored=0
node-3                     : ok=75   changed=37   unreachable=0    failed=0    skipped=26   rescued=0    ignored=0

$ vagrant ssh node-1
$ sudo su - postgres
$ pcp_node_info -h vip-1
node-1 5432 1 0.333333 waiting up primary primary 0 none none 2025-03-07 22:01:50
node-2 5432 1 0.333333 waiting up standby standby 0 streaming async 2025-03-07 22:03:54
node-3 5432 1 0.333333 waiting up standby standby 0 streaming async 2025-03-07 22:03:03
$ pcp_watchdog_info -h vip-1
3 3 YES node-1:9999 Linux node-1.example.com node-1

node-1:9999 Linux node-1.example.com node-1 9999 9000 4 LEADER 0 MEMBER
node-2:9999 Linux node-2.example.com node-2 9999 9000 7 STANDBY 0 MEMBER
node-3:9999 Linux node-3.example.com node-3 9999 9000 7 STANDBY 0 MEMBER
```

By default, PostgreSQL and Pgpool-II are set up on all three hosts.

The hosts on which PostgreSQL and Pgpool-II are set up are specified by
the `groups` variable in `Vagrantfile`. It is also possible to set up
PostgreSQL and Pgpool-II on separate hosts.

```Ruby:Vagrantfile
  groups = {
    "postgresql" => ["node-1", "node-2", "node-3"],
    "pgpool2" => ["node-1", "node-2", "node-3"]
  }
```

License
-------

BSD

Author Information
------------------

Tomoaki Sato
