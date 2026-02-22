# WitFoo Conductor v1.5.0

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

---
*Release notes generated by Claude Opus 4.6*
*Repository: witfoo-dev/conductor_suite*
