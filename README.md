# GTR Homelab

This repository contains a homelab setup using Docker Compose, currently featuring Traefik and Keycloak as active services.

## Active Services

### Traefik
- Location: `services/traefik`
- Purpose: Reverse proxy and SSL termination
- Configuration: Uses `traefik.yml`, `config.yml`, and supports dynamic configuration
- Integrates with Keycloak for secure routing

### Keycloak
- Location: `services/keycloak`
- Purpose: Identity and Access Management
- Configuration: Uses `.env` and `.env.sample` for environment variables
- Backed by a PostgreSQL database container
- Sensitive files like `.env` and generated data are excluded from version control (see `.gitignore`)

## Getting Started
1. Copy `.env.sample` to `.env` in `services/keycloak` and update values as needed
2. Run `docker-compose up -d` from the root to start Traefik and Keycloak

## Security
- Never commit `.env` files with secrets
- Review `.gitignore` for excluded files

## Documentation
- See service folders for more details