# WitFoo Agent (WFA) v2.1.8

**Released:** 2026-05-13 06:34:01 UTC

## Changes

de33a19c release: WFA 2.1.8 — plumb anthropic_api_key to api-svc (Issue #178 follow-up)
92b142bc release: WFA 2.1.7 — container-manager Stop() nil-mgr guard
8f785e79 release: WFA 2.1.6 — broker-health Init() context-cancel fix
d3760e68 fix(broker-health): surface ctx.Err() in Init() retry loops to prevent Run() nil-deref
d18a5677 fix: inject AI_ENCRYPTION_KEY + AUTH_CONFIG_ENCRYPTION_KEY for analytics services (WFA 2.1.5)
3a9c0084 chore: update dependencies and bump versions
2b4a422f chore: bump version to 2.1.3
cf2e7386 fix: pass WF_DEMO_MODE to incident-engine container
64dc4cb9 chore: update Dockerfiles and CI workflow to Go 1.26.2
605d03b8 fix: provide yes input for hardware check on CI runners
f761cfbb fix: CI test failures — hardware-independent role test + revive lint
55ef2a5f release: WFA 2.1.2 — PR315 configure wizard fixes + Go 1.26.2
eca3b97e fix: WFA configure wizard fixes + production resilience (PR315)
8659ad94 release: WFA 2.1.1 — disk space resilience (closes analytics#146)
75ba24b7 chore: update common dependency to v1.5.16
323caf29 fix: add disk space awareness to container manager (closes #146)
63543075 fix: persist CA key across upgrades to prevent TOFU trust breaks
270a2468 fix: gofmt configure.go for release 2.1.0
9e1433f0 release: WFA 2.1.0 — 7th Gen licensing model
5a426c1c feat: add Reporter Lite to product selection menu (option 8)

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
