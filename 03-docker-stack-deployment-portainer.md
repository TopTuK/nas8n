# Deploy n8n + PostgreSQL Stack in Portainer

This guide uses Portainer Stacks deployment.

## 1) Open Portainer Stack editor

In Portainer:

- `Home -> Live connect` (if needed)
- `Stacks -> + Add stack`
- **Name**: `n8n`

## 2) Paste compose file

Create a `docker-compose.yml` for the stack:

```yaml
services:
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
      TZ: Europe/Bucharest
      POSTGRES_DB: n8n
      POSTGRES_USER: n8nuser
      POSTGRES_PASSWORD: CHANGE_ME_DB_PASSWORD
    restart: on-failure:5

  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    healthcheck:
      test: ["CMD-SHELL", "nc -z 127.0.0.1 5678 || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 90s
    hostname: n8n
    user: 0:0
    security_opt:
      - no-new-privileges:true
    ports:
      - 5678:5678
    volumes:
      - /volume1/docker/n8n/data:/root/.n8n:rw
      - /volume1/docker/n8n/files:/files:rw
    environment:
      N8N_HOST: n8n.yourname.synology.me
      WEBHOOK_URL: https://n8n.yourname.synology.me
      N8N_EDITOR_BASE_URL: https://n8n.yourname.synology.me
      GENERIC_TIMEZONE: Europe/Bucharest
      TZ: Europe/Bucharest
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
```

## 3) Update required values before deploy

- `TZ` and `GENERIC_TIMEZONE` to your timezone
- `N8N_HOST` to your domain without `https://`
- `WEBHOOK_URL` and `N8N_EDITOR_BASE_URL` with `https://`
- `POSTGRES_PASSWORD` and `DB_POSTGRESDB_PASSWORD` to a strong password
- `N8N_ENCRYPTION_KEY` to a unique random 64-character string

## 4) Deploy

- Click **Deploy the stack**
- Wait until Portainer reports success

## 5) Verify container health

Both services should be running:

- `n8n-DB` healthy
- `n8n` healthy and listening on `5678`
