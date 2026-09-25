# PROJECT ICARUS — Azure Cloud Security Lab

A hands-on Azure security lab focused on detecting risky configuration changes, investigating alerts, verifying remediation, and documenting incident response.

**Azure Activity Logs → Microsoft Sentinel Detection → Investigation → Remediation Verification → Incident Closure**

---

## Project Goals

- Establish a secure Azure lab baseline.
- Collect and investigate Azure Activity Logs.
- Build and validate Microsoft Sentinel detection rules.
- Investigate storage exposure and excessive permissions.
- Document findings, response actions, and security improvements.

---

## Lab Environment

| Component | Purpose |
|---|---|
| Microsoft Azure | Cloud lab environment |
| RG-PROJECT-ICARUS | Resource group containing lab resources |
| sticaruslabjoey | Storage account used for controlled testing |
| law-project-icarus | Log Analytics workspace |
| Microsoft Sentinel | Alert detection and incident investigation |
| Azure Activity Logs | Evidence of resource configuration changes |

---

## Skills Demonstrated

- Azure security configuration
- Activity Log collection and analysis
- Kusto Query Language (KQL)
- JSON request-body parsing
- Scheduled analytics rule configuration
- Alert triage and incident ownership
- Event correlation and completion verification
- Remediation validation
- Incident classification and documentation

---

## Project Status

| Scenario | Focus | Status |
|---|---|---|
| ICARUS-01 | Detect storage account all-networks access | Test and incident workflow complete; write-up in progress |
| ICARUS-02 | Investigate excessive permissions | Planned |
| Security posture review | Review policy and configuration improvements | Planned |

---

## ICARUS-01 — Storage Account All-Networks Access

[Read the full investigation and response case study](Labs/ICARUS-01-Storage-Network-Exposure.md)

### Objective

Detect an update request that enables public network access and sets the storage account's network default action to Allow.

### Completed Workflow

1. Created a scheduled Microsoft Sentinel analytics rule.
2. Performed an authorized all-networks configuration change.
3. Immediately restored selected-network access.
4. Confirmed the rule generated an alert and incident.
5. Investigated the underlying Azure Activity Log event.
6. Matched the request to its successful completion using its correlation ID.
7. Verified restoration of selected-network access.
8. Assigned and closed the incident as **Benign Positive — Suspicious but expected**.

### Investigation Outcome

The rule detected the intended configuration change. Incident #1 was investigated and closed as authorized lab activity.

The evidence confirms a network configuration change. It does not establish anonymous blob access or data exfiltration.

### Improvement Identified

Repeated alerts were observed for the same event time, consistent with overlapping query windows. Alert deduplication is a planned tuning improvement.

---

## Key Security Lessons

- A request event alone does not prove that a change succeeded.
- Correlation IDs connect requests to their completion events.
- Restoring a setting does not remove the historical evidence of the change.
- A correct detection can represent authorized activity.
- Incident grouping and alert deduplication serve different purposes.

---

## Interview Summary

Built and validated a Microsoft Sentinel detection for Azure Storage network exposure. Traced the alert to its source event, verified successful execution and remediation, and documented incident closure for an authorized test.

---

## Next Steps

- Complete the ICARUS-01 evidence write-up.
- Tune repeated-alert behavior.
- Complete ICARUS-02 excessive-permissions testing.
- Review Azure Policy and security posture.
- Assemble the final portfolio evidence.

---

## Lab Scope

All testing was performed in an authorized personal Azure lab. Completed work and planned improvements are identified separately.
