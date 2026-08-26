---
name: nasuni-ransomware-block-response
description: Contain a suspected ransomware or misbehaving-client incident on a Nasuni estate by blocking the offending IP address or username at the Edge Appliance through the NMC API, then unblocking it once cleared.
api: Nasuni Management Console (NMC) API
spec: openapi/nasuni-nmc-v1-2-openapi.yml
generated: '2026-08-26'
method: generated
source: >-
  Grounded in verified paths and methods from openapi/_original/nasuni-nmc-v1-2-openapi.json.
  The NMC API publishes NO operationIds (0 of 120 operations in v1.2), so operations are named
  here by METHOD + PATH exactly as they appear in the contract.
operations:
  - 'POST /auth/login/'
  - 'GET /filers/'
  - 'GET /filers/{filer_serial}/'
  - 'PUT /filers/{filer_serial}/blocked-sources/ip-addresses/{blocked_ip}/'
  - 'DELETE /filers/{filer_serial}/blocked-sources/ip-addresses/{blocked_ip}/'
  - 'PUT /filers/{filer_serial}/blocked-sources/usernames/{blocked_username}/'
  - 'DELETE /filers/{filer_serial}/blocked-sources/usernames/{blocked_username}/'
  - 'POST /filers/{filer_serial}/blocked-clients/'
  - 'DELETE /filers/{filer_serial}/blocked-clients/{blocked_ip}/'
  - 'GET /filers/cifsclients/'
  - 'GET /filers/{filer_serial}/cifsclients/'
  - 'GET /filers/{filer_serial}/cifslocks/'
  - 'POST /auth/logout/'
---

# Ransomware / abusive-client block response

This skill **writes**. Every action it takes is reversible and the reversal is documented below.

## Version requirement

Blocked Sources (block by IP **or** by username) arrived in NMC 25.2 and needs API 1.2.
Blocked Clients (IP only) arrived in NMC 22.3. If the appliance is older the contract returns
`unsupported_error` — that is the API's version-skew signal, not a client bug.

## Before you start

- Base URL is the customer's own NMC: `https://<nmc-hostname>/api/v1.2`. There is no Nasuni-hosted
  NMC endpoint; the appliance is customer-operated.
- `POST /auth/login/` with `{"username": ..., "password": ...}` returns `{token, expires}`. Send it
  as `Authorization: Token <token>`.
- SSO accounts are **not** supported for NMC API auth. Use a native or domain account. Domain
  usernames work as UPN (`user@domain.com`) or `DOMAIN\\samaccountname`.
- The account needs the NMC group permission **Enable NMC API Access** plus the specific permission
  for blocking. Without it you get `perm_error` / 403, not 404.
- **Write rate limit is 1 request per second.** Blocking twenty IPs takes at least twenty seconds.
  Sleep 1.1s between writes; there is no `Retry-After` header on the NMC API's 429 so you cannot
  discover the interval at runtime.

## Steps

1. Authenticate: `POST /auth/login/`.
2. Identify the appliance: `GET /filers/` for the fleet, `GET /filers/{filer_serial}/` for one.
3. Establish evidence before blocking anything:
   - `GET /filers/{filer_serial}/cifsclients/` — who is connected.
   - `GET /filers/{filer_serial}/cifslocks/` — what they are holding open.
   Record this. You are about to disconnect someone.
4. Block. Prefer Blocked Sources, which lets you block the identity rather than the address:
   - by IP: `PUT /filers/{filer_serial}/blocked-sources/ip-addresses/{blocked_ip}/`
   - by user: `PUT /filers/{filer_serial}/blocked-sources/usernames/{blocked_username}/`
   Both are `PUT` to a fully-addressed resource, which makes them naturally idempotent — re-sending
   the same block is safe. On pre-25.2 appliances fall back to
   `POST /filers/{filer_serial}/blocked-clients/`, which is a POST to a collection and is **not**
   idempotent; check `GET /filers/{filer_serial}/blocked-clients/` before re-sending.
5. Repeat per appliance. A block is scoped to one appliance — blocking on one filer does not block
   on the rest of the fleet. For a fleet-wide containment you must iterate every `filer_serial`,
   one write per second.
6. Confirm: re-read the blocked list, and re-read `cifsclients` to confirm the session is gone.
7. `POST /auth/logout/` to de-authenticate the token when finished.

## Reversibility

Every block in this skill is fully reversible with no time limit:

| Action | Reversal |
|---|---|
| `PUT .../blocked-sources/ip-addresses/{blocked_ip}/` | `DELETE .../blocked-sources/ip-addresses/{blocked_ip}/` |
| `PUT .../blocked-sources/usernames/{blocked_username}/` | `DELETE .../blocked-sources/usernames/{blocked_username}/` |
| `POST .../blocked-clients/` | `DELETE .../blocked-clients/{blocked_ip}/` |

The block persists until explicitly removed. There is no expiry and no auto-unblock, so an
unattended agent that blocks must also own the unblock.

## Errors

The NMC envelope is `{"error": {"code": ..., "description": ...}}` with a closed code enum.
Codes that matter here:

- `auth_error` (401) — re-login.
- `perm_error` (403) — missing NMC permission. Not retryable.
- `unsupported_error` — appliance build predates this operation.
- `unmanaged_error` — the appliance is not under NMC management.
- `sync_error` — the NMC accepted the call but the appliance did not apply it. **Treat this as
  "not blocked" and retry**; it is the one error here that can leave you believing you contained
  something you did not.
- `throttled_error` (429) — you exceeded 1 write/sec. Sleep and retry.

Full catalog: `errors/nasuni-problem-types.yml`.
