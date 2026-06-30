# WitFoo Console v1.9.0

**Release Date:** 2026-06-30

## Summary

WitFoo Console 1.9.0 is a focused follow-up to the 1.8.0 management release. It makes connected-node status trustworthy with a true online/offline indicator, brings the Console UI to phones and tablets with a responsive layout, and folds in the platform-wide pre-1.0 security and code-quality drain. It ships alongside the **WitFoo Analytics 1.0.0** general-availability release and **WitFoo Conductor 1.8.0**.

**Minor feature and hardening release — upgrade recommended.**

## New Features

### Accurate Node Online/Offline Indicator (#273)

- A connected node's status indicator is now derived from its **last-seen time** (online when seen within the last ten minutes) rather than a stored status field that never decayed. A node that has gone away now reads as Offline instead of showing a stale "online" forever.

### Responsive Console (#261)

- The Console UI is now usable on phones and tablets: below desktop widths the primary navigation collapses to a drawer, and tables, modals, and controls adapt to touch with larger targets.

## Improvements

### Security & Hardening

This release folds in the platform-wide pre-1.0 security review. No exploit details are published here; the categories below describe the protections now in place.

- **Log-injection hardening** — additional user-controlled values are sanitized before being written to logs.
- **Recurrence prevention** — the management console is covered by the same automated build-pipeline gates (secret scanning, static analysis, dependency and image scanning) as the rest of the platform.

## Platform Release Context

Console 1.9.0 is released together with **[WitFoo Analytics 1.0.0](1.0.0.md)** (the GA milestone) and **[WitFoo Conductor 1.8.0](conductor-1.8.0.md)**. Highlights operators will notice:

- **Two new executive reports** — Vulnerability Management and Threat Model.
- **AI provider control & cost governance** — per-provider model catalogs, pre-save model validation, and economy/best AI profiles.
- **Responsive Analytics and Conductor UIs** to match this Console release.

See the linked notes for the full Analytics and Conductor feature sets.

## Component Versions

| Component | Version |
|-----------|---------|
| console-ui | 1.9.0 |
| common | 1.5.39 |
| Go | 1.26.4 |

## Upgrade Notes

- **No breaking API changes** and **no manual database migration** — Console schema migrations apply automatically on upgrade.
- The online/offline indicator is automatic; no configuration is required.

## Breaking Changes

- None. This is a backwards-compatible release.

---
*Repository: witfoo-dev/analytics*
