Ansible Kibana playbook
=====

This role installs and configures Kibana on a server.

Requirements
------------

This role requires Ansible 1.4 or higher and platform requirements are listed
in the metadata file.

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows.

```
# URL address to reach kibana
dns_url_kibana: kibana.domain.lan
# Folder to store Kibana
kibana_path: /usr/share/nginx/www/kibana
# Kibana version from GitHub tag
kibana_tag_version: v3.1.0
# ES version
es_version: 1.4
```

Examples
========

```
- name: log server
  hosts: logs
  user: root
  roles:
    - elasticsearch
    - role: nginx
      nginx_sites:
        - server:
           file_name: kibana.domain.lan
           server_name: kibana.domain.lan
           listen: 80
           root: /usr/share/nginx/www/kibana/src
           location1: {name: /, try_files: "$uri $uri/ /index.html"}
  vars_files:
    - "host_vars/kibana.yml"
```

Dependencies
------------

- Nginx: ansible-galaxy install deimosfr.ansible-nginx

License
-------

GPL

Author Information
------------------

Pierre Mavro / deimosfr


