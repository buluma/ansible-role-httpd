# [Ansible role httpd](#httpd)

Install and configure httpd on your system.

|GitHub|GitLab|Downloads|Version|Issues|Pull Requests|
|------|------|-------|-------|------|-------------|
|[![github](https://github.com/buluma/ansible-role-httpd/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-httpd/actions/workflows/molecule.yml)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-httpd/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-httpd)|[![downloads](https://img.shields.io/ansible/role/d/4733)](https://galaxy.ansible.com/buluma/httpd)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-httpd.svg)](https://github.com/buluma/ansible-role-httpd/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-httpd.svg)](https://github.com/buluma/ansible-role-httpd/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-httpd.svg)](https://github.com/buluma/ansible-role-httpd/pulls/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-httpd/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  vars_files:
    - ../../vars/main.yml
    - ../../defaults/main.yml

  roles:
    - role: buluma.httpd
      # https_ssl_enable: yes
      httpd_port: 8080
      httpd_ssl_port: 8443
      httpd_locations:
        - name: my_location
          location: /my_location
          backend_url: "http://localhost:8080/myapplication"
      # httpd_vhosts:
      #   - name: my_vhost_docroot
      #     servername: www1.example.com
      #     documentroot: "{{ httpd_data_directory }}/www1.example.com"
      #   - name: my_vhost_backend_http
      #     servername: www2.example.com
      #     backend_url: "http://www.example.com/"
      #     serveralias:
      #       - example.com
      #       - www.example.com
      #   - name: my_vhost_remote
      #     servername: www3.example.com
      #     remote: "http://localhost:3128/"
      #   - name: my_vhost_backend_https
      #     servername: www4.example.com
      #     backend_url: "https://www.example.com/"
      #   - name: my_vhost_piratebay
      #     servername: piratebay.nl
      #     backend_url: "https://thepirate-bay.org/"
      #     proxy_preserve_host: Off
      #     proxy_requests: Off
      #     setenv:
      #       - name: force-proxy-request-1.0
      #         value: 1
      #       - name: proxy-nokeepalive
      #         value: 1
      #       - name: proxy-initial-not-pooled
      #       - name: proxy-sendchunks
      #         value: 1
      #   - name: no_doc_root
      #     servername: nodocroot.example.com
      #     documentroot: /var/www/html/nodocroot
      #     create_docroot: no
      httpd_directories:
        - name: my_directory
          path: "{{ httpd_data_directory }}/my_directory"
          # options:
          #   - Indexes
          #   - FollowSymLinks
          allow_override: All
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-httpd/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: buluma.bootstrap
    - role: buluma.epel
    - role: buluma.buildtools
    - role: buluma.python_pip
    - role: buluma.openssl
      openssl_items:
        - name: apache-httpd
          common_name: "{{ ansible_fqdn }}"
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-httpd/blob/master/defaults/main.yml):

```yaml
---
# defaults file for httpd

# The servername to use.
httpd_servername: "{{ ansible_fqdn }}"

# The non-SSL port to use.
httpd_port: 80

# Enable (self-signed certificates) SSL?
https_ssl_enable: no

# To configure https, set the hostname to listen to.
httpd_ssl_servername: "{{ ansible_fqdn }}"

# For SSL a TCP port is required.
httpd_ssl_port: 443

# SSL Certificate:
httpd_openssl_crt: "{{ httpd_openssl_crt_directory }}/apache-httpd.crt"

# SSL Key
httpd_openssl_key: "{{ httpd_openssl_key_directory }}/apache-httpd.key"

# If the "it works" page should be kept
httpd_remove_example: no

# Additionnal httpd module to install

httpd_additionnal_modules: []

apache_global_vhost_settings: |
  DirectoryIndex index.php index.html
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-httpd/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Build Status GitHub](https://github.com/buluma/ansible-role-buildtools/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-buildtools/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-buildtools)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Build Status GitHub](https://github.com/buluma/ansible-role-epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-epel/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-epel/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-epel)|
|[buluma.openssl](https://galaxy.ansible.com/buluma/openssl)|[![Build Status GitHub](https://github.com/buluma/ansible-role-openssl/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-openssl/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-openssl/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-openssl)|
|[buluma.python_pip](https://galaxy.ansible.com/buluma/python_pip)|[![Build Status GitHub](https://github.com/buluma/ansible-role-python_pip/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-python_pip/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-python_pip/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-python_pip)|
|[buluma.selinux](https://galaxy.ansible.com/buluma/selinux)|[![Build Status GitHub](https://github.com/buluma/ansible-role-selinux/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-selinux/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-selinux/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-selinux)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-httpd/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/repository/docker/buluma/enterpriselinux/general)|8|
|[Debian](https://hub.docker.com/repository/docker/buluma/debian/general)|all|
|[Fedora](https://hub.docker.com/repository/docker/buluma/fedora/general)|all|
|[opensuse](https://hub.docker.com/repository/docker/buluma/opensuse/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|all|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-httpd/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-httpd/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-httpd/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)

Please consider [sponsoring me](https://github.com/sponsors/buluma).

### [Special Thanks](#special-thanks)

Template inspired by [Robert de Bock](https://github.com/robertdebock)
