# Threat-Detection Pipeline — Elastic Stack

## Objective
Deploy and validate an end-to-end detection-engineering pipeline capable of ingesting
Windows endpoint telemetry and triggering alerts on specific attacker behaviors.

## Environment
- Fleet Server
- Windows 10 endpoint with an enrolled Elastic Agent
- Dockerized Elasticsearch + Kibana stack

## Methodology
1. Deployed Fleet Server and enrolled a Windows 10 Elastic Agent
2. Diagnosed a network misconfiguration (localhost resolution + port-binding issue) that was silently blocking telemetry delivery from endpoint to SIEM
3. Resolved the misconfiguration and confirmed end-to-end telemetry flow
4. Built and validated two custom detection rules
5. Triggered each rule end-to-end to confirm alerting worked as intended

## Detection Rules
| Rule Type | Trigger | MITRE ATT&CK |
|---|---|---|
| Threshold | Repeated failed logons (Event ID 4625) | Credential Access |
| Query (direct match) | Local Administrators group modification (Event ID 4732) | T1098.007 – Account Manipulation: Additional Local or Domain Group Membership |

## Findings
The initial telemetry pipeline appeared deployed correctly but silently failed to
deliver data end-to-end due to a localhost resolution and port-binding conflict —
a class of failure that produces no obvious error and would let a real detection
gap go unnoticed without active validation.

## Mitigations / Recommendations
- Validate telemetry delivery end-to-end after any Fleet/Agent deployment, not just agent enrollment status
- Extend detection coverage beyond the two validated rules to cover additional ATT&CK techniques relevant to the environment
- Monitor Fleet Server health and agent check-in status continuously, not just at initial setup

## Lessons Learned
A detection pipeline that looks "deployed" isn't the same as one that's "working" —
the agent showed as enrolled while telemetry was silently blocked. Building the
detection rules was the easier half of the exercise; diagnosing why data wasn't
arriving in the first place was the real engineering work.

---
*This project was built as part of the CyManII OT Cybersecurity Bootcamp
(ISCS-3523 Lab) in a lab environment.*
