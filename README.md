# Counter-Strike 1.6 Server Deployment

Infrastructure-as-Code project for deploying Counter-Strike 1.6 dedicated servers on DigitalOcean or locally using Docker and Ansible.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- [doctl](https://docs.digitalocean.com/reference/doctl/how-to/install/) - DigitalOcean CLI
- [Ahoy CLI](https://github.com/ahoy-cli/ahoy) - Task runner
- DigitalOcean account with API token configured
- SSH key pair in `~/.ssh/`

## Quick Start

### DigitalOcean Deployment

1. **Configure environment variables:**
   ```bash
   ahoy do env
   ```
   This interactively sets up droplet size, region, Ubuntu image, and SSH key.

2. **Create and deploy the droplet:**
   ```bash
   ahoy do up
   ```

3. **Provision the server with Docker and CS 1.6:**
   ```bash
   ahoy ansible setup
   ```

4. **Connect to your server:**
   ```bash
   ahoy do ssh
   ```

5. **View server logs:**
   ```bash
   ahoy do logs
   ```

### Local Development

Run the CS 1.6 server locally for testing:

```bash
ahoy docker build
ahoy docker up
```

## Available Commands

### DigitalOcean (ahoy do)

| Command | Description |
|---------|-------------|
| `ahoy do env` | Setup all environment variables (size, region, image, SSH key) |
| `ahoy do up` | Create and deploy DigitalOcean droplet |
| `ahoy do ssh` | SSH into the server |
| `ahoy do logs` | View Docker Compose logs on remote server |
| `ahoy do destroy` | Delete the droplet and associated resources |

### Ansible (ahoy ansible)

| Command | Description |
|---------|-------------|
| `ahoy ansible setup` | Install Docker and deploy CS 1.6 server |
| `ahoy ansible create-hosts-file` | Generate Ansible inventory from .env |

### Docker (ahoy docker)

| Command | Description |
|---------|-------------|
| `ahoy docker build` | Build the CS 1.6 Docker image |
| `ahoy docker up` | Start containers locally |
| `ahoy docker down` | Stop containers |
| `ahoy docker logs` | View container logs |

## Cleanup

To destroy all DigitalOcean resources:

```bash
ahoy do destroy
```

To clean up local Docker resources:

```bash
ahoy docker cleanup
```
