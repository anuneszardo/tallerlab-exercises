# Exercise 2 — Engagement & Workflow Redesign

## 2a. Primary Business Outcome & Quality Guardrails

### Primary Outcome Specification

- **Metric**: P90 Provider Credentialing Elapsed Time (application receipt to system enablement across all 4 operational platforms).
- **Baseline**: **41 days** (Average: 34 days; verified via ServiceNow historical case creation and completion timestamps over the preceding 12 months).
- **Target**: **$\le$ 28 days**.
- **Target Date**: 90 days from engagement kickoff.
- **Reproducible Measurement Method**: Weekly SQL query against ServiceNow case completion timestamps calculating the 90th percentile elapsed duration of all cases closed in the prior 7 days.
- **Falsification Criteria**: The engagement hypothesis is falsified if:
  1. P90 elapsed time remains $> 30$ days at Day 90; OR
  2. Credentialing rework rate increases by $> 0.5\%$ above baseline.

### Secondary Quality Guardrail

- **Quality Metric**: Credentialing Error/Rework Rate (applications returned post-enablement due to verification errors or compliance audit exceptions).
- **Baseline**: $1.8\%$.
- **Guardrail Ceiling**: $\le 2.0\%$ (Ensures turnaround acceleration is not achieved by compromising compliance diligence).

---

## 2b. Bounded Scope (First 90 Days)

### In Scope
1. **Provider Credentialing Workflow**: End-to-end redesign from intake to enablement.
2. **Reference Outreach Automation**: Automated follow-up and response tracking for provider references.
3. **Scoped Shared Context**: Normalization of credentialing policies, exception rules, and verification standards.
4. **Asynchronous Committee Review**: Digital pre-packaging and async approval workflows for routine credentialing cases.
5. **Traceable Audit Logging**: End-to-end execution logging in Azure Log Analytics and ServiceNow.

### Explicitly Out of Scope
- Organization-wide ingestion of all 400,000 SharePoint documents.
- Replacement or migration of ServiceNow or Oracle core databases.
- Mainframe eligibility system modernization.
- Enterprise-wide AI transformation or platform license purchases.
- Broad engineering debt remediation outside credentialing release pipelines.

### First Production Shipment (Day 14 Slice)
- **Shipment**: Automated Reference Outreach & Case Tracking Slice.
- **Functionality**: Triggers automated email/portal reference requests immediately upon intake validation, tracks responses, logs evidence into ServiceNow, and alerts verification specialists to missing items.

---

## 2c. Workflow Redesign: Driveshaft vs. Systemic Redesign

### 1. Driveshaft Version (Task-Level Automation)

The Driveshaft approach maintains the sequential linear pipeline and applies AI to accelerate individual tasks:

```text
[Intake] -> [Completeness] -> [Primary Verification] -> [Malpractice] -> [Reference Outreach] -> [Committee Prep] -> [Committee] -> [Contracting] -> [Enablement]
```

#### Mathematical Proof of Driveshaft Failure
- **Total Working Time**: 770 minutes.
- **Hypothetical AI Improvement**: 50% reduction in all active working time.
- **Time Saved**: $770 \text{ min} \times 50\% = 385 \text{ min} \approx 6.42 \text{ hours} \approx 0.27 \text{ days}$.
- **New Average Elapsed Time**: $34 \text{ days} - 0.27 \text{ days} = 33.73 \text{ days}$.

*Conclusion*: Accelerating active task time by 50% reduces overall turnaround by less than 7 hours because 98.43% of elapsed time is idle waiting. The Driveshaft approach fails the business outcome.

---

### 2. Redesign Version (Systemic Workflow Re-Architecture)

The Systemic Redesign restructures task execution, eliminates sequential blocking waits, and parallelizes independent streams:

```text
               ┌──> [Parallel Stream A: Primary Source Verification & Malpractice] ──┐
               │                                                                    │
[Application] ─┼──> [Parallel Stream B: Automated Reference Outreach & Follow-up]   ├──> [Async Committee / Exception Review] -> [Automated Enablement]
               │                                                                    │
               └──> [Parallel Stream C: Contract Pre-population & Draft] ───────────┘
```

#### Key Structural Changes
1. **Immediate Parallel Reference Outreach**: Triggered at intake validation rather than waiting for primary verification completion (eliminating 11 days of blocking wait).
2. **Parallel Verification Streams**: Primary source verification and malpractice reviews execute concurrently.
3. **Asynchronous Committee Review**: Standard low-risk credentialing applications are reviewed and approved asynchronously within 24 hours; bi-weekly committee meetings are reserved strictly for high-risk exception cases (reducing committee wait from 7 days to < 1 day).
4. **Unified Working Case Context**: Case evidence compounds into a single digital record, eliminating handoff re-reading time.

#### Projected Performance Outcome (Hypothesis)
- **Average Elapsed Time**: Reduced from 34 days to **18–22 days**.
- **P90 Elapsed Time**: Reduced from 41 days to **$\le$ 28 days**.

---

### 3. Human Cost & Change Management

The redesign fundamentally alters worker habits:
- Verification specialists shift from manual phone/email chasing to exception evaluation and rule governance.
- **Addressing Headcount Fear**: Executive leadership must explicitly communicate that efficiency gains will absorb projected provider volume growth rather than reduce headcount. Verification specialists' tacit knowledge is essential for overseeing agent exception boundaries.

---

## 2d. Three Conditions via Existing Meridian Technology

```text
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                       MERIDIAN EXISTING TECH STACK                                     │
│                                                                                                        │
│  ┌───────────────────────┐   ┌───────────────────────┐   ┌──────────────────┐   ┌───────────────────┐  │
│  │   SharePoint Online   │   │     Oracle Database   │   │  ServiceNow ITSM │   │  Azure Tenancy    │  │
│  │ (Source Policy PDFs)  │   │  (Provider Records)   │   │  (Case State)    │   │ (Entra ID & Logs) │  │
│  └───────────┬───────────┘   └───────────┬───────────┘   └────────┬─────────┘   └─────────┬─────────┘  │
└──────────────┼───────────────────────────┼────────────────────────┼───────────────────────┼────────────┘
               │                           │                        │                       │
               ▼                           ▼                        ▼                       ▼
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                       TALLER ENABLEMENT ARCHITECTURE                                   │
│                                                                                                        │
│  ┌──────────────────────────────────────────────────────────────────────────────────────────────────┐  │
│  │ 1. SCOPED SHARED CONTEXT                                                                         │  │
│  │ Azure SQL / Blob Storage indexing credentialing rules, templates, and active case evidence.       │  │
│  └────────────────────────────────────────────────┬─────────────────────────────────────────────────┘  │
│                                                   │                                                    │
│  ┌────────────────────────────────────────────────▼─────────────────────────────────────────────────┐  │
│  │ 2. DISCRETE AGENT IDENTITIES (Entra ID Managed Identities & Azure RBAC)                           │  │
│  │  - Intake Agent (Read Application / Write Case State)                                            │  │
│  │  - Verification Assistant Agent (Read Provider DB / Write Evidence Draft)                        │  │
│  │  - Reference Outreach Agent (Send Communication / Write Outreach Logs)                           │  │
│  └────────────────────────────────────────────────┬─────────────────────────────────────────────────┘  │
│                                                   │                                                    │
│  ┌────────────────────────────────────────────────▼─────────────────────────────────────────────────┐  │
│  │ 3. ACCOUNTABLE EXECUTION TRACE LOGGING                                                           │  │
│  │ Immutable trace schema in Azure Log Analytics linking Case ID, Agent ID, Source Doc, & Human Sign-off.│  │
│  └──────────────────────────────────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2e. Hybrid Responsibility Split Matrix

| Workflow Step | Responsibility | Justification | Context Handoff Details |
|---|---|---|---|
| **Application Intake & Logging** | **Agent** | Low consequence of error; deterministic data Extraction. | Structured JSON application payload sent to ServiceNow case. |
| **Completeness Check** | **Both** (Agent + Human) | Agent flags missing fields; Human evaluates ambiguous document submissions. | Agent generates missing item checklist; Human approves provider outreach. |
| **Primary Source Verification** | **Both** (Agent + Human) | **High regulatory risk**. Agent queries state boards and synthesizes findings; Human specialist performs final verification sign-off. | Agent packages evidence dossier with source URLs; Human signs off decision record. |
| **Malpractice History Review** | **Both** (Agent + Human) | Legal exposure requires human judgment on claim significance. | Agent extracts claims history summary; Human specialist rates risk profile. |
| **Reference Outreach & Follow-up** | **Agent** | High frequency, routine communication tracking. | Agent manages email triggers and responses; updates case status. |
| **Reference Exceptions** | **Human** | Unresponsive references or adverse feedback require human intervention. | Agent escalates flagged response to specialist queue. |
| **Committee Prep & Packaging** | **Agent** | Pure document synthesis and summary generation. | Agent compiles standardized digital committee packet. |
| **Routine Committee Decision** | **Human** (Async) | Consequential medical network access determination reserved for licensed human reviewers. | Digital sign-off queue in ServiceNow with 24-hour SLA. |
| **Exception Committee Review** | **Human** (Meeting) | Complex disputes or adverse findings require committee discussion. | Full case history and specialist notes surfaced in meeting agenda. |
| **Contract Generation** | **Both** (Agent + Human) | Agent populates standard contract template; Human verifies non-standard terms. | Draft PDF contract generated; Contracting specialist executes release. |
| **System Enablement (4 Systems)**| **Both** (Agent + Human) | High technical impact across legacy mainframe/Oracle systems. | Agent stages configuration updates; Operations verifies final sync. |
