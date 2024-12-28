# PostgreSQL Cluster Role for Master-Replica Setup

This role allows you to set up a PostgreSQL cluster with one master and one replica node. It supports configuring PostgreSQL, managing databases and users, changing the data directory, and setting up streaming replication.

## Table of Contents

- [Requirements](#requirements)
- [Role Variables](#role-variables)
- [Example Inventory](#example-inventory)
- [How to Use](#how-to-use)
  - [Setting Up the Master Node](#setting-up-the-master-node)
  - [Setting Up the Replica Node](#setting-up-the-replica-node)
- [Testing with Molecule](#testing-with-molecule)
- [License](#license)
- [Author Information](#author-information)

## Requirements

- Ansible 2.9+
- Ubuntu or Debian-based systems
- Python psycopg2 library for database management tasks

## Role Variables

Here are some key variables used in this role:

| Variable | Description | Default |
|----------|-------------|---------|
| postgresql_version | PostgreSQL version to install | 15 |
| postgresql_role | Define the node type: master or replica | master |
| postgresql_data_dir | Default data directory for PostgreSQL | /var/lib/postgresql/{{ postgresql_version }} |
| postgresql_new_data_dir | New data directory, if changing the default | /tmp/postgresql |
| postgresql_users | List of users to create | See defaults/main.yml |
| postgresql_databases | List of databases to create | See defaults/main.yml |

For a full list of variables, refer to defaults/main.yml.

## Example Inventory

Define your master and replica nodes in the inventory file as follows:


[master]
master-node ansible_host=192.168.56.101

[replica]
replica-node ansible_host=192.168.56.102


## How to Use

### Setting Up the Master Node

1. Assign the master role to your master node in the inventory.
2. Run the following playbook:


- name: Setup Master Node
  hosts: master
  become: true
  roles:
    - role: your_postgresql_role
      vars:
        postgresql_role: master


### Setting Up the Replica Node

1. Assign the replica role to your replica node in the inventory.
2. Run the following playbook:


- name: Setup Replica Node
  hosts: replica
  become: true
  roles:
    - role: your_postgresql_role
      vars:
        postgresql_role: replica


### Additional Steps for Streaming Replication

1. Ensure the following variables are configured correctly:
   - postgresql_postgres_password for both nodes.
   - The postgresql_replication_hba_method set to md5 or trust.
2. For the master node, configure replication settings in postgresql.conf and pg_hba.conf. This is automated by the role.
3. The replica node will use pg_basebackup to clone the master node. Ensure the master is reachable over the network.

## Testing with Molecule

This role includes Molecule tests for validation.

1. Install Molecule:
   
   pip install molecule ansible-lint docker
   
2. Run the tests:
   
   molecule test
   

## License

This role is licensed under [MIT License](LICENSE).

## Author Information

This role was created by Viktoriia and Polina at ITMO University.
