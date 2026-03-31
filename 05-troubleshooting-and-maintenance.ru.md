# Диагностика и обслуживание

## Частые проблемы

## Reverse proxy отдает bad gateway

- проверьте, что destination в reverse proxy: `localhost:5678`
- убедитесь, что контейнер `n8n` запущен и healthy
- повторно проверьте WebSocket custom headers в правиле DSM reverse proxy

## n8n не подключается к PostgreSQL

- проверьте, что `DB_POSTGRESDB_HOST` равен `n8n-db`
- убедитесь, что учетные данные БД совпадают в обоих сервисах
- проверьте статус healthcheck и логи БД

## Проблемы с входом/сессией при HTTPS

- оставьте `N8N_PROTOCOL=https`
- используйте валидный SSL-сертификат для вашего hostname
- оставьте `N8N_SECURE_COOKIE=true` при использовании HTTPS

## Ограничения доступа к файлам

- если используете file nodes, проверьте путь монтирования:
  - `/volume1/docker/n8n/files` на хосте
  - `/files` внутри контейнера
- оставьте `N8N_RESTRICT_FILE_ACCESS_TO=/files`

## Чеклист обслуживания

- регулярно обновляйте образы (`n8nio/n8n`, `postgres`)
- выполняйте резервное копирование:
  - `/volume1/docker/n8n/data`
  - `/volume1/docker/n8n/db`
  - `/volume1/docker/n8n/files`
- контролируйте дисковое пространство и очищайте неиспользуемые Docker-артефакты
- ротируйте секреты при подозрении на компрометацию
- хотя бы один раз протестируйте процедуру восстановления

## Источник

Оригинальная статья:

- [How to Install n8n on Your Synology NAS](https://mariushosting.com/how-to-install-n8n-on-your-synology-nas/)
