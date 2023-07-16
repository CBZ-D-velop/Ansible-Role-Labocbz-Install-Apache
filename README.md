# Ansible role: labocbz.install_apache

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Apache2](https://img.shields.io/badge/Tech-Apache2-orange)
![Tag: HTTPD](https://img.shields.io/badge/Tech-HTTPD-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL/TLS-orange)
![Tag: Modsecurity](https://img.shields.io/badge/Tech-Modsecurity-orange)
![Tag: QOS](https://img.shields.io/badge/Tech-QOS-orange)
![Tag: Deflate](https://img.shields.io/badge/Tech-Deflate-orange)
![Tag: Proxy/ReverseProxy](https://img.shields.io/badge/Tech-Proxy/ReverseProxy-orange)
![Tag: OWASP](https://img.shields.io/badge/Tech-OWASP-orange)
![Tag: Certbot](https://img.shields.io/badge/Tech-Certbot-orange)

An Ansible role to install and configure Apache2 HTTP on your host.

The Ansible role installs Apache2 HTTPD and configures the server along with specific modules based on customizable parameters. Administrators can customize the Apache installation by selecting the desired modules through the role's options.

The role offers the flexibility to enable or disable default virtual hosts and provides the option to rename these virtual hosts to include incremental values. This ensures appropriate virtual host selection in cases where the domain does not match explicitly.

To optimize server performance and enhance security, the role also includes the Mod Qos module, offering a comprehensive solution for performance and security-related challenges. Additionally, administrators can enable the ModSecurity module, a Web Application Firewall (WAF) component, through a Git repository. This further fortifies the server's security capabilities while maintaining performance efficiency.

Furthermore, the role supports the removal of all non-default virtual hosts, providing a clean slate for adding new configurations. This streamlines virtual host management and promotes a more organized server environment.

In summary, the Apache2 HTTPD role simplifies the installation and configuration of Apache2, allowing administrators to tailor the installation by selecting specific modules. With the option to enable Mod Qos, enable ModSecurity via Git, and manage virtual hosts efficiently, the role provides a powerful solution for optimizing Apache2 performance and prioritizing security aspects.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
apache_https_listen_port: 443
apache_http_listen_port: 80

apache_modules:
  - "ssl"
  - "rewrite"
  - "filter"
  - "headers"

apache_enable_qos: false
apache_qos_client_entries: "100000"
apache_qos_srv_max_conn_per_ip: "50"
apache_qos_max_clients: "256"
apache_qos_srv_max_conn_close: "180"
apache_qos_srv_min_data_rate: "150 1200"

apache_enable_default_vhosts: true
apache_remove_all_no_default_vhosts: true

apache_enable_security: false
apache_security_core_version: "3.3.0"

apache_ssl_files_path: "/etc/letsencrypt/live"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_apache_https_listen_port: 443
inv_apache_http_listen_port: 80

inv_apache_modules:
  - "ssl"
  - "rewrite"
  - "proxy"
  - "proxy_http"
  - "ldap"
  - "authnz_ldap"
  - "filter"
  - "deflate"
  - "headers"
  - "proxy_wstunnel"

inv_apache_enable_qos: true
inv_apache_qos_client_entries: "100000"
inv_apache_qos_srv_max_conn_per_ip: "50"
inv_apache_qos_max_clients: "256"
inv_apache_qos_srv_max_conn_close: "180"
inv_apache_qos_srv_min_data_rate: "150 1200"

inv_apache_enable_security: true
inv_apache_enable_default_vhosts: false

```

```YAML
# From AWX / Tower
---
tower_apache_security_core_version: "3.3.0"
tower_apache_enable_default_vhosts: true
tower_apache_remove_all_no_default_vhosts: true

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_apache"
  tags:
    - "labocbz.install_apache"
  vars:
    apache_https_listen_port: "{{ inv_apache_https_listen_port }}"
    apache_http_listen_port: "{{ inv_apache_http_listen_port }}"
    apache_modules: "{{ inv_apache_modules }}"
    apache_enable_qos: "{{ inv_apache_enable_qos }}"
    apache_qos_client_entries: "{{ inv_apache_qos_client_entries }}"
    apache_qos_srv_max_conn_per_ip: "{{ inv_apache_qos_srv_max_conn_per_ip }}"
    apache_qos_max_clients: "{{ inv_apache_qos_max_clients }}"
    apache_qos_srv_max_conn_close: "{{ inv_apache_qos_srv_max_conn_close }}"
    apache_qos_srv_min_data_rate: "{{ inv_apache_qos_srv_min_data_rate }}"
    apache_enable_security: "{{ inv_apache_enable_security }}"
    apache_security_core_version: "{{ tower_apache_security_core_version }}"
    apache_enable_default_vhosts: "{{ tower_apache_enable_default_vhosts }}"
    apache_remove_all_no_default_vhosts: "{{ tower_apache_remove_all_no_default_vhosts }}"
  ansible.builtin.include_role:
    name: "labocbz.install_apache"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-28: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez
* Remove all previous conf, ssl and errors if we have to run the install playbook again because: "In general, it is a good practice to only add configurations when they are needed, as this minimizes the chance of errors or conflicts. In your case, it makes sense to install Apache and configure it, but not add any vhosts until they are needed by the app. However, you should also consider how to handle the scenario where you need to reinstall Apache. If you reinstall Apache without cleaning up the old vhosts, you may end up with conflicting configurations, which could cause issues with your app or even your entire system." ChatGPT.

### 2023-04-18: Defaults vhosts

* All vhosts are disabled on install, because you have to add any conf after the install.
* If you perform a reinstall, you can call the add_apache_confs to re import removed conf.
* Default vhost (HTTP/HTTPS) can be enabled or disabled on install, in order to have a minimal blach hole for all unmatched domains.

### 2023-04-19: Clean others vhosts

* You can now enable vhost cleaning (removing from available + enabled)

### 2023-05-30: Cryptographic update

* SSL/TLS Materials are not handled by the role
* Certs/CA have to be installed previously/after this role use

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
