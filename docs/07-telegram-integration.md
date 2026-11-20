# 7. Telegram Notifications

## Purpose

Telegram is the notification channel selected for this lab. Email notification was not part of the implemented workflow.

## Bot setup

A Telegram bot was used by Shuffle to send a message to the analyst. The bot token is a secret credential. Do not put it in this repository, screenshots, workflow exports, or public chat messages.

Configure the Telegram app/action in Shuffle using the secret token and the intended chat identifier. The exact fields depend on the Telegram app version installed in Shuffle.

## Notification content

A useful SOC notification should be short and actionable. For example, include:

- Alert title
- Severity and Wazuh rule ID
- Endpoint/agent name
- Source IP, if available
- AbuseIPDB confidence score, if returned
- TheHive case identifier or link, if available

Avoid sending full raw logs or sensitive endpoint details to a group chat unless the channel is appropriately restricted.

## Recorded validation

During the 16 November 2026 end-to-end test, the Telegram send-message action returned HTTP 200.

## Security

If a bot token has been exposed, revoke/regenerate it using the official Telegram bot management process and update the Shuffle credential. Never reuse an exposed token.
