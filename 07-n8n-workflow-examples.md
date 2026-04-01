# n8n Workflow Examples

This page contains reusable workflow examples that you can import into your n8n instance.

## 1) GitHub PR Code Review Workflow

This example automates pull request reviews in GitHub:

- triggers when a `pull_request` event is received,
- fetches PR file diffs via GitHub API,
- builds a review prompt from changed files,
- sends the prompt to an AI agent,
- posts the generated review back to the PR,
- sends a Telegram message with the review result,
- optionally adds the `ReviewedByAI` label.

Workflow JSON file:

- [`examples/workflows/code-review-workflow.json`](./examples/workflows/code-review-workflow.json)

## 2) How to import in n8n

1. Open your n8n editor.
2. Click `Import from file`.
3. Select `examples/workflows/code-review-workflow.json`.
4. Configure credentials in nodes:
   - GitHub OAuth2 / GitHub API
   - OpenAI API
   - Google Sheets OAuth2 (optional tool)
   - Telegram API
5. Set repository-specific values (`owner`, `repository`, labels) where required.
6. Set `YOUR_TELEGRAM_CHAT_ID` in the `Telegram Notification` node.
7. Activate the workflow when ready.

## 3) Important notes

- Review generated comments before using this in production workflows.
- Keep API credentials and secrets outside of versioned public files.
- Tune model and prompt instructions to match your team's review standards.
