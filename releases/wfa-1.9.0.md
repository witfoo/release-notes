# WitFoo Agent (WFA) v1.9.0

**Released:** 2026-02-23 00:48:11 UTC

## Changes

700cc561 fix: RPM publish finds RPMs in artifact subdirectories
48c5b363 fix: APT publish uses flat repo layout matching existing structure
f94fbcb5 fix: create APT repo dirs before copy, shallow clone for speed
16a1f12a fix: handle base64-encoded GPG keys in release workflow
1cd32faa fix: align release workflow secrets with repo secret names
dc578e39 fix: align release workflow secrets with repo secret names
6481f127 chore: update version to 1.9.0
b18fb4f8 fix: gofmt alignment in initialization_test.go
b73a4136 fix: update common to v1.4.8 (includes OrgID field)
1a72ecb8 fix: resolve revive if-return lint warnings in configure.go
4e11f0a2 chore: update dependencies and bump versions
72efd1d4 feat: auto-populate org_id from license, EULA gate, init monitor (PR150b/c)
a8718816 chore: update dependencies and bump versions
a25cb8ea chore: update dependencies and bump versions
6a994707 fix: update common dependency to v1.4.7
ca00f6fd fix: add wfa-helper as dependency of wfa DEB package
6418581f fix: metrics-prom-bridge and Grafana Agent respect IsTLSDisabled()
3564aad1 chore: update common dependency to v1.4.6
80208d5c fix: update DisableTLS references and set version 1.8.31
4e84b12e fix: gofmt formatting on analytics and nodestack files

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
