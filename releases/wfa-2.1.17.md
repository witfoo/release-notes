# WitFoo Agent (WFA) v2.1.17

**Released:** 2026-05-29 22:32:29 UTC

## Changes

94232fde style(wfa): gofmt import ordering in journal_events + unknowns_events
2e210d32 chore(wfa): bump version 2.1.16 -> 2.1.17 to publish accumulated 2.1.11-2.1.16 work
5e59fd95 docs(CLAUDE): add WFA 2.1.0-2.1.13 recent-work section (was frozen at Feb 12)
e8a08db3 fix(build): bump common v1.5.19 -> v1.5.20 (closes streamname.TenableIO standalone build break)
435e0a78 Bump version to 2.1.16
077d0cf2 Bump version to 2.1.15
609724b1 Bump version to 2.1.14
b727ad3c PR345: Add SSL_CERT_FILE env to reverse-proxy container config
d650fd65 release: WFA 2.1.13 — container env-drift self-heal (closes #200 finding 1)
5497af96 release: WFA 2.1.12 — sweep rxd Init startup-race class across every wfa service
618e07b7 release: WFA 2.1.11 — streams-manager bounded broker-health watch (fix #198)
6527829a chore: update dependencies and bump versions
f4a29ba4 chore(deps): bump x/net v0.55.0, x/crypto v0.52.0, go-jose v4.1.4 (closes 14 CVEs)
b485fc08 chore(go): bump to Go 1.26.3 (closes 5 stdlib CVEs)
4b3922f4 fix(certs): preserve generated CA; validate cert/key pair (v2.1.10)
0f7d5115 fix(container-manager): self-heal missing containers (2.1.9)
de33a19c release: WFA 2.1.8 — plumb anthropic_api_key to api-svc (Issue #178 follow-up)
92b142bc release: WFA 2.1.7 — container-manager Stop() nil-mgr guard
8f785e79 release: WFA 2.1.6 — broker-health Init() context-cancel fix
d3760e68 fix(broker-health): surface ctx.Err() in Init() retry loops to prevent Run() nil-deref

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
