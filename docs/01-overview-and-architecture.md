# 1. Lab Overview and Architecture

## Objective

Build a small, low-cost SOC automation lab to practise alert monitoring, enrichment, case creation, and analyst notification using open-source tools.

## Architecture

The lab runs on a Windows computer. WSL 2 provides Ubuntu, and Docker runs the services.

- **Wazuh** collects endpoint telemetry and generates security alerts.
- **Shuffle** receives Wazuh alerts through a webhook and coordinates response steps.
- **AbuseIPDB** provides IP reputation information for enrichment.
- **TheHive** stores an incident as a case for investigation.
- **Telegram** delivers a notification to the analyst.

```text
Windows 11 host
  └── WSL 2 (Ubuntu)
       └── Docker Engine / Docker Compose
            ├── Wazuh manager
            ├── Wazuh indexer
            ├── Wazuh dashboard
            ├── Shuffle backend / frontend / worker services
            └── TheHive

Wazuh alert → Shuffle webhook → IP reputation lookup
                              → alert summary
                              → TheHive case
                              → Telegram notification
```

## Event flow

1. An event occurs on a monitored endpoint.
2. Wazuh analyses the event and generates an alert.
3. Wazuh forwards the alert to the configured Shuffle webhook integration.
4. Shuffle receives the event and passes the relevant fields to subsequent workflow actions.
5. Shuffle queries AbuseIPDB for IP reputation details.
6. Shuffle prepares a readable summary.
7. Shuffle creates a case in TheHive.
8. Shuffle sends a Telegram notification.

## Scope

This is a learning and demonstration environment. It is not a production SOC deployment. The lab focuses on a Wazuh-to-Shuffle automation path and does not include email notifications or MITRE ATT&CK mapping.

## Important distinction

A successful test execution was recorded during development, but a later run showed a Shuffle branch-condition error. Refer to [Testing and troubleshooting](08-testing-and-troubleshooting.md) for the evidence and current known limitations.
