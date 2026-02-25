# WitFoo Agent (WFA) v2.0.3

**Released:** 2026-02-25 20:52:08 UTC

## Changes

4ff51c1e bump: version 2.0.3 for CDN cache invalidation
bcb7ed07 fix: update common to v1.5.3 for PruneChildren fix
adbac21a feat: image update watcher resilience for post-startup stability
e7a8d212 feat: AIO+Conductor startup resilience (PR163e)
ba9d5a24 chore: update version to 2.0.0
a5ecad42 fix: clamp memory budget to system RAM on small CI runners
fdae0014 feat: fix analytics startup crash, dynamic memory budget, health checks
0b1614e7 chore: update dependencies and bump versions
44dc8db9 feat: add Docker socket access for analytics services
cc29e443 chore: update dependencies and bump versions
71e82212 feat: add wfad-linux-amd64 binary for Linux AMD64 architecture
3ddcd836 chore: bump version to 1.9.23
894c7673 feat: cross-node CA exchange via TOFU for TLS trust
c87d34e3 fix: generate proper CA+leaf cert chain for local TLS
05493112 fix: dynamic TLS cert generation, legacy path migration
f1be43ad chore: update common pin (bcrypt Load fix)
8fe8bb2a chore: update common pin (bcrypt env var fix)
88e7f314 fix: disable swap in init scripts, fix data path mismatch
add242aa fix: update nats.go dependency to v1.49.0
202d4a6a fix: add missing docker import in roles_test.go

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
