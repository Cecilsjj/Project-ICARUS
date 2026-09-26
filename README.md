# PROJECT ICARUS — Azure Cloud Security Lab

A hands-on Azure security lab focused on detecting risky configuration and access-control changes, investigating Microsoft Sentinel alerts, validating remediation, and documenting incident response.

**Azure Activity Logs → Microsoft Sentinel Detection → Investigation → Remediation Verification → Incident Closure**

---

## Project Goals

- Establish and validate secure Azure lab baselines.
- Collect and investigate Azure Activity Logs.
- Build and validate Microsoft Sentinel detection rules.
- Investigate risky storage-network configuration changes.
- Investigate excessive Azure RBAC permissions.
- Correlate security events with their underlying Azure operations.
- Verify remediation and restoration of expected security controls.
- Document findings, response actions, and final incident disposition.

---

## Lab Environment

| Component | Purpose |
|---|---|
| Microsoft Azure | Cloud lab environment |
| RG-PROJECT-ICARUS | Resource group containing lab resources |
| sticaruslabjoey | Storage account used for controlled configuration testing |
| law-project-icarus | Log Analytics workspace |
| Microsoft Sentinel | Detection, alerting, and incident investigation |
| Azure Activity Logs | Control-plane evidence of configuration and access changes |
| Azure RBAC | Identity and permission-management testing |

---

## Skills Demonstrated

- Microsoft Azure security
- Microsoft Sentinel
- Azure Activity Log investigation
- Azure Role-Based Access Control (RBAC)
- Kusto Query Language (KQL)
- JSON request-body parsing
- Scheduled analytics rule configuration
- Cloud-security detection engineering
- Alert triage
- Incident investigation
- Event correlation
- Correlation ID analysis
- Privilege-change investigation
- Remediation validation
- Access-baseline verification
- Incident classification
- Security documentation

---

## Project Status

| Scenario | Focus | Status |
|---|---|---|
| ICARUS-01 | Detect Azure Storage all-networks access | Completed |
| ICARUS-02 | Detect and investigate excessive Azure RBAC permissions | Completed |

**Project ICARUS technical lab: Complete**

The remaining work is portfolio maintenance and future detection tuning rather than completion of the core lab scenarios.

---

## ICARUS-01 — Storage Account All-Networks Access

[Read the full investigation and response case study](Labs/ICARUS-01-Storage-Network-Exposure.md)

### Objective

Detect an Azure Storage update that allows access from all networks, investigate whether the change succeeded, and verify restoration of selected-network access.

### Completed Workflow

1. Established the storage account's expected network-access baseline.
2. Created a scheduled Microsoft Sentinel analytics rule.
3. Performed an authorized all-networks configuration change.
4. Immediately restored selected-network access.
5. Queried Azure Activity Logs to investigate both operations.
6. Confirmed the rule generated an alert and incident.
7. Traced the alert to the underlying Azure Activity Log request.
8. Matched the request to successful completion using its correlation ID.
9. Verified restoration of selected-network access.
10. Closed the incident as **Benign Positive — Suspicious but expected**.

### Investigation Outcome

The Sentinel rule successfully detected the intended storage-network configuration change.

The investigation confirmed that the all-networks update completed successfully and that the subsequent restoration also completed successfully.

The available evidence establishes a network configuration change. It does not establish anonymous blob access, data access, or data exfiltration.

### Improvement Identified

Repeated alerts were observed for the same event time, consistent with overlapping query windows.

Future tuning should evaluate deduplication while preserving detection coverage for delayed events.

---

## ICARUS-02 — Excessive RBAC Permissions

[Read the full investigation and response case study](Labs/ICARUS-02-Excessive-RBAC-Permissions.md)

### Objective

Detect an Azure RBAC change that grants the ICARUS Test User permissions beyond the intended Reader baseline, investigate whether the role assignment succeeded, and verify removal of the excessive access.

### Completed Workflow

1. Confirmed the ICARUS Test User had Reader access.
2. Temporarily assigned the Contributor role.
3. Verified that Reader and Contributor were both present in Access Control (IAM).
4. Generated Azure Activity Log telemetry for the role-assignment change.
5. Confirmed Microsoft Sentinel generated a Medium-severity incident.
6. Investigated the underlying `Microsoft.Authorization/roleAssignments/write` event.
7. Reviewed the caller, resource group, operation, and correlation ID.
8. Removed the temporary Contributor role.
9. Verified the role-assignment deletion in Azure Activity Logs.
10. Confirmed the ICARUS Test User returned to Reader-only access.
11. Resolved the incident as a **Benign Positive — Security Testing**.

### Investigation Outcome

Microsoft Sentinel successfully detected the RBAC role-assignment activity.

The underlying Azure Activity Log event confirmed a successful role-assignment write in `RG-PROJECT-ICARUS`.

The temporary Contributor assignment was removed, and the test user was verified as returned to the intended Reader-only baseline.

The observed evidence confirms that excessive permissions were temporarily assigned and later removed. It does not establish that the elevated permissions were used to modify Azure resources.

### Improvement Identified

The activity event used during the investigation did not directly expose the human-readable role name.

Future improvements should evaluate:

- Entity mapping
- Custom alert details
- Direct role-definition enrichment
- Higher-privilege role filtering
- Duplicate-alert tuning

---

## Investigation Flow

```text
Controlled Azure Security Change
            |
            v
Azure Activity Log Telemetry
            |
            v
Microsoft Sentinel Detection
            |
            v
Alert / Incident Generation
            |
            v
Underlying Event Investigation
            |
            v
Remediation
            |
            v
Baseline Validation
            |
            v
Incident Classification and Closure
```

---

## Key Security Lessons

- A security alert is the beginning of an investigation, not the conclusion.
- A request event does not always prove that an operation completed successfully.
- Correlation IDs can connect related Azure control-plane events.
- Azure Activity Logs provide valuable evidence for configuration and access-control changes.
- RBAC changes should be reviewed in context because legitimate administrative activity can resemble suspicious privilege escalation.
- A correct security detection can represent authorized activity.
- Remediation should be followed by validation that the expected baseline was actually restored.
- Historical logs remain valuable even after a risky configuration or permission has been removed.
- Detection logic should reflect the limits of the available telemetry.
- Security findings should not extend beyond what the evidence supports.

---

## Detection Engineering Lessons

Project ICARUS reinforced several practical detection-engineering principles:

- Detection logic should target meaningful security events rather than simply collecting logs.
- Request and completion events may need to be investigated separately.
- Alert context should expose enough information for an analyst to understand what changed.
- Overlapping query windows can create repeated detections and require tuning.
- Entity mapping and custom alert details can improve investigation efficiency.
- Detection rules should be validated through controlled test activity before broader deployment.

---

## Evidence Strategy

The repository intentionally uses a limited set of screenshots that advance the investigation story.

Evidence is focused on:

- the expected baseline
- the risky configuration or permission change
- Sentinel detection
- investigation findings
- remediation
- final validation
- incident closure

Intermediate setup screens and repetitive configuration steps are intentionally excluded from the main case studies unless they materially support the investigation.

---

## Interview Summary

Built and validated an Azure cloud-security lab using Microsoft Sentinel and Azure Activity Logs.

Created detections for risky storage-network exposure and excessive Azure RBAC permissions, investigated the underlying Azure control-plane events with KQL, correlated activity using operation details and correlation IDs, verified remediation, restored expected security baselines, and documented incident closure using evidence-based classifications.

The project demonstrates practical experience with Microsoft Sentinel, Azure Activity Logs, KQL, RBAC investigation, cloud detection engineering, alert triage, remediation validation, and SOC-style incident response.

---

## Project Status

```text
PROJECT ICARUS: COMPLETE

ICARUS-01 — Storage Network Exposure Detection: Complete
ICARUS-02 — Excessive RBAC Permission Detection: Complete
```

Future work will focus on detection tuning and enrichment rather than completion of the core project.
