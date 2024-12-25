# Ansible Role: OpenVPN

This role installs and configures an OpenVPN server and clients, leveraging easy-rsa for certificate management. It also ensures firewall rules are properly configured for OpenVPN traffic.

## Requirements

- Ansible version >= 2.1
- Supported platforms:
  - Any platform with easy-rsa and OpenVPN package support (Debian-based distributions are tested).

## Role Variables

### Easy-RSA Variables

| Variable                          | Default Value                        | Description                              |
|-----------------------------------|--------------------------------------|------------------------------------------|
| easy_rsa_dir                    | {{ ansible_env.HOME }}/easy-rsa    | Directory for Easy-RSA                   |
| easy_rsa_binary                 | /usr/share/easy-rsa                | Path to Easy-RSA binary                  |
| easy_rsa_vars_template          | templates/easy-rsa3.j2             | Template for Easy-RSA variables          |
| easy_rsa_vars_EASYRSA_REQ_COUNTRY | RU                                | Country for certificates                 |
| easy_rsa_vars_EASYRSA_REQ_PROVINCE | Los Angeles                      | Province for certificates                |
| easy_rsa_vars_EASYRSA_REQ_CITY   | Los Angeles                       | City for certificates                    |
| easy_rsa_vars_EASYRSA_REQ_ORG    | org                               | Organization name                        |
| easy_rsa_vars_EASYRSA_REQ_EMAIL  | rtz@mail.ru                       | Email address                            |
| easy_rsa_vars_EASYRSA_ALGO       | ec                                | Algorithm for keys                       |
| easy_rsa_vars_EASYRSA_DIGEST     | sha512                            | Digest algorithm                         |

### OpenVPN Server Variables

| Variable                          | Default Value                        | Description                              |
|-----------------------------------|--------------------------------------|------------------------------------------|
| openvpn_server_common_name      | server                            | Server's common name                     |
| openvpn_dir                     | /etc/openvpn/server               | Directory for OpenVPN server files       |
| openvpn_server_template         | templates/openvpn-server.conf.j2  | Template for OpenVPN server configuration|
| openvpn_server_vars_ca          | ca.crt                            | Path to the CA certificate               |
| openvpn_server_vars_cert        | {{ openvpn_server_common_name }}.crt | Server certificate file name           |
| openvpn_server_vars_key         | {{ openvpn_server_common_name }}.key | Server private key file name            |
| openvpn_server_vars_port        | 1194                              | OpenVPN server port                      |
| openvpn_server_vars_proto       | udp                               | Protocol for OpenVPN server              |
| openvpn_server_vars_tls_crypt   | ta.key                            | TLS Crypt key                            |
| openvpn_server_vars_cipher      | cipher AES-256-GCM\nauth SHA256   | Cipher and authentication algorithms     |

### OpenVPN Client Variables
| Variable                          | Default Value                        | Description                              |
|-----------------------------------|--------------------------------------|------------------------------------------|
| openvpn_client_common_name      | client1                           | Default client name                      |
| client_configs_dir              | {{ ansible_env.HOME }}/client-configs | Directory for client configuration files |
| openvpn_client_template         | templates/openvpn-client.conf.j2  | Template for OpenVPN client configuration|
| openvpn_client_vars_remote      | {{ ansible_default_ipv4.address }} | OpenVPN server IP address                |
| openvpn_client_vars_remote_port | 1194                              | OpenVPN server port                      |
| openvpn_client_vars_proto       | udp                               | Protocol for OpenVPN client              |
| openvpn_client_vars_cipher      | cipher AES-256-GCM\nauth SHA256   | Cipher and authentication algorithms     |

### UFW Firewall Variables

| Variable                          | Default Value                        | Description                              |
|-----------------------------------|--------------------------------------|------------------------------------------|
| ufw_before_path                 | /etc/ufw/before.rules             | Path to UFW pre-routing rules            |
| ufw_before_content              | NAT masquerade rules                 | NAT masquerade configuration             |
| ufw_forwarding_default_path     | /etc/default/ufw                  | Path to UFW default policy configuration |
| ufw_forwarding_policy           | DEFAULT_FORWARD_POLICY="ACCEPT" | Forwarding policy for UFW                |
| ufw_rule                        | allow                             | UFW rule for OpenVPN                     |
| ufw_port                        | 1194                              | UFW allowed port                         |
| ufw_proto                       | udp                               | UFW protocol                             |

## Dependencies

This role has no dependencies.

## Example Playbook


- hosts: all
  become: true
  roles:
    - role: openvpn
      vars:
        easy_rsa_vars_EASYRSA_REQ_COUNTRY: "US"
        openvpn_server_vars_port: 1194
        openvpn_client_vars_remote: "vpn.example.com"


## License

GPL-2.0-or-later, MIT, etc.

## Author Information

This role was created by Viktoriia and Polina, ITMO University.
