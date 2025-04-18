# Ansible Role: ansible_role_traefik_with_cloudflare

![Ansible](https://img.shields.io/badge/ansible-ready-blue.svg)
![Platform](https://img.shields.io/badge/platform-Ubuntu-lightgrey)
![License](https://img.shields.io/badge/license-Unlicense-green)

## Description

This Ansible role installs and configures **Traefik** as a reverse proxy using a containerized setup with Docker Compose.  
It includes configuration for automatic HTTPS via ACME DNS challenge through **Cloudflare**, sets up a systemd unit to manage the stack, and exposes the Traefik dashboard behind basic authentication.

### Features:
- Deploys Traefik via Docker Compose
- Enables ACME DNS challenge with Cloudflare
- Configures static and dynamic Traefik settings
- Activates and secures the dashboard endpoint
- Provides a custom systemd service for lifecycle management

## Role Variables

The following variables can be set (see `defaults/main.yml`):

| Variable | Default | Description |
|----------|---------|-------------|
| `ansible_role_traefik_with_cloudflare_email` | `admin@example.com` | Email address for ACME registration |
| `ansible_role_traefik_with_cloudflare_traefik_user` | `traefik` | Linux user to run the Traefik systemd unit |
| `ansible_role_traefik_with_cloudflare_traefik_group` | `traefik` | Linux group for Traefik |
| `ansible_role_traefik_with_cloudflare_docker_compose_dir` | `/opt/traefik` | Directory where Traefik's Docker Compose stack is deployed |

## Usage Example

```yaml
- name: Deploy Traefik with Cloudflare DNS challenge
  hosts: all
  become: true
  roles:
    - role: ansible_role_traefik_with_cloudflare
```
> **Important:**  
> ⚠️ **If you skip this step, the role will fail.**  
> Before running this role, you **must** set your Cloudflare credentials in a `.env` file.
>
> The simplest way to do this:
>
> 1. Copy the example file to the deployment target:
>    ```bash
>    cp example.env {{ ansible_role_traefik_with_cloudflare_docker_compose_dir }}/.env
>    ```
> 2. Edit the copied `.env` file and set the following variables with your Cloudflare API credentials:
>    - `CF_DNS_API_TOKEN=`
>
> **Advanced alternatives for production setups:**
>
> - **Ansible Vault:**  
>   Encrypt your `.env` file with `ansible-vault` and store it in your private inventory or repository.  
>   The role will copy it to the correct location during execution.
>
> - **Secret Manager Integration:**  
>   For production or CI/CD use cases, consider secret managers like HashiCorp Vault or AWS Secrets Manager.  
>   You can dynamically inject `CF_DNS_API_TOKEN` using Ansible plugins for maximum security.

## Sanity Checks

This role ensures:
- Traefik is deployed via a Docker Compose stack
- Required ports (`80`, `443`, `8080`) are exposed
- Static and dynamic config files are in place
- Systemd unit is available to manage the stack
- ACME challenge setup is valid and ready for certificate issuing
- Docker labels are correctly set for container routing

## Directory Structure

Follows standard Ansible role layout:

```
ansible_role_traefik_with_cloudflare/
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
│   ├── docker-compose.yml.j2
│   ├── traefik.yml.j2
│   └── traefik-docker.service.j2
└── README.md
```

✔️ Designed for secure, automated reverse proxy setups in homelab and production environments.

## License

Unlicense

## Author Information

Role maintained by Max Bergmann.  
Feedback, suggestions, and contributions are always welcome! 🚀
