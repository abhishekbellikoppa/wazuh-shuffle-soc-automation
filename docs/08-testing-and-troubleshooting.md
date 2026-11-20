# 8. Testing and Troubleshooting

## Test approach

Use a controlled endpoint event to validate the pipeline in stages:

1. Wazuh agent is connected and active.
2. Wazuh generates an alert for the test event.
3. Wazuh integration delivers the alert to the Shuffle webhook.
4. Shuffle receives and parses the alert.
5. AbuseIPDB enrichment returns a response.
6. Shuffle prepares the report/summary.
7. TheHive case creation succeeds.
8. Telegram notification succeeds.

Check each stage independently. A workflow execution marked FINISHED does not necessarily mean every node ran successfully; inspect node statuses and outputs.

## Recorded successful execution

A successful end-to-end execution was recorded on 16 November 2026:

| Stage | Recorded result |
|---|---|
| Wazuh webhook event | Received by Shuffle |
| Shuffle Tools report preparation | Completed in the successful run |
| AbuseIPDB v2 check | HTTP 200 |
| TheHive case creation | HTTP 201 |
| Telegram send message | HTTP 200 |

The AbuseIPDB result for that test showed 100/100 abuse confidence, 170 reports, and country code DE. These are historical test values, not current reputation data.

## Later issue observed

A later rerun on 16 November showed a branch error at the first Shuffle Tools node:

```text
Minimum of one branch's conditions must be correct to continue.
Total: 0 of 1
```

The downstream actions were skipped in that run. This means the earlier successful execution should not be interpreted as proof that the current workflow is fixed.

## Troubleshooting checklist

### Wazuh does not send an alert

- Confirm the endpoint agent is active.
- Confirm the event is visible in Wazuh.
- Check the manager integration configuration.
- Check manager logs and integration process status.
- Confirm the manager can resolve/reach the Shuffle webhook service.

### Shuffle webhook receives no event

- Confirm the webhook is enabled and the correct private URL is configured in Wazuh.
- Confirm the Wazuh and Shuffle containers have the required network connectivity.
- Inspect Shuffle backend logs and workflow execution history.

### Shuffle worker does not execute nodes

- Check Orborus and worker containers.
- Check Docker Swarm status if the deployment uses Swarm workers.
- Confirm worker and backend services can resolve one another on the expected network.

### Branch condition error

- Open the workflow and inspect the branch configuration on the first Shuffle Tools node.
- Verify the condition references an actual output field and that the expected value exists.
- If the workflow does not require branching, remove the branch only if the node's execution model allows a direct path.
- Run a fresh test and inspect the node output. Do not assume a saved workflow edit repaired a previous execution.

### TheHive or Telegram action fails

- Verify the service URL is reachable from the Shuffle worker.
- Verify the credential is valid and has the required permissions.
- Inspect the HTTP status and response body, without sharing tokens or sensitive values.

## Service health reminder

Containers showing `Up` is not sufficient proof that Wazuh Dashboard, Indexer, Shuffle, or TheHive is fully operational. Check application logs and perform a real test action.
