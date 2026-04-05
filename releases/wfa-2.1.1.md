# WitFoo Agent (WFA) v2.1.1

**Released:** 2026-04-05 23:17:32 UTC

## Changes

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
c8bfc561 chore: update dependencies and bump versions
072ecf22 feat: PR206 — reboot-required detection and command file monitoring
1bae0928 fix: increase Cassandra healthcheck StartPeriod to 300s
5582f841 fix: start NATS before Cassandra to unblock broker-dependent services
abb7ba39 fix: add healthcheck to conductor-ui container configuration
579f8c07 fix: Cassandra healthcheck race condition — require init completion sentinel
b88a6c70 chore: update dependencies and bump versions
7383c187 chore: update dependencies and bump versions
5bd700bc chore: update dependencies and bump versions

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
