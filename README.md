# Ansible Role: dockerhost

![Ansible](https://img.shields.io/badge/ansible-%3E%3D%202.10-EE0000?logo=ansible&logoColor=white)
![Platform](https://img.shields.io/badge/platform-Ubuntu-E95420?logo=ubuntu&logoColor=white)
![License](https://img.shields.io/badge/license-Unlicense-blue)

This role installs Docker via the official Docker APT repository and configures the Docker service on the target host.

Installed by default:

- Docker Engine
- Docker CLI
- containerd
- Docker Compose v2 as a CLI plugin
- `python3-docker` for Ansible's Docker modules

## Requirements

- Ansible >= 2.10
- `become: true`, because system packages, APT sources, and the service are managed
- Fact gathering must be enabled because the role uses `ansible_facts` and `ansible_facts['lsb']['codename']`
- Network access to `download.docker.com`

## Role Behavior

This role:

- installs the APT packages required for the Docker repository
- creates `/etc/apt/keyrings` and downloads the Docker GPG key there
- configures the Docker repository with a configurable release channel
- maps supported Ansible architectures explicitly to Docker APT architectures and fails early on unsupported systems
- installs the configured Docker packages
- enables and starts the `docker` service
- verifies the installation with `docker --version` and `docker compose version`

## Important Notes

- The default repository URL points to Ubuntu: `https://download.docker.com/linux/ubuntu`.
- Supported architectures are defined in [vars/main.yml](/home/max/Projects/ansible_roles/ansible_role_dockerhost/vars/main.yml) via `ansible_role_dockerhost_architecture_map`.

## Default Variables

| Variable                                  | Default                                                                                              | Description                           |
|-------------------------------------------|------------------------------------------------------------------------------------------------------|---------------------------------------|
| `ansible_role_dockerhost_repo_url`        | `"https://download.docker.com/linux/ubuntu"`                                                         | Base URL of the Docker APT repository |
| `ansible_role_dockerhost_gpg_key_url`     | `"https://download.docker.com/linux/ubuntu/gpg"`                                                     | URL of the Docker GPG key             |
| `ansible_role_dockerhost_packages`        | `["docker-ce", "docker-ce-cli", "containerd.io", "docker-compose-plugin", "python3-docker"]`         | Packages to install                   |
| `ansible_role_dockerhost_release_channel` | `"stable"`                                                                                           | Docker release channel                |
| `ansible_role_dockerhost_package_state`   | `"present"`                                                                                          | Desired package state                 |

## Role Variables

The role also defines a fixed architecture mapping in [vars/main.yml](/home/max/Projects/ansible_roles/ansible_role_dockerhost/vars/main.yml):

| Variable                                  | Value                                     | Description                         |
|-------------------------------------------|-------------------------------------------|-------------------------------------|
| `ansible_role_dockerhost_architecture_map`| `{"x86_64": "amd64", "aarch64": "arm64"}` | Mapping from Ansible to Docker arch |

## Example

```yaml
- name: Install Docker on hosts
  hosts: docker_hosts
  become: true
  roles:
    - role: ansible_role_dockerhost
```

An example with a different package strategy:

```yaml
- name: Install Docker from the test channel
  hosts: docker_hosts
  become: true
  roles:
    - role: ansible_role_dockerhost
      vars:
        ansible_role_dockerhost_release_channel: test
        ansible_role_dockerhost_package_state: latest
```

## Tags

| Tag                   | Purpose                                  |
|-----------------------|------------------------------------------|
| `docker_repository`   | Runs Docker repository setup             |
| `docker_install`      | Runs Docker installation                 |
| `docker_check`        | Runs Docker verification                 |

## License

Unlicense

## Author

Max Bergmann
