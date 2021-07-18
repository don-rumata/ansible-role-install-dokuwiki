# Ansible Role: Install DokuWiki

[![License][license-image]][license-url] [![Ansible Galaxy][ansible-galaxy-image]][ansible-galaxy-url] [![Ansible Galaxy Quality][ansible-galaxy-quality-image]][ansible-galaxy-url]

Install [DokuWiki](https://www.dokuwiki.org/) for Linux.

## Work on

### Ansible Galaxy style

```yaml
  platforms:
    - name: Fedora
      versions:
        - 33
        - 34
    - name: Ubuntu
      versions:
        - bionic
        - focal
    - name: Debian
      version:
        - stretch
        - buster
        - oldstable
        - stable
        - testing
    - name: EL (CentOS)
      versions:
        - 8
    - name: opensuse
      vesrion:
        - 15.3
        # TODO
        # - tumbleweed
```

### Table style

- :heavy_check_mark: - work, tested, ok.
- :construction: - TODO. Work in progress.
- :x: - not work. Don't try.

|.           |Apache            |lighttpd          |nginx             |
|------------|------------------|------------------|------------------|
|**Ubuntu**  |                  |                  |                  |
|focal       |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|bionic      |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|xenial      |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|**Debian**  |                  |                  |                  |
|stretch     |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|buster      |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|stable      |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|testing     |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|**CentOS**  |                  |                  |                  |
|8           |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|**openSUSE**|                  |                  |                  |
|tumbleweed  |:construction:    |:construction:    |:construction:    |
|leap        |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|**Fedora**  |                  |                  |                  |
|33          |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|
|34          |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|

## Requirements

None.

## Role Variables

```yaml
---
#--- Distrib settings ---#

dokuwiki_url_archive: https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz
# dokuwiki_url_archive: http://10.10.10.10/soft/dokuwiki/dokuwiki-stable.tgz
# dokuwiki_url_archive: https://download.dokuwiki.org/src/dokuwiki/dokuwiki-oldstable.tgz
# dokuwiki_url_archive: http://github.com/splitbrain/dokuwiki/tarball/master
# dokuwiki_url_archive: https://download.dokuwiki.org/out/dokuwiki-51907416a10f9ac6818c982ae15d7b2a.tgz

#--- Local FS settings ---#

dokuwiki_fs_dir: '{{ dokuwiki_home_web_server }}/dokuwiki'
dokuwiki_chmod: u+rw,g+rw,o-rwx

# ps aux | grep lighttpd
# www-data   25177  0.0  0.9   7908  4748 ?        Ss   12:07   0:00 /usr/sbin/lighttpd -D -f /etc/lighttpd/lighttpd.conf
dokuwiki_user_web_server: www-data
dokuwiki_group_web_server: www-data
dokuwiki_home_web_server: /var/www

#--- Webserver settings ---#

# dokuwiki_web_server_name: apache
# dokuwiki_web_server_name: lighttpd
dokuwiki_web_server_name: nginx

dokuwiki_www_dir: /dokuwiki
dokuwiki_www_port: 80

# This: /etc/nginx/sites-enabled/80-{{ dokuwiki_web_server_config_name }}.conf
dokuwiki_web_server_config_name: dokuwiki

# TODO. This: "server { server_name wiki.ulyaoth.net; }"
# dokuwiki_server_name: dokuwiki.example.com

#--- PHP-FPM setting ---#

# ps aux | grep php-fpm
# www-data   24009  0.0  1.3 209628  6696 ?        S    12:06   0:00 php-fpm: pool www
dokuwiki_php_fpm_user: www-data
dokuwiki_php_fpm_group: www-data

# This: /etc/php-fpm.d/www.conf -> [www] -> listen = /run/php-fpm/www.sock
dokuwiki_php_fpm_connection_type: socket
# dokuwiki_php_fpm_connection_type: ip

dokuwiki_php_fpm_connection_address: /run/php-fpm/www.sock
# dokuwiki_php_fpm_connection_address: 127.0.0.1:9000

#--- Site admin settings  ---#

dokuwiki_admin_login: sysadmin
dokuwiki_admin_password: qazwsxedc
dokuwiki_admin_email: mail@example.com
dokuwiki_admin_real_name: Malcolm Reynolds
dokuwiki_title: My shiny metal dokuwiki
dokuwiki_interface_lang: en

#--- Plugin settings ---#

dokuwiki_install_plugins: true
# dokuwiki_install_plugins: false

#--- Distro spicific vars ---#

# zypper info php7
dokuwiki_opensuse_php_major_version: 7
```

## HowTo

### How to install role

Over `ansible-galaxy`:

```bash
ansible-galaxy install don_rumata.ansible_role_install_dokuwiki
```

Over `bash+git`:

```bash
mkdir -p "$HOME/.ansible/roles"
cd "$HOME/.ansible/roles"
git clone https://github.com/don-rumata/ansible-role-install-dokuwiki don_rumata.ansible_role_install_dokuwiki
```

## Example Playbooks

### I

Install latest stable `DokuWiki` on Linux with `nginx`:

`install-dokuwiki.yml`:

```yaml
- name: Install DokuWiki
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - don_rumata.ansible_role_install_dokuwiki
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

### II

Install latest stable `DokuWiki` on Linux with `apache`, super secret password and russian language:

`install-dokuwiki.yml`:

```yaml
- name: Install DokuWiki
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - role: don_rumata.ansible_role_install_dokuwiki
      dokuwiki_web_server_name: apache
      dokuwiki_admin_password: damtoneruoyfinevedamerauoyyhwnialpxeotdrahyrevevahsufotsomehtekildamneebeviwonkidamneebsyawlaevi
      dokuwiki_interface_lang: ru
  tasks:
```

## License

Apache License, Version 2.0

## Author Information

[don Rumata](https://github.com/don-rumata)

## TODO

- Add tests.

## Known issue

- openSUSE Tumbleweed [not work](https://ru.stackoverflow.com/q/1303755/191416) with php-fpm `socket`. Use `dokuwiki_php_fpm_connection_type: ip`, `dokuwiki_php_fpm_connection_address: 127.0.0.1:9000`.
- Error:

```none
"module_stdout": "\r\nTraceback (most recent call last):\r\n  File \"/home/ansible-test-user/.ansible/tmp/ansible-tmp-1626521566.63-15344-118601205128264/AnsiballZ_ini_file.py\", line 102, in <module>\r\n    _ansiballz_main()\r\n  File \"/home/ansible-test-user/.ansible/tmp/ansible-tmp-1626521566.63-15344-118601205128264/AnsiballZ_ini_file.py\", line 94, in _ansiballz_main\r\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\r\n  File \"/home/ansible-test-user/.ansible/tmp/ansible-tmp-1626521566.63-15344-118601205128264/AnsiballZ_ini_file.py\", line 40, in invoke_module\r\n    runpy.run_module(mod_name='ansible.modules.files.ini_file', init_globals=None, run_name='__main__', alter_sys=True)\r\n  File \"/usr/lib/python3.8/runpy.py\", line 207, in run_module\r\n    return _run_module_code(code, init_globals, run_name, mod_spec)\r\n  File \"/usr/lib/python3.8/runpy.py\", line 97, in _run_module_code\r\n    _run_code(code, mod_globals, init_globals,\r\n  File \"/usr/lib/python3.8/runpy.py\", line 87, in _run_code\r\n    exec(code, run_globals)\r\n  File \"/tmp/ansible_ini_file_payload_c_pldhft/ansible_ini_file_payload.zip/ansible/modules/files/ini_file.py\", line 342, in <module>\r\n  File \"/tmp/ansible_ini_file_payload_c_pldhft/ansible_ini_file_payload.zip/ansible/modules/files/ini_file.py\", line 322, in main\r\n  File \"/tmp/ansible_ini_file_payload_c_pldhft/ansible_ini_file_payload.zip/ansible/modules/files/ini_file.py\", line 156, in do_ini\r\n  File \"/usr/lib/python3.8/encodings/ascii.py\", line 26, in decode\r\n    return codecs.ascii_decode(input, self.errors)[0]\r\nUnicodeDecodeError: 'ascii' codec can't decode byte 0xc2 in position 1032: ordinal not in range(128)\r\n"
```

For fix it, use: `LC_CTYPE: en_US.UTF-8` or `LC_CTYPE: C` in `environment`. Like:

```yaml
  - name: Install DokuWiki
    hosts: all
    strategy: free
    serial:
      - "100%"
    roles:
      - role: don_rumata.ansible_role_install_dokuwiki
        environment:
          LC_CTYPE: en_US.UTF-8
```

[license-image]: https://img.shields.io/github/license/don-rumata/ansible-role-install-dokuwiki.svg
[license-url]: https://opensource.org/licenses/Apache-2.0

[ansible-galaxy-image]: https://img.shields.io/badge/ansible_galaxy-don__rumata.ansible__role__install__dokuwiki-blue.svg
[ansible-galaxy-url]: https://galaxy.ansible.com/don_rumata/ansible_role_install_dokuwiki

[ansible-galaxy-quality-image]: https://img.shields.io/ansible/quality/55672
