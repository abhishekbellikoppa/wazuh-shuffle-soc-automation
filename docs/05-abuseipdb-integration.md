# 5. AbuseIPDB IP Reputation Enrichment

## Purpose

AbuseIPDB is used as an enrichment source. When an alert contains a source IP address, Shuffle can request reputation information and add the result to the analyst-facing summary.

## Shuffle workflow action

The workflow used an HTTP request action to call the AbuseIPDB v2 check endpoint:

```text
GET https://api.abuseipdb.com/api/v2/check
```

The request should include the IP address as a query parameter and an API key in the request headers. Keep the key in a secure credential/secret field supported by the installed Shuffle version. Never put a real key in a public workflow export or screenshot.

Typical request fields:

- Query parameter: `ipAddress` — the IP extracted from the Wazuh alert.
- Header: `Key` — private AbuseIPDB API key.
- Header: `Accept` — `application/json`.

Consult the current AbuseIPDB API documentation for optional parameters and response fields before changing the request.

## Response data

The API response can contain fields such as:

- Abuse confidence score
- Number of reports
- Country code
- ISP and usage information, where available
- Last reported time

Only include fields that are actually present in the returned response. A reputation score is an enrichment signal, not proof that an IP address is malicious.

## Recorded test

During an end-to-end test on 16 November 2026, the AbuseIPDB request returned HTTP 200. The tested response showed an abuse confidence score of 100/100 and 170 reports, with country code DE.

This is the result for that test IP and time only; it should not be generalised to other addresses.

## Troubleshooting

- HTTP 401/403: check the API key and account/API access.
- HTTP 400: verify the IP parameter and request format.
- Empty or missing IP: inspect the incoming Wazuh alert and map the correct field.
- Rate limits: reduce repeated lookups and review the provider's current limits.

Do not query or publish reputation data for addresses unless doing so is permitted by your use case and applicable policies.
