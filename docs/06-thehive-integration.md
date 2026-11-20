# 6. TheHive Case Management Integration

## Purpose

TheHive is used to create a case from a security alert so that an analyst can review and track the incident.

## Integration flow

The Shuffle workflow prepares case data from the incoming Wazuh alert and enrichment result, then invokes the TheHive application action to create a case.

The case summary should be concise and useful to an analyst. Suggested information includes:

- Alert title or rule description
- Wazuh rule ID and severity
- Agent/endpoint name
- Source IP, when present
- AbuseIPDB reputation summary, when available
- Event timestamp
- Original alert reference or relevant event details

Map only fields that exist in the incoming event. Avoid putting secrets or unnecessary personal information in case descriptions.

## Recorded validation

On 16 November 2026, the Shuffle TheHive case-creation action returned HTTP 201, indicating that the API accepted the case creation request. Earlier manual test cases were also created in TheHive.

## Troubleshooting

- HTTP 401/403: verify the TheHive API credential and permissions.
- HTTP 400: inspect required case fields and the JSON/body mapping.
- Connection errors: check the TheHive URL and Docker network reachability from the Shuffle execution environment.
- Duplicate cases: consider adding a stable alert identifier or deduplication check before case creation.

The exact TheHive deployment configuration and API token are intentionally not included in this public documentation. Store credentials in the tool's secret/credential configuration.
