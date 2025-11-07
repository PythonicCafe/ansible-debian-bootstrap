Ansible Debian bootstrap
====================================================

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-pythoniccafe.debian_bootstrap-blue.svg)](https://galaxy.ansible.com/pythoniccafe/debian_bootstrap)

**Forked from** [HanXHX/ansible-debian-bootstrap](https://github.com/HanXHX/ansible-debian-bootstrap) to deploy only Debian servers. Do not use it if you want to bootstrap Ubuntu/Devuan/Raspbian servers.

This role bootstraps Debian hosts:

- Configure APT (sources.list)
- Install minimal packages (vim, htop...)
------------------

- Add groups, users with SSH key, sudoers
- Deploy bashrc, vimrc for root
- Update few alternatives
- Configure system: hostname, timezone and locale
- Sysctl tuning

Supported versions

| OS                     | Working      | Stable (active support) |
| ---------------------- | -------      | ----------------------- |
| Debian Bullseye (11)   | Yes          | Yes                     |
| Debian Bookworm (12)   | Yes          | Yes                     |

Requirements
------------

- Ansible >= 2.11
- Collections: [ansible.posix collection](https://galaxy.ansible.com/ansible/posix) / [community.general](https://galaxy.ansible.com/community/general)

Role Variables
--------------

### APT configuration

Theses variables define hostname to configure APT (normal repo and backports):

- `dbs_apt_default_host`: repository host. It can replace the last one (installed with this role) with a new one
- `dbs_apt_use_src`: install "deb-src" repositories (default: false)
- `dbs_apt_components`: components uses in sources.list (default: "main contrib non-free")

### Role setup

- `dbs_set_hostname`: if true, change hostname
- `dbs_clean_hosts`: if true, manages `/etc/hosts` file
- `dbs_set_locale`: if true, configure locales
- `dbs_set_timezone`: if true, set timezone
- `dbs_set_apt`: if true, configure APT repository

### System configuration

- `dbs_hostname`: system hostname
- `dbs_hostname_use_strategy`: strategy used to set hostname check "use" in [hostname module](https://docs.ansible.com/ansible/latest/modules/hostname_module.html). You should update this var only if hostname fails (in LXC for example).
- `dbs_default_locale`: default system locale
- `dbs_locales`: list of installed locales
- `dbs_timezone`: system timezone. If you need a "standard" timezone like UTC, you must use prefix "Etc/" (ex: "Etc/UTC")
- `dbs_sysctl_config`: hash of kernel parameters, see: [default/main.yml](default/main.yml)
- `dbs_use_systemd`: delete systemd if set to false (persistent)
- `dbs_use_dotfiles`: overwrite root dotfiles (bashrc, screenrc, vimrc)
- `dbs_uninstall_packages`: packages list to uninstall

### Alternatives

- `dbs_alternative_editor`
- `dbs_alternative_awk`

### Group

- `dbs_groups`: list of groups

Each row have few keys:

- `name`: (M) username on system
- `system`: (O) yes/no (default: no)
- `state`: (O) present/absent (default: present)

(M) Mandatory
(O) Optionnal

### User

- `dbs_users`: list of user

Each row have few keys:

- `name`: (M) username on system
- `password`: (O) password with hash format (see [ansible doc](http://docs.ansible.com/ansible/latest/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module))
- `clear_password`: (O) password as clear format (not recommanded)
- `update_password`: (O) always / on\_create
- `shell`: (O) default is /bin/bash
- `comment`: (O) default is an empty string
- `sudo`: (O) boolean (true = can sudo)
- `group`: (O) main group (default is `name` without password)
- `groups`: (O) comma separated list of groups
- `createhome`: (O) yes/no (default is yes)
- `system`: (O) yes/no (default: no)
- `ssh_keys`: (O) ssh public keys list
- `state`: (O) present/absent (default: present)

(M) Mandatory
(O) Optionnal

Notes:

- if `password` is specified, `clear_password` is not used!
- `clear_password` is not idempotent with `update_password` = always (default)

For more information, look [ansible user module doc](http://docs.ansible.com/ansible/latest/user_module.html).

### Readonly vars

- `dbs_packages`: list of packages to install
- `dbs_distro_packages`: list specific package to install (related to OS version)
- `dbs_is_docker`: boolean. Is true if current is a docker container

Dependencies
------------

None.

Example Playbook
----------------Author Information
------------------

- Twitter: [@hanxhx_](https://twitter.com/hanxhx_)

    - hosts: servers
      roles:
         - { role: HanXHX.debian_bootstrap }


About Docker
------------

Due to Docker limitations, theses features are disabled:

- Setting hostname
- Configure sysctl


How to develop and test this role
---------------------------------

### Vagrant way

Install vagrant + libvirt or docker

```commandline
vagrant up debian-bullseye # with libvirt (or whatever)
vagrant up docker-debian-bullseye # with docker
```

License
-------

GPLv2

Author Information
------------------

- Twitter: [@hanxhx_](https://twitter.com/hanxhx_)
