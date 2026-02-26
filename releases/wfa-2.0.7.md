# WitFoo Agent (WFA) v2.0.7

**Released:** 2026-02-26 00:35:21 UTC

## Changes

1779dc47 fix: use HTTPS for conductor-ui + TLS skip in reverse-proxy
c6ac141f fix: filter duplicate WF_DISABLE_TLS from ToEnvSlice defaults
b1377233 fix: use container DNS name and disable TLS for internal conductor-ui
04366968 feat: PR165 AIO+Conductor SSO — dual-network, REVERSE_PROXY_MODE, JWT_SECRET
5fbc1b48 fix: update common to v1.5.4, bump version to 2.0.4
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

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
