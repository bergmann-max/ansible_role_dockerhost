# Ansible Role: dockerhost

![Ansible](https://img.shields.io/badge/ansible-ready-blue.svg)
![Platform](https://img.shields.io/badge/platform-Debian%2FUbuntu-lightgrey)
![License](https://img.shields.io/badge/license-Unlicense-green)

## Description

This Ansible role installs the **Docker Engine**, **Docker CLI**, and **Docker Compose v2 plugin** on any APT-based system (Debian, Ubuntu).  
It uses the **official Docker repository**, ensuring the latest stable versions.

### Features:
- Supports **all Debian-family systems** (Debian, Ubuntu)
- **Architecture-independent**: Automatically detects `amd64` or `arm64`
- Docker Daemon is enabled and running
- Docker Compose v2 installed as **plugin**
- **Sanity Checks** verifying installation success
- Includes a **Handler** to restart Docker service

---

## Supported Platforms

- Debian-based systems (Debian, Ubuntu)
- Architectures: `amd64`, `arm64` (automatically detected)

---

## Role Variables

| Variable                                      | Default Value                                     | Description                                  |
|-----------------------------------------------|--------------------------------------------------|----------------------------------------------|
| `ansible_role_dockerhost_repo_url`            | `https://download.docker.com/linux/ubuntu`       | Docker repository base URL                   |
| `ansible_role_dockerhost_gpg_key_url`         | `https://download.docker.com/linux/ubuntu/gpg`   | URL to Docker's GPG key                      |
| `ansible_role_dockerhost_packages`            | `[docker-ce, docker-ce-cli, containerd.io, docker-compose-plugin]` | Docker packages to install |
| `ansible_role_dockerhost_release_channel`     | `stable`                                         | Docker release channel (e.g., stable, nightly)|
| `ansible_role_dockerhost_package_state`       | `present`                                        | Package state (present, latest)              |

> **Note:**  
> The role dynamically detects system architecture and adjusts repository config automatically (`amd64`, `arm64`).

---

## Usage Example

```yaml
- name: Install Docker Engine, CLI and Compose
  hosts: all
  become: true
  roles:
    - role: ansible_role_dockerhost
```

---

## Tags

| Tag    | Purpose                             |
|-------|------------------------------------|
| docker | Install Docker Engine, CLI & Compose v2 |

---

## Sanity Checks

After installation, the role verifies:

- `docker --version`
- `docker compose version`

Ensuring successful installation on the target host.

---

## Handlers

| Handler         | Description                    |
|----------------|--------------------------------|
| Restart Docker | Restarts the Docker service    |

---

## License

Unlicense

---

## Author Information

Role maintained by **Max Bergmann**.  
Feel free to contribute or raise issues!
