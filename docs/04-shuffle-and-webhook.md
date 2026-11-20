# 4. Shuffle Setup and Wazuh Webhook

## Shuffle services

Shuffle was deployed from:

```bash
cd ~/soc-lab/Shuffle
```

The lab used Shuffle backend, frontend, OpenSearch, and Orborus/worker components. The frontend was exposed on host port 3001 (HTTP) and 3443 (HTTPS), and the backend on port 5001 in the recorded Compose setup. OpenSearch was exposed on host port 9201.

Check services:

```bash
docker compose ps
docker compose logs --tail=100
```

## Docker networking

Wazuh and Shuffle were on separate Docker networks initially. The Wazuh manager needed to be connected to a network where it could resolve the Shuffle frontend service name. The manager was subsequently attached to the Shuffle network, and DNS resolution to the Shuffle frontend was verified.

Docker network names are installation-specific. Inspect them before connecting containers:

```bash
docker network ls
docker inspect <container_name>
```

Do not copy an IP address from an old run; container IPs can change.

## Wazuh integration configuration

The Wazuh Docker deployment used a mounted manager configuration file under the Wazuh configuration directory. The integration was configured with:

- Integration name: `shuffle`
- A Shuffle webhook URL
- Alert level threshold
- JSON alert format

The webhook URL contains a unique hook identifier. Treat it as a secret endpoint: do not publish the complete URL.

A conceptual example (not a working endpoint):

```xml
<integration>
  <name>shuffle</name>
  <hook_url>YOUR_PRIVATE_SHUFFLE_WEBHOOK_URL</hook_url>
  <level>3</level>
  <alert_format>json</alert_format>
</integration>
```

Use the exact syntax and integration path required by the Wazuh version deployed in your lab. The live configuration was mounted into the manager container; verify the active configuration rather than editing an unrelated host copy.

## Validation

1. Confirm the Shuffle webhook is enabled.
2. Confirm the Wazuh manager can resolve and reach the Shuffle frontend/service endpoint.
3. Generate a controlled Wazuh test alert.
4. Open Shuffle workflow executions and confirm the webhook-triggered execution appears.
5. Inspect the received event fields before building later workflow actions.

## Common issues encountered

- HTTP 400 when a test payload omitted a field expected by the workflow.
- Wazuh integration process not running or integration not being loaded.
- DNS resolution failure when Wazuh and Shuffle were not on a shared network.
- Shuffle Orborus/worker execution problems while Docker Swarm was not initialised or the worker network was not connected.

The lab later initialised Docker Swarm and connected the relevant worker network. Recheck the current network and worker state if executions remain queued or fail.
