# n8n на Synology NAS (Docker)

Инструкции по установке `n8n` в Docker на Synology NAS, разбитые на отдельные Markdown-файлы.

Базовая статья-источник:

- [How to Install n8n on Your Synology NAS](https://mariushosting.com/how-to-install-n8n-on-your-synology-nas/)

## Содержание документации

- [01 - Обзор и требования](./01-overview-and-prerequisites.ru.md)
- [02 - Reverse Proxy и настройки DSM](./02-synology-reverse-proxy-and-system-settings.ru.md)
- [03 - Развертывание Docker Stack (Portainer)](./03-docker-stack-deployment-portainer.ru.md)
- [04 - Первый вход и лицензия](./04-first-login-and-license.ru.md)
- [05 - Диагностика и обслуживание](./05-troubleshooting-and-maintenance.ru.md)
- [06 - Опционально: трафик n8n через VPN VLESS Reality (Xray)](./06-optional-n8n-through-vpn-xray.ru.md)

## Быстрый старт

1. Выполните настройки Synology и подготовьте папки по файлу `02`.
2. Разверните стек из `03` (сначала замените секреты и значения хоста).
3. Откройте HTTPS URL и завершите первичную настройку из `04`.
4. Опционально: если нужен выходной трафик n8n через VPN, выполните `06`.
5. Для поддержки и диагностики используйте `05`.

## Примечания

- Перед использованием в продакшене замените все значения-заглушки.
- Не публикуйте реальные пароли и ключи шифрования в открытом репозитории.
