zabbix_web
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-zabbix_web.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-zabbix_web)

Install and configure zabbix_web on your system.

Example Playbook
----------------

This example is taken from `molecule/default/playbook.yml`:
```yaml
---
- name: Converge
  hosts: all
  gather_facts: false
  become: true

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.buildtools
    - role: robertdebock.python_pip
    - role: robertdebock.zabbix_repository
    - role: robertdebock.zabbix_web

```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

Role Variables
--------------

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for zabbix_web

# Details to connect to the database.
zabbix_web_database_host: localhost
zabbix_web_database_port: 3306
zabbix_web_database_name: zabbix
zabbix_web_database_user: zabbix
zabbix_web_database_pass: zabbix

# Details to connect to Zabbix.
zabbix_web_server: https://localhost/zabbix
zabbix_web_server_port: 10051
zabbix_web_server_name: zabbix

# Details to connect to the Zabbix API.
zabbix_web_username: Admin
zabbix_web_password: zabbix
zabbix_web_validate_certs: no

# You can provision Zabbix groups.
# Most options map directly to the documentation:
# https://docs.ansible.com/ansible/latest/modules/zabbix_group_module.html
zabbix_web_groups:
  - name: Linux servers

# Add hosts to Zabbix.
# Most options map directly to the documentation:
# https://docs.ansible.com/ansible/latest/modules/zabbix_host_module.html
zabbix_web_hosts:
  - name: Example server 1
    interface_ip: 192.168.127.127
    interface_dns: server1.example.com
    visible_name: Example server 1 name
    description: Example server 1 description
    groups:
      - Linux servers
    link_templates:
      - Template OS Linux

# To update packages this role places on the system, set `zabbix_web_package_state` to `latest`.
zabbix_web_package_state: present

# Some Docker containers do not allow managing services, rebooting and writing
# to some locations in /etc. The role skips tasks that will typically fail in
# Docker. With this parameter you can tell the role to -not- skip these tasks.
zabbix_web_ignore_docker: yes

```

Requirements
------------

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the last 3 release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.epel
- robertdebock.buildtools
- robertdebock.python_pip
- robertdebock.zabbix_repository

```

Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/zabbix_web.png "Dependency")


Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.6|ansible 2.7|ansible devel|
|------------|-----------|-----------|-------------|
|alpine-edge*|no|no|no*|
|alpine-latest|no|no|no*|
|archlinux|no|no|no*|
|centos-6|no|no|no*|
|centos-latest|yes|yes|yes*|
|debian-latest|yes|yes|yes*|
|debian-stable|yes|yes|yes*|
|debian-unstable*|yes|yes|yes*|
|fedora-latest|no|no|no*|
|fedora-rawhide*|no|no|no*|
|opensuse-leap|no|no|no*|
|opensuse-tumbleweed|no|no|no*|
|ubuntu-artful|no|no|no*|
|ubuntu-devel*|yes|yes|yes*|
|ubuntu-latest|yes|yes|yes*|

A single star means the build may fail, it's marked as an experimental build.

Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-zabbix_web) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-zabbix_web/issues)

To test this role locally please use [Molecule](https://github.com/metacloud/molecule):
```
pip install molecule
molecule test
```

To test on Amazon EC2, configure [~/.aws/credentials](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html) and `export AWS_REGION=eu-central-1` before running `molecule test --scenario-name ec2`.

There are many specific scenarios available, please have a look in the `molecule/` directory.

Run the [ansible-galaxy](https://github.com/ansible/galaxy-lint-rules) and [my](https://github.com/robertdebock/ansible-lint-rules) lint rules if you want your change to be merges:

```shell
git clone https://github.com/ansible/ansible-lint.git /tmp/ansible-lint
ansible-lint -r /tmp/ansible-lint/lib/ansiblelint/rules .

git clone https://github.com/robertdebock/ansible-lint /tmp/my-ansible-lint
ansible-lint -r /tmp/my-ansible-lint/rules .
```

License
-------

Apache-2.0


Author Information
------------------

[Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
