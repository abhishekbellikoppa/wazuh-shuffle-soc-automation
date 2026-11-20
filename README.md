# Wazuh + Shuffle SOC Automation Lab

A hands-on home Security Operations Center (SOC) lab built with open-source tools. The project connects Wazuh for security monitoring with Shuffle for workflow automation, enriches suspicious IP addresses using AbuseIPDB, creates cases in TheHive, and sends Telegram notifications.

> **Project status note:** This repository documents the lab build and the tests performed during development. Some components were successfully tested end-to-end, while later workflow and service restarts exposed issues that still need validation. See [Testing and current status](docs/08-testing-and-troubleshooting.md) before treating the workflow as production-ready.

## Architecture

```text
Windows host
  └── WSL 2 / Ubuntu
       └── Docker
            ├── Wazuh Manager + Indexer + Dashboard
            ├── Shuffle (SOAR)
            └── TheHive

Wazuh alert
   └── Shuffle webhook
        ├── Parse / pass alert data
        ├── AbuseIPDB IP reputation lookup
        ├── Build an alert summary
        ├── Create a case in TheHive
        └── Send a Telegram notification
```

## Tools used

| Tool | Purpose |
|---|---|
| Wazuh | SIEM/XDR: endpoint monitoring, log collection, alert generation |
| Shuffle | SOAR: webhook-based workflow orchestration |
| AbuseIPDB | External IP reputation enrichment |
| TheHive | Security incident and case management |
| Telegram Bot | Analyst notification channel |
| Docker / Docker Compose | Run the lab services |
| WSL 2 / Ubuntu | Linux environment on the Windows host |

## Documentation

1. [Lab overview and architecture](docs/01-overview-and-architecture.md)
2. [Environment and Docker setup](docs/02-environment-and-docker-setup.md)
3. [Wazuh setup and agent connection](docs/03-wazuh-setup.md)
4. [Shuffle setup and Wazuh webhook](docs/04-shuffle-and-webhook.md)
5. [AbuseIPDB enrichment](docs/05-abuseipdb-integration.md)
6. [TheHive integration](docs/06-thehive-integration.md)
7. [Telegram notifications](docs/07-telegram-integration.md)
8. [Workflow testing and troubleshooting](docs/08-testing-and-troubleshooting.md)
9. [Security notes and safe GitHub publishing](docs/09-security-and-github.md)

## What the workflow is intended to do

1. Wazuh detects an event on a monitored endpoint.
2. Wazuh sends the alert to a Shuffle webhook.
3. Shuffle extracts the relevant alert information.
4. Shuffle queries AbuseIPDB to check the source IP reputation.
5. Shuffle prepares a readable alert summary.
6. Shuffle creates a case in TheHive.
7. Shuffle sends a Telegram message to notify the analyst.

## Test evidence recorded during development

A successful end-to-end execution was recorded on **16 November 2026**:
- Wazuh webhook event received by Shuffle.
- AbuseIPDB API request returned HTTP 200; the tested IP reputation response showed 100% abuse confidence and 170 reports.
- TheHive case creation returned HTTP 201.
- Telegram message sending returned HTTP 200.

A later rerun showed a Shuffle branch-condition error at the first Shuffle Tools node, causing downstream nodes to be skipped. Therefore, the successful earlier test is evidence of a working run at that point, not proof that the current workflow is continuously healthy.

## Important security warning

This is a learning lab, not a production deployment. Do not publish API keys, Telegram bot tokens, passwords, private keys, certificates, or secret environment files. Review configuration files and screenshots before committing them. See [Security notes](docs/09-security-and-github.md).

## Disclaimer

This project is for educational and defensive security monitoring purposes in an environment you own or are authorized to administer.
