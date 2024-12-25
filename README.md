# Ansible Role: Nginx

## Description
This Ansible role installs and configures the Nginx web server. It sets up a default web page and allows configuration of the server name and static file root. The role is designed to be flexible and customizable to meet the needs of various environments.

## Requirements

- Minimum Ansible version: 2.1
- Supported platforms:
  - Not explicitly defined. Add specific platforms in meta/main.yml.

## Role Variables

The following variables are available for customization. Defaults are provided in defaults/main.yml:

| Variable                     | Default Value                | Description                                   |
|------------------------------|------------------------------|-----------------------------------------------|
| nginx_state                | latest                    | Defines the state of the nginx package.       |
| nginx_default_html_src     | files/index.html          | Path to the default HTML file on the control node. |
| nginx_default_html_dest    | /var/www/html             | Path where the default HTML file will be copied. |
| nginx_config_src           | templates/nginx.conf.j2   | Path to the nginx configuration template.     |
| nginx_config_available     | /etc/nginx/sites-available/default | Path to the configuration in sites-available. |
| nginx_config_enabled       | /etc/nginx/sites-enabled/default | Path to the configuration in sites-enabled.  |
| nginx_template_server_name | localhost                 | Server name in the nginx configuration.       |
| nginx_template_static_root | /var/www/html             | Root directory for static files.              |

## Dependencies

None.

## Example Playbook


- hosts: all
  roles:
    - role: nginx
      vars:
        nginx_state: "present"
        nginx_template_server_name: "example.com"


## Handlers

This role defines the following handlers:

| Name            | Action         |
|-----------------|----------------|
| Restart Nginx | Restarts the nginx service. |

## Templates

The role uses the following templates:

- templates/nginx.conf.j2: Nginx server configuration. Variables such as nginx_template_server_name and nginx_template_static_root can be customized.

## Files

The role includes the following files:

- files/index.html: Default HTML file served by nginx.

## Author Information

Role created by Viktoriia and Polina at ITMO.

## License

This role is licensed under a valid SPDX license ID, as defined in meta/main.yml (e.g., GPL-2.0-or-later, MIT).
