# Identity & Permissions Architecture (Azure Entra ID & RBAC)

## Identity Enforcement Model

To satisfy security and compliance requirements without shared service accounts, every automated agent operates under a discrete **Microsoft Entra ID Managed Identity** (or dedicated Service Principal) scoped with least-privilege Azure RBAC roles.

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                MICROSOFT ENTRA ID TENANT                               │
│                                                                                        │
│   ┌────────────────────────┐  ┌────────────────────────┐  ┌────────────────────────┐   │
│   │  mi-intake-agent       │  │ mi-verification-agent  │  │ mi-outreach-agent      │   │
│   │  (Managed Identity)    │  │ (Managed Identity)     │  │ (Managed Identity)     │   │
│   └───────────┬────────────┘  └───────────┬────────────┘  └───────────┬────────────┘   │
└───────────────┼───────────────────────────┼───────────────────────────┼────────────────┘
                │                           │                           │
                ▼                           ▼                           ▼
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AZURE RESOURCE PERMISSION SCOPING                         │
│                                                                                        │
│   - ServiceNow Case REST API  - State Portal Adapter        - SendGrid / Twilio API    │
│   - Staging SQL (Write Case)  - Azure SQL (Write Evidence)  - Azure SQL (Outreach Log) │
│   - Read Application Drop     - Read Provider Metadata      - Read Referee Contacts    │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Agent Identity & Permission Matrix

| Agent Identity | Entra ID Identity Name | Scoped Azure Role | Resource Access Scope | Write Permission Ceiling |
|---|---|---|---|---|
| **Intake Agent** | `mi-intake-agent-prod` | `CredentialingIntakeWriter` | Staging Blob `app-intake/`<br>ServiceNow `sn_customerservice_case` | Create Case, Write Initial Json Metadata |
| **Verification Assistant** | `mi-verification-assistant-prod` | `VerificationEvidenceWriter` | State Portal API Adapter<br>Azure SQL `dbo.VerificationEvidence` | Write Draft Evidence Dossier |
| **Reference Outreach Agent** | `mi-reference-outreach-prod` | `ReferenceOutreachExecutor` | Communication Gateway (SendGrid/Twilio)<br>Azure SQL `dbo.ReferenceLog` | Write Outreach Event Log, Update Reference Status |

---

## Security Blast Radius Analysis

1. **Compromise of `Reference Outreach Agent`**:
   - **Blast Radius**: Limited strictly to reading reference contact info and triggering email/SMS templates.
   - **Isolations**: Cannot read provider malpractice data, cannot access state licensing credentials, cannot modify ServiceNow approval states, and cannot write to Oracle master tables.

2. **Compromise of `Verification Assistant Agent`**:
   - **Blast Radius**: Limited to querying external state licensing APIs and writing draft evidence records into `dbo.VerificationEvidence`.
   - **Isolations**: Cannot execute final credentialing approvals, cannot alter provider payout rates, and cannot modify active network membership flags.

3. **No Shared Service Accounts Rule**:
   - All legacy service accounts (`svc-credentialing-auto`) are revoked and replaced with individual Managed Identities tied directly to Azure Log Analytics trace streams.
