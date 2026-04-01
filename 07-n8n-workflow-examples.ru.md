# Примеры n8n workflow

На этой странице собраны переиспользуемые примеры workflow, которые можно импортировать в ваш n8n.

## 1) Workflow для Code Review PR в GitHub

Этот пример автоматизирует review pull request в GitHub:

- срабатывает по событию `pull_request`,
- получает diff измененных файлов через GitHub API,
- формирует prompt для ревью на основе изменений,
- отправляет prompt AI-агенту,
- публикует сгенерированный review в PR,
- отправляет сообщение в Telegram с результатом review,
- опционально добавляет метку `ReviewedByAI`.

JSON-файл workflow:

- [`examples/workflows/code-review-workflow.json`](./examples/workflows/code-review-workflow.json)

## 2) Как импортировать в n8n

1. Откройте редактор n8n.
2. Нажмите `Import from file`.
3. Выберите `examples/workflows/code-review-workflow.json`.
4. Настройте credentials в узлах:
   - GitHub OAuth2 / GitHub API
   - OpenAI API
   - Google Sheets OAuth2 (опциональный tool)
   - Telegram API
5. Укажите значения для вашего репозитория (`owner`, `repository`, labels), где требуется.
6. Укажите `YOUR_TELEGRAM_CHAT_ID` в узле `Telegram Notification`.
7. Активируйте workflow после проверки.

## 3) Важные замечания

- Проверяйте сгенерированные комментарии перед использованием в продакшене.
- Храните API-ключи и секреты вне публичных versioned-файлов.
- Настройте модель и инструкции prompt под стандарты code review вашей команды.
