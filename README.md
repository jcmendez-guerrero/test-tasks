# Test-Tasks

This project is a example repo for creating a cluster with MariaDB, Redis and RabbitMQ

## PROJECT STRUCTURE

```text

├── README.md
├── Vagrantfile
├── artifacts.yml
├── constants-maria.yml
├── constants-rabbit.yml
├── constants.yml
├── group_vars
│   └── all
├── install-mariadb.yml
├── install-rabbitmq.yml
├── install.yml
├── inventory.yml
├── main.yml
├── roles
│   ├── add-firewall-services
│   │   └── tasks
│   │       └── main.yml
│   ├── pip-installer
│   │   └── tasks
│   │       └── main.yml
│   └── yum-installer
│       └── tasks
│           └── main.yml
└── templates
    ├── ctl.j2
    ├── firewalld
    │   └── firewalld_service.j2
    ├── mariadb
    │   ├── Dockerfile.j2
    │   ├── docker-entrypoint.sh
    │   ├── new_cluster.cnf.j2
    │   ├── postinstall.sql.j2
    │   ├── site.cnf.j2
    │   └── wsrep.cnf.j2
    ├── rabbitmq
    │   ├── Dockerfile.j2
    │   ├── advanced.conf.j2
    │   ├── docker-entrypoint.sh
    │   └── rabbitmq.conf.j2
    ├── redis
    │   └── replica.conf.j2
    └── redis-sentinel
        └── sentinel.conf.j2

```

### Run Vagrant

Vagrantfile includes the configuration to create 4 machines

1. Control: Machine with ansible capable to connect to all machines and with ansible installed to provision the machines
2. MachineX: All role machines that will be installed as part of the cluster. This machines can´t communicate via ssh without password. To access them use the control machine or the `vagrant ssh`

In order to run this it is required to previouslly install `vagrant-hostmanager`

```bash
vagrant plugin install vagrant-hostmanager
```

All machines follow hostname namming convention

`<COUNTRY><CITY><UNIQUE_CODE><ROLE><SLA><ID>`

1. ESBCNACSPROD001: SPAIN BARCELONA A CONTROL_SERVER PROD 0001
2. ESBCNAASPROD00X: SPAIN BARCELONA A APP_SERVER PROD 00X

This has used VirtualBox as default provisioner

In order to provision the machines run:

`vagrant up`

#### System Requirements

It is recommended to have at least 8GB Memory and 4cores CPU
This project requires Virtualization capabilities 

### ANSIBLE STRUCTURE

This are the following definitions

1. global_vars: Variables considered global and not possible to change
2. roles
    1. add-firewall-services: Used to provision firewalld
    2. pip-installer: used to provision pip packages
    3. yum-installer: used to provision RPM based packages
3. templates: files that can be used for configuration which require variables previouslly defined
4. artifacts.yml: list of different artifacts to provision (Docker, pip, yum)
5. constants-maria.yml: currently has the firewall template for MariaDB
6. constants-rabbit.yml: currently has the firewall template for Rabbit
7. constants.yml: currently has the firewall template for Redis
8. install-X.yml: Install tasks for different roles
9. inventory.yml: Inventory of hosts as well as the variables that can be modified
10. main.yml: main package install

All the files are available in control machine from `/vagrant`

#### RUNNING PLAYBOOKS

In order to install everything connect to control `vagrant ssh control` go to `/vagrant` and run

```bash
ansible-laybook -i inventory.yml main.yml
```

in case you want a single role run the specific playbook

#### Variables

Please check service.properties.yaml for more information.
