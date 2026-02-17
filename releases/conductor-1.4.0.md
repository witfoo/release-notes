# WitFoo Conductor v1.4.0

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
