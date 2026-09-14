# Scoped Context Storage & Governance Model

## Context Boundary & System Mapping

Rather than attempting to build an unconstrained enterprise knowledge graph over 400,000 unindexed SharePoint documents, the shared context model is strictly bounded to **Provider Credentialing**.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AUTHORITATIVE DATA SOURCES                                │
│                                                                                        │
│  ┌──────────────────────┐    ┌──────────────────────┐    ┌──────────────────────────┐  │
│  │  SharePoint Online   │    │   Oracle Database    │    │  ServiceNow ITSM         │  │
│  │  - Policy PDFs       │    │   - Provider Master  │    │  - Active Case State     │  │
│  │  - Rules & Criteria  │    │   - Claims Data      │    │  - Task Assignments      │  │
│  └──────────┬───────────┘    └──────────┬───────────┘    └────────────┬─────────────┘  │
└─────────────┼───────────────────────────┼─────────────────────────────┼────────────────┘
              │                           │                             │
              ▼                           ▼                             ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        SCOPED CREDENTIALING CONTEXT MODEL                              │
│                        (Deployed in Meridian Azure SQL / Blob)                         │
│                                                                                        │
│  - Document Scopes: Credentialing policies, verification checklists, email templates   │
│  - System State: Active case evidence payloads, reference status, exception flags     │
│  - Governance: Managed versioning, human approval gates for policy updates            │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Data Schema & Storage Architecture

1. **Policy Rules Store (`Azure SQL: dbo.CredentialingPolicies`)**:
   - Stores versioned, parsed rule sets governing license checks, malpractice thresholds, and reference requirements.
   - Updates require sign-off from the Credentialing Governance Manager.

2. **Active Case Evidence Store (`Azure Blob: /cases/{caseId}/evidence/`)**:
   - Contains raw PDF downloads, state license API response JSONs, and reference response logs tied to a specific case.

3. **Temporary Working Context (`Azure SQL: dbo.CaseWorkingContext`)**:
   - Maintains real-time status across parallel streams (Primary Source, Reference Outreach, Contract Draft) for agent and human specialist access.

---

## Maintenance & Update Protocol

- **Policy Changes**: When Meridian updates a credentialing policy in SharePoint, an automated Azure Function triggers a parsing job, creates a new pending version in `dbo.CredentialingPolicies`, and alerts the Credentialing Manager for digital sign-off.
- **Agent Querying**: Agents query `dbo.CredentialingPolicies` using active version tags (`IsActive == 1`), ensuring models never rely on stale or unapproved policy drafts.
