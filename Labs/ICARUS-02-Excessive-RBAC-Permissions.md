# ICARUS-02: Excessive RBAC Permissions
## Outcome

I built and validated a Microsoft Sentinel detection workflow for Azure Role-Based Access Control (RBAC) changes involving excessive permissions.

In this controlled lab, an ICARUS test user with a Reader baseline was temporarily granted the Contributor role. Microsoft Sentinel detected the RBAC change, generated a Medium-severity incident, and provided the underlying Azure Activity Log evidence for investigation.

The investigation confirmed a successful `Microsoft.Authorization/roleAssignments/write` event in `RG-PROJECT-ICARUS`. The Contributor assignment was then removed, the user was verified as returned to Reader-only access, and the incident was resolved as authorized security testing.
