# Traefik Docker Compose (Single Node)

This directory contains the basic configuration for [Traefik](https://traefik.io/). It is recommended to read the [documentation](https://doc.traefik.io/traefik/) for more detailed information about Traefik and how it works. This README provides a step-by-step guide to configure Traefik as a reverse proxy to expose your services via HTTPS.

## Prerequisites

- Docker installed on the host
- A domain (this guide uses Cloudflare)

## Install

1. **Edit the `.env` file according to your preferences.**  
   For example, if your domain is `awesomeopensource.org`, you would have the following configuration:

   ```plaintext
   TRAEFIK_YAML=./config/traefik.yml # Path to traefik.yml
   TRAEFIK_ACME=./config/acme.json # Path to acme.json (Must be chmod 600)
   CONFIG_YAML=./config/config.yml # Path to config.yml
   HOST=traefik.awesomeopensource.org # This is your Traefik dashboard
   RESOLVER=cloudflare # Choose a resolver name (e.g., cloudflare)
   DOMAIN=awesomeopensource.org
   SANS=*.awesomeopensource.org
   CF_API=./config/cf_api_token.txt # Path to cf_api_token.txt
   TRAEFIK_LOGS=./config/logs # Traefik logs path
   ```

2. **Change the permissions of the `acme.json` file and/or create it if it doesn't exist.**

   ```bash
   touch config/acme.json # Create acme.json if it does not exist
   chmod 600 config/acme.json # Change permissions
   ```

3. **Add your Cloudflare account email to `config/traefik.yml`.**

   ```yaml
   ---
   cloudflare:
     acme:
       email: examplemail@awesomeopensource.org # Your Cloudflare email
       storage: acme.json
       caServer: https://acme-v02.api.letsencrypt.org/directory # prod (default)
       # caServer: https://acme-staging-v02.api.letsencrypt.org/directory # staging
   ```

4. **Create an API key on Cloudflare with the following permissions:**

   - Zone: Zone read
   - Zone: DNS edit
   - Include: (specific zone or all zones, you can decide)

5. **Paste the API key into the `config/cf_api_token.txt` file.**

6. **Create the proxy network:**

   ```bash
   docker network create proxy
   ```

7. **Start the container:**

   ```bash
   docker compose up -d
   ```

8. **Depending on the configuration in the `config/config.yml` file, deploy CrowdSec and Authentik before accessing the dashboard.**

   The guide continues with the setup and configuration of CrowdSec.

## Usage

1. **Open port 443 on your modem/switch to point to your Docker host.**

2. **From the Cloudflare dashboard, create an A record and a CNAME record:**

   - **A record:** Point to your IP address.
   - **CNAME record:** Point to the host you just created for your Traefik installation (e.g., `traefik` with the target `awesomeopensource.org`).

3. **Use Traefik with other containers and services:**
   - Traefik can now be used by all other containers you deploy.
   - Traefik can also be used by services running outside of Docker by modifying the `config.yml` file.
