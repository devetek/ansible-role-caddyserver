Ansible role: Caddyserver
=========

This Ansible role installs and configures Caddy Server as a modern, automatic HTTPS router and reverse proxy. It allows declarative setup of virtual domains (vhosts) via a YAML-based configuration, making it easy to manage multi-site deployments, staging environments, or microservices with automatic SSL certificates.

Requirements
------------

None.

Role Variables
--------------

List of variables in ansible-role-caddyserver:

```sh
---
# action name
router_action: "install"

# user attribute
router_user: router
router_group: www-data
router_home: /home/{{ router_user }}

# github attribute
router_git_version: "tags/v0.0.2"
router_os: linux
router_github_token: ""
router_download_timeout: 300

# systemd attribute
router_service_name: "dpanel-router"
router_additional_args: ""
router_environment_files: []
router_environment_variables: {}
router_systemd_network_dependency: true
router_systemd_capabilities_enabled: false
router_systemd_capabilities: "CAP_NET_BIND_SERVICE"
router_systemd_restart: "always" # always, on-success, on-failure, on-abnormal, on-abort, on-watchdog
router_systemd_restart_startlimitinterval: "86400"
router_systemd_restart_startlimitburst: "5"
router_systemd_private_tmp: "true"
router_systemd_private_devices: "true"
# recommendation from caddy team
router_systemd_protect_home: "false"
router_systemd_protect_system: "full"
router_systemd_nproc_limit: 0
router_setcap: true

# caddy config
router_bin_dir: /usr/local/bin
router_conf_filename: Caddyfile
router_conf_dir: /etc/caddy
router_html_dir: /var/www/public
router_conf_snippets_dir: /etc/caddy/snippets
router_conf_vhosts_dir: /etc/caddy/vhosts
router_log_dir: /var/log/caddy
router_certs_dir: /etc/ssl/caddy
router_log_file: stdout
router_config: "{{ lookup('template', 'templates/Caddyfile.j2') }}"
router_config_extra: # souce (templates/[source_folder]): destination (machine target folder)
  - name: "Snippet PHP Code Igniter"
    source: snippets/php-codeigniter
    destination: snippets/php-codeigniter
  - name: "Snippet PHP Laravel"
    source: snippets/php-laravel
    destination: snippets/php-laravel
  - name: "Snippet PHP Wordpress"
    source: snippets/php-wordpress
    destination: snippets/php-wordpress
  - name: "Snippet PHP Wordpress Next"
    source: snippets/php-wordpress-next
    destination: snippets/php-wordpress-next
  - name: "Snippet Response String"
    source: snippets/response-string
    destination: snippets/response-string
  - name: "Snippet Static File"
    source: snippets/static-file
    destination: snippets/static-file
  - name: "Snippet Proxy Pass"
    source: snippets/proxypass
    destination: snippets/proxypass
  - name: "Snippet Domain Redirection"
    source: snippets/domain-redirect
    destination: snippets/domain-redirect
  - name: "PHP General FastCGI"
    source: snippets/phpfastcgi
    destination: snippets/phpfastcgi
  - name: "Default localhost vhost"
    source: vhosts/default
    destination: vhosts/default
  - name: "Default public vhost"
    source: vhosts/default-http
    destination: vhosts/default-http
router_default_view:
  - name: "404 View"
    source: public/error-404.html
    destination: error-404.html
  - name: "Error View"
    source: public/error.html
    destination: error.html

# create vhost for specific domain
# router_filename: "" # use this as primary filename, if not exist will use router_domain
router_domain: ""
router_content: ""
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
    - devetek.caddyserver
```

License
-------

GNU General Public License v3.0 or later

Author Information
------------------

[Nedya Prakasa]. Role created for [dPanel].

[dPanel]: https://cloud.terpusat.com/
[Nedya Prakasa]: https://github.com/prakasa1904
[devetek]: https://github.com/devetek