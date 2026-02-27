# WitFoo Agent (WFA) v2.0.26

**Released:** 2026-02-27 05:51:08 UTC

## Changes

07158c34 chore: update version to 2.0.26
e87f75bf chore: update dependencies and bump versions
f5998f10 feat: add DisableTLS and ReverseProxyPort to analytics configuration
81662dad chore: update version to 2.0.23
4640edae Fix: demo login button — propagate IsDemo to WF_DEMO_MODE on analytics-api-svc
a281e7bb fix: prioritize configured NodeHostname over OS hostname in metrics and logging services
ec978a1b Fix: demo mode login button and demo-data Cassandra connectivity
a77055c7 chore: update version to 2.0.21
cf2518c5 Fix: strip port from CASSANDRA_HOST for demo-data-svc entrypoint
a6ac36ec chore: update version to 2.0.20
9c58e201 Fix: auto-create /data/clamav before starting analytics containers
a1f802a6 Fix artifact-ingestion TLS port 8443 and demo-data-svc node.json path
888991aa fix: create /data/clamav directory in AIO init scripts
5977b39f chore: update dependencies and bump versions
c29dc805 chore: update dependencies and bump versions
60fa394f PR174c: Add demo-data container to WFA nodestack
042a63f0 Fix ClamAV health check: use clamdscan --ping instead of clamdcheck
bb4611da Fix: use HTTPS for CONDUCTOR_URL (conductor-ui has TLS enabled)
3452d899 Fix: SecondaryNetworks only on AIOConductor (conductor-net not found)
7400af40 feat: TOFU fingerprint trust store for MITM detection

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
