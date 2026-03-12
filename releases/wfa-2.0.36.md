# WitFoo Agent (WFA) v2.0.36

**Release Date:** 2026-03-12

## Summary

WFA v2.0.36 includes dependency updates and version bumps aligned with the Analytics v0.9.3 release.

## Changes

- Update Go module dependencies across all conductor_suite services
- Update npm dependencies for conductor-ui frontend
- SAML provider wizard support added to conductor-ui (PR192)
- Version alignment with Analytics v0.9.3 release

## Upgrade

WFA packages will auto-update via the standard apt/yum update mechanism when `auto_update: true` is set in node.json.
