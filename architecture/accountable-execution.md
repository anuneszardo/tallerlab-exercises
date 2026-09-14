# Accountable Execution & Auditability Architecture

## Traceability Pipeline

Accountable Execution requires that any action, decision, or evidence item can be fully reconstructed six months (or years) after the event. The system writes immutable trace events to **Azure Log Analytics** and **ServiceNow Audit History**.

```text
[Case Trigger] ──> [Agent Action] ──> [Context Reference Hash] ──> [Human Review Checkpoint] ──> [Immutable Audit Log]
```

---

## Audit Event Schema (JSON Specification)

```json
{
  "eventId": "evt_98410294-a18d-4f32-841d-910293102931",
  "timestampUtc": "2026-10-14T14:22:10.451Z",
  "caseId": "CRD-2026-88102",
  "providerNpi": "1928374650",
  "agentIdentity": "mi-verification-assistant-prod@meridianhealth.azure",
  "action": "FETCH_PRIMARY_SOURCE_LICENSE",
  "contextReferences": [
    {
      "sourceType": "SHAREPOINT_POLICY_PDF",
      "documentUri": "https://meridian.sharepoint.com/sites/credentialing/Policies/2026_Medical_License_Verification_Rules.pdf",
      "contentHashSha256": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855"
    },
    {
      "sourceType": "EXTERNAL_STATE_API",
      "endpoint": "https://license.medicalboard.state.gov/api/v1/verify",
      "responseStatus": 200,
      "payloadHashSha256": "4a8271109a28c41d102938102938102938102938102938102938102938102938"
    }
  ],
  "agentOutputSummary": "License #MD99102 Active with zero disciplinary actions.",
  "humanReviewer": {
    "specialistId": "EMP-44019",
    "reviewerName": "Sarah Jenkins, CPCS",
    "reviewTimestampUtc": "2026-10-14T15:05:12.110Z",
    "determination": "APPROVED_PRIMARY_VERIFICATION"
  }
}
```

---

## Six-Month Reconstruction Audit Story

When compliance or external auditors review a provider credentialing decision six months after completion, they follow a 4-step query protocol:

1. **Step 1: ServiceNow Case Lookup**:
   - Query case ID `CRD-2026-88102` in ServiceNow to retrieve overall lifecycle timestamps, assigned specialist (`Sarah Jenkins`), and final approval state.
2. **Step 2: Azure Log Analytics Event Extraction**:
   - Query Azure Log Analytics using `caseId == 'CRD-2026-88102'` to pull the chronological list of all agent executions (`mi-intake-agent`, `mi-verification-assistant`, `mi-reference-outreach`).
3. **Step 3: Context & Evidence Validation**:
   - Inspect `contextReferences` to verify the exact version of the policy PDF and raw API responses used by the agent during verification.
4. **Step 4: Human Approval Verification**:
   - Confirm the specialist ID (`EMP-44019`) and digital signature timestamp proving a qualified human reviewed the evidence dossier before final system enablement.
