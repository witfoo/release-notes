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

**Release Date:** 2026-02-22

## Summary

Conductor 1.5.0 delivers enterprise notification infrastructure, 18 new integration connectors, per-exporter predicate filtering, LDAP security hardening, and 6 new auto-generated parsers. This release also includes performance benchmarks across all pipeline services, critical panic recovery fixes, and significant UI/UX improvements.

## New Features

### Notification System (PR148e)
- Full notification stack for Conductor UI: email (SMTP), Slack (webhook), and generic webhook channels
- Rule-based event routing with configurable cooldown periods to prevent notification storms
- 7 event types: parser failure, exporter failure, capacity warning, service down, certificate expiring, authentication failure, configuration changed
- Notification history with delivery status tracking (sent/failed/skipped)
- Channel test endpoint for validating configuration before going live
- Sensitive configuration fields (passwords, tokens, webhook URLs) automatically redacted in API responses

### Per-Exporter Predicate Filtering
- New shared `common/predicate` package for evaluating filter rules against artifacts
- Predicate filter UI added to all exporter settings forms in Conductor UI
- Integration in artifact-exporter and artifact-filter for runtime filtering
- Supports field-based matching with operators for flexible artifact routing

### 18 New Integration Connectors (PR139)
- **Signal Client:** Tenable.io, Palo Alto Cortex XDR, Proofpoint CASB, Proofpoint Protect, Netskope, Okta, LimaCharlie, Mimecast, Deep Instinct, Druva, Cisco Umbrella, Cisco Meraki, Cisco Duo, Cisco AMP, and more
- CrowdStrike connector fully refactored with separated types, alert API batching at 1000-item boundary, and v2 device detail endpoints
- Azure v2 evidence dispatching with typed evidence extraction (Mailbox, Message, URL, MailCluster) and HTTP 429 cooldown
- Search and filter UI across all connectors with grouped accordion layout

### Auto-Generated Parsers
- 6 new parsers auto-generated via AI pipeline: GreyNoise, Kafka, WitFoo Console, WitFoo Intel, Nginx, Filebeat
- Signal-parser fingerprint disambiguation with two-phase pattern (PR135): `IsFormatParsed()` syslog header check (fast path) followed by regex fallback (slow path)
- Fingerprint export tool for checking existing patterns

### MITRE ATT&CK v18 Types
- Taxonomy types and endpoint constants added to common module
- Support for technique, sub-technique, and tactic data structures

### Detection Meta Types
- MO definitions, lead rules, behavioral sets, and MO connection definition types in common module
- Sync endpoint constants for Intel API integration

## Improvements

### LDAP Security Hardening (PR148d)
- **Critical fix:** LDAP injection vulnerability (CWE-90) resolved with `ldap.EscapeFilter()` sanitization
- TLS 1.2+ enforcement for all LDAP connections
- Connection timeout configuration to prevent hanging connections
- Sanitized error messages to prevent information leakage
- Enhanced attribute extraction from LDAP responses

### Pipeline Reliability
- **SendEvent panic recovery:** Fixed send-on-closed-channel crash across all services via common module update
- Signal-server now auto-enables all log servers when NATS KV bucket is empty on first deployment
- CA certificate bundle loading added to Conductor UI Docker image for proper TLS verification
- ConsoleTLSSkipVerify support for flexible TLS configuration

### Performance Benchmarks
- Splunk HEC event processing and serialization benchmarks (artifact-exporter)
- STIX pattern parsing and indicator processing benchmarks (artifact-filter)
- JetStream operations benchmarks (broker-edge)
- Flow functions and ArtifactRaw serialization benchmarks (signal-server)

### UI/UX Enhancements
- Settings submenu icons with Beacon Yellow directional arrows
- Improved conductor-ui defaults and UX flow dividers
- Favicon and web manifest assets for proper site branding
- Carbon Charts ResizeObserver memory leak prevention (`resizable: false`)
- Standalone conductor system status behavior improvements

### CI/CD Improvements
- Quality gates and release branch handling for signal-parser and conductor-ui
- Fuzz seed corpus validation across all services
- Slack notification migration to v2.1.1 (slackapi/slack-github-action)
- WFA release workflow consolidated from 8 separate workflows into one
- Security scans made blocking in CI across all components

### Node Self-Registration
- ConsoleClient service in conductor-ui for auto-registration with WitFoo Console
- Node self-registration domain types and EventTypeRegister in common module
- KV key prefix stripping for proper module config handling

### Security
- CodeQL-driven fixes: allocation overflow in UDP packet parsing (artifact-exporter), allocation overflow in ciscoasa and off-by-one in panglobalprotect (signal-parser)
- Log injection sanitization improvements across conductor-ui
- golang.org/x/crypto bumped to v0.48.0
- Unused variable and import cleanup

## Bug Fixes

- Fixed send-on-closed-channel panic in common SendEvent across all pipeline services
- Fixed CA bundle loading for console TLS verification in conductor-ui
- Fixed system status tests for standalone conductor behavior
- Fixed KV key prefix stripping in module config writes
- Fixed duplicate GenerateBrokerClientPassword method from parsers merge
- Fixed explicit file close error handling in etc_hosts.go
- Fixed revive linter exit codes across all components (errorCode=1, warningCode=1)
- Fixed NATS KV bucket key auto-migration for legacy keys on startup
- Fixed trailing newline handling in DetectFormat for Go regexp compatibility
- Fixed bun ORM pinned to v1.2.11 (v1.2.16+ breaks migration API)

## Component Versions

| Component | Version |
|-----------|---------|
| WitFoo Agent (WFA) | 1.8.35 |
| conductor-ui | 1.5.0 |
| signal-server | 1.5.0 |
| signal-parser | 1.5.0 |
| signal-client | 1.5.0 |
| artifact-exporter | 1.5.0 |
| artifact-filter | 1.5.0 |
| broker-edge | 1.5.0 |
| common | 1.5.0 |
| grafana-agent | 1.5.0 |
| Go | 1.26.0 |

## Breaking Changes

- None. This is a backwards-compatible release.
Full 1.5.0 release notes: [conductor-ui-1.5.0.md](conductor-ui-1.5.0.md)

## WitFoo Conductor v1.4.0

**Release Date:** 2026-02-17

## Summary

Conductor 1.4.0 is a major release encompassing all improvements since v1.1.1. This release delivers a complete UI rewrite with Svelte 5 runes, multi-instance connector support, Analytics integration, the WFA 6-role expansion, ProtoGraph 11-tuple deduplication, automated parser generation via AI, and significant pipeline reliability improvements.

## New Features

### Multi-Instance Connector Support
- Connectors now support multiple independent instances of the same type (e.g., multiple Splunk or OpenSearch exporters)
- Each instance gets its own goroutine, metrics, status reporting, and human-readable display name
- Grouped accordion UI with per-instance controls and instance naming dialog
- Configurable max instances per connector type (default: 5)

### Conductor UI Full Stack Refactor (PR124)
- Complete migration to Svelte 5 runes-based reactivity
- Replaced axios with native fetch API throughout
- Standardized Go backend patterns across all handlers
- New system status proxy and banner for Analytics integration

### Analytics Integration
- New WitFoo Analytics artifact exporter over secure WebSocket (TLS) connection
- Reverse proxy auth middleware for Analytics cross-service authentication
- System status proxy relays Analytics appliance health to Conductor UI
- JWT-based authentication for Conductor routes

### WFA 6-Role Expansion (PR113)
- Expanded from 3 to 10 node roles: Conductor, AIO, Console, Data, API, Ingestion, Dispatcher, Graph, Intel, Reporter
- Analytics-specific container, image, and network constants
- Demo-data container support for demonstration environments

### Automated Parser Generation
- AI-powered parser generation using Claude Opus 4.6 via Anthropic API
- Quality gate runner with gofmt, go vet, revive lint, and compilation checks
- Unknown log type detection from Grafana/Loki metrics
- Auto-generated parsers pushed directly to main with Slack notifications

### Pipeline Error Monitoring & Auto-Remediation
- Automated pipeline error detection from Prometheus/Loki metrics
- Claude-powered root cause analysis and remediation script generation
- WFA service health monitoring and metrics bridge detection
- Enriched Slack notifications with affected org_ids and investigation details

### Pipeline Gate & Capacity Enforcement
- Pipeline gate in signal-parser for Analytics capacity enforcement
- Analytics status client monitors downstream capacity
- Backpressure propagation when Analytics node is at capacity

### STIX/TAXII Enrichment
- STIX indicator enrichment in artifact-filter pipeline stage
- Support for multiple STIX/TAXII feeds with API key management
- Indicator matching for IPs, domains, URLs, hashes, and email addresses
- Auto-generated feed IDs and uniqueness validation

## Improvements

### ProtoGraph Deduplication
- Expanded deduplication from 6-tuple to 11-tuple (added SenderHost, DestHost, DestPort, Protocol, SenderPort)
- Implemented sliding window per-hash deduplication to replace time-bucket approach
- Windowed output health check replaces percentage-based dedup monitoring

### Pipeline Reliability
- NATS KV config watcher race condition fix with single-consumer pattern
- Compare-And-Set for NATS KV config updates to prevent conflicts
- NATS connection retry logic with stabilization delay
- Consumer cleanup on startup for legacy consumer management
- Pipeline metrics instrumentation across all services (consumed/published/rejected tracking)

### Security & Quality
- Upgraded to Go 1.26.0 across all services
- Added security scanning and race condition checks to all CI workflows
- Consolidated PAT secrets to COMMON_REPO_TOKEN org secret
- TLS certificate verification made configurable for exporters
- Cassandra password validation and credential tests

### Signal Parser Enhancements
- Auto-enable all parsers when KV bucket is empty on first deployment
- New parsers: dnsmasqdhcp, Netgear GF Syslog, Netgear NTP, Sophos XGS Firewall, HPE Proliant Health, Qualcomm WLAN AP, Redis, SSSD, Telco MDM, VMware ESXi
- SvcWatcher race condition fix for input pipeline stability
- Nil settings rejection to prevent silent parser disabling

### Artifact Exporter Enhancements
- New connectors: WitFoo Analytics (WebSocket/TLS), Microsoft Sentinel, OpenSearch, UDP Syslog with IP masquerading, SCP Transfer
- Multi-stage Docker builds with baked-in CA certificate bundles
- TLS skip verify option for development environments
- File accumulation with size limits for local filesystem and SCP exporters

### Signal Client Enhancements
- New API connectors: Tenable, Palo Alto Cortex, Proofpoint (CASB, Protect), Netskope, Okta, LimaCharlie, Mimecast, Deep Instinct, Druva, Cisco Umbrella, Cisco Meraki, Cisco Duo, Cisco AMP
- Checkpoint-based incremental data fetching
- Multi-instance manager wiring for independent connector instances

### Broker Edge
- NATS CLI added to container for debugging and administration
- MaxAge configuration on DATA stream for retention management
- TLS configuration support with configurable verification

### Common Module
- Multi-instance connector infrastructure (InstanceConfig, manager, prefix routing)
- Config protocol types for remote node configuration management
- Stream metrics collector for capacity monitoring
- Pipeline metrics package for observability
- Shared licensing domain packages and Intel API client
- Enhanced CQL: CircuitBreaker, QueryTracker, EnhancedRetryPolicy

### Conductor UI
- SideNav positioning fix — no longer obscures main header navigation
- WebSocket status moved from navbar to Appliance Status modal
- Exporter save form fix (native form element for reportValidity)
- Reporter → WitFoo Analytics rename throughout
- Memory leak prevention with timer/interval cleanup
- Performance optimizations for metric store and chart rendering

## Bug Fixes

- Fixed multi-line remediation_code handling in pipeline error monitor
- Fixed config cycling from concurrent sync operations in conductor-ui
- Fixed NATS KV config watcher race condition causing missed updates
- Fixed SvcWatcher race blocking signal-parser input pipeline
- Fixed stixenrich passthrough mode without configuration
- Fixed duplicate chart metrics causing duplicate data points
- Fixed memory leaks from uncleaned timers and intervals in UI
- Fixed edge case with timer.C panic when no time window in exporters
- Fixed consumer name errors across multiple services
- Fixed Analytics image names to use correct `analytics-` prefix
- Fixed AIO validation that incorrectly blocked Analytics AIO role

## Component Versions

| Component | Version |
|-----------|---------|
| WitFoo Agent (WFA) | 1.8.10 |
| conductor-ui | 1.4.0 |
| signal-server | 1.4.0 |
| signal-parser | 1.4.0 |
| signal-client | 1.4.0 |
| artifact-exporter | 1.4.0 |
| artifact-filter | 1.4.0 |
| broker-edge | 1.4.0 |
| common | 1.4.0 |
| grafana-agent | 1.4.0 |
| Go | 1.26.0 |

## Breaking Changes

- Go 1.26.0 required (upgraded from 1.25.x)
- Package renames to avoid stdlib conflicts: `constant` → `constants`, `debug` → `debugutil`, `syslog` → `syslogutil`, `embed` → `embedtpl`, `metrics` → `parsermetrics`
- ProtoGraph deduplication expanded from 6-tuple to 11-tuple (existing dedup windows will reset)

---
*Release notes generated by Claude Opus 4.6*
*Repository: witfoo-dev/conductor_suite*
