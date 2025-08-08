<a id="readme-top"></a>

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="_images/Grayskull_Icon.png" />
    <img src="_images/Grayskull_Icon.png" alt="Castle Grayskull Homelab Logo" width="140" />
  </picture>
  <h1>GeekTekRob Homelab</h1>
  <p>A modular, self‑hosted homelab stack powered by Docker & Traefik – reverse proxy, identity, sync, dashboard, management & secure remote access.</p>
  <p>
  <a href="https://github.com/GeekTekRob/gtr_homelab/issues">Report Bug</a>
    ·
  <a href="https://github.com/GeekTekRob/gtr_homelab/issues">Request Feature</a>
  </p>
</div>

<div align="center">

![Status](https://img.shields.io/badge/status-active-success?style=flat-square)
![License](https://img.shields.io/github/license/GeekTekRob/gtr_homelab?style=flat-square)
![Last Commit](https://img.shields.io/github/last-commit/GeekTekRob/gtr_homelab?style=flat-square)
![Issues](https://img.shields.io/github/issues/GeekTekRob/gtr_homelab?style=flat-square)
![Pull Requests](https://img.shields.io/github/issues-pr/GeekTekRob/gtr_homelab?style=flat-square)

</div>

---

## 📚 Table of Contents

- [📚 Table of Contents](#-table-of-contents)
- [🏰 About](#-about)
- [✨ Features](#-features)
- [🧱 Stack Overview](#-stack-overview)
- [🗂 Repository Layout](#-repository-layout)
- [⚡ Quick Start](#-quick-start)
  - [Prerequisites](#prerequisites)
  - [Clone](#clone)
  - [Configuration](#configuration)
- [🛎 Services](#-services)
- [🔐 Environment Variables](#-environment-variables)
- [🛡 Security Notes](#-security-notes)
- [🗄 Backups](#-backups)
- [🧪 Troubleshooting](#-troubleshooting)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)
- [📬 Contact](#-contact)

---

## 🏰 About

The GeekTekRob Homelab is a flexible Docker Compose collection for running a personal/home infrastructure. It emphasizes:

* A single entry point (Traefik) for TLS, routing & observability.
* Centralized authentication (Keycloak) so downstream apps delegate auth & SSO.
* Clear separation of concerns by service directory (each with its own compose file) enabling partial deployments.
* Simplicity: only widely adopted, actively maintained containers.

> Goal: Make it easy to spin up, tear down, and evolve a homelab stack safely.

## ✨ Features

* Reverse proxy & automatic HTTPS (Let's Encrypt via Traefik)
* Central identity & SSO (Authentik)
* File synchronization (FreeFileSync containerization wrapper)
* Dashboard landing page (Homepage) with service tiles
* Container lifecycle & metrics visibility (Portainer)
* Secure remote access (WireGuard VPN)
* Modular service composition – start only what you need
* Clean directory structure & sample configs
* Designed for `.env` overrides and future expansion

## 🧱 Stack Overview

| Layer | Purpose | Key Components |
|-------|---------|----------------|
| Edge / Ingress | TLS termination, routing, metrics | Traefik |
| Identity | Authentication, SSO, policies | Keycloak |
| Applications | Dashboard, sync | Homepage, FreeFileSync |
| Management | Orchestration & monitoring | Portainer, Watchtower (if added) |
| Access | Remote connectivity | WireGuard |

## 🗂 Repository Layout

```
./
├── docker-compose.yml        # (Optional) root aggregator / future meta compose
├── services/
│   ├── traefik/              # Reverse proxy + ACME data
│   ├── keycloak/             # (If evaluating alternative IdP)
│   ├── homepage/             # Dashboard
│   ├── freefilesync/         # File sync container definition
│   ├── portainer/            # Management UI
│   ├── wireguard/            # VPN access
│   └── watchtower/           # Automated image updates
└── _images/                  # Project artwork (logo etc.)
```

> All services listed are present in this repository. Add more by creating a new folder under `services/` with its own compose file.

## ⚡ Quick Start

### Prerequisites

Install:
* [Docker Engine](https://docs.docker.com/get-docker/)

Optional helpful tools:
* `make` (Linux/macOS) or PowerShell aliases on Windows
* A domain & DNS provider that supports ACME DNS challenge (if using wildcard certs)

### Clone

```bash
git clone https://github.com/GeekTekRob/gtr_homelab.git
cd gtr_homelab
```

### Configuration

1. Copy any provided sample files (e.g. `traefik/data/traefik-sample.yml`) to their active names (`traefik.yml`, `config.yml`, etc.).
2. If a `.env.example` exists, copy to `.env` and adjust values (domain, email, timezone, network names, etc.).
3. Review each `services/<service>/docker-compose.yml` for volume paths you may want to adapt (bind mounts vs. named volumes).
4. Create required external Docker networks up-front if referenced (e.g. `proxy`):
   ```bash
   docker network create proxy || true
   ```

## 🛎 Services

| Service | Role | Upstream Docs |
|---------|------|---------------|
| Traefik | Reverse proxy, ACME, middleware | https://doc.traefik.io |
| Keycloak | Identity Provider | https://www.keycloak.org |
| Homepage | Dashboard / launchpad | https://gethomepage.dev |
| Portainer | Container management UI | https://docs.portainer.io |
| WireGuard | Encrypted VPN tunneling | https://www.wireguard.com |
| FreeFileSync | File sync tool (containerized) | https://freefilesync.org |
| Watchtower | Auto update images (optional) | https://containrrr.dev/watchtower/ |
| (Add more) | Extend with additional apps by adding service folders | — |

## 🔐 Environment Variables

Place global values in `.env` at repo root. Examples (illustrative only):

```dotenv
TZ=America/Chicago
DOMAIN=example.com
TRAEFIK_EMAIL=you@example.com
TRAEFIK_ACME_CA_SERVER=https://acme-v02.api.letsencrypt.org/directory
```

Service‑specific overrides often live in dedicated subfolders or additional env files referenced with `env_file:` in compose.

## 🛡 Security Notes

* Never commit real API tokens (e.g. `traefik/cf_api_token.txt` should be in `.gitignore`).
* Prefer DNS‑01 challenges for wildcard certs so internal services stay behind VPN / zero trust.
* Restrict dashboard (Traefik, Portainer) via IP allowlist or auth middleware.
* Keep Watchtower usage deliberate – auto updating everything can introduce breaking changes.
* WireGuard peers should use keys generated locally; rotate on compromise.

## 🗄 Backups

Recommended backup targets:

* Traefik ACME JSON (certificates)
* Identity provider database (e.g., Postgres if used by Authentik/Keycloak)
* WireGuard configs (peer public keys only – keep private keys secure)

## 🧪 Troubleshooting

| Symptom | Tip |
|---------|-----|
| 404 / router not found | Check labels & that container is on the `proxy` network |
| TLS fails / rate limit | Verify ACME email & challenge method, remove stale ACME JSON only if necessary |
| Auth redirects loop | Check trusted proxy / public URL config in IdP |
| Service not resolving | Ensure DNS (local or public) points to Traefik host IP |
| WireGuard handshake fails | Check UDP port forwarding & peer endpoint correctness |

## 🤝 Contributing

Contributions, ideas & issues are welcome. Please:

1. Open an issue to discuss significant changes.
2. Fork & create a feature branch: `git checkout -b feat/your-idea`.
3. Commit with conventional style (e.g., `feat: add watchtower service`).
4. Open a PR and link any related issues.

## 📄 License

Distributed under the MIT License. See [`LICENSE`](LICENSE) for details.

## 📬 Contact

Project Link: https://github.com/GeekTekRob/gtr_homelab

---

<p align="center"><sub>Powered by curiosity, caffeine, and Castle Grayskull.</sub></p>

<p align="right">(<a href="#readme-top">back to top</a>)</p>
