# WitFoo Conductor v1.6.0

**Release Date:** 2026-03-02

## Summary

Conductor 1.6.0 adds 4 new exporters to the pipeline: USM Anywhere webhook (PR181), S3/MinIO/R2 object storage, Azure Blob Storage, and Google Cloud Storage (PR182). This release also includes Conductor UI stability improvements for tab inactivity, pipeline visualization fixes, and toggle styling enhancements.

## New Features

### USM Anywhere Webhook Exporter (PR181)
- HTTP webhook exporter for LevelBlue USM Anywhere (formerly AlienVault)
- JSON array batching (default 50 events, max 10,000) with gzip compression
- API_KEY header authentication per USM Anywhere webhook API specification
- Configurable batch size and TLS certificate verification
- Sankey pipeline flow visualization with "USM Anywhere" display name
- Source subject support: finalized artifacts or discarded duplicates
- 12 Go tests (8 sender + 4 connector) + 7 Vitest validator tests

### S3 Object Storage Exporter (PR182)
- Exports artifacts to S3-compatible storage as compressed NDJSON objects
- Supports AWS S3, MinIO, Cloudflare R2, Backblaze B2, Wasabi, DigitalOcean Spaces
- Time-partitioned object keys: `{prefix}/{org_id}/YYYY/MM/DD/HH/{timestamp}-{batch_id}.ndjson.gz`
- Configurable storage class (Standard, Intelligent-Tiering, Glacier, Deep Archive)
- SSE-KMS encryption with configurable KMS key ARN
- Custom endpoint URL + path-style addressing for non-AWS providers
- Static credentials or SDK default credential chain (IAM role, env vars)
- Custom form processor for proper type conversion from FormData
- 17 Go tests (10 sender + 5 connector + 2 pipeline metrics) + 6 Vitest validator tests

### Azure Blob Storage Exporter (PR182)
- Exports artifacts to Azure Blob Storage as compressed NDJSON blobs
- Connection string or shared key authentication
- Configurable access tiers: Hot, Cool, Cold, Archive
- Time-partitioned blob names matching S3 path format
- 14 Go tests (8 sender + 4 connector + 2 pipeline metrics) + 4 Vitest validator tests

### Google Cloud Storage Exporter (PR182)
- Exports artifacts to Google Cloud Storage as compressed NDJSON objects
- Service account JSON or Application Default Credentials
- Configurable storage classes: Standard, Nearline, Coldline, Archive
- Time-partitioned object names matching S3 path format
- 14 Go tests (8 sender + 4 connector + 2 pipeline metrics) + 4 Vitest validator tests

## Improvements

### Conductor UI Stability
- Tab inactivity fix: WebSocket pause/resume with `visibilitychange` listener
- Metrics store flush on tab resume prevents stale data batch dumps
- Chart remount pattern (`renderKey`) fixes 0x0px rendering in hidden Carbon TabContent
- Skeleton loading indicators shown consistently during tab transitions

### Pipeline Sankey Visualization
- Discarded node fix: moved from slice 1 (Input) to slice 4 (Dedup Result)
- Split EXIT_NODES into EXIT_NODES + DIMMED_NODES for proper bridge node handling
- effectiveDiscardedCount fallback from discard-export volume
- Display names registered for all 4 new exporters

### Pipeline Toggle Styling
- Flow (default) position styled as active (green) via CSS inversion
- Time Series position styled as inactive (grey)

### AlienVault Connector Clarification
- Existing `alien-vault` connector documentation updated to clarify it is a generic TCP/UDP syslog exporter using RFC5424 wrapping, not specific to USM Anywhere

## Component Versions

| Component | Version |
|-----------|---------|
| conductor-ui | 1.6.0 |
| signal-server | 1.5.0 |
| signal-parser | 1.5.0 |
| signal-client | 1.5.0 |
| artifact-exporter | 1.6.0 |
| artifact-filter | 1.5.0 |
| broker-edge | 1.5.0 |
| common | 1.5.12 |
| Go | 1.26.0 |

## Test Summary

| Category | Tests |
|----------|-------|
| Go: USM Anywhere (sender + connector) | 12 |
| Go: S3 (sender + connector + metrics) | 17 |
| Go: Azure Blob (sender + connector + metrics) | 14 |
| Go: GCS (sender + connector + metrics) | 14 |
| Vitest: Validator tests | 21 |
| Vitest: Sankey tests | 66 |
| Vitest: Total UI tests | 225 |
| **Total new tests** | **78** |

## Breaking Changes

- None. This is a backwards-compatible release.

## Previous Release Notes

### WitFoo Conductor v1.5.0

Conductor 1.5.0 delivered enterprise notification infrastructure, 18 new integration connectors, per-exporter predicate filtering, LDAP security hardening, and 6 new auto-generated parsers. Key features included:

- Full notification stack: email (SMTP), Slack (webhook), and generic webhook channels with rule-based event routing
- Per-exporter predicate filtering with shared `common/predicate` package
- 18 new signal client connectors: Tenable.io, Palo Alto Cortex XDR, Proofpoint, Netskope, Okta, LimaCharlie, Mimecast, Deep Instinct, Druva, Cisco (Umbrella, Meraki, Duo, AMP), CrowdStrike refactor, Azure v2 evidence
- 6 auto-generated parsers: GreyNoise, Kafka, WitFoo Console, WitFoo Intel, Nginx, Filebeat
- LDAP injection fix (CWE-90) with TLS 1.2+ enforcement
- SendEvent panic recovery across all pipeline services
- Performance benchmarks for Splunk HEC, STIX parsing, JetStream, and flow functions
- Node self-registration with WitFoo Console
- CodeQL-driven security fixes: allocation overflow in UDP parsing, ciscoasa, panglobalprotect

Full 1.5.0 release notes: [conductor-1.5.0.md](conductor-1.5.0.md)

### WitFoo Management Console (Conductor UI) v1.5.0

- Security hardening: unused vars, log injection sanitization (CodeQL)

Full 1.5.0 release notes: [conductor-ui-1.5.0.md](conductor-ui-1.5.0.md)

---
*Release notes generated by Claude Opus 4.6*
*Repository: witfoo-dev/conductor_suite*
