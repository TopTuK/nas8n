# Synology Reverse Proxy and DSM Settings

This file covers the Synology DSM setup required before deploying the n8n stack.

## 1) Create reverse proxy entry

In DSM:

`Control Panel -> Login Portal -> Advanced -> Reverse Proxy -> Create`

Use these values:

- **Name**: `n8n`
- **Source protocol**: `HTTPS`
- **Source hostname**: `n8n.yourname.synology.me`
- **Source port**: `443`
- **Enable HSTS**: `true`
- **Destination protocol**: `HTTP`
- **Destination hostname**: `localhost`
- **Destination port**: `5678`

## 2) Add WebSocket headers

Inside the same reverse proxy rule:

- Open **Custom Header**
- Click **Create**
- Select **WebSocket**
- Save

This is required for n8n realtime/editor behavior behind reverse proxy.

## 3) Enable DSM network optimizations

In DSM:

- `Control Panel -> Network -> Connectivity`: enable `HTTP/2`
- `Control Panel -> Security -> Advanced`: enable `HTTP Compression`

## 4) Prepare folders on NAS storage

Create:

- `/volume1/docker/n8n`
- `/volume1/docker/n8n/data`
- `/volume1/docker/n8n/db`
- `/volume1/docker/n8n/files`

Use lowercase names exactly as shown.
