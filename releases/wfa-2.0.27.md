# WitFoo Agent (WFA) v2.0.27

**Released:** 2026-03-01 19:17:13 UTC

## Changes

5cf4ef43 chore: update dependencies and bump versions
2cdf59b8 chore: update common v1.5.9 → v1.5.10, fix Docker build submodule .git reference
f946fb61 chore: update dependencies and bump versions
58ff00e3 feat(PR166b): conductor memory budgets with GOMEMLIMIT
7ed3ca10 feat(PR173): TOFU trust reset CLI + remote job agent integration
480e78ec chore: update common to v1.5.9 (broadcast flow metric fix)
2289b880 chore: update common dependency to v1.5.8
3847e4e6 chore: update dependencies and bump versions
7bd87bf4 fix: update appliance ID requirement to include all roles for tracking and console registration
87f4c4d4 fix: TLS client cert generation, supportpack HTTP, data node UUID, standalone conductor exporter
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

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
