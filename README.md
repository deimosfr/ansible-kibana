Ansible Kibana role
===================

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
# dns_url_kibana: kibana.domain.lan
dns_url_kibana: kibana

# Folder to store Kibana
kibana_path: /usr/share/nginx/www/kibana

# Set Kibana major version
kibana_major_version: 4
#kibana_major_version: 3

## Kibana version
# For v4 use latest version (https://www.elastic.co/downloads/kibana)
kibana_full_version: 4.0.1
# For v3, use GitHub tag
#kibana_full_version: v3.1.2

# ES version
es_version: 1.4

# kibana4 configuration options
kibana_port: 5601
kibana_host: '0.0.0.0'
kibana_elasticsearch_url: 'http://{{dns_url_kibana}}:9200'
kibana_elasticsearch_preserve_host: true
kibana_index: '.kibana'
kibana_enable_authentication: false
kibana_elasticsearch_username: user
kibana_elasticsearch_password: pass
kibana_default_app_id: discover
kibana_request_timeout: 300000
kibana_shard_timeout: 0
kibana_verify_ssl: true
kibana_pid_file: '/var/run/kibana.pid'
```

Examples
========

For Kibana 3:

```
- name: log server
  hosts: logs
  user: root
  roles:
    - elasticsearch
    - role: deimosfr.nginx
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

For Kibana 4:

```
- name: log server
  hosts: logs
  user: root
  roles:
    - deimosfr.kibana
    - role: jdauphant.nginx
      nginx_sites:
        kibana.domain.lan:
          - listen 80"
          - server_name kibana.domain.lan
          - access_log /var/log/nginx/kibana.domain.lan_access.log nm_combined
          - error_log /var/log/nginx/kibana.domain.lan_error.log
          - error_page   500 502 503 504  /50x.html
          - location = /50x.html {
              root /usr/share/nginx/html;
            }
          - location / {
              more_clear_headers  "Access-Control-Request-Headers";
              add_header          Access-Control-Request-Headers "accept, x-auth-token";
              proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
              proxy_set_header    Host $http_host;
              proxy_set_header    X-Real-IP $remote_addr;
              proxy_pass          http://127.0.0.1:5601/;
            }
  vars_files:
    - "host_vars/kibana.yml"
```

Dependencies
------------

You can use any web server configuration like:
- Nginx: ansible-galaxy install deimosfr.ansible-nginx
or
- Nginx: ansible-galaxy install jdauphant.nginx

License
-------

GPL

Author Information
------------------

Pierre Mavro / deimosfr


