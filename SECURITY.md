# Security Guidelines

This repository should contain workflow logic and documentation only.

Never publish:

- OAuth access or refresh tokens
- Gmail credentials or personal email content
- Google service-account private keys
- Google spreadsheet IDs tied to private data
- Telegram bot tokens or private chat IDs
- n8n encryption keys
- Webhook URLs that accept unauthenticated requests
- Real applicant, employer, or client information

Use placeholders in public examples and reconnect credentials after importing the workflow.

If a secret is committed accidentally, remove it from the repository history and rotate it immediately. Deleting only the latest visible copy is not sufficient.
