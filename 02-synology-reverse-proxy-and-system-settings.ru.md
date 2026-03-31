# Synology Reverse Proxy и настройки DSM

Этот файл описывает настройки Synology DSM, которые нужно выполнить перед развертыванием стека n8n.

## 1) Создайте правило reverse proxy

В DSM:

`Control Panel -> Login Portal -> Advanced -> Reverse Proxy -> Create`

Используйте значения:

- **Name**: `n8n`
- **Source protocol**: `HTTPS`
- **Source hostname**: `n8n.yourname.synology.me`
- **Source port**: `443`
- **Enable HSTS**: `true`
- **Destination protocol**: `HTTP`
- **Destination hostname**: `localhost`
- **Destination port**: `5678`

## 2) Добавьте WebSocket-заголовки

В том же правиле reverse proxy:

- откройте **Custom Header**
- нажмите **Create**
- выберите **WebSocket**
- сохраните

Это необходимо для корректной работы realtime/editor функций n8n за reverse proxy.

## 3) Включите сетевые оптимизации DSM

В DSM:

- `Control Panel -> Network -> Connectivity`: включите `HTTP/2`
- `Control Panel -> Security -> Advanced`: включите `HTTP Compression`

## 4) Подготовьте папки на NAS

Создайте:

- `/volume1/docker/n8n`
- `/volume1/docker/n8n/data`
- `/volume1/docker/n8n/db`
- `/volume1/docker/n8n/files`

Используйте имена только в нижнем регистре, как показано выше.
