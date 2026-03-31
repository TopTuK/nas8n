# n8n на Synology NAS: обзор и требования

Этот репозиторий содержит пошаговые инструкции по установке `n8n` в Docker на Synology NAS на основе исходной статьи:

- [How to Install n8n on Your Synology NAS (Marius Hosting)](https://mariushosting.com/how-to-install-n8n-on-your-synology-nas/)

## Что будет развернуто

- контейнер `n8n` (`n8nio/n8n:latest`)
- контейнер PostgreSQL (`postgres:18`) для данных workflow
- постоянные тома на хранилище Synology
- HTTPS-доступ через Synology Reverse Proxy и DDNS-имя

## Требования

- Synology NAS с поддержкой Docker/Container Manager
- административный доступ к DSM
- установленный Portainer (опционально, но используется в этих инструкциях)
- публичное DDNS-имя (например, `yourname.synology.me`)
- валидный SSL-сертификат (wildcard или для конкретного хоста)

## Рекомендуемая подготовка

1. Убедитесь, что модель NAS поддерживает Docker.
2. Установите/обновите Portainer, если планируете деплой через Stacks.
3. Подготовьте hostname для n8n, например:
   - `n8n.yourname.synology.me`
4. Определите свой часовой пояс (например: `Europe/Bucharest`, `UTC`, `America/New_York`).
5. Сгенерируйте и сохраните надежный 64-символьный `N8N_ENCRYPTION_KEY`.

## Заметки по безопасности

- Не коммитьте реальные секреты (пароли БД, ключи шифрования) в публичный репозиторий.
- Используйте длинные случайные значения для:
  - `POSTGRES_PASSWORD`
  - `N8N_ENCRYPTION_KEY`
- При доступе из интернета обязательно используйте HTTPS и регулярно обновляйте контейнеры.
