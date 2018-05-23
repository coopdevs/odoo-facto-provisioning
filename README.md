# Ansible script to provision and deploy Odoo for FACTO Assesors

These are [Ansible](http://docs.ansible.com/ansible/) playbooks (scripts) for managing an [Odoo](https://github.com/odoo/odoo) server.

## Requirements

You will need Ansible on your machine to run the playbooks.
These playbooks will install the PostgreSQL database, NodeJS and Python virtualenv to manage python packages.

It has currently been tested on **Ubuntu 16.04 Xenial (64 bit)**.


Install dependencies running:
```
ansible-galaxy install -r requirements.yml
```

## Playbooks

### sys_admins.yml

This playbook uses the community role [sys-admins-role](https://github.com/coopdevs/sys-admins-role).

This playbook will prepare the host to allow access to all the system administrators.

```yaml
# playbooks/my_playbook.yml
- name: Create all the users for system administration
  roles:
    - role: coopdevs.sys-admins-role
      vars:
        sys_admin_group: sysadmin
        sys_admins: "{{ system_administrators }}"
```

In each environment (`dev`, `staging`, `production`) we can find the list of users that will be created as system administrators.

We use `host_vars` to declare per environment variables:
```yaml
# inventory/host_vars/<YOUR_HOST>/config.yml

system_administrators:
  - name: pepe
    ssh_key: "../pub_keys/pepe.pub"
    state: present
    - name: paco
    ssh_key: "../pub_keys/paco.pub"
    state: present
```

The first time you run it against a brand new host you need to run it as `root` user.
You'll also need passwordless SSH access to the `root` user.
```
ansible-playbook playbooks/sys_admins.yml --limit=<environment_name> -u root
```

For the following executions, the script will asssume that your user is included in the system administrators list for the given host.

For example in the case of `development` environment the script will assume that the user that is running it is included in the system administrators [list](https://github.com/coopdevs/timeoverflow-provisioning/blob/master/inventory/host_vars/local.timeoverflow.org/config.yml#L5) for that environment.

To run the playbook as a system administrator just use the following command:
```
ansible-playbook playbooks/sys_admins.yml --limit=dev
```
Ansible will try to connect to the host using the system user. If your user as a system administrator is different than your local system user please run this playbook with the correct user using the `-u` flag.
```
ansible-playbook playbooks/sys_admins.yml --limit=dev -u <username>
```

### Provision
`provision.yml` - Installs and configures all required software on the server.

TASKS:
- Create odoo user
- Create directories structure
- Install common packages
- Install PostgreSQL database and create a user
- Install NodeJS and LESS
- Install Odoo


## DB Admin Password

Add password to the `super admin` that manage the dbs.
In `inventory/host_vars/host_name/secrets.yml` add the key `admin_passwd` to protect the creation and management of dbs.
`secrets.yml` is a encrypted vault. Run `ansible-vault edit secrets.yml` to change this password.

# Development

In the development environment (`local.odoo.net`) you must use the sysadmin user `odoo`.

`ssh odoo@local.odoo.net`

## Using LXC Containers

In order to create a development environment, we use the [devenv](https://github.com/coopdevs/devenv) tool. It uses [LXC](https://linuxcontainers.org/) to isolate the development environment.

This tool search a configuration dotfile in the project path. You must create a `.devenv`  file with the next declared variables:

```yaml
# odoo-provisioning/.devenv
NAME="odoo"
DISTRIBUTION="ubuntu"
RELEASE="xenial"
ARCH="amd64"
HOST="local.$NAME.coop"
PROJECT_NAME="odoo"
PROJECT_PATH="${PWD%/*}/$PROJECT_NAME"
DEVENV_USER="odoo"
DEVENV_GROUP="odoo"
```

You can modify them to change the environment.

Devenv will:

* Create container
* Mount your project directory into container in `/opt/<project_name>`
* Add container IP to `/etc/hosts`
* Create `odoo` group with same `gid` of project directory
* Create `odoo` user with same `uid` and `gid` of project directory
* Add system user's SSH public key to `odoo` user
* Install python2.7 in container
* Run `sys_admins.yml` playbook

When the execution ends, you have a container ready to execute sys_admins playbook and provision.
