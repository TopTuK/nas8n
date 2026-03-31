# n8n on Synology NAS: Overview and Prerequisites

This repository provides step-by-step instructions to install `n8n` with Docker on a Synology NAS, based on the source guide:

- [How to Install n8n on Your Synology NAS (Marius Hosting)](https://mariushosting.com/how-to-install-n8n-on-your-synology-nas/)

## What you will deploy

- `n8n` container (`n8nio/n8n:latest`)
- PostgreSQL container (`postgres:18`) for workflow data
- Persistent volumes on Synology storage
- HTTPS access via Synology Reverse Proxy and DDNS hostname

## Requirements

- Synology NAS with Docker/Container Manager support
- DSM admin access
- Portainer installed (optional but used in these instructions)
- A public DDNS name (for example `yourname.synology.me`)
- A valid SSL certificate (wildcard or host-specific)

## Recommended preparation

1. Confirm your NAS model supports Docker.
2. Install/update Portainer if you plan to deploy via Stacks.
3. Prepare your hostname for n8n, for example:
   - `n8n.yourname.synology.me`
4. Decide your timezone (examples: `Europe/Bucharest`, `UTC`, `America/New_York`).
5. Generate and save a strong 64-character `N8N_ENCRYPTION_KEY`.

## Security notes

- Do not commit real secrets (DB passwords, encryption keys) to a public repository.
- Use long random values for:
  - `POSTGRES_PASSWORD`
  - `N8N_ENCRYPTION_KEY`
- If exposed to the internet, keep HTTPS enabled and update containers regularly.
