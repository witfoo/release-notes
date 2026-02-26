# WitFoo Agent (WFA) v2.0.22

**Released:** 2026-02-26 23:18:41 UTC

## Changes

4640edae Fix: demo login button — propagate IsDemo to WF_DEMO_MODE on analytics-api-svc
a77055c7 chore: update version to 2.0.21
cf2518c5 Fix: strip port from CASSANDRA_HOST for demo-data-svc entrypoint
a6ac36ec chore: update version to 2.0.20
9c58e201 Fix: auto-create /data/clamav before starting analytics containers
a1f802a6 Fix artifact-ingestion TLS port 8443 and demo-data-svc node.json path
5977b39f chore: update dependencies and bump versions
c29dc805 chore: update dependencies and bump versions
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

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
