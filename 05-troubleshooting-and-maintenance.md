# Troubleshooting and Maintenance

## Common issues

## Reverse proxy returns bad gateway

- Verify reverse proxy destination is `localhost:5678`
- Confirm `n8n` container is running and healthy
- Re-check WebSocket custom headers in DSM reverse proxy rule

## n8n cannot connect to PostgreSQL

- Verify `DB_POSTGRESDB_HOST` is `n8n-db`
- Ensure DB credentials match exactly in both services
- Check DB healthcheck status and logs

## Login/session problems behind HTTPS

- Keep `N8N_PROTOCOL=https`
- Use valid SSL certificate for your hostname
- Keep `N8N_SECURE_COOKIE=true` when HTTPS is enabled

## File access restrictions

- If using file nodes, ensure mapped path exists:
  - `/volume1/docker/n8n/files` on host
  - `/files` inside container
- Keep `N8N_RESTRICT_FILE_ACCESS_TO=/files`

## Maintenance checklist

- Update images periodically (`n8nio/n8n`, `postgres`)
- Back up:
  - `/volume1/docker/n8n/data`
  - `/volume1/docker/n8n/db`
  - `/volume1/docker/n8n/files`
- Track disk usage and clean unused Docker artifacts
- Rotate secrets if exposed
- Test restore procedure at least once

## Reference

Original source article:

- [How to Install n8n on Your Synology NAS](https://mariushosting.com/how-to-install-n8n-on-your-synology-nas/)
