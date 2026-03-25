# WitFoo Agent (WFA) v2.0.43

**Released:** 2026-03-25 05:34:38 UTC

## Changes

5582f841 fix: start NATS before Cassandra to unblock broker-dependent services
abb7ba39 fix: add healthcheck to conductor-ui container configuration
579f8c07 fix: Cassandra healthcheck race condition — require init completion sentinel
b88a6c70 chore: update dependencies and bump versions
7383c187 chore: update dependencies and bump versions
5bd700bc chore: update dependencies and bump versions
c584413e chore: update dependencies and bump versions
154ed301 chore: update dependencies and bump versions
12015881 chore: update dependencies and bump versions
925c7cf2 chore: update dependencies and bump versions
d9bbc1c3 feat: MetricsPromBridge stability fix + systemd OOM recovery (PR204)
d68f76f0 chore: update dependencies and bump versions
7259aedb chore: update dependencies and bump versions
81fb55ef feat: reduce Prometheus scrape interval from 15s to 60s
33071e41 fix: gofmt loki_bridge.go struct field alignment
fd15b47a fix: rename unused test stub parameters for revive lint
596a2024 chore: update dependencies and bump versions
48d07d15 feat: add journal entry filtering and unknown dedup to loki bridge (PR201b/e)
134ec73f chore: update dependencies and bump versions
461abbc8 chore: update dependencies and bump versions

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
