# WitFoo Conductor v1.9.0

**Release Date:** 2026-09-02

## Summary

Conductor 1.9.0 is a minor release for the signal-processing pipeline that corrects how web-service request failures are classified, adds a native export destination for **CrowdStrike Falcon Next-Gen SIEM**, and fixes the Conductor UI rendering of formatted descriptions. It ships on the `stable`, `beta`, `staging` and `main` release lines.

Upgrade recommended.

## New Features

### CrowdStrike Falcon Next-Gen SIEM Exporter

- A new **CrowdStrike Falcon Next-Gen SIEM** exporter sends artifacts to a Falcon **HEC / HTTP Event Data Connector**. In the Falcon console (Next-Gen SIEM → Data ingestion → Data connectors) create the connector, then paste its **API URL** and **API key** into Conductor → Admin → Settings → Artifact Exporters.
- Artifacts are delivered as HEC events with the artifact as the event **object**, so every artifact field is searchable in Falcon without a custom parser; batches are sent as newline-delimited JSON and acknowledged per event, with optional gzip compression and an optional source-type (parser) override.
- The destination is pinned to the CrowdStrike ingest domains over HTTPS with certificate verification, and redirects are re-checked against the same rule, so an exporter can never be steered to another host.
- Supports multiple instances, predicate filters and the discarded-duplicates source, like every other Conductor exporter.

## Improvements

### Web-service failures classify as reconnaissance

- **404-style web-service failures are now `recon` messages, not `degraded_service`.** A client requesting a file the web server does not have, a forbidden directory index, or a path blocked by a deny rule is a probe against the service, not a failure of it. These lines — from nginx and Apache error logs and from the WitFoo Console's own access log — were previously stamped `degraded_service` and raised Degraded Service work units for scanner traffic such as `/.env`, `/wp-login.php` and `favicon.ico` probes.
- A new **`recon`** message type and a new **"Web Reconnaissance Detected"** lead rule (rule 330, Recon behavioral sets) keep these probes visible as work units under the correct category, with the probing client IP and requested path extracted onto the artifact.
- Genuine service failures — upstream connection failures, timeouts, TLS handshake failures, 5xx responses — continue to classify as `degraded_service`. Ordinary access-log records are unchanged.
- A single shared HTTP-status classification (5xx → degraded service; 400/404/405/410/414/416 → recon) is now used by the web-facing parsers so the same status classifies the same way regardless of product.

### Conductor UI

- **Formatted descriptions render correctly.** Connector descriptions that contain light formatting (line breaks, emphasis, paragraphs) — most visibly on Admin → Settings → Log Servers — showed the raw markup as text. They now render as intended through an escape-first renderer that permits only a small set of formatting tags, so the fix does not open a script-injection path.

## Component Versions

| Component | Version |
|-----------|---------|
| conductor-ui | 1.9.0 |
| signal-server | 1.9.0 |
| signal-parser | 1.9.0 |
| signal-client | 1.9.0 |
| artifact-exporter | 1.9.0 |
| artifact-filter | 1.9.0 |
| broker-edge | 1.9.0 |
| common | 1.5.39 |
| Go | 1.26.6 |

## Upgrade Notes

- **Minor Conductor release (1.8.x → 1.9.0).** All pipeline components advance with it.
- **Lead rules.** The new "Web Reconnaissance Detected" rule is seeded on fresh installs and delivered to licensed appliances through the Intel metadata sync. Until an appliance has the rule, reclassified web probes are still stored and searchable as `recon` artifacts but do not raise work units.
- **Dashboards and searches** that keyed on `message_type = degraded_service` for web-server not-found / forbidden lines should be updated to `recon`.
- **No migrations.** The new exporter is opt-in and configured per Conductor; no existing exporter configuration changes.

## Breaking Changes

- None. This is a backwards-compatible release.

## Verification

```bash
./scripts/testing/unit-tests.sh
./scripts/testing/system-tests.sh
./scripts/testing/full-testing-with-conductor.sh
```

After upgrading: send a request for a non-existent path to a monitored web server and confirm the resulting artifact carries `message_type = recon` with the client IP; configure the CrowdStrike Falcon Next-Gen SIEM exporter and confirm events under the connector's **Show events** view in the Falcon console; open Admin → Settings → Log Servers and confirm the descriptions render with line breaks and bold text.

---
*Repository: witfoo-dev/analytics*
