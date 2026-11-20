# 9. Security Notes and GitHub Publishing

## Never publish secrets

Before uploading project files, inspect every file for:

- API keys (including AbuseIPDB keys)
- Telegram bot tokens
- TheHive API tokens
- Passwords and default credentials
- Shuffle webhook URLs containing private hook identifiers
- Private keys and TLS certificates
- `.env` files and secret configuration files
- Real personal data or sensitive endpoint logs

A public repository should contain sanitised examples only. Replace secret values with placeholders such as `YOUR_API_KEY` and `YOUR_PRIVATE_WEBHOOK_URL`.

## Rotate exposed credentials

If a real Telegram bot token or API key has previously been exposed in terminal output, screenshots, workflow exports, or chat, treat it as compromised. Revoke/regenerate it with the provider and update the corresponding Shuffle credential.

## Suggested .gitignore

Review and adapt this list to the actual project layout:

```gitignore
.env
.env.*
!.env.example
*.pem
*.key
*.p12
*.pfx
*.crt
*.cer
secrets/
certs/
credentials/
**/config/secrets*
**/config/private*
.DS_Store
Thumbs.db
```

Do not rely on `.gitignore` alone: files already tracked by Git remain tracked. Inspect staged files before committing.

## Safe publishing checklist

1. Review the repository tree.
2. Review Compose files and configuration for inline passwords and tokens.
3. Replace credentials with placeholders in example files.
4. Remove certificates, private keys, local databases, and runtime data.
5. Review screenshots for browser URLs, tokens, email addresses, hostnames, and personal information.
6. Add a `.gitignore`.
7. Check the final diff before pushing.

Useful local checks:

```bash
git status --short
git diff --cached
git grep -n -I -E 'token|api[_-]?key|password|secret|hook_url' -- .
```

These searches can produce false positives, but they help identify files that need manual review. Do not paste discovered secrets into issues or commit messages.

## Public repository guidance

Keep documentation, sanitised configuration examples, architecture diagrams, and redacted screenshots in GitHub. Keep live credentials, local runtime state, private logs, and certificate material outside the repository.
