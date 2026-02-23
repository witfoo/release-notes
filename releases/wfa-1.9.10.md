# WitFoo Agent (WFA) v1.9.10

**Released:** 2026-02-23 06:50:58 UTC

## Changes

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
c2776807 fix: resolve DEB file conflict on /witfoo/wfa/bin/systemd/stop.sh
700cc561 fix: RPM publish finds RPMs in artifact subdirectories
48c5b363 fix: APT publish uses flat repo layout matching existing structure
f94fbcb5 fix: create APT repo dirs before copy, shallow clone for speed
16a1f12a fix: handle base64-encoded GPG keys in release workflow
dc578e39 fix: align release workflow secrets with repo secret names
b18fb4f8 fix: gofmt alignment in initialization_test.go
b73a4136 fix: update common to v1.4.8 (includes OrgID field)

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
