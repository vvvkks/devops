Role Name
=========

This Ansible role installs and configures Docker on Ubuntu-based systems. It ensures Docker and its dependencies are installed, the Docker GPG key and repository are added, and users are optionally added to the Docker group.

Requirements
------------
- Ansible version 2.1 or higher.
- Supported platforms:
  - Ubuntu 20.04 (Focal)
- Internet access for downloading Docker packages and keys.

Role Variables
--------------

The following variables can be configured to customize the role's behavior:

### Default Variables (from `defaults/main.yml`):
| Variable                 | Default Value                                                | Description                                                              |
|--------------------------|-------------------------------------------------------------|--------------------------------------------------------------------------|
| apt_packages           | ['ca-certificates', 'curl', 'gnupg']                      | List of required APT packages.                                           |
| apt_packages_state     | latest                                                    | State of APT packages (`present` or `latest`).                           |
| gpg_key_repo_url       | https://download.docker.com/linux/ubuntu/gpg              | URL for the Docker GPG key.                                              |
| apt_repos_state        | present                                                   | State of the Docker repository (`present` or `absent`).                  |
| docker_repo_url        | deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable | URL for the Docker APT repository.                                       |
| apt_docker_packages    | ['docker', 'docker.io', 'docker-compose', 'docker-registry'] | List of Docker-related packages to install.                             |
| apt_docker_packages_state | latest                                                | State of Docker packages (`present` or `latest`).                        |
| docker_users           | []                                                        | List of users to add to the Docker group.                                |


Dependencies
------------

This role has no external dependencies.

Example Playbook
----------------

Here's an example of how to use this role:

```yaml
- name: Install Docker and configure users
  hosts: all
  roles:
    - role: itmo.docker
      vars:
        docker_users:
          - user1
          - user2

