# WitFoo Agent (WFA) v1.9.22

**Released:** 2026-02-24 07:29:16 UTC

## Changes

c87d34e3 fix: generate proper CA+leaf cert chain for local TLS
05493112 fix: dynamic TLS cert generation, legacy path migration
f1be43ad chore: update common pin (bcrypt Load fix)
8fe8bb2a chore: update common pin (bcrypt env var fix)
88e7f314 fix: disable swap in init scripts, fix data path mismatch
add242aa fix: update nats.go dependency to v1.49.0
202d4a6a fix: add missing docker import in roles_test.go
0af3c0d5 feat: add analytics exporter config seeding for AIO+Conductor mode
bd33f43a fix: use uuid.Nil comparison for license key checks instead of String() == ""
a6fd126f fix: prevent nil pointer panics in broker-dependent services
ffdf0b90 chore: update version to 1.9.14
f26c7ae9 fix: update common to v1.4.9 for NATS auth password fix
02393a84 chore: update version to 1.9.13
fc15fae7 fix: resolve broker-health startup race condition and streams-manager nil panic
cc1930ca fix: remove unused functions aioContainerConf, dataContainerConf, hostPorts
bf3a5e16 fix: propagate Cassandra credentials to all analytics containers
1bce47c8 feat: add AUTH_CONFIG_ENCRYPTION_KEY and wire health checkers
02392e5f chore: update version to 1.9.10
811b3550 fix: pass CASSANDRA_USERNAME to Cassandra container for password init
de98d00e chore: update version to 1.9.9

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
