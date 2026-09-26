```markdown
# ICARUS-02 — Excessive RBAC Permission Detection

## Objective

Detect an Azure Role-Based Access Control (RBAC) change that grants the ICARUS test user permissions beyond the intended Reader baseline, investigate whether the role assignment succeeded, and verify removal of the excessive access.

This was an authorized test in a personal Azure lab.

---

## Lab Environment

| Component | Resource |
|---|---|
| Resource group | RG-PROJECT-ICARUS |
| Detection platform | Microsoft Sentinel |
| Log source | AzureActivity |
| Access-control model | Azure RBAC |
| Test identity | ICARUS Test User |
| Baseline role | Reader |
| Elevated test role | Contributor |
| Test date | September 25, 2026 |

---

## Key Concepts

- **Reader:** Allows the user to view Azure resources without modifying them.
- **Contributor:** Allows the user to manage Azure resources but does not allow management of Azure RBAC role assignments.
- **Role assignment write:** Records the creation or modification of an Azure RBAC role assignment.
- **Role assignment delete:** Records the removal of an Azure RBAC role assignment.
- **CorrelationId:** Connects related Azure Activity Log events from the same operation.
- **Baseline validation:** Confirms that an identity returned to its intended access level after remediation.

A successful role-assignment event establishes that an RBAC change occurred. It does not,
