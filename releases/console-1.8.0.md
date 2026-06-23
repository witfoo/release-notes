# WitFoo Console v1.8.0

**Release Date:** 2026-06-23

## Summary

WitFoo Console 1.8.0 is a major release for MSSP operators. Its headline is **zero-touch node onboarding**: the new **Console Configuration Generator** lets an administrator pre-build a customer node configuration and publish a one-time URL, and the customer brings the node online with a single `wfa fetch <url>` command. This release also deepens **remote lifecycle management** of connected Analytics and Data nodes — start, stop, restart, upgrade, and image-pull, all from the Console with Cassandra data-node safety guards — and surfaces **Beats agent up/down/health status** forwarded from connected appliances. It ships alongside WitFoo Analytics 0.9.8 and Conductor 1.7.0 as part of a broad security-hardening and operational-resilience line.

**Security-hardening and major-feature release — upgrade recommended.**

## New Features

### Console Configuration Generator + `wfa fetch` (#244)

- A guided wizard in Settings pre-builds a customer node configuration (organization, license, product/role, network, and feature answers) and publishes a **one-time, expiring URL**.
- The customer runs `wfa fetch <url>` on the new node; it pulls the pre-built configuration, collects only the VM-specific values and admin password locally, and finalizes — zero-touch provisioning with no manual config-file editing.
- A configuration manager lists generated configs with Created / Fetched status and supports reset and delete.
- Security model: configurations are encrypted at rest, the URL token is stored hashed, the link is single-use, and it expires after 72 hours by default. Treat a generated link as a credential.

### Deeper Remote Node Management (#160)

- Start, stop, restart, upgrade, and pull-images for connected Analytics and Data nodes directly from the node Settings tab.
- Cassandra-bearing **Data node** safety guards require an explicit cluster-impact confirmation before any destructive action; a destructive action on a data-bearing node is refused unless explicitly confirmed.
- Lifecycle actions are rejected cleanly when a node is offline rather than queued silently, and are restricted to Org Admin and above.

### Beats Agent Visibility (#269)

- The node detail view surfaces a **Beats Agents** panel populated from status that connected appliances forward to the Console, so an operator can see per-agent up/down/last-seen state without leaving the management console.

## Improvements

### Security & Hardening

This release folds in a platform-wide security drain. No exploit details are published here; the categories below describe the protections now in place.

- **Log-injection hardening** — the management console now sanitizes user-controlled values before they are written to logs (a class of log-injection findings resolved).
- **Outbound-connection protection** — a shared anti-rebinding / SSRF guard validates outbound destinations before any credentials are attached.
- **Tighter access controls** — per-route permission enforcement across configuration and node-management APIs, with a build-time guard that prevents an ungated route from shipping.
- **Stronger transport validation** — hardened TLS / certificate validation on appliance health checks.
- **Recurrence prevention** — secret scanning, static analysis, dependency drain, and signed-image / SBOM gates added to the build pipeline.

## Platform Release Context

Console 1.8.0 is released together with **[WitFoo Analytics 0.9.8](0.9.8.md)** and **[WitFoo Conductor 1.7.0](conductor-1.7.0.md)**. Highlights from the broader release that Console operators will notice:

- **Compliance Readiness no longer strands at 0%** after a redeploy (a snapshot readiness guard preserves the last healthy figure).
- **AI cost optimization** — prompt caching and cheaper model tiering for summaries substantially reduce AI spend.
- **Beats agent up/down/health tracking** originates in Conductor (a new Agents page plus up/down/stale alerting) and is forwarded here.
- **Broker-startup deadlock resilience** in WFA 2.3.0, so a connected appliance can no longer get permanently wedged during initialization.

See the linked notes for the full Analytics and Conductor feature sets.

## Component Versions

| Component | Version |
|-----------|---------|
| console-ui | 1.8.0 |
| common | 1.5.39 |
| Go | 1.26.4 |

## Upgrade Notes

- **Pairs with a current WitFoo Agent.** The Console Configuration Generator and remote node management require connected appliances running a current WFA (2.2.x or later); the fleet auto-updates to **WFA 2.3.0**. Beats agent forwarding requires WFA 2.2.9 or later.
- **One-time configuration URLs are single-use and expiring.** Generated links can be fetched once and expire after 72 hours by default; configurations are encrypted at rest and the URL token is stored hashed.
- **Data-node actions require confirmation.** Destructive lifecycle actions (restart, stop, upgrade) on Cassandra-bearing Data nodes require an explicit cluster-impact confirmation; remote management is restricted to Org Admin and above.
- **No breaking API changes** and **no manual database migration** — Console schema migrations apply automatically on upgrade.

## Breaking Changes

- None. This is a backwards-compatible release.

---
*Repository: witfoo-dev/analytics*
