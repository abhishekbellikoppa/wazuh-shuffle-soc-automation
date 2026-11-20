# 3. Wazuh Setup and Agent Monitoring

## Components

The lab used the Wazuh single-node Docker deployment, with these main services:

- Wazuh manager
- Wazuh indexer
- Wazuh dashboard

The deployment directory was:

```bash
cd ~/soc-lab/wazuh/wazuh-docker/single-node
```

## Start Wazuh

From the deployment directory:

```bash
docker compose up -d
docker compose ps
```

Check service logs when a component does not become healthy:

```bash
docker compose logs --tail=100 wazuh.manager
docker compose logs --tail=100 wazuh.indexer
docker compose logs --tail=100 wazuh.dashboard
```

Service names may vary with the Compose file. Use `docker compose config --services` to list the actual names.

## Dashboard and ports

The lab Compose configuration exposed the dashboard on host port 443, the indexer on 9200, and Wazuh agent/registration ports 1514 and 1515. The manager API was exposed on port 55000 in the recorded setup.

Use the dashboard URL appropriate to your local configuration. A browser certificate warning can occur with a locally generated/self-signed certificate; do not disable certificate validation for general internet browsing.

## Windows endpoint agent

A Windows endpoint agent was enrolled for monitoring. The endpoint was previously shown in the Wazuh dashboard as `Abhi-Windows`. Agent naming can change depending on the registration used.

After installation and registration, verify:

1. The endpoint appears in the Wazuh dashboard.
2. The agent status is active.
3. Recent events are visible.
4. A controlled test event produces an alert.

## Test event

A test failed-logon event was used during workflow testing. A custom alert was also observed with rule ID `99911`, severity level 10, and the event description `Test failed logon`. A source IP was included in the test data.

Use only test events in systems you own or are authorised to monitor.

## Troubleshooting

- Confirm containers are running with `docker compose ps`.
- Review manager and indexer logs.
- Confirm the Windows agent points to the reachable Wazuh manager address, not an incorrect loopback address.
- Check connectivity to the configured agent ports from the endpoint.
- Confirm the agent appears active in the dashboard before testing the Shuffle integration.

A container showing `Up` does not, by itself, prove that the dashboard or indexer is fully ready. Validate the application and service health separately.
