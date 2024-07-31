Ansible Playbook for Pgpool-II
==============================

Ansible playbook that set up a PostgreSQL cluster with Pgpool-II.

Requirements
------------

The playbook is tested with the following software versions:

* VirtualBox 7.0.x
* Vagrant 2.4.x
* Vagrant box [rockylinux/9](https://app.vagrantup.com/rockylinux/boxes/9)
* Ansible 2.15.x

Usage
-----

To run the playbook:

```ShellSession
$ git clone https://github.com/tom-sato/ansible-pgpool2.git
$ cd ansible-pgpool2
$ vagrant up --provision
(snip)
PLAY RECAP *********************************************************************
node-1                     : ok=28   changed=19   unreachable=0    failed=0    skipped=9    rescued=0    ignored=0
node-2                     : ok=25   changed=15   unreachable=0    failed=0    skipped=9    rescued=0    ignored=0
node-3                     : ok=25   changed=16   unreachable=0    failed=0    skipped=9    rescued=0    ignored=0
node-4                     : ok=78   changed=50   unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
node-5                     : ok=45   changed=30   unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
node-6                     : ok=45   changed=30   unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

$ vagrant ssh node-1
$ sudo su - postgres
$ pcp_node_info -h vip-1
node-1 5432 1 0.333333 waiting up primary primary 0 none none 2024-07-31 23:26:42
node-2 5432 1 0.333333 waiting up standby standby 0 streaming async 2024-07-31 23:26:42
node-3 5432 1 0.333333 waiting up standby standby 0 streaming async 2024-07-31 23:26:42
$ pcp_watchdog_info -h vip-1
3 3 YES node-5:9999 Linux node-5.example.com node-5

node-5:9999 Linux node-5.example.com node-5 9999 9000 4 LEADER 0 MEMBER
node-4:9999 Linux node-4.example.com node-4 9999 9000 7 STANDBY 0 MEMBER
node-6:9999 Linux node-6.example.com node-6 9999 9000 7 STANDBY 0 MEMBER
```

By default, three PostgreSQL and three Pgpool-II are set up on
different hosts.

The hosts on which to set up PostgreSQL and Pgpool-II are specified by
the `groups` variable in `Vagrantfile`. It is also possible to set up
both PostgreSQL and Pgpool-II on the same host.

```Ruby:Vagrantfile
  groups = {
    "postgresql" => ["node-1", "node-2", "node-3"],
    "pgpool2" => ["node-4", "node-5", "node-6"]
  }
```

License
-------

BSD

Author Information
------------------

Tomoaki Sato
