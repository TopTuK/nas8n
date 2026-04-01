# n8n on Synology NAS (Docker)

Installation instructions for running `n8n` in Docker on Synology NAS, organized into reusable Markdown guides.

Base article used as source:

- [How to Install n8n on Your Synology NAS](https://mariushosting.com/how-to-install-n8n-on-your-synology-nas/)

## Localization

- Russian version: [README.ru.md](./README.ru.md)

## Documentation Index

- [01 - Overview and Prerequisites](./01-overview-and-prerequisites.md)
- [02 - Synology Reverse Proxy and System Settings](./02-synology-reverse-proxy-and-system-settings.md)
- [03 - Docker Stack Deployment (Portainer)](./03-docker-stack-deployment-portainer.md)
- [04 - First Login and License](./04-first-login-and-license.md)
- [05 - Troubleshooting and Maintenance](./05-troubleshooting-and-maintenance.md)
- [06 - Optional: Route n8n Traffic Through VLESS Reality VPN (Xray)](./06-optional-n8n-through-vpn-xray.md)

## Quick Start

1. Complete Synology setup and folders from `02`.
2. Deploy the stack from `03` (edit secrets and host values first).
3. Open your HTTPS URL and complete onboarding in `04`.
4. Optional: if you need all n8n outbound traffic over VPN, apply `06`.
5. Use `05` for operations and troubleshooting.

## Notes

- Replace all placeholder values before production use.
- Never commit real credentials or encryption keys to public repositories.
