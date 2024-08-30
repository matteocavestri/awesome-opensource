# Networking Services

Networking services are essential for configuring a single-node Docker host to expose your services to the internet and protect against potential attacks.

There are several services available, but only two are essential:

- **Traefik:** A reverse proxy to securely route all services through port 443.
- **CrowdSec:** An open-source cybersecurity service.

## Getting Started

To begin, you need to deploy the two main services. Follow the instructions in their respective guides:

- [Traefik Deployment](networking/traefik/README.md)
- [CrowdSec Deployment](networking/crowdsec/README.md)

## Next Steps

- **Deploy Authentik:**  
   After setting up these minimal services, deploy Authentik to establish a complete minimal infrastructure.

## Support

For questions or issues, feel free to open an issue or use the GitHub discussion feature.
