# Exercise 1 — Operational & Business Diagnosis

## 1a. Flow Analysis

### Quantitative Baseline Metrics

- **Total Working Time**: 770 minutes (~12.83 hours) across 9 discrete operational steps.
- **Average Elapsed Time**: 34 days (48,960 minutes).
- **P90 Elapsed Time**: 41 days.
- **Contractual SLA Commitment**: 30 days.
- **Contractual SLA Miss Rate**: ~40% of cases miss the 30-day deadline, triggering financial penalties and provider attrition to competing networks.

### Flow Efficiency Calculation

$$\text{Flow Efficiency} = \frac{\text{Active Working Time}}{\text{Total Elapsed Time}} = \frac{770 \text{ minutes}}{34 \times 24 \times 60 \text{ minutes}} = \frac{770}{48,960} \approx 1.57\%$$

$$\text{Waiting Share} = 100\% - 1.57\% = 98.43\%$$

### Three Longest Waits

1. **Reference Outreach and Follow-up**: 11 days of wait time (Working time: 2 hours).
2. **Committee Decision**: 7 days of wait time (Working time: 15 minutes; driven by bi-weekly meeting cadence).
3. **Primary Source Verification**: 5 days of wait time (Working time: 3.5 hours).

### True Systemic Constraint

The true constraint is not worker task velocity or individual processing efficiency; it is **systemic waiting time and handoff queueing**. Provider credentialing is structured as a sequential assembly line where work sits idle for 98.43% of its lifespan waiting for batch committee meetings, external provider response cycles, and manual queue transfers.

### Executive Summary for Diane Okafor (COO to CEO)

> "Our provider credentialing process requires only 13 hours of actual work, yet cases take an average of 34 days to complete because 98% of that time is spent waiting idle between steps. Attempting to make individual tasks faster will not solve our 40% SLA miss rate. We must eliminate queueing delays by parallelizing reference verification and replacing bi-weekly committee batching with continuous review."

---

## 1b. Post-Mortem of Previous AI Initiatives

| Initiative | Surface Result | Mechanistic Failure Diagnosis | Missing Condition |
|---|---|---|---|
| **Internal Policy Chatbot** | 4% weekly adoption | Deployed as a standalone destination outside operational tools. Users had to leave ServiceNow/Oracle, query the chatbot, manually verify results, and copy answers back. High context-switching friction yielded zero workflow impact. | Shared Context & Workflow Embedding |
| **Engineering Copilot** | 140 licenses; engineers report feeling faster; release cadence remains 2 weeks | Local code generation accelerated developer activity, but system throughput remained constrained by downstream QA, compliance review, and deployment pipelines. Accelerating a non-constraint does not increase system velocity. | Workflow Optimization (Technical debt in testing/release pipelines remains the bottleneck) |
| **Claims Classification Pilot** | 91% test accuracy; never reached production | Compliance blocked deployment because the system lacked auditable exception paths. When a claim was misclassified, the business could not identify why, who was accountable, or how to remediate the error safely. | Accountable Execution & Trust |
| **3 Retrieval (RAG) Systems** | Three teams independently built document search | Three separate engineering teams duplicated vector infrastructure to query overlapping policy PDFs in SharePoint without central context governance, creating fragmented truth and wasted effort. | Governed Shared Context |

*Note on Technical Debt*: The VP of Engineering's focus on technical debt is valid. When release pipelines, testing frameworks, and legacy databases are fragile, local developer speedups from Copilot accumulate as inventory before release bottlenecks.

---

## 1c. Assessment of the Three Conditions

### 1. Shared Context
- **Current State**: Fragmented across ~400,000 unindexed SharePoint files, Oracle database tables, mainframe legacy structures, and an ungoverned verification spreadsheet.
- **Reconstruction Cost**: Every verification specialist manually queries multiple systems, re-reading rules and copy-pasting data per case.
- **Required State**: A scoped credentialing context repository unifying policy rules, verification evidence standards, and active case state accessible to human workers and agents.

### 2. Identity
- **Current State**: Automation scripts run under generic shared service accounts or user credentials.
- **Reconstruction Cost**: Security and compliance cannot distinguish automated actions from human actions, destroying audit trails.
- **Required State**: Discrete Azure Entra ID Managed Identities per agent role (`Intake Agent`, `Verification Assistant`, `Reference Outreach Agent`) with strict RBAC scoping.

### 3. Accountable Execution
- **Current State**: Spreadsheets and informal emails track handoffs; no centralized event store links evidence to final determinations.
- **Reconstruction Cost**: Six months later, auditing why a provider was credentialed requires manual email thread reconstruction and folder searches.
- **Required State**: Immutable case audit logging linking agent action, exact context version, human reviewer identity, decision rationale, and downstream outcome.

### Severity Ranking & Justification

1. **Accountable Execution (Highest Severity)**: Without deterministic auditability, compliance will block any automation regardless of model accuracy (as demonstrated by the claims pilot).
2. **Shared Context (Medium Severity)**: Fragmented knowledge forces human workers to spend hours re-gathering context, creating long wait times between steps.
3. **Identity (Lower Relative Severity)**: Identity enforcement relies on existing Azure infrastructure; while critical for security, it is straightforward to configure once accountable execution paths are defined.

---

## 1d. The Uncomfortable Finding (Human & Operational Constraint)

### Tacit Knowledge Bottleneck

The verification team consists of 11 specialists with an average tenure of 9 years. They hold the operational rules, exception handling logic, and informal relationships in their heads. The business process exists as tribal knowledge rather than documented specifications.

### Strategic Communication Plan

#### 1. Communication to Diane Okafor (COO)
> "Diane, our primary risk is not model capability or technology integration—it is that 100% of Meridian's credentialing rules live in the heads of 11 people who have worked here for 9 years. If we treat them as workers to be automated, they will resist, and the project will fail. We must position them as the domain experts who codify the rules, oversee exception handling, and govern the agents."

#### 2. Communication to the Verification Team
> "You are the experts who know how credentialing actually works at Meridian. Our goal is to eliminate the tedious phone calls, email chasing, and double data entry that fill 90% of your day. Agents will handle initial data gathering and outreach tracking, freeing you to focus on complex cases, policy oversight, and final verification decisions."
