# Ansible role: tool.bootstrap_role

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
![Tag: Boilerplate](https://img.shields.io/badge/Tech-Boilerplate-orange)
![Tag: Bootstrap](https://img.shields.io/badge/Tech-Bootstrap-orange)
![Tag: Molecule](https://img.shields.io/badge/Tech-Molecule-orange)

An Ansible role to bootstrap and create other roles.

The Role Bootstrap role automates the creation of a basic structure for an Ansible role. It sets up the necessary folders, files, and configurations to jumpstart your role development process. With a focus on best practices, this role includes the following features:

Predefined Folder Structure: The role creates a well-organized folder structure, including defaults, files, handlers, meta, molecule, tasks, templates, tests, vars, and more. This ensures consistency and ease of maintenance throughout your role development.

Code Quality Tools: The role includes configuration files such as .ansible-lint, .ansible.cfg, and .yamllint to help maintain high-quality code and adhere to best practices.

Collaboration Support: The inclusion of a CODEOWNERS file enables easy collaboration and ownership assignment within your Git repository.

Molecule Integration: The role comes preconfigured with Molecule, a testing framework for Ansible roles. It includes default molecule scenarios and GitLab CI configuration for continuous integration.

By using the Role Bootstrap role, you can accelerate the creation of new Ansible roles, adhere to best practices, and focus on the core functionality of your roles.

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
---
# The current user and group to create file
bootstrap_role_user: "root"
# The base path on where you want your role
bootstrap_role_base_path: "/root"

# Your author name
bootstrap_role_meta_author: "Lord Robin Crombez"
# The namespace of your role
bootstrap_role_meta_namespace: "labocbz"
# The role name
bootstrap_role_meta_role_name: "my_new_role"
# The little description for the meta file and the readme
bootstrap_role_meta_description: "This is a limited description for the meta."
# Your compangy / lab name
bootstrap_role_meta_company: "Labo-CBZ"
# The licence you wanna put on it (MIT file imported)
bootstrap_role_meta_license: "MIT"
# Some tags you want to put in plus of the base one
bootstrap_role_tags:
  - "UNIX"

# The driver you use for molecule
bootstrap_role_molecule_driver: "docker"

# The path of your future role
bootstrap_role_path: "{{ bootstrap_role_base_path }}/{{ bootstrap_role_meta_namespace }}.{{ bootstrap_role_meta_role_name }}"

# A list of folder to create
bootstrap_role_folders:
  - "defaults"
  - "files"
  - "handlers"
  - "meta"
  - "molecule"
  - "molecule/default"
  - "molecule/gitlabci"
  - "tasks"
  - "templates"
  - "tests"
  - "tests/tower"
  - "tests/inventory"
  - "tests/inventory/group_vars"
  - "tests/inventory/host_vars"
  - "tests/certs"
  - "vars"

# Some file to import in the root folder of the future role
bootstrap_root_files:
  - ".ansible-lint"
  - ".ansible.cfg"
  - ".yamllint"
  - "CODEOWNERS"
  - ".gitlab-ci.yml"
```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_bootstrap_role_base_path: "/root"
inv_bootstrap_role_meta_role_name: "my_new_role"
inv_bootstrap_role_meta_namespace: "labocbz"
inv_bootstrap_role_technologies:
  - "UNIX"
  - "Ansible"
  - "Shell"
  - "PHP"

```

```YAML
# From AWX / Tower
---
all vars from to put/from AWX / Tower
```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include tool.bootstrap_role"
  tags:
    - "tool.bootstrap_role"
  vars:
    bootstrap_role_base_path: "{{ inv_bootstrap_role_base_path }}"
    bootstrap_role_meta_role_name: "{{ inv_bootstrap_role_meta_role_name }}"
    bootstrap_role_meta_namespace: "{{ inv_bootstrap_role_meta_namespace }}"
    bootstrap_role_technologies: "{{ inv_bootstrap_role_technologies }}"
  ansible.builtin.include_role:
    name: "tool.bootstrap_role"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-05-05: SSL/TLS Testing

* Addin a /certs folder in the test directory, to test the SSL/TLS if needed
* Import the bootstrap_ssl_files to create the certs is possible to

### 2023-05-30: Cryptographic update

* SSL/TLS Materials are not handled by the role
* Certs/CA have to be installed previously/after this role use

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
