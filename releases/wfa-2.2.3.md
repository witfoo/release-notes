# WitFoo Agent (WFA) v2.2.3

**Released:** 2026-06-17 20:00:51 UTC

## Changes

ece51500 chore(release): WFA 2.2.3 — console-client Init role-gate fix (#160)
6322f1ae fix(console-client): drop duplicate role gate in Init + re-init lifecycle channels (#160)
3d630870 chore(release): WFA 2.2.2 — console-client for Analytics/Data roles (#160)
3156431d fix(helper): start console-client for ALL roles with ConsoleFQDN (#160)
4c3c2dbb chore(release): WFA 2.2.1 — console-ui LICENSING_API_SECRET injection (#244)
521a9259 fix(console): inject LICENSING_API_SECRET into the console-ui container (#244)
5c6e112d fix(fetch): print full fetched-config summary with secrets masked (audit P2.1 expanded, #244)
66b64b70 fix(configure): surface Console TLS-verify posture at review (audit P2.1, #244)
f1d61a4c chore(release): WFA 2.2.0 — remote lifecycle handler + wfa fetch (#160 #244)
82694244 chore(deps): bump common to v1.5.37 (PR388/389 #244 #160)
fc1006cc feat(cli): wfa fetch — claim config from a Console one-time URL (PR388c #244)
b381f3c4 feat(service): WFA remote lifecycle command handler over the Console WebSocket (#160)
fc2a6507 chore: update dependencies and bump versions
354114d9 chore: bump common v1.5.36 (tier propagation)
9729f4ba chore: bump common v1.5.36 (pin cleanup)
e7d79432 chore(deps): bump common v1.5.29 -> v1.5.35 (pin uniformity)
d8698208 fix(cassandra): Data-role advertises node host IP via CASSANDRA_BROADCAST_RPC_ADDRESS (2.1.22)
fe654e9b chore: bump appversion 2.1.20 -> 2.1.21
71621d53 chore(security): bump Go build toolchain -> 1.26.4 (GO-2026-5037, GO-2026-5039)
3593b960 chore: update dependencies and bump versions

## Packages

- **DEB:** Available in WitFoo APT repository
- **RPM (RHEL 9):** Available in WitFoo RPM repository
- **RPM (RHEL 10):** Available in WitFoo RPM repository
