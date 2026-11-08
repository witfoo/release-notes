# WitFoo Console v1.10.0

**Release Date:** 2026-11-08

## Summary

WitFoo Console 1.10.0 ships with WitFoo Analytics 1.2.0 and WitFoo Conductor 1.10.0. SAML sign-in and the session key are tightened (see Action required), user administration is restricted (see Upgrade), the web server and node events socket gain request and message limits (see Improvements), and the Console UI and the Conductor UI draw one pipeline view. Upgrade recommended.

## Action required

### If you run Console UI with SECRET_KEY set

- **A session key that shipped with an earlier release or an example file is refused.** Console UI refuses to start when `SECRET_KEY` holds the value earlier releases carried, the value the Console's example deployment files showed, or a value that is not a usable key. Before you upgrade, either unset `SECRET_KEY`, so the program keeps a key of its own in `.secret_key` beside its database, or set it to a new value of 64 hexadecimal characters (for example the output of `openssl rand -hex 32`). Keep `.secret_key` with the database: a program that finds its database without the file generates a new key, which ends every session.

### If you use single sign-on

- **SAML sign-in.** Console UI SAML sign-in now signs in only accounts whose auth mode is SAML; a Local or LDAP account with the same username is refused. A SAML sign-in whose username (the e-mail address the identity provider sends) matches no account creates one, with the Staff role and no organization: review new accounts on the Users page and assign them an organization.

## New Features

- **One pipeline view in both management UIs.** The Conductor UI and the Console UI draw the pipeline view and the severity, status and suspicion colours from one shared set of visualization sources, so the same appliance shows the same pipeline in both management UIs.
- **Helper connections.** The Console keeps the conductor-ui and wfad-helper connections side by side, so lifecycle commands reach the helper. (See Known limitations: "Console configuration push" and "Console connection limit".)

## Improvements

- **A node's active setting** is changed only by its heartbeat or by an administrator who states it; a status report, an edit of the node or its assignment to an organization leaves it as it is.
- **Management web server.** A request body is limited to 1 MiB, and to 16 KiB on login, logout, node self-registration and other routes that take one small document; over the limit is answered 413. A request has 30 seconds to arrive (10 seconds for its headers) and an idle connection is closed after two minutes, except a WebSocket upgrade.
- **Node events socket (/api/v1/events).** A message over 4 MiB ends the connection (close code 1009); a node's status reports are held to a burst of ten and then one every five seconds per connection, and a report over that is dropped. A connection with no whole message for five minutes, or a write to a node that takes more than 10 seconds, is closed and the node reconnects; no other node waits for it.
- **Confirmation dialogs do not run on Enter.** Confirmation dialogs with a red confirm button (Delete, Disable, Remove, Reset, Reboot Now and similar) do not submit on Enter, even while the Cancel button has focus.
- **Smaller fixes.** The deployment configuration create route takes a JSON body only; the number fields in its wizard, node settings and notification settings show their labels; the Recipients field of an email notification channel is saved; first-party images built on the Alpine base image move to Alpine 3.24.

## Security

This release includes security hardening. The changes that affect operators are listed under Action required and Upgrade (single sign-on, user administration, the session key) and under Improvements (request and message limits on the management web server and the node events socket).

## Upgrade

- **Sessions and deployment configurations.** Every browser session ends once, because the session key changes (unless `SECRET_KEY` was already set to a key of your own). A deployment configuration generated before the upgrade and not yet fetched can no longer be fetched (it answers 500 and uses up its one-time URL): delete it and generate it again. This affects deployment configurations generated in the 72 hours before the upgrade.
- **User administration.** On the Users page, only a Superuser can give an account the Org Admin or Superuser role or delete an Org Admin or Superuser account, and no one can delete their own account. The page still offers Delete on every account and the Org Admin role to an Org Admin; a refused change shows only a general message such as 'Failed to update user'.
- **Organization update must state `is_active`.** The Console organization update answers HTTP 400 when the body does not state `is_active`, where an empty body used to mean off. A script that sent an empty body to mean off must state the value.
- **Console database.** One new migration step (0008, an index on the node status history) runs when the Console first starts after the upgrade.
- **WFA appliances (WFA 2.5.1 or later; install WFA 2.5.4, the current release; 2.5.3 shipped with 1.2.0).**
  - The Console container runs an init process, so its TLS health check no longer leaves defunct helper processes that made the container read unhealthy after a few days. The appliance helper drops a failed Console connection, restarts after a crash, and no longer has to be killed to stop.
  - WFA 2.5.1 and later do not offer FIPS Compliance mode: `wfa fetch` and the Console's remote configuration refuse a configuration that turns it on ("FIPS Compliance mode is not offered by this WFA release"). An appliance that WFA 2.4.14 to 2.4.16 put in FIPS mode now runs without it; the next Console configuration update removes the setting.
  - The first start of WFA 2.5.1 or later recreates every container (not repeated if the appliance already installed WFA 2.5.0, 2.5.1 or 2.5.2): plan the WFA upgrade for a quiet period. Like every WFA package upgrade, the upgrade to WFA 2.5.4 still restarts every container once.
- **Image signing and release tags.** Image signing and SBOM attestation sign only a validated digest; for the appliance images they are advisory (a build whose digest cannot be validated skips signing with a warning). Signatures are keyless: verify them with `cosign verify` against the OIDC issuer `https://token.actions.githubusercontent.com` and the certificate identity `https://github.com/witfoo-dev/analytics/.github/workflows/build-and-push.yml@` followed by the branch the image was built on. Registry clean-up may remove signature and attestation tags. From this release the semver and latest tags carry exactly the digest of the build they name; earlier semver tags keep their original digest, which signatures made by digest may not match.

## Known limitations

- **A running container that Docker marks unhealthy now counts as a down service:** the Console shows its node as Error and a critical cluster problem report is raised; one already unhealthy before the upgrade starts alerting once you upgrade (on WFA appliances, with WFA 2.5.1 or later).
- **An exited or stopped container is not counted as a down service** (only a running container that fails its health check is).
- **Console configuration push.** A configuration pushed while only the node's Conductor UI is connected (not its appliance helper) is marked sent, although nothing applied it.
- **Console connection limit.** A node holds up to four Console connections; a fifth closes the oldest, whichever client (Conductor UI or appliance helper) it belongs to.
- **File permissions on console and conductor nodes.** The console database and the TLS server and client keys are readable by local accounts on the appliance, and the UI session key is briefly readable when WFA starts. Limit shell access on appliances to administrators.
- **A data node is missing from the console.** A data node does not register with the console and does not appear in its node list.

## Platform Release Context

WitFoo Console 1.10.0 is released together with [WitFoo Analytics 1.2.0](1.2.0.md) and [WitFoo Conductor 1.10.0](conductor-1.10.0.md).

## Component Versions

| Component | Version |
|-----------|---------|
| console-ui | 1.10.0 |
