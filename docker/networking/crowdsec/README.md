# CrowdSec Docker Compose (Single Node)

[CrowdSec](https://www.crowdsec.net/) is an open-source cybersecurity service. With the following Docker Compose setup, we will deploy CrowdSec to monitor and protect everything routed through Traefik. It is recommended to read the [documentation](https://docs.crowdsec.net/docs/intro/) for more information on how it works and to customize the configuration according to your needs.

## Prerequisites

- Traefik installed

## Installation

1. **Edit** the `.env` file without assigning an API KEY.
   For example, if Traefik logs are located at `/var/log/traefik`, the configuration would be:

   ```plaintext
   ACQUIS=./config/acquis.yaml # Acquisition file path
   DB=./config/database # CrowdSec database path
   CONFIG=./config/data # CrowdSec config data path
   TRAEFIK_LOGS=/var/log/traefik # Traefik logs path (see your configuration in networking/traefik/.env)
   API_KEY= # null during your first run, you'll get it after
   ```

2. **Create the necessary directories if they do not exist:**

   ```bash
   mkdir config/database
   mkdir config/data
   ```

3. **Start the main CrowdSec container, but not the bouncer:**

   ```bash
   docker compose up -d crowdsec
   ```

4. **Create a new bouncer to monitor Traefik.**  
   This will return the API KEY to be used in the `.env` file:

   ```bash
   docker exec crowdsec cscli bouncers add bouncer-traefik
   ```

5. **Copy the API KEY into the `.env` file and restart the container:**

   ```bash
   # Copy the API key into .env
   docker compose down
   docker compose up -d --force-recreate
   ```

6. **CrowdSec configuration is complete.**  
   The next step is to set up Authentik as an authentication system to begin securely deploying services. The guide continues with Authentik setup.

## Usage

- **CrowdSec runs in the background using minimal resources.**  
  It is very stable and requires little to no maintenance.

- **For monitoring, you can use Grafana and Prometheus.**  
  However, Authentik must be configured first.
