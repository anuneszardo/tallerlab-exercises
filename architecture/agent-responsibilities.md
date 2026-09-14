# Agent Catalog & Responsibility Matrix

## Agent Definitions & Authorization Boundaries

### 1. `Intake Agent`
- **Role**: Validates provider application submissions, performs OCR extraction, and initializes case records.
- **Allowed Actions**:
  - Read incoming application documents from secure ingestion drop.
  - Read scoped credentialing application policies.
  - Create new case records in ServiceNow.
  - Write normalized provider metadata to temporary staging Azure SQL.
- **Prohibited Actions**:
  - Cannot approve credentialing cases.
  - Cannot modify authoritative Oracle provider master records.
  - Cannot alter mainframe eligibility states.

### 2. `Verification Assistant Agent`
- **Role**: Retrieves primary source licensing evidence, checks malpractice registries, and prepares evidence summaries.
- **Allowed Actions**:
  - Read provider state license numbers and board credentials.
  - Query state licensing portals and NPDB registry API endpoints.
  - Write draft verification evidence dossiers into ServiceNow case records.
  - Flag license discrepancies for human specialist review.
- **Prohibited Actions**:
  - Cannot make final credentialing determinations.
  - Cannot sign off on verification checks without human review.
  - Cannot delete or overwrite historical verification logs.

### 3. `Reference Outreach Agent`
- **Role**: Automates reference contact notifications, tracks responses, and manages reminder schedules.
- **Allowed Actions**:
  - Read referee contact details from application record.
  - Send authenticated reference requests via email and SMS gateway.
  - Log referee responses into ServiceNow case attachments.
  - Update reference completion state in case workflow.
- **Prohibited Actions**:
  - Cannot approve provider credentialing status.
  - Cannot access provider claims history or billing databases.
  - Cannot communicate with referees using non-approved message templates.

---

## Human vs. Agent Responsibility Boundary

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              HUMAN SPECIALIST RESPONSIBILITY                           │
│  - Evaluate complex malpractice claims                                                 │
│  - Resolve license discrepancy flags                                                   │
│  - Conduct physician committee exception reviews                                       │
│  - Issue final legal credentialing approval                                            │
└───────────────────────────────────────────▲────────────────────────────────────────────┘
                                            │ Escalations & Sign-offs
────────────────────────────────────────────┼─────────────────────────────────────────────
                                            │ Draft Dossiers & Logs
┌───────────────────────────────────────────▼────────────────────────────────────────────┐
│                                   AGENT RESPONSIBILITY                                 │
│  - Application field extraction & OCR                                                  │
│  - Automated portal query & license evidence fetching                                  │
│  - Reference outreach, SMS reminder, & response tracking                               │
│  - Committee digital packet pre-population                                             │
└────────────────────────────────────────────────────────────────────────────────────────┘
```
