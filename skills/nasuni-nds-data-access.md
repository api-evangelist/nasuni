---
name: nasuni-nds-data-access
description: Read files out of a Nasuni volume from AWS or Azure analytics and AI tooling using the Nasuni Data Service S3-compatible and Azure Blob-compatible endpoints, without copying the data.
api: Nasuni Data Service (NDS)
spec: openapi/nasuni-nasuni-data-service-aws-openapi.yml
generated: '2026-08-26'
method: generated
source: >-
  Grounded in verified operationIds from openapi/_original/nasuni-nasuni-data-service-aws-openapi.json
  and nasuni-nasuni-data-service-azure-openapi.json, plus
  https://docs.nasuni.com/docs/nds-aws-api-overview and .../nds-azure-api-overview
operations:
  - 'GET /?list-type=2 (ListObjectsV2, NDS for AWS)'
  - 'GET /{key} (GetObject, NDS for AWS)'
  - 'HEAD /{key} (HeadObject, NDS for AWS)'
  - 'GET / (List Containers, NDS for Azure)'
  - 'GET /{container}/ (List Blobs, NDS for Azure)'
  - 'GET /{container}/{blob} (Get Blob, NDS for Azure)'
  - 'HEAD /{container}/{blob} (Get Blob Properties, NDS for Azure)'
  - 'get_blob_access_keys_uaas_stacks__stack_id__nds_blob_keys_get (Portal API)'
  - 'generate_sas_token_uaas_stacks__stack_id__nds_sas_token_post (Portal API)'
---

# Reading Nasuni data through NDS

**Read-only.** Neither NDS surface exposes a write operation. That is the point: analytics and AI
workloads get the data without any risk of mutating the file system of record.

## Which surface

| You are in | Use | Compatible with |
|---|---|---|
| AWS | NDS for AWS (S3 Object Lambda) | Anything that speaks S3 — boto3, AWS SDKs, s3fs, Athena, SageMaker, DuckDB |
| Azure | NDS for Azure | Anything that speaks Azure Blob — azcopy, Azure SDKs, Synapse, Fabric, Azure AI Search |

A **container / bucket maps to a Nasuni volume**, and a **blob / object key maps to a file**. Point
an existing S3 or Blob client at the NDS endpoint and it works unmodified.

## Endpoint

NDS is deployed into the customer's own AWS or Azure account, so there is no Nasuni-operated host
and neither published spec carries a `servers[]` block. Take the endpoint from your NDS deployment.

## Credentials

- **AWS**: authenticate exactly as you would to S3 — SigV4 signed headers
  (`Authorization: AWS4-HMAC-SHA256 ...`, `X-Amz-Date`, `X-Amz-Content-Sha256: UNSIGNED-PAYLOAD`,
  and `X-Amz-Security-Token` for temporary credentials), or a presigned URL.
- **Azure**: either a Shared Key in the `Authorization` header, or a SAS token supplied as query
  parameters (`sv`, `ss`, `srt`, `sp`, `se`, `st`, `sig`). Do **not** combine the two.
- Both key sets are generated at NDS deployment time. They can be listed and rotated through the
  Portal API: `get_blob_access_keys_uaas_stacks__stack_id__nds_blob_keys_get`,
  `regenerate_blob_access_key_uaas_stacks__stack_id__nds_blob_keys__blob_key_id__patch`, and
  `generate_sas_token_uaas_stacks__stack_id__nds_sas_token_post`.

## Steps

1. List volumes: `GET /?list-type=2` (AWS) or `GET /` (Azure).
2. List files in a volume: `GET /{container}/` (Azure) or the S3 list with a prefix (AWS).
3. Read metadata before content — `HEAD` is far cheaper than `GET` and returns everything you need
   to decide whether to fetch: `ctime`, `mtime`, `size`, `Content-Type`.
4. Fetch content: `GET /{key}` or `GET /{container}/{blob}`. **Single byte-range requests are
   supported** (`206 Partial Content`); a multi-range request is not.

## Metadata caveat

`security_ntacl` (AWS spells it `security-ntacl`) carries base64-encoded NTFS ACLs. Nasuni states
plainly that decoding it needs custom tooling outside the scope of Nasuni support, and that any SIDs
must be resolved against the right Active Directory domain. Do not build access decisions on it
casually.

`atime`, `uid`, `gid`, `mode`, `version`, `handle`, `firsthandle` and `user-dosattrib` are marked
**for Nasuni use only — not supported for customer use**. Ignore them.

## Errors

NDS returns the native error envelope of the service it emulates, not a Nasuni one.

- AWS: S3 XML — `<Error><Code/><Message/><Resource/><RequestId/></Error>`. `RequestId` is the only
  correlation identifier available anywhere in the Nasuni API estate; capture it.
- Azure: Azure Blob XML errors.
- `424 Failed Dependency` (4 AWS operations) is NDS-specific — the Object Lambda could not reach the
  backing volume. Retry with backoff; it is not a client error.
- `501 Not Implemented` (4 AWS operations) means you used an S3 feature NDS does not emulate.
  Multi-range reads and any write verb land here.
- `416` — invalid byte range. `413` (Azure) — request too large.
