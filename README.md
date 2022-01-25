enix.teleport
=============

A role for deploying and configuring [teleport](https://goteleport.com) and extensions on unix hosts using [Ansible](http://www.ansible.com/).

Requirements
------------

Supported targets:

- Ubuntu 18.04 "Bionic"
- Ubuntu 20.04 "Focal"
- Debian 10 "Buster"
- Debian 11 "Bullseye"

Role Variables
--------------

This roles comes preloaded with almost every available default. You can override each one in your hosts/group vars, in your inventory, or in your play. See the annotated defaults in `defaults/main.yml` for help in configuration. All provided variables start with `teleport__`.

- `teleport__version: ""` - Version of the binary to install and hold, default install latest available in the repo, but can precise any debian package release version.
- `teleport__agent: false` - Configure and Enable the teleport agent software.
- `teleport__nodename: {{ inventory_hostname }}` - Name the teleport agent report to it's connected proxy.
- `teleport__node: false` - Configure and Enable teleport node role.
- `teleport__node_token: ""` - Token used to join the proxy.
- `teleport__node_server: ""` - Proxy server url.
- `teleport__proxy: false` - Enable the proxy mode in teleport.
- `teleport__proxy_public_addr: ""` - Public address the proxy expose.
- `teleport__proxy_acme: false` - Enable ACME protocol for public certificate.
- `teleport__proxy_acme_email: ""` - Email for the ACME request.
- `teleport__auth: false` - Enable teleport auth role.
- `teleport__auth_cluster_name: ""` - Teleport auth cluster name.
- `teleport__ssh: false` - Enable teleport ssh module.
- `teleport__ssh_labels: ''` - Add labels to the ssh module (yaml format).

Dependencies
------------

- None

Usage
-----

Use Ansible galaxy requirements.yml

    # teleport from enix
    # private role
    - src: git+ssh://git@gitlab.enix.io/ansible/ansible-teleport.git
      name: enix.teleport

And add it to your play's roles:

```yaml
# Node example
    - hosts: all
      roles:
        - role enix.teleport:
            teleport__agent: true
            teleport__nodename: "test.node"
            teleport__node: true
            teleport__node_token: "gjlksfdjglkfsdjlkgfds9423"
            teleport__node_server: "https://toto.tp.com:3025"
            teleport__ssh: true
            teleport__ssh_labels:
              tenant: toto.com
```

```yaml
# Proxy example
    - hosts: all
      roles:
        - role enix.teleport:
            teleport__agent: true
            teleport__nodename: "toto.proxy"
            teleport__proxy: true
            teleport__proxy_public_addr: "toto.tp.com:443"
            teleport__proxy_acme: false
            teleport__proxy_acme_email: "test@toto.com"
            teleport__auth: true
            teleport__auth_cluster_name: "toto.tp.com"
            teleport__ssh: true
            teleport__ssh_labels:
              tenant: toto.com
```

Still to do
-----------

- Install centos7 binary version for older OS (override binary path systemd en +). For debian stretch, ubuntu 16.04, ...
- Manage mode configuration options (static ssl certs)
- Add app service in proxy configuration

Changelog
---------

### 0.9.0

Initial version.

License
-------

GPLv2

Author Information
------------------

Laurent Corbes <laurent.corbes@enix.fr> - http://www.enix.io
