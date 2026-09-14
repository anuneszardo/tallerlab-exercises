# Baseline Workflow Specification — Provider Credentialing

## Current State Pipeline

The current provider credentialing workflow is a sequential linear process managed across 11 verification specialists, an intake team, a credentialing analyst, a physician credentialing committee, contracting, and operations.

| Step # | Step Name | Owner / Role | Working Time | Queue Wait Before Step | Primary Tools / Systems Used |
|---|---|---|---:|---:|---|
| **1** | Application Received & Logged | Intake Team | 20 min | 0 days | Email, ServiceNow, Oracle |
| **2** | Completeness Check | Intake Team | 45 min | 1.5 days | ServiceNow, Manual PDF review |
| **3** | Primary Source Verification | Verification Team | 3.5 hours | 5.0 days | State licensing portals, Shared Spreadsheet |
| **4** | Malpractice History Review | Verification Team | 1.5 hours | 2.0 days | NPDB portal, Shared Spreadsheet |
| **5** | Reference Outreach & Follow-up | Verification Team | 2.0 hours | 11.0 days | Email, Phone, Shared Spreadsheet |
| **6** | Committee Review Prep | Credentialing Analyst | 2.0 hours | 3.0 days | Word, PDF, Email |
| **7** | Committee Decision | Credentialing Committee | 15 min | 7.0 days | Bi-weekly Meeting, Paper Packets |
| **8** | Contract Generation & Countersign | Contracting | 1.0 hour | 2.0 days | Word, Email, DocuSign |
| **9** | System Enablement across 4 Systems | Operations | 1.5 hours | 1.0 day | Mainframe, Oracle, Claims DB, Portal |

---

## Baseline Summary Metrics

- **Total Active Working Time**: 770 minutes (~12.83 hours).
- **Average Elapsed Duration**: 34.0 days (48,960 minutes).
- **90th Percentile (P90) Duration**: 41.0 days.
- **Contractual Commitment**: 30.0 days.
- **Baseline SLA Miss Rate**: ~40%.
- **Flow Efficiency**: $\frac{770}{48,960} \approx 1.57\%$.
- **Queue / Idle Share**: $98.43\%$.

---

## Key Operational Bottlenecks

1. **Step 5 (Reference Outreach - 11 Days Wait)**:
   - Serial blocking step. Verification specialists do not initiate reference calls until primary source verification and malpractice reviews are completed. Manual phone call tag and email follow-ups stall the case for over 1.5 weeks.

2. **Step 7 (Committee Decision - 7 Days Wait)**:
   - Artificial batch constraint. The Credentialing Committee meets bi-weekly. Applications finished on Day 1 of a cycle wait up to 14 days for the next scheduled meeting, regardless of how clean the verification evidence is.

3. **Step 3 (Primary Source Verification - 5 Days Wait)**:
   - Queue backlog. 11 verification specialists manually query state medical boards, state license registries, and board certification databases sequentially. Cases sit in shared queues waiting for assignment.
