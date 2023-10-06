
# Ansible playbook: tool.bootstrap_ssl_files

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
![Tag: OpenSSL](https://img.shields.io/badge/Tech-OpenSSL-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: KEY](https://img.shields.io/badge/Tech-KEY-orange)
![Tag: REQ](https://img.shields.io/badge/Tech-REQ-orange)
![Tag: CSR](https://img.shields.io/badge/Tech-CSR-orange)
![Tag: CRT](https://img.shields.io/badge/Tech-CRT-orange)
![Tag: PEM](https://img.shields.io/badge/Tech-PEM-orange)
![Tag: CA](https://img.shields.io/badge/Tech-CA-orange)

An Ansible playbook create a list of defined files related to SSL/TLS

The Certificate Management playbook automates the creation and organization of certificates, including Certificate Authorities (CAs), root CAs, intermediate CAs, and end certificates. This playbook simplifies the process of generating certificates and provides a streamlined approach for managing SSL/TLS infrastructure. Key features of this playbook include:

Certificate Creation: The playbook generates various types of certificates, including root CAs, intermediate CAs, and end certificates. It allows you to define the desired SSL/TLS information, such as Common Name (CN), Country (C), State (ST), Locality (L), Organization (O), Organizational Unit (OU), and email address.

Certificate Filing: The playbook organizes the generated certificates into separate components and creates a ZIP archive for each component. This makes it easy to deploy and distribute the certificates to the desired target systems.

Customization Options: The playbook provides flexibility by allowing you to specify the validity period, key size, and base path for storing the certificate files. You can customize these parameters to meet your specific requirements.

By utilizing the Certificate Management playbook, you can simplify the creation and management of SSL/TLS certificates, ensure proper organization and filing of certificates, and streamline the deployment process.

## Deployment diagramm

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

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the playbook) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This playbook contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your playbook
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

To install this playbook, just copy/import this playbook or raw file into your fresh playbook repository or call it with the "include_playbook/import_playbook" module.

## Usage

### Vars

```YAML
# From inventory
---

```

```YAML
# From AWX / Tower
---
# The current user and group to create files
input_bootstrap_ssl_files_user: "root"
# The base path on where you want your certs files
input_bootstrap_ssl_files_base_path: "/tmp/ssl/MyPKI"
# The validity of your certs files
input_bootstrap_ssl_files_ca_validity: 3650
input_bootstrap_ssl_files_cert_validity: 90
# The size of your key
input_bootstrap_ssl_files_key_size: 4096
# Files wanted and where you want them

# SSL/TLS informations
input_bootstrap_ssl_files_root_ca:
  cn: "My Local Ansible Root CA"
  c: "FR"
  st: "state"
  l: "city"
  o: "Local Ansible"
  ou: "My Local Ansible Root CA"
  email_address: "contact@your.domain.tld"
  password: "m3EH3A56h5mNY"

input_bootstrap_ssl_files_intermediates_ca:
  - cn: "My Local Ansible Intermediate CA 1"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "My Local Ansible Intermediate CA 1"
    email_address: "contact@your.domain.tld"
    password: "m3EH3A56h5mNY"
    certification_ca: "My Local Ansible Root CA"

  - cn: "My Local Ansible Intermediate CA 2"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "My Local Ansible Intermediate CA 2"
    email_address: "contact@your.domain.tld"
    password: "m3EH3A56*ùph5mNY"
    certification_ca: "My Local Ansible Intermediate CA 1"

input_bootstrap_ssl_files_end_certs:
  - cn: "my-end-certificate-1.domain.tld"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "Local Dev IT"
    email_address: "contact@your.domain.tld"
    alternatives:
      - "127.0.0.1"
      - "localhost"
      - "my-website.tld"
    certification_ca: "My Local Ansible Intermediate CA 1"

  - cn: "my-end-certificate-2.domain.tld"
    c: "FR"
    st: "state"
    l: "city"
    o: "My Local Intermediate CA Two"
    ou: "Local Dev IT"
    email_address: "contact@your.domain.tld"
    alternatives:
      - "127.0.0.1"
      - "localhost"
      - "my-website.tld"
    certification_ca: "My Local Ansible Intermediate CA 2"

  - cn: "my-end-certificate-3.domain.tld"
    c: "FR"
    st: "state"
    l: "city"
    o: "Local Ansible"
    ou: "Local Dev IT"
    email_address: "contact@your.domain.tld"
    alternatives:
      - "127.0.0.1"
      - "localhost"
      - "my-website.tld"
    certification_ca: "My Local Ansible Intermediate CA 2"

```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-07-24: First Init

* First init of this playbook with the bootstrap_playbook playbook by Lord Robin Crombez

### 2023-07-26: Crypto update

* Prepare files are now compatible witht he bootstrap_ssl_files v2.0

### 2023-08-06: Bug / Reminders fix

* Fix some bug / incorrect comportment

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

## Authors

* Lord Robin Crombez

## Sources

* [Ansible playbook documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_playbooks.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
