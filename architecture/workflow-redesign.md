# Redesigned Workflow Blueprint — Provider Credentialing

## Target State Architectural Pipeline

The redesigned workflow restructures task execution into parallel streams, eliminates sequential blocking wait times, introduces continuous digital approvals, and establishes a single compounded case context.

```text
                               ┌──> Stream A: Primary Source & Malpractice Verification ──┐
                               │    (Agent Gathers / Specialist Reviews)                 │
[Application Received] ────────┼──> Stream B: Automated Reference Outreach & Follow-up   ├──> [Unified Case Pack] ──> [Async / Meeting Review] ──> [Automated Enablement]
(Intake Agent Validates)       │    (Agent Sends / Follows Up / Escalates)               │
                               └──> Stream C: Draft Contract Pre-population ──────────────┘
```

---

## Redesigned Step Matrix

| Step # | Stream | Step Name | Owner / Agent | Target Active Time | Target Wait Time | Structural Change |
|---|---|---|---|---:|---:|---|
| **1** | Main | Intake & Validation | `Intake Agent` | 5 min | 0 days | Automated OCR & field extraction; creates ServiceNow case. |
| **2A** | Stream A | Primary Source Verification | `Verification Assistant` + Human | 45 min | 1.0 day | Agent auto-queries portals; Specialist reviews summary dossier. |
| **2B** | Stream B | Reference Outreach & Tracking | `Reference Outreach Agent` | 10 min | 3.0 days | **Parallel Trigger**: Started immediately at Step 1; automated SMS/Email. |
| **2C** | Stream C | Contract Pre-population | System Auto | 5 min | 0 days | Draft contract generated concurrently using provider template. |
| **3** | Main | Unified Case Consolidation | System Auto | 2 min | 0 days | Evidence streams merge into single digital case record. |
| **4** | Main | Credentialing Approval | Human (Async / Committee) | 10 min | 1.0 day | **Async Queue**: Clean cases approved in <24h; meetings for exceptions. |
| **5** | Main | System Enablement | Operations + Agent | 20 min | 0.5 days | Agent stages updates across 4 systems; Ops confirms sync. |

---

## Performance Targets

- **Projected Active Working Time**: ~97 minutes (Down from 770 minutes).
- **Projected Average Elapsed Duration**: **18–22 days** (Down from 34 days).
- **Projected P90 Elapsed Duration**: **$\le$ 28 days** (Down from 41 days).
- **Projected SLA Miss Rate**: **$< 5\%$** (Down from 40%).
- **Target Flow Efficiency**: $\frac{97}{20 \times 24 \times 60} \approx 0.34\%$ active work with 18-22 days total throughput (98%+ waiting eliminated).

---

## Handoff & Governance Controls

1. **Parallel Execution Gate**: Step 2A, 2B, and 2C execute concurrently. The case cannot proceed to Step 4 until all streams report completion or explicit exception flags.
2. **Asynchronous Routing Logic**:
   - If `Primary Source = Clean` AND `Malpractice = Clean` AND `References = 2+ Validated` $\rightarrow$ Route to **Async Physician Review Queue** (24h SLA).
   - If any verification check returns a discrepancy $\rightarrow$ Route to **Bi-weekly Committee Exception Agenda**.
