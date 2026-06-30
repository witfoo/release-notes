# WitFoo Conductor v1.8.0

**Release Date:** 2026-06-30

## Summary

Conductor 1.8.0 is a minor release for the signal-processing pipeline that broadens detection coverage, deepens integration visibility, and brings the Conductor UI to mobile. It enables VMware NSX-T parsing, corrects the Cisco Umbrella integration's configuration fields, expands the Integration Health view, and makes the Conductor login default to single sign-on when SAML is configured (with the SSO reliability issues from the field resolved). It ships alongside the **WitFoo Analytics 1.0.0** general-availability release and **WitFoo Console 1.9.0**.

Upgrade recommended.

## New Features

### VMware NSX-T Parsing (#293, #295, #308)

- The auto-generated NSX-T parsers (`nsxopsagent`, `vmwarensxproxy`) are now enabled and catalogued. NSX-T operational and degraded-service events are classified to a real stream name and message type instead of landing in unknown, so NSX-T health and management activity becomes searchable and actionable.

### Expanded Integration Health (#254)

- The Conductor Integration Health view surfaces richer per-integration detail, making it easier to see which connectors are delivering and which need attention.

### Responsive Conductor UI (#261)

- The Conductor UI is now usable on phones and tablets, with a drawer-based primary navigation and touch-friendly tables and controls below desktop widths.

## Improvements

### Integrations & Sign-In

- **Cisco Umbrella configuration corrected (#288).** The Umbrella integration now exposes the correct fields (FQDN, Client ID, Org ID, Secret), with legacy saved settings folded forward automatically — no re-entry required for existing connectors beyond the new Org ID.
- **SAML default login (#312).** When SAML single sign-on is enabled, the Conductor login page now defaults to the SSO tab while always keeping local login reachable.
- **SSO reliability (#210).** The Conductor SSO assertion handling and base-path redirects were corrected so valid logins are no longer caught in a redirect loop or rejected with a parse error.
- **Conductor link gating (#253).** The Conductor link and health panel are now hidden on nodes that are not Conductors, instead of showing a dead link.

### Reliability & Monitoring

- **Improved automated monitoring (#315).** Telemetry leaving an appliance is redacted at the source under a configurable policy, and appliances can report cluster problems back to WitFoo (opt-in, PII-free) so issues are caught proactively.

## Component Versions

| Component | Version |
|-----------|---------|
| WitFoo Agent (WFA) | 2.4.2 |
| conductor-ui | 1.8.0 |
| signal-server | 1.8.0 |
| signal-parser | 1.8.0 |
| signal-client | 1.8.0 |
| artifact-exporter | 1.8.0 |
| artifact-filter | 1.8.0 |
| broker-edge | 1.8.0 |
| common | 1.5.39 |
| Go | 1.26.4 |

## Upgrade Notes

- **Minor Conductor release (1.7.x → 1.8.0).** All pipeline components advance with it (see Component Versions above).
- **Cisco Umbrella connectors** gain a required **Org ID** field; existing FQDN/Client-ID/Secret values are folded forward automatically.
- **No migrations.** Parser enablement and redaction policy are applied automatically with safe defaults; telemetry redaction (`off` / `standard` / `strict`) is an optional operator policy.

## Breaking Changes

- None. This is a backwards-compatible release.

## Verification

```bash
./scripts/testing/unit-tests.sh
./scripts/testing/system-tests.sh
./scripts/testing/e2e-tests.sh
./scripts/testing/security.sh --json
./scripts/testing/full-testing-with-conductor.sh
```

## Resolved Issues

#210, #253, #254, #261, #288, #293, #295, #308, #312, #315

---
*Repository: witfoo-dev/analytics*
