# Ansible Role: Install DokuWiki

[![License][license-image]][license-url] [![Ansible Galaxy][ansible-galaxy-image]][ansible-galaxy-url]

Install [DokuWiki](https://www.dokuwiki.org/) for Linux.

## Work on

### Ansible Galaxy style

```yaml
  platforms:
    - name: Fedora
      versions:
        - 33
    - name: Ubuntu
      versions:
        - bionic
        - focal
    - name: Debian
      version:
        - jessie
        - stretch
        - buster
        - oldstable
        - stable
        - testing
    - name: EL (CentOS)
      versions:
        - 7
        - 8
    - name: opensuse
      vesrion:
        - tumbleweed
```

## Requirements

None.

## Role Variables

```yaml
---
#--- Distrib settings ---#

# dokuwiki_url_archive: http://10.10.10.10/soft/dokuwiki/dokuwiki-stable.tgz
dokuwiki_url_archive: https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz

#--- Local FS settings ---#

dokuwiki_fs_dir: /var/www/dokuwiki
dokuwiki_chmod: u+rw,g+rw,o-rwx

#--- Webserver settings ---#

# dokuwiki_web_server_name: apache
# dokuwiki_web_server_name: lighttpd
dokuwiki_web_server_name: nginx

dokuwiki_www_dir: /dokuwiki
dokuwiki_www_port: 80

# This: /etc/nginx/sites-enabled/80-{{ dokuwiki_web_server_config_name }}.conf
dokuwiki_web_server_config_name: dokuwiki

#--- Admin settings  ---#

dokuwiki_admin_login: sysadmin
dokuwiki_admin_password: qazwsxedc
dokuwiki_admin_email: mail@example.com
dokuwiki_admin_real_name: Malcolm Reynolds
dokuwiki_title: My shiny metal dokuwiki
dokuwiki_interface_lang: en
```

## Example Playbooks

### I

Install latest stable DokuWiki on Linux:

`install-dokuwiki.yml`:

```yaml
- name: Install DokuWiki
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - ansible-role-install-dokuwiki
  tasks:
```

`dokuwiki-inventory.ini`:

```ini
[dokuwiki]
172.16.10.10
```

```bash
ansible-playbook -i ./dokuwiki-inventory.ini ./install-dokuwiki.yml
```

## License

Apache License, Version 2.0

## Author Information

[don Rumata](https://github.com/don-rumata)

## TODO

- Add tests.

[license-image]: https://img.shields.io/github/license/don-rumata/ansible-role-install-dokuwiki.svg
[license-url]: https://opensource.org/licenses/Apache-2.0

[ansible-galaxy-image]: https://img.shields.io/badge/ansible_galaxy-don__rumata.ansible__role__install__dokuwiki-blue.svg
[ansible-galaxy-url]: https://galaxy.ansible.com/don_rumata/ansible_role_install_dokuwiki
