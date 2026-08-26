---
name: nasuni-fleet-health-review
description: Review the health and performance of every Nasuni Edge Appliance in an account using the Nasuni Portal API Ops IQ telemetry surface, and produce a ranked list of appliances needing attention.
api: Nasuni Portal API
spec: openapi/nasuni-portal-v0-openapi.yml
generated: '2026-08-26'
method: generated
source: Grounded in verified operationIds from openapi/_original/nasuni-portal-v0-openapi.json
operations:
  - get_health_get
  - get_edges_edges_get
  - get_edges_edge_edges__edge_id__get
  - get_appliance_health_score_telemetry_appliances__serial_number__appliance_health_score_post
  - get_current_appliance_performance_telemetry_appliances__serial_number__current_appliance_performance_post
  - get_appliance_cpu_usage_telemetry_appliances__serial_number__cpu_utilization_post
  - get_appliance_memory_utilization_telemetry_appliances__serial_number__memory_utilization_post
  - get_appliance_cache_utilization_telemetry_appliances__serial_number__cache_utilization_post
  - get_appliance_cache_hits_misses_telemetry_appliances__serial_number__cache_hits_misses_post
  - get_appliance_cache_disk_io_latency_telemetry_appliances__serial_number__cache_disk_io_latency_post
  - get_appliance_smb_connections_telemetry_appliances__serial_number__smb_connections_post
---

# Fleet health review

Read-only. Nothing in this skill mutates the estate.

## Before you start

- Base URL is regional. Use `https://am1.portal.api.nasuni.com` (Americas),
  `https://eu1.portal.api.nasuni.com` (EU) or `https://ap1.portal.api.nasuni.com` (Asia-Pacific).
  Pick the one assigned to the account; calling the wrong region will not fall through.
- Authenticate with `POST /auth/token` (`authorize_auth_token_post`) using
  `grant_type=client_credentials`, `client_id` (service key) and `client_secret` (service secret),
  form-encoded. Send the returned JWT as `Authorization: Bearer <token>` thereafter.
- Rate limit: **500 requests per 30-second window** and 1,000,000 per day. On a 429, read
  `Retry-After` (seconds) — but note the daily-limit 429 omits that header, so treat a 429 with no
  `Retry-After` as a hard stop for the day, not a retryable blip.
- This is a fan-out workflow. A 50-appliance fleet at 7 telemetry calls each is 350 requests —
  comfortably inside the burst window only if you pace it. Batch per appliance, not per metric.

## Steps

1. `get_health_get` — `GET /health`. If this is not healthy, stop and report; downstream telemetry
   will be unreliable.
2. `get_edges_edges_get` — `GET /edges`. This is the fleet roster. Keep both `edge_id` and the
   appliance serial: the telemetry paths key on `serial_number`, **not** on `edge_id`.
3. For each appliance, `get_edges_edge_edges__edge_id__get` — `GET /edges/{edge_id}` — for machine
   specs (CPU count, RAM, disks) and build version. You need the core count to interpret load
   average, and the build version because memory telemetry returns a detailed breakdown only on
   v10.0.4+ and legacy counters below it.
4. For each appliance serial, call in this order and stop early on a clear failure:
   - `get_appliance_health_score_telemetry_appliances__serial_number__appliance_health_score_post`
   - `get_current_appliance_performance_telemetry_appliances__serial_number__current_appliance_performance_post`
   - `get_appliance_cpu_usage_telemetry_appliances__serial_number__cpu_utilization_post`
   - `get_appliance_memory_utilization_telemetry_appliances__serial_number__memory_utilization_post`
   - `get_appliance_cache_utilization_telemetry_appliances__serial_number__cache_utilization_post`
   - `get_appliance_cache_hits_misses_telemetry_appliances__serial_number__cache_hits_misses_post`
   - `get_appliance_cache_disk_io_latency_telemetry_appliances__serial_number__cache_disk_io_latency_post`
   - `get_appliance_smb_connections_telemetry_appliances__serial_number__smb_connections_post`

   These are all **POST** operations even though they read data — the time window goes in the
   request body, not the query string.
5. Rank. A low cache hit ratio with high cache utilisation and rising cache-disk latency is the
   classic under-sized-cache signature; report it as a sizing finding, not an outage.

## Errors

- `401` — token expired. Re-issue at `POST /auth/token`; there is also
  `refresh_auth_token_refresh_post`.
- `403` — the service key's role lacks the permission. Check with `get_my_roles_and_permissions_iam_me_get`
  (`GET /iam/me`) before assuming the appliance is missing.
- `408` — 43 Portal operations declare a 408; telemetry queries over long windows are the usual
  cause. Narrow the window and retry.
- `422` — validation. The Portal returns `{message, detail}`; `detail` is untyped, so log it raw.
- `500` — 116 operations declare a 500. Retry once, then report; do not loop.

Full envelope reference: `errors/nasuni-problem-types.yml`.
