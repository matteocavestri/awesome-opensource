# Docker Single Node Guide

To have a complete configuration on a single-node host, you need basic services that support other deployments you want to run. This guide works whether you choose to store data on the node itself or via an NFS share.

## Prerequisites

- An internet domain (this guide uses [Cloudflare](https://www.cloudflare.com) as a provider)

## Install Docker

To install [Docker](https://www.docker.com/) on major operating systems and distributions, follow the guide, but it's recommended to read the [Docker documentation](https://docs.docker.com/engine/install/).

### Debian

1. **Set up Docker's apt repository.**

   ```bash
   # Add Docker's official GPG key:
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc

   # Add the repository to Apt sources:
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get update
   ```

2. **Install the Docker packages.**

   ```bash
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

### Fedora

1. **Install the dnf-plugins-core package (which provides the commands to manage your DNF repositories) and set up the repository.**

   ```bash
   sudo dnf -y install dnf-plugins-core
   sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
   ```

2. **Install Docker Engine, containerd, and Docker Compose.**

   ```bash
   sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

3. **Start and enable Docker.**

   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

### Post Installation

To run Docker as a non-root user, you need to create a Docker group:

```bash
sudo groupadd docker
```

Add your user to the Docker group.

```bash
sudo usermod -aG docker $USER
```

## Deploy Basic Services

The basic services consist of:

- [Traefik](https://traefik.io/)
- [CrowdSec](https://www.crowdsec.net/)
- [Authentik](https://goauthentik.io/)

These services form the minimum necessary to securely host everything else. However, there are other optional basic services, such as:

- [Minio](https://min.io/)

To continue with the deployment of these services, read the [Networking README](../docker/networking/README.md):

- [Install Traefik](../docker/traefik/README.md)
- [Install CrowdSec](../docker/crowdsec/README.md)
- [Install Authentik](../docker/authentik/README.md)
