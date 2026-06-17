# WitFoo Agent (WFA) v2.2.0

**Released:** 2026-06-17 07:41:05 UTC

## Changes

f1d61a4c chore(release): WFA 2.2.0 — remote lifecycle handler + wfa fetch (#160 #244)
82694244 chore(deps): bump common to v1.5.37 (PR388/389 #244 #160)
fc1006cc feat(cli): wfa fetch — claim config from a Console one-time URL (PR388c #244)
b381f3c4 feat(service): WFA remote lifecycle command handler over the Console WebSocket (#160)
fc2a6507 chore: update dependencies and bump versions
354114d9 chore: bump common v1.5.36 (tier propagation)
9729f4ba chore: bump common v1.5.36 (pin cleanup)
e7d79432 chore(deps): bump common v1.5.29 -> v1.5.35 (pin uniformity)
d8698208 fix(cassandra): Data-role advertises node host IP via CASSANDRA_BROADCAST_RPC_ADDRESS (2.1.22)
fe654e9b chore: bump appversion 2.1.20 -> 2.1.21
71621d53 chore(security): bump Go build toolchain -> 1.26.4 (GO-2026-5037, GO-2026-5039)
3593b960 chore: update dependencies and bump versions
e3a9d7eb chore: bump common v1.5.23 -> v1.5.24 (PR350c — 5xx + conn-closed health codes)
9d224a85 chore: bump common v1.5.21 -> v1.5.23 (PR350c — emit health_code)
f15026ed chore: update dependencies and bump versions
0a4012d4 fix(analytics): inject Cassandra env into artifact-ingestion for conductor-feed registration; pin source_subject (PR347)
6eee83a9 fix(streams-manager): wait for broker-health=Run before connect (analytics #202)
325e8e7d Revert "fix(streams-manager): level-triggered broker-health wait (closes #202, #201)"
06684061 fix(streams-manager): level-triggered broker-health wait (closes #202, #201)
94232fde style(wfa): gofmt import ordering in journal_events + unknowns_events

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
