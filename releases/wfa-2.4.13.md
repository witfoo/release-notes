# WitFoo Agent (WFA) v2.4.13

**Released:** 2026-08-10 20:34:36 UTC

## Summary

WFA 2.4.13 is the current agent release. It ships the **role-change cleanup fix** and consolidates
every agent release since **2.2.5** — nineteen versions of appliance reliability work, remote
management, deployment options, and privacy hardening.

Appliances with automatic updates enabled (`auto_update: true`) pick this up on their next check;
the APT and RPM repositories both carry it. Because these notes span the whole 2.2.5 → 2.4.13
range, an operator upgrading from any recent version can read the themed sections below for what
changes, and the per-version index at the end for a specific release.

**Highlight — role changes now complete cleanly (2.4.13).** Reconfiguring an appliance from one
role to another (for example Conductor → All-in-One) previously left the *previous* role's
containers running and holding their network ports, so the new role's web front end could not
start and the agent log repeated:

```
error starting container conductor-ui-svc: ... Bind for :::443 failed: port is already allocated
```

The agent computes its container set once per start from the current role, and every Docker
operation was scoped to those names — so a previous role's containers were invisible to all of
them while still running under a restart policy that survives reboots. WFA now reclaims them
before starting the new stack, and again on its regular reconcile pass. Only WitFoo's own
containers are ever reclaimed, matched by exact name against a closed list, so anything else
running on the host is untouched. Shutdown was hardened in the same change: it now attempts every
container and reports all failures together, instead of abandoning the rest at the first problem —
which was itself one of the ways a stale stack got left behind.

## What's New Since 2.2.5

### Appliance reliability and self-healing

- **Role changes reclaim the previous role's stack** (2.4.13, above).
- **A registry outage no longer strands a node** (2.4.6). If the image registry is unreachable,
  the agent falls back to whatever images are present locally — a node with 15 of 16 images starts
  15, not zero — retries the pull once per five-minute tick, and preserves the original failure
  cause instead of overwriting it.
- **Support packages are complete and safe to share** (2.4.10, 2.4.11). Conductor API captures
  were hitting the Analytics API and being written out as 401 stubs, so packs from analytics-role
  appliances arrived without integration config or pipeline throughput; a non-200 is now *recorded
  as a failed capture* rather than masquerading as data. Only one NATS broker was captured on
  appliances running two, omitting the one that owns the data streams and every KV bucket — both
  are now collected. And the artifact-exporter environment dump shipped several credentials in
  plaintext while `node.json` in the same package was redacted; those values are now masked with
  the same classifier (keys stay visible).
- **All-in-One + Conductor appliances report pipeline health** (2.4.12). That role runs the full
  conductor stack but was omitted from the metrics-source roster, so connector and pipeline health
  had no remote signal at all. A guard now derives both sides from the service registry.
- **Standalone Conductor dashboards show metrics again** (2.4.3). A node's own host and Docker
  gauges were being dropped when the message broker came up, because the two metric sources were
  treated as mutually exclusive when they actually carry different families. They are now additive,
  which also makes a node's own metrics immune to broker restarts.
- **Service start-up deadlocks resolved** (2.2.7, 2.3.0). A broker connection attempt could block a
  service in initialization indefinitely — only an agent restart recovered it. Attempts are now
  time-bounded so the service fails fast, cycles back through its health gate, and self-heals. The
  circular start-up dependency that could permanently wedge a node (broker health waiting on
  buckets that the blocked service creates) is broken by a bootstrap step that creates them first,
  with non-essential buckets no longer able to hold the pipeline back.
- **Agent password reset repairs older installs** (2.4.9). `wfa user-reset` failed on appliances
  whose analytics database predates the per-user theme column; it now applies the same idempotent
  schema heal before reading, so it works against old and new schemas alike.

### Remote management and provisioning

- **Reset the Analytics admin account from the appliance** (2.4.8). The new `wfa user-reset`
  command lists the existing initial admin account, optionally changes its login email (refusing an
  address another account already uses), sets a new password, and re-activates a deactivated
  account. It works even after a prior email change, and takes effect at next login with no service
  restart.
- **Remote lifecycle management from the Console** (2.2.0). Start, stop, restart, pull-images and
  upgrade can be driven from the Console over its WebSocket connection. Appliances that carry a
  database refuse a destructive action unless it is explicitly confirmed.
- **Zero-touch provisioning** (2.2.0). The new `wfa fetch` command claims a Console-generated
  one-time URL, applies the pre-built configuration, collects only the machine-specific fields and
  admin password locally, and finalizes — intended for MSSP fleet roll-outs.
- **All node roles report to the Console** (2.2.2, 2.2.3, 2.2.4). Analytics and Data nodes now
  start their Console client, self-register before authenticating, and no longer fail the handshake
  indefinitely.
- **Console-forwarded agent inventory stopped churning the broker** (2.2.8, 2.2.9). The Beats
  projection sent to the Console was rebuilding a heavy broker client every fifteen seconds and
  timing out on shutdown each time — 73 connection-drain timeouts in seventeen minutes on a live
  appliance. It now uses a short-lived read-only connection.

### Deployment options

- **Console deployment option** (2.4.7). `wfa configure` offers the free Console product, so any
  fleet — conductor, reporter or monitor licensed — can stand up a Console management node.
  Previously the Console role was advertised in the menu and then rejected for every product except
  analytics. The role menu no longer offers roles the selected product cannot launch.
- **Unattended upgrades reconcile without a restart** (2.4.4). Toggling automatic updates from the
  Console now takes effect immediately in both directions, and a corrupt or stale configuration
  self-heals on every reconcile. Reporter nodes gain the timer and service overrides they were
  missing. Third-party packages remain deliberately excluded from unattended upgrades but are now
  surfaced as "requires maintenance" in the Health UI, to be applied through the Upgrade Node job
  during a maintenance window.
- **Distributed Data-role databases are reachable** (2.1.22, carried in 2.2.x). A Data-role
  Cassandra now advertises the node's reachable host IP rather than its container-internal address,
  fixing "no hosts available" failures on distributed deployments.
- **Content-security-policy pass-through** (2.4.5). Optional CSP source extensions are passed from
  the agent's environment into the reverse-proxy container — opt-in, with an empty default, so
  disconnected appliances are byte-identical. Used by cloud-fronted appliances whose CDN injects a
  monitoring beacon.

### Privacy and security

- **Telemetry is redacted at the source** (2.4.0). Journal events are redacted where they are
  produced, so every downstream consumer inherits the policy, and the host label is hashed
  wherever it appears. Configurable **off / standard / strict**; standard redaction is
  pseudonymization rather than anonymization, and is documented as such.
- **Opt-in, sanitized problem reporting** (2.4.0). Appliances can ship PII-free problem metadata to
  WitFoo Intel for support triage; the automatic support package is opt-in and secret-sanitized,
  and honors the strict redaction policy.
- **Readiness probe verifies its TLS peer** (2.2.5, the previous published notes). The web-UI
  readiness check no longer trusts any certificate: it loads the appliance CA bundle and either
  chain-verifies or pins the known appliance identity.
- **Embedded monitoring credentials** (2.4.0 → 2.4.1). 2.4.0 removed two hardcoded telemetry write
  tokens and failed closed without provisioned replacements; 2.4.1 restored the defaults so fleet
  metrics and log forwarding continue to work without per-node provisioning. Operator-provisioned
  credentials still take precedence, and 2.4.0's producer-side redaction and hashed host label are
  retained. Removing the embedded credentials properly is tracked for a future release.

## Upgrade Notes

- **No action required for most fleets.** Appliances with `auto_update: true` pick 2.4.13 up on
  their next hourly check. Note that the package index is served through a CDN, so a newly
  published version can take one to three hours to become visible.
- **Manual upgrade:** `apt update && apt install --only-upgrade wfa` (Debian/Ubuntu) or
  `dnf upgrade wfa` (RHEL 9/10), then `systemctl restart wfad`.
- **RHEL fleets:** RPM publishing was interrupted for 2.4.7 and resumed at 2.4.8, so RHEL
  appliances may appear to jump several versions. 2.4.13 supersedes all of them.
- **Changing a node's role** is the operation 2.4.13 fixes. Upgrade to 2.4.13 *before* reconfiguring
  an appliance from one role to another; on earlier agents the previous role's containers must be
  removed by hand if the new role's front end fails to bind its port.
- **Disconnected/air-gapped appliances** are unaffected by the CSP and telemetry changes — both
  default to inert.

## Packages

- **DEB:** Available in the WitFoo APT repository
- **RPM (RHEL 9):** Available in the WitFoo RPM repository
- **RPM (RHEL 10):** Available in the WitFoo RPM repository

## Version Index (2.2.6 – 2.4.13)

| Version | Date | Change |
| --- | --- | --- |
| 2.4.13 | 2026-08-10 | Role change reclaims the previous role's containers; shutdown aggregates errors |
| 2.4.12 | 2026-08-05 | All-in-One + Conductor appliances emit pipeline metrics in node status |
| 2.4.11 | 2026-08-05 | Support package captures both NATS brokers |
| 2.4.10 | 2026-08-05 | Support package conductor captures corrected; exporter environment secrets masked |
| 2.4.9 | 2026-08-04 | `wfa user-reset` heals the older-install schema difference |
| 2.4.8 | 2026-08-04 | New `wfa user-reset` command |
| 2.4.7 | 2026-08-03 | Console deployment option in `wfa configure` |
| 2.4.6 | 2026-07-28 | Registry-outage resilience: local-image fallback + pull retry |
| 2.4.5 | 2026-07-19 | CSP source extensions passed through to the reverse proxy |
| 2.4.4 | 2026-07-11 | Unattended-upgrade config reconciles without a restart; Reporter overrides added |
| 2.4.3 | 2026-07-09 | Node's own host/Docker metrics no longer dropped when the broker starts |
| 2.4.2 | 2026-06-30 | In-app Conductor link shown only on co-hosted appliances |
| 2.4.1 | 2026-06-30 | Restore embedded telemetry write tokens removed in 2.4.0 |
| 2.4.0 | 2026-06-30 | Producer-side telemetry redaction, hashed host label, opt-in problem reporting |
| 2.3.0 | 2026-06-23 | Service-init deadlock resilience: broker bootstrap + bucket classification |
| 2.2.9 | 2026-06-23 | Console agent-inventory read no longer churns the broker |
| 2.2.8 | 2026-06-23 | Beats → Console forwarding release |
| 2.2.7 | 2026-06-22 | Bounded broker connect in every broker-dependent service |
| 2.2.6 | 2026-06-20 | Rebuild to correct a stale published package binary |
