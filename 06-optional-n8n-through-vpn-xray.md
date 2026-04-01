# Optional: Route n8n Traffic Through VLESS Reality VPN (Xray)

Use this guide if you want all outbound connections from `n8n` to go through a VPN client container (`xray`) while keeping inbound HTTPS access through Synology Reverse Proxy.

## 1) Prepare folders

Create folders on Synology (example):

- `/volume1/docker/vpnconfig/xray`
- `/volume1/docker/n8n/db`
- `/volume1/docker/n8n/data`
- `/volume1/docker/n8n/files`

Place your Xray client config in:

- `/volume1/docker/vpnconfig/xray/config.json`

## 2) Create one Portainer stack

In Portainer:

- `Home -> Live connect` (if needed)
- `Stacks -> + Add stack`
- **Name**: `n8n-vpn`

Use this compose file:

```yaml
services:
  xray:
    image: teddysun/xray:latest
    container_name: xray-vpn
    hostname: xray-vpn
    security_opt:
      - no-new-privileges:true
    cap_add:
      - NET_ADMIN
    ports:
      - "127.0.0.1:5678:5678"
    volumes:
      - /volume1/docker/vpnconfig/xray/config.json:/etc/xray/config.json:ro
    command: ["xray", "-config", "/etc/xray/config.json"]
    restart: on-failure:5

  db:
    image: postgres:18
    container_name: n8n-DB
    hostname: n8n-db
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: ["CMD", "pg_isready", "-q", "-d", "n8n", "-U", "n8nuser"]
      timeout: 45s
      interval: 10s
      retries: 10
    volumes:
      - /volume1/docker/n8n/db:/var/lib/postgresql:rw
    environment:
      TZ: Europe/YourCity
      POSTGRES_DB: n8n
      POSTGRES_USER: n8nuser
      POSTGRES_PASSWORD: CHANGE_ME_DB_PASSWORD
    restart: on-failure:5

  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    user: 0:0
    network_mode: "service:xray"
    security_opt:
      - no-new-privileges:true
    healthcheck:
      test: ["CMD-SHELL", "nc -z 127.0.0.1 5678 || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 90s
    volumes:
      - /volume1/docker/n8n/data:/root/.n8n:rw
      - /volume1/docker/n8n/files:/files:rw
    environment:
      N8N_HOST: n8n.example.com
      WEBHOOK_URL: https://n8n.example.com
      N8N_EDITOR_BASE_URL: https://n8n.example.com
      GENERIC_TIMEZONE: Europe/YourCity
      TZ: Europe/YourCity
      N8N_PORT: 5678
      N8N_PROXY_HOPS: 4
      N8N_ENCRYPTION_KEY: CHANGE_ME_64_CHAR_RANDOM_KEY
      N8N_PROTOCOL: https
      NODE_ENV: production
      N8N_DIAGNOSTICS_ENABLED: false
      N8N_RUNNERS_ENABLED: true
      N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS: true
      N8N_RESTRICT_FILE_ACCESS_TO: /files
      N8N_SECURE_COOKIE: true
      DB_TYPE: postgresdb
      DB_POSTGRESDB_DATABASE: n8n
      DB_POSTGRESDB_HOST: n8n-db
      DB_POSTGRESDB_PORT: 5432
      DB_POSTGRESDB_USER: n8nuser
      DB_POSTGRESDB_PASSWORD: CHANGE_ME_DB_PASSWORD
    restart: on-failure:5
    depends_on:
      db:
        condition: service_healthy
      xray:
        condition: service_started
```

## 3) Replace placeholder values before deploy

- `Europe/YourCity` -> your timezone
- `n8n.example.com` -> your public n8n domain
- `CHANGE_ME_DB_PASSWORD` -> strong database password
- `CHANGE_ME_64_CHAR_RANDOM_KEY` -> unique 64-char random key

## 4) Reverse Proxy target

Keep Synology Reverse Proxy destination as:

- `http://127.0.0.1:5678`

Do not expose port `5678` directly on `n8n`; it is published by `xray`.

## 5) Verify VPN egress

After deploy, open n8n container console and check:

```sh
curl -s https://api.ipify.org
```

Expected result: VPN egress IP, not your NAS public IP.

## 6) Common pitfalls

- If you set `network_mode: "container:..."` or `network_mode: "service:..."`, do not set `hostname` on `n8n`.
- Keep this setup in one stack to avoid cross-stack network resolution issues.
- Never publish real VPN URLs, UUIDs, private keys, public keys, short IDs, domains, or IPs in docs.
