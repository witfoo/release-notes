# WitFoo Agent (WFA) v2.0.16

**Released:** 2026-02-26 17:19:46 UTC

## Changes

60fa394f PR174c: Add demo-data container to WFA nodestack
042a63f0 Fix ClamAV health check: use clamdscan --ping instead of clamdcheck
bb4611da Fix: use HTTPS for CONDUCTOR_URL (conductor-ui has TLS enabled)
3452d899 Fix: SecondaryNetworks only on AIOConductor (conductor-net not found)
7400af40 feat: TOFU fingerprint trust store for MITM detection
5d118457 Update common v1.5.7: fix remoteImageDigest Docker Hub auth
d1e3e73a Update common v1.5.6: fix GHCR credentials sent to Docker Hub
b58ea8c0 Fix gofmt formatting and memory budget overflow on small systems
73bb21fa PR165: Add CONDUCTOR_URL, SecondaryNetworks, and proxy-mode env vars
317e04fb chore: update dependencies and bump versions
7918cbe7 chore: update dependencies and bump versions
9fb665b2 chore: update dependencies and bump versions
729314f7 chore: update dependencies and bump versions
76fffd50 chore: update dependencies and bump versions
1bedec45 chore: update dependencies and bump versions
5cb1a850 chore: update common v1.5.0 → v1.5.5
fdae0014 feat: fix analytics startup crash, dynamic memory budget, health checks
d5b13871 fix: ensure docker group exists and write full TLS cert chain
0b1614e7 chore: update dependencies and bump versions
44dc8db9 feat: add Docker socket access for analytics services

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
