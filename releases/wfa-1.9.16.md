# WitFoo Agent (WFA) v1.9.16

**Released:** 2026-02-23 20:54:46 UTC

## Changes

bd33f43a fix: use uuid.Nil comparison for license key checks instead of String() == ""
a6fd126f fix: prevent nil pointer panics in broker-dependent services
ffdf0b90 chore: update version to 1.9.14
f26c7ae9 fix: update common to v1.4.9 for NATS auth password fix
02393a84 chore: update version to 1.9.13
fc15fae7 fix: resolve broker-health startup race condition and streams-manager nil panic
bf3a5e16 fix: propagate Cassandra credentials to all analytics containers
1bce47c8 feat: add AUTH_CONFIG_ENCRYPTION_KEY and wire health checkers
02392e5f chore: update version to 1.9.10
811b3550 fix: pass CASSANDRA_USERNAME to Cassandra container for password init
de98d00e chore: update version to 1.9.9
156fbad6 fix: use nodetool for Cassandra healthcheck instead of cqlsh
ce284038 chore: update version to 1.9.8
3391d5c4 fix: use configured credentials in Cassandra healthcheck
ca000d5b fix: RPM release assets and RHEL package manager detection
3b4059d1 fix: generate JWT_SECRET for analytics containers
b136cf06 fix: analytics containers use correct network and data paths
62a87100 fix: align analytics image names with GHCR registry
10c8a266 fix: correct role selection display numbering
80ab4435 fix: remove legacy /usr/local/bin/wfa during DEB install

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
