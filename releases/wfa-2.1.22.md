# WitFoo Agent (WFA) v2.1.22

**Released:** 2026-06-04 22:46:16 UTC

## Changes

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
2e210d32 chore(wfa): bump version 2.1.16 -> 2.1.17 to publish accumulated 2.1.11-2.1.16 work
5e59fd95 docs(CLAUDE): add WFA 2.1.0-2.1.13 recent-work section (was frozen at Feb 12)
e8a08db3 fix(build): bump common v1.5.19 -> v1.5.20 (closes streamname.TenableIO standalone build break)
435e0a78 Bump version to 2.1.16
077d0cf2 Bump version to 2.1.15
609724b1 Bump version to 2.1.14
b727ad3c PR345: Add SSL_CERT_FILE env to reverse-proxy container config
d650fd65 release: WFA 2.1.13 — container env-drift self-heal (closes #200 finding 1)

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
