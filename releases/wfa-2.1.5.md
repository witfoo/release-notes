# WitFoo Agent (WFA) v2.1.5

**Released:** 2026-04-30 05:46:33 UTC

## Changes

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
9c7a6bc3 chore: bump common v1.5.13 → v1.5.14 for PR309 product types
b9079f93 feat: PR309 — wfa configure product selection prompt
8a68bf3a fix: remove CMD-SHELL healthcheck from conductor-ui scratch container
a56d0595 chore: update dependencies and bump versions

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
