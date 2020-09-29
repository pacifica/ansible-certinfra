Pacifica Certificate Infrastructure
===================================

This ansible role manages your own OpenSSL Certificate Authority. The
role also manages the intermediate, server and client certificates.
This role provides client certificates to secure the communication
between applications and services in either Apache or NGINX.

Requirements
------------

 * Ansible >= 2.9
 * Python Cryptography >= 3.0

Role Variables
--------------

There are a lot of variables in the [default](defaults/main.yml) that
I won't cover here. However, I'll cover the basics here.

  * SSL Organizational Information - you should overwrite this!
    * `country_name` (default: US)
    * `state_or_province_name` (default: Washington) (because that's where I live)
    * `locality_name` (default: Richland) (ditto)
    * `organization_name` (default: Pacifica Software)
    * `organizational_unit_name` (default: Pacifica Certificate)
    * `email_address` (default: rootca@pacifica.io) (doesn't work actually)
    * `root_ca_common_name` (default: Pacifica Software ROOT CA)
    * `intermediate_common_name` (default: Pacifica Software Intermediate ROOT CA)
  * Servers List - `servers` is a list of objects the keys are as follows
    * `name` FQDN of the server and common_name (required)
    * `subject_alt_name` passed straight to [here](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssl_csr_module.html#parameter-subject_alt_name) (required)
    * `key_path` private key for the server (default is computed)
    * `csr_path` certificate signing request path (default is computed)
    * `basic_constraints` passed to [here](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssl_csr_module.html#parameter-basic_constraints) (default is default_server_basic_constraints)
    * `key_usage` passed to [here](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssl_csr_module.html#parameter-key_usage) (default is default_server_key_usage)
  * Clients List - `clients` is a list of objects the keys are as follows
    * `name` username for the client and common_name (required)
    * `key_path` private key for the client (default is computed)
    * `csr_path` certificate signing request path (default is computed)
    * `basic_constraints` passed to [here](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssl_csr_module.html#parameter-basic_constraints) (default is default_client_basic_constraints)
    * `key_usage` passed to [here](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssl_csr_module.html#parameter-key_usage) (default is default_client_key_usage)
    * `extended_key_usage` passed to [here](https://docs.ansible.com/ansible/latest/collections/community/crypto/openssl_csr_module.html#parameter-extended_key_usage) (default is default_client_extended_key_usage)

Dependencies
------------

 * Community Crypto Collection >= 1.1.1

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: pacifica.ansible_certinfra }

License
-------

LGPL-3.0
