# WitFoo Empathetic Parsers — February 2026

**Release Date:** 2026-02-19

## Summary

WitFoo Empathetic Parsers is a new capability in the WitFoo platform that automatically detects unknown log formats in customer environments and generates parsers to support them. When the platform encounters log messages it cannot classify, it analyzes the message patterns, generates a new parser, validates it through a full quality gate suite, and deploys it — all without manual intervention.

This inaugural release includes seven new parsers generated from live customer environments.

## New Parsers

| Parser | Log Format | Ingestion Method |
|--------|-----------|-----------------|
| **Filebeat** | Elastic Filebeat event logs | TCP (Lumberjack protocol) |
| **GreyNoise** | GreyNoise threat intelligence feed logs | UDP Syslog |
| **Kafka** | Apache Kafka broker and connector logs | UDP Syslog |
| **Netgear GF Syslog** | Netgear GlideFire appliance syslog | UDP Syslog |
| **Nginx** | Nginx web server access and error logs | UDP Syslog |
| **WitFoo Console** | WitFoo Management Console application logs | UDP Syslog |
| **WitFoo Intel** | WitFoo Intel API service logs | UDP Syslog |

## How It Works

The Empathetic Parser pipeline runs continuously and follows a five-stage process:

1. **Detection** — Monitors log ingestion for unrecognized message patterns
2. **Analysis** — Groups unknown messages by format and identifies parser candidates
3. **Generation** — Produces a Go parser with fingerprint matching patterns
4. **Validation** — Runs quality gates: compilation, unit tests, lint, fingerprint validation, and registry registration
5. **Deployment** — Pushes the validated parser to the signal-parser service

Parsers that do not pass all five quality gates are rejected and never deployed.

## Parser Library

With these additions, the WitFoo signal-parser library now includes **256 parsers** covering security, network, infrastructure, cloud, and application log formats.

---
*WitFoo Empathetic Parsers — automatically understanding your environment*
*Repository: witfoo-dev/analytics*
