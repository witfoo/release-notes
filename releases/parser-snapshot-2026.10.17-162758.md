# WitFoo Parsers v2026.10.17-162758

**Release Date:** 2026-10-17

## Summary

Release v2026.10.17-162758 of WitFoo Parsers.

## Changes

- 4cf9b4f5b test(parsers): fortigate_cef reads the syslog front through the window the claim uses
- b4b876494 test(parsers): pin the header and the window by value, and a counted line that quotes the header
- e7776ee1a test(parsers): the roster states who claims a key=value line that quotes a Fortinet CEF header, and who claims a record
- c58889ba1 fix(parsers): the key=value fortigate parser refuses a Fortinet CEF record, not a line that quotes the header
- 7a04f650e test(parsers): the Nozomi N2OS parser itself against the recorded verdicts of the shared syslog grammar
- 30178ec62 feat(parsers): dragos_platform - Dragos Platform notifications, CEF over syslog
- 6ff148925 docs+test(parsers): FindCEF - state the RFC 3339 zone offset bound as Go reads it (+24:60), and pin it
- 2d80430fe docs+test(parsers): FindCEF - say what the code does, pin the glued-word shapes, and close the window mutant that survived
- 9630e3701 fix(parsers): fortigate_cef claims a record only when its CEF header is the message of a syslog record (shared syslogheader.FindCEF)
- 083faa3ce feat(parsers): nozomi_n2os - Nozomi Networks N2OS CEF over syslog (alert, audit, health)
- 56b4abbd2 test(parsers): fortigate_cef, vmwarensxcorfu - pin five boundaries a mutation review found unpinned
- 59387ff67 feat(parsers): vmwareidentitysts, vmwareapplmgmt, vmwarevcenter - claim more vCenter appliance idents
- 0facc46dd feat(parsers): vmwarevlcm, vmwareuidataservice - vCenter appliance service logs (VMware vCenter)
- 92f82e1c2 feat(parsers): netifd - OpenWrt network interface daemon link and interface state lines
- ae67c41a6 feat(parsers): vmwarensxcorfu - CorfuDB inbound connection failures (VMware NSX)
- 41cb42e3d feat(parsers): microsoft_defender_linux_installer - MDE on Linux package script logs
- 0b8f9a330 feat(parsers): fortigate_cef - FortiGate CEF-over-syslog (FortiOS 7.x)
- b16d4fd79 fix(parsers): fortigate - leave Fortinet CEF records to fortigate_cef

## Quality Assurance

### Test Results

- **System Tests:** Unknown
- **E2E Tests:** Unknown

---
*Release notes generated without AI assistance (API unavailable)*
*Repository: witfoo-dev/analytics*