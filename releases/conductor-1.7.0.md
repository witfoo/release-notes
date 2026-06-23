# WitFoo Conductor v1.7.0

**Release Date:** 2026-06-23

## Summary

Conductor 1.7.0 is a major release for the signal-processing pipeline, centered on a fleet-wide Parser Audit, Beats agent up/down/health tracking, and node startup resilience. The Parser Audit brings RFC-compliant event severity, a five-way metadata cross-reference quality gate, dozens of product-mapping corrections, and broader Microsoft Graph and Defender coverage so more security data parses cleanly instead of landing in unknown. A new Agents page in Conductor — with up/down/stale alerting and status forwarded to the WitFoo Console — gives operators direct visibility into Beats agent health. The pipeline now self-heals when a parser is present in the build but not enabled, so parsers can no longer ship "dark," and a service-init resilience fix ensures a node can no longer get permanently stuck on startup.

Upgrade recommended.

## New Features

### Fleet-Wide Parser Audit (#266)

- Every event now carries an RFC-compliant severity value. Out-of-range or non-numeric severities are normalized at a single pipeline chokepoint, so no parser can persist a malformed severity.
- A new five-way metadata cross-reference quality gate validates product, framework, MITRE, and lead-rule mappings on every change, catching mapping drift before it ships.
- A grounded audit applied roughly 50 product stream-name corrections, each tied to the product's real parser output rather than guessed, shrinking the mapping backlog.
- Microsoft Graph and Microsoft Defender coverage was extended to claim event shapes that previously fell through to unknown.

### Beats Agent Up/Down/Health Tracking (#269)

- A new **Agents** page in Conductor (`/conductor/admin/agents`) lists every Beats agent with its live status, search, and refresh.
- The pipeline tracks each agent's liveness directly from its data flow — no separate heartbeat required — and raises **up / down / stale** notification alerts on status transitions.
- A bounded agent-status summary is forwarded to the WitFoo Console so agent health is visible there as well, on the existing node-status report.

### Expanded Parser Coverage

- Microsoft Defender Vulnerability Management, Azure security / Graph sign-ins, directory audits, and Defender incidents now parse correctly instead of landing in unknown.
- Additional log sources — including system package-update, identity-protection, and modem-manager logs — are now recognized and parsed automatically.

### Pipeline Self-Heal for Unenabled Parsers (#245, #246)

- When a first-party parser is compiled into the build but absent from the enabled-parser configuration, the pipeline now auto-enables it, while still respecting any explicit operator disable.
- Parsers can no longer ship "dark" and silently route matching data to unknown.
- Self-reconciliation can be tuned via the existing `PARSER_RECONCILE` setting (`enabled` default / `report-only` / `off`).

## Improvements

### Pipeline Reliability

- **Service-init startup resilience (#283).** A node's agent now self-creates its required broker streams and key/value buckets before the broker-health gate evaluates them, and that gate fails open on best-effort objects and is bounded and observable. This removes a startup condition where a node could remain permanently wedged in initialization. The startup gate exposes an optional `BROKER_HEALTH_GATE_WARN_SECONDS` threshold (default 120s) for observability.
- **Idle keep-alive noise fix (#247).** Idle Beats keep-alive connections no longer produce a connection-scan warning flood; a clean idle timeout is now treated like a normal end-of-connection rather than an error.

## Component Versions

| Component | Version |
|-----------|---------|
| WitFoo Agent (WFA) | 2.3.0 |
| conductor-ui | 1.7.0 |
| signal-server | 1.7.0 |
| signal-parser | 1.7.0 |
| signal-client | 1.7.0 |
| artifact-exporter | 1.7.0 |
| artifact-filter | 1.7.0 |
| broker-edge | 1.7.0 |
| common | 1.5.39 |
| Go | 1.26.4 |

## Upgrade Notes

- **Major Conductor release (1.6.x → 1.7.0).** All pipeline components advance with it (see Component Versions above), and the bundled WitFoo Agent advances to 2.3.0.
- **No required configuration changes.** Beats agent tracking and parser self-heal are enabled by default with safe behavior. The `PARSER_RECONCILE` and `BROKER_HEALTH_GATE_WARN_SECONDS` settings above are optional tuning knobs; neither is required to upgrade.
- **No migrations.** Beats agent status is stored in an appliance-scoped broker key/value bucket that is created automatically; the Console ingests the forwarded agent summary as part of the existing node-status report with no schema change.

## Breaking Changes

- None. This is a backwards-compatible release. Existing notifications are unchanged unless an agent up/down/stale event applies.

## Verification

```bash
./scripts/testing/unit-tests.sh
./scripts/testing/system-tests.sh
./scripts/testing/e2e-tests.sh
./scripts/testing/security.sh --json
./scripts/testing/full-testing-with-conductor.sh
```

## Resolved Issues

#266, #269, #283, #245, #246, #247, #208, #203, #205, #206

---
*Repository: witfoo-dev/analytics*
