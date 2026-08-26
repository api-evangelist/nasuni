---
name: nasuni-share-provisioning
description: Provision SMB/CIFS shares and NFS exports on a Nasuni volume through the NMC API, including folder quotas and global file locking, with the version and permission checks that make it work first time.
api: Nasuni Management Console (NMC) API
spec: openapi/nasuni-nmc-v1-2-openapi.yml
generated: '2026-08-26'
method: generated
source: >-
  Grounded in verified paths and methods from openapi/_original/nasuni-nmc-v1-2-openapi.json.
  The NMC API publishes no operationIds, so operations are named by METHOD + PATH.
operations:
  - 'POST /auth/login/'
  - 'GET /volumes/'
  - 'GET /volumes/{volume_guid}/'
  - 'GET /volumes/{volume_guid}/filers/'
  - 'GET /volumes/{volume_guid}/filers/{filer_serial}/shares/'
  - 'POST /volumes/{volume_guid}/filers/{filer_serial}/shares/'
  - 'PATCH /volumes/{volume_guid}/filers/{filer_serial}/shares/{share_id}/'
  - 'DELETE /volumes/{volume_guid}/filers/{filer_serial}/shares/{share_id}/'
  - 'POST /volumes/{volume_guid}/filers/{filer_serial}/exports/'
  - 'PATCH /volumes/{volume_guid}/filers/{filer_serial}/exports/{export_id}/'
  - 'DELETE /volumes/{volume_guid}/filers/{filer_serial}/exports/{export_id}/'
  - 'POST /volumes/{volume_guid}/filers/{filer_serial}/exports/{export_id}/nfs_host_options/'
  - 'POST /volumes/{volume_guid}/folder-quotas/'
  - 'DELETE /volumes/{volume_guid}/folder-quotas/{folder_quota_id}/'
  - 'POST /volumes/{volume_guid}/global-lock-folders/'
  - 'DELETE /volumes/{volume_guid}/global-lock-folders/{path}'
  - 'GET /messages/'
  - 'GET /messages/{message_id}/'
---

# Share and export provisioning

This skill **writes**. Several of its actions are not reversible — read the reversibility section.

## Version requirements

- Writes at all require API **1.1** (Nasuni Edge Appliance 8.0+). API 1.0 is read-only.
- NFS exports and host options arrived in NMC 21.2.
- Global locking Oplocks arrived in NMC 23.2.
- On an older appliance the contract returns `unsupported_error`.

## Before you start

- Base URL `https://<nmc-hostname>/api/v1.2`; auth as in the ransomware skill
  (`POST /auth/login/`, then `Authorization: Token <token>`).
- The account needs **Enable NMC API Access** plus the matching per-action permission — e.g.
  folder-quota writes need **Manage Folder Quotas**.
- **1 write per second.** Creating a hundred shares is a hundred-plus seconds of wall clock.
  The first-party PowerShell module `Import-NasuniShare` exists precisely for CSV-driven bulk
  creation and already paces itself; prefer it for bulk work.
- **There is no idempotency key.** Share, export, quota and host-option creation are POSTs to a
  collection. If a POST times out, do **not** blind-retry — re-read
  `GET /volumes/{volume_guid}/filers/{filer_serial}/shares/` and check whether it landed. Retrying
  blind creates duplicates.

## Steps

1. `POST /auth/login/`.
2. `GET /volumes/` then `GET /volumes/{volume_guid}/filers/` — a share belongs to one volume **on
   one appliance**, so you need both identifiers. `volume_guid` is a GUID; `filer_serial` is the
   appliance serial.
3. `GET /volumes/{volume_guid}/filers/{filer_serial}/shares/` — check for an existing share of the
   same name before creating.
4. Create: `POST /volumes/{volume_guid}/filers/{filer_serial}/shares/`.
   For NFS instead: `POST /volumes/{volume_guid}/filers/{filer_serial}/exports/`, then
   `POST .../exports/{export_id}/nfs_host_options/` per allowed host.
5. Many of these are **asynchronous**: v1.2 declares `202 Accepted` on 36 operations. On a 202,
   poll `GET /messages/` (or `GET /messages/{message_id}/`) until the operation reports complete.
   Do not treat 202 as success.
6. Optional guardrails on the volume:
   - quota: `POST /volumes/{volume_guid}/folder-quotas/`
   - global file lock on a path: `POST /volumes/{volume_guid}/global-lock-folders/`
7. Confirm by re-reading the shares/exports collection.

## Reversibility

| Action | Reversal | Window |
|---|---|---|
| `POST .../global-lock-folders/` | `DELETE .../global-lock-folders/{path}` | unbounded |
| `POST .../pinned-folders/` | `DELETE .../pinned-folder/{path}` | unbounded |
| `POST .../auto-cached-folders/` | `DELETE .../auto-cached-folder/{path}` | unbounded |
| `POST .../shares/` | none — `DELETE .../shares/{share_id}/` removes it but there is **no undelete** | n/a |
| `POST .../exports/` | none — deletion is permanent | n/a |
| `POST .../folder-quotas/` | none — deletion is permanent | n/a |
| `POST /volumes/` (create volume) | **none at all** — the NMC API has no volume-delete operation | n/a |

Deleting a share does not delete data; it removes the access point and its configuration, and the
configuration must be re-entered by hand to restore it. Creating a volume through the API is a
one-way door as far as the API is concerned.

## Errors

`validation_error` returns a `ValidationError` body with per-field detail — read it rather than
guessing. `conflict_error` (409, 17 operations in v1.2) usually means a name collision or a
concurrent change; re-read state before retrying. `sync_error` means the NMC accepted the request
but the appliance did not apply it. `throttled_error` means you exceeded 1 write/sec.

Full catalog: `errors/nasuni-problem-types.yml`. Conventions: `conventions/nasuni-conventions.yml`.
