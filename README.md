# Icinga server (including NSCA)

Ansible role to install and configure an Icinga 1.X server (with NSCA) on Debian
and Ubuntu platforms.

Icinga is installed from the standard APT repositories, and all the values of
the configuration options defined in this role are the default ones, excepting:

- the `check_external_commands` option is set to `1` to ensure that the NSCA
  server works well;
- the permissions of the `/var/lib/icinga` and `/var/lib/icinga/rw`
  directories are changed to allow write access for Apache and NSCA servers;

The Icinga web interface will be available on
[http://SERVER/icinga/](http://SERVER/icinga/) by default.

## Supported Platforms

* Debian
    - Stretch   (9)

## Example (Playbook)

Standard installation:

```yaml
- name: Icinga server
  hosts: icinga_server
  become: yes
  roles:
    - role: icinga
```

Installation with your configuration stored in one (or several) custom
directory, and an NSCA server password (the NSCA password used in the
*icinga-client* role must be identical):

```yaml
- name: Icinga server
  hosts: icinga_server
  become: yes
  roles:
    - role: icinga
      icinga_config_options:
        - option: cfg_dir:
          value: /etc/icinga/my_config/
          multi: True
      nsca_config_options:
        - option: password:
          value: PASSWORD
```

Installation with your configuration cloned from one (or several) Git
repository:

```yaml
- name: Icinga server
  hosts: icinga_server
  become: yes
  roles:
    - role: icinga
      icinga_config_cfg_dirs_git:
        - repo: https://SERVER/ICINGA_CONFIG.git
          dest: /etc/icinga/my_config
          version: master
```

The same but from one (or several) Mercurial repository:

```yaml
- name: Icinga server
  hosts: icinga_server
  become: yes
  roles:
    - role: icinga
      icinga_config_cfg_dirs_hg:
        - repo: https://SERVER/ICINGA_CONFIG
          dest: /etc/icinga/my_config
          version: default
```

See the *Variables* section below for details.

## Variables

See the [defaults/main.yml](defaults/main.yml) file.
