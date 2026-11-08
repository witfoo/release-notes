# WitFoo Conductor v1.10.0

**Release Date:** 2026-11-08

## Summary

The Conductor gains seven pull connectors, a NetFlow and IPFIX collector, OCSF 1.8.0 encoding in the artifact exporter, and new parsers. Exporters start again after a broker restart, and signal-parser resumes parsing on its own after a broker outage. Conductor UI SAML sign-in now signs in only accounts whose Auth Source is SAML (SSO). Upgrade every Conductor to 1.10.0 (the Conductor released with Analytics 1.2.0) at the same time as Analytics or before it. Upgrade recommended.

## Action required

- **Upgrade every Conductor with Analytics or before it.** Upgrade every Conductor, including separate or remote Conductor nodes, to 1.10.0 (the Conductor released with Analytics 1.2.0) at the same time as Analytics or before it; all-in-one appliances upgrade both together. Conductors released with Analytics 1.1.5 or older (Conductor 1.9.0 or older) that send to Analytics 1.2.0 keep only about one record per page or poll for Azure Security, Microsoft Sentinel, Cisco AMP, Cisco Umbrella, Cisco Meraki, Splunk and Stealthwatch, and the batch is still answered as accepted. They also send batches with no size bound, which an Analytics 1.2.0 receiver on the small body profile (appliances with less than about 18.5 GiB of host memory) can refuse with 413.
- **WFA-managed appliances: update the analytics images to 1.2.0, then install WFA 2.5.4 (the current release; 2.5.3 shipped with 1.2.0) soon after.** If WFA automatic updates are off, note that WFA 2.5.3 was published before the 1.2.0 images: update the images first anyway. Until WFA 2.5.1 or later runs, the broker holds about 30 KB of publish ids in memory for each artifact per second of ingest on the Conductor DATA stream.
- **Plan the WFA upgrade for a quiet period.** The first start of WFA 2.5.1 or later recreates every container, so ingestion pauses (on a WitFoo test appliance, between 3.5 and about 9 minutes on an AIO plus Conductor node). If the appliance already installed WFA 2.5.0, 2.5.1 or 2.5.2, that is not repeated, but like every WFA package upgrade, the upgrade to WFA 2.5.4 still restarts every container once.
- **SAML SSO nodes: convert accounts before upgrading.** On a Conductor node that serves its own sign-in page (on an all-in-one node, Analytics signs users in) and whose Authentication Mode is SAML SSO, set each Local or LDAP account that signs in through SAML to the SAML (SSO) Auth Source (Conductor Admin, Users; the Users page lists it as Auth Mode) before upgrading: after the upgrade an account left on Local or LDAP cannot sign in at all, a local administrator included. The Users page does not let a Superuser change its own Auth Source, so if the only Superuser is one of those accounts, first create a second Superuser with the SAML (SSO) Auth Source, using the username that SAML sign-in derives for another person, and convert the first account from it, or switch Authentication Mode (Conductor Admin, Settings, General) to Local/SAML (Mixed) until the accounts are converted. Instructions, including the way back for a node upgraded first, are in the Conductor documentation on docs.witfoo.com.
- **`SECRET_KEY`.** Conductor UI refuses to start when `SECRET_KEY` holds the value earlier releases carried, the value the Console's example deployment files showed, or a value that is not a usable key. Before you upgrade, either unset `SECRET_KEY`, so the program keeps a key of its own in `.secret_key` beside its database, or set it to a new value of 64 hexadecimal characters (for example the output of `openssl rand -hex 32`). Keep `.secret_key` with the database: a program that finds its database without the file generates a new key, which ends every session.
- **NetFlow and IPFIX port.** On Conductor and AIO-Conductor appliances, WFA 2.5.1 and later publish port 2055/udp and open it in the host firewall (the listener stays off until you enable it under Log Servers). Under WFA 2.4.x, publish it by hand as the Configuration documentation on docs.witfoo.com describes, or upgrade WFA to 2.5.4, the current release (2.5.1 or later publishes it). On a Docker Compose stack add `2055:2055/udp` to the signal server's ports.
- **Hand-typed `/ingestion/` endpoints.** The Analytics 1.2.0 reverse proxy no longer serves the `/ingestion/` path. Conductors send artifacts to the analytics host directly by default (port 8443, or the internal port 8090 on an AIO plus Conductor appliance), so nothing changes for them. A Conductor whose artifact endpoint was typed by hand with an `/ingestion/` prefix now receives 404 and must be pointed at the direct address.

## New Features

- **Flow telemetry and new pull connectors.** Seven pull connectors, each with its Conductor UI form: Cloudflare, VPC flow logs from AWS, Azure, GCP and OCI, SNMP (v2c and v3) and an HTTP poll. A NetFlow (v5, v7, v9) and IPFIX collector joins them; its listeners (UDP 2055 and HTTP 8008) stay off on a new node until you enable them under Log Servers. The artifact exporter gains OCSF 1.8.0 encoding.
- **Parsers 1.1.0** (the Conductor released with Analytics 1.1.5 shipped 1.0.5): new parsers vmwarensxvdpi, vmwarensxcontextmux, nozomi_n2os, dragos_platform, fortigate_cef, microsoft_defender_linux_installer, vmwarensxcorfu, netifd, vmwarevlcm and vmwareuidataservice; vmwareidentitysts now claims the vCenter SSO audit-logger events. Nothing to set.
- **One pipeline view.** The Conductor UI and the Console UI draw the pipeline view and its colours from one shared set of visualization sources.

## Improvements

- **Users (Conductor UI).** Saving a user as Inactive deactivates the account and ends its sessions; no one can deactivate their own account.
- **Management web server (Conductor UI).** Request bodies are limited to 1 MiB (16 KiB on small-document routes in standalone mode); a request has 30 seconds to arrive; an idle connection is closed after two minutes.
- **Refused and busy receivers (analytics and reporter connectors).** A batch the receiver refuses for what it holds (400, 413, 422) is divided until the artifact it refuses is found; an artifact refused sent alone is set aside (reported once, acknowledged, not sent again) only on evidence that the receiver works for others, else kept and given back to the stream after 30 s; three refused flushes in a row stop the instance and nothing is discarded. A busy receiver (503/429/5xx) is waited for; a slow destination no longer causes duplicate deliveries.
- **Analytics connector.** A partial batch is sent 2 seconds after its first artifact; an artifact above the receiver's 9.5 MiB limit is reported and not sent; a batch is sent at 16 MiB (was 32 MiB); rejected counts of a 202 go to pipeline_messages_receiver_rejected_total.
- **Connector retry and health.** A transient failure is re-attempted after 5, 10, 20, 40, then 60 seconds (was always 15 minutes). Fewer wrong "auth failed / rate limited" health labels; a rejected API key no longer takes the whole signal-client process down.
- **Meraki.** The Meraki connector reads events in the format Meraki documents, follows its paging, keeps its progress when a pass runs out of time, reports rate limits, and keeps going when one organization's networks are refused. A redirect is followed only over https on port 443, to one of the five documented Meraki API hosts or a meraki.com subdomain.
- **Predicate filters** acknowledge the messages they drop, and a numeric rule whose value is not a number is refused when the filter is built.
- **DNS zone transfer** polls every 60 minutes (was 60 seconds) and transfers a zone only when its SOA serial changed or 12 hours have passed.
- **Also:** exporter metrics count only data messages; every first-party image built on the Alpine base image moves to Alpine 3.24 (broker-edge leaves the end-of-life 3.19).

## Fixes

- **Exporters after a broker restart** start their instances again (in the Conductors released with Analytics 0.9.8 to 1.1.5 they stayed stopped).
- **Signal parser after a broker outage** resumes parsing on its own (it could stop for good with "broadcast hub is closed"; present in the Conductors released with Analytics 0.9.8 to 1.1.5).
- **Per-record ids.** The Cisco AMP, Umbrella and Meraki, Azure Security, Microsoft Sentinel, Splunk and Stealthwatch connectors give every record its own artifact id, so records that arrive together are no longer collapsed into one.
- **Palo Alto Networks (PAN-OS).** TRAFFIC records, read from shifted columns by the Conductors released with Analytics 1.1.2 to 1.1.5, are read correctly again.
- **Smaller fixes:** one OCI flow record with a negative byte count no longer fails every fetch cycle · a connector disabled right after a burst still delivers its queued events · corrected TCP/7514 and UDP/514 listener help text · the email channel Recipients field is saved.

## Security

This release includes security hardening. The changes that affect operators are listed under Action required and Upgrade.

- **Single sign-on:** SAML sign-in refuses accounts whose Auth Source is Local or LDAP.
- **Session keys:** a `SECRET_KEY` earlier releases carried, or one that is not a usable key, is refused.
- **User administration:** only a Superuser can give the Administrator or Superuser role or deactivate such an account.
- **Request limits:** the Conductor UI web server limits request bodies and the time a request has to arrive.

## Upgrade

- **Conductor UI sessions end once**, because the session key changes (unless `SECRET_KEY` was already set to a key of your own).
- **Broker edge on loopback.** With WFA 2.5.1 or later, NATS (4222, 8222) and the broker edge (4223) are published on the appliance's loopback address only, so a client off the appliance that reached them directly loses access.
- **WFA 2.5.1 or later on the appliance (install WFA 2.5.4, the current release; 2.5.3 shipped with 1.2.0).** node.json is now readable by root only (mode 0600), so any non-root reader you added loses access to it. WFA 2.5.1 and later do not offer FIPS Compliance mode: an appliance that WFA 2.4.14 to 2.4.16 put in FIPS mode now runs without it, and `wfa fetch` and the Console's remote configuration refuse a configuration that turns it on.
- **Requests that switch one setting must state its value.** The Conductor appliance-configuration update requires `enabled` on every update element and answers HTTP 400 when the body does not state it; a script that sent an empty body to mean off must send `{"enabled": false}`.
- **Integration regions.** A Rapid7 InsightIDR, AWS GuardDuty or AWS Security Hub region holding a dot, slash, @, colon or inner space is refused when the integration runs (Rapid7 regions are trimmed and lower-cased first). Check these region fields after the upgrade.
- **Image signing and tags.** Image signing and SBOM attestation sign only a validated digest; for the appliance images they are advisory: a build whose digest cannot be validated skips signing with a warning and nothing re-signs it. Signatures are keyless: verify them with `cosign verify` against the OIDC issuer `https://token.actions.githubusercontent.com` and a certificate identity naming `https://github.com/witfoo-dev/analytics/.github/workflows/build-and-push.yml@` followed by the branch the image was built on. Registry clean-up may remove signature and attestation tags on its own schedule. From this release the semver and latest tags carry exactly the digest of the build they name; earlier semver tags keep their original digest, which signatures made by digest may not match.

### Analytics 1.2.0 changes that affect Conductors

- **Per-sender share.** One sender (by connection address; an IPv6 address as its /64) may hold at most half of the receiver's budget (128 MiB default profile, 64 MiB small); past that it is answered 503, which the exporter reads as a delay and not a loss.
- **Conductors behind one address.** More than about 40 to 60 conductors behind ONE address (a NAT, a load balancer) or one IPv6 /64 share 128 connections. Where you set the receiver's environment, put that address in WF_CONN_TRUSTED; a WFA appliance has no setting for it in Analytics 1.2.0, so give those conductors more than one address or receiver (refused connections are retried; nothing is lost, but throughput drops).
- **Publish failures.** A batch whose publish fails in a way another attempt can pass is answered 503 (was 202, which the exporter took as delivered); on the resend, an artifact with no time_id can be published twice. On the WebSocket stream the receiver retries for 20 seconds, then closes with 1013; messages written after the held one and not yet read are lost.
- **Artifact ids** that are not version-1 UUIDs are replaced, and the sender's id is kept as `source_time_id`.
- **Feed approval (Analytics).** At most 1,000 pending feeds per organization (the 1,001st registration is dropped, and the sender is told nothing); a pending feed expires after 7 days and registers again on its conductor's next batch.

## Known limitations

- **Syslog export is UDP only;** TCP and TLS arrive in Conductor 1.11.0, released with Analytics 1.2.1.
- **Flow connectors.** An OCI flow longer than the connector's four-minute guard is never read; one failed GCP flow-log hand-off pauses pulls for ten minutes; with the NetFlow collector's flow-state store full, a restart can read stale baselines.
- **Raised batch_size behind one address.** An exporter with batch_size raised to 1000 and artifacts of 16 KiB or more can be slowed behind one address; on a WFA appliance keep the default.
- **Slow uplinks.** A flush near the 16 MiB bound needs about 9 Mbit/s for that Conductor alone; below that every attempt times out and nothing is delivered, though nothing is lost. The default batch size of 100 is not affected.
- **Resent artifacts (Analytics 1.2.0).** Analytics stores a resent artifact whose id it replaced twice if the resend arrives after its one-minute duplicate window.
- **The Conductor dashboard's Container Status tab** shows run state only; a container failing its health check still reads Running.
- **File permissions on conductor nodes.** TLS keys are readable by local accounts and the UI session key briefly when WFA starts; limit shell access to administrators.

## Platform Release Context

WitFoo Conductor 1.10.0 ships as part of the 1.2.0 platform release. See [WitFoo Analytics 1.2.0](1.2.0.md) and [WitFoo Console 1.10.0](console-1.10.0.md).

## Component Versions

| Component | Version |
| --- | --- |
| signal-server | 1.10.0 |
| signal-client | 1.10.0 |
| signal-parser | 1.1.0 |
| artifact-filter | 1.10.0 |
| artifact-exporter | 1.10.0 |
| broker-edge | 1.10.0 |
| conductor-ui | 1.10.0 |
