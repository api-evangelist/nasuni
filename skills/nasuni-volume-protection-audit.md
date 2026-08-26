---
name: nasuni-volume-protection-audit
description: Audit snapshot protection and propagation across every Nasuni volume — how often data is protected, how long it takes to reach other sites, and which volumes are falling behind — using the Nasuni Portal API.
api: Nasuni Portal API
spec: openapi/nasuni-portal-v0-openapi.yml
generated: '2026-08-26'
method: generated
source: Grounded in verified operationIds from openapi/_original/nasuni-portal-v0-openapi.json
operations:
  - get_volumes_volumes_get
  - get_volume_volumes__volume_id__get
  - get_volume_details_volumes__volume_id__details_get
  - get_volume_capacity_history_volumes__volume_id__capacity_history_get
  - get_volume_snapshot_details_telemetry_volumes__volume_id__snapshot_details_post
  - get_volume_snapshot_timeline_telemetry_volumes__volume_id__snapshot_timeline_post
  - get_volume_snapshot_content_telemetry_volumes__volume_id__snapshot_content_post
  - get_volume_snapshot_propagation_by_appliance_telemetry_volumes__volume_id__snapshot_propagation_by_appliance_post
  - get_volume_average_time_to_protect_telemetry_volumes__volume_id__average_time_to_protect_post
  - get_volume_connections_volume_connections_get
  - get_snapshot_status_volumes__volume_id__edges__edge_id__snapshots_get
---

# Volume protection audit

Read-only except for the optional step 6, which starts a snapshot. Read the reversibility note
before you use it.

## Steps

1. `get_volumes_volumes_get` — `GET /volumes`. Roster of volumes.
2. `get_volume_connections_volume_connections_get` — `GET /volume_connections`. Which appliances are
   attached to which volume. Propagation only means something relative to this.
3. Per volume, `get_volume_details_volumes__volume_id__details_get` for capacity and configuration.
4. Per volume, the protection metrics:
   - `get_volume_average_time_to_protect_telemetry_volumes__volume_id__average_time_to_protect_post`
     — the headline number: how long from a write landing on an appliance to it being protected.
   - `get_volume_snapshot_details_telemetry_volumes__volume_id__snapshot_details_post`
   - `get_volume_snapshot_timeline_telemetry_volumes__volume_id__snapshot_timeline_post`
   - `get_volume_snapshot_content_telemetry_volumes__volume_id__snapshot_content_post`
5. Per volume, propagation:
   `get_volume_snapshot_propagation_by_appliance_telemetry_volumes__volume_id__snapshot_propagation_by_appliance_post`.
   Total file-availability time is protection time **plus** propagation time to the destination
   appliance; report both, and their sum, or the number is misleading.
6. Optional, and only on explicit human instruction:
   `start_snapshot_volumes__volume_id__edges__edge_id__snapshots_post`
   (`POST /volumes/{volume_id}/edges/{edge_id}/snapshots`) forces a snapshot.
   Check with `get_snapshot_status_volumes__volume_id__edges__edge_id__snapshots_get` first.

## Reversibility

A snapshot in flight can be cancelled with
`cancel_snapshot_volumes__volume_id__edges__edge_id__snapshots_delete`
(`DELETE /volumes/{volume_id}/edges/{edge_id}/snapshots`). Nasuni does **not** state how long that
window stays open, so do not promise one. Once the snapshot completes there is no API operation to
remove it — Nasuni's snapshots are immutable by design and no published contract exposes a delete,
restore or rollback. See `conventions/nasuni-conventions.yml`.

## Errors

`401` re-auth, `403` permission, `404` volume not found (check you are on the right regional base
URL), `408` narrow the telemetry window, `429` back off on `Retry-After`.
