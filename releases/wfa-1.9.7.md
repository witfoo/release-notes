# WitFoo Agent (WFA) v1.9.7

**Released:** 2026-02-23 05:06:14 UTC

## Changes

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
1a72ecb8 fix: resolve revive if-return lint warnings in configure.go
4e11f0a2 chore: update dependencies and bump versions
72efd1d4 feat: auto-populate org_id from license, EULA gate, init monitor (PR150b/c)
a8718816 chore: update dependencies and bump versions
a25cb8ea chore: update dependencies and bump versions
6a994707 fix: update common dependency to v1.4.7

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
