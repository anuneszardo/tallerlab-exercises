# Exercise 5 — Frontier Engineer Self-Assessment

## Evaluation of 8 Readiness Evidence Items

| Evidence Item | Status | Supporting Repository Artifact | Honest Gap Analysis |
|---|---|---|---|
| **1. Explain Taller & Productivity Gap** | **Demonstrated** | `README.md` & `module-8/01-diagnosis.md` | Clear articulation of flow efficiency (1.57%) vs local task speed; no generic AI marketing jargon. |
| **2. Diagnose via 3 Conditions** | **Demonstrated** | `module-8/01-diagnosis.md` (Sections 1b & 1c) | Evaluated policy chatbot, Copilot, claims pilot, and RAG systems; ranked conditions with justification. |
| **3. Practical Implementation without Echo/Chiron** | **Demonstrated** | `module-8/02-engagement-design.md` (Section 2d) & `architecture/` | Solved using Meridian's native Azure Entra ID, Azure SQL/Log Analytics, ServiceNow, and SharePoint. |
| **4. Scope & Lead Bounded Work** | **Demonstrated** | `module-8/02-engagement-design.md` (Section 2b) | Rigorously bounded 90-day scope; defined 14-day production slice; explicit out-of-scope list. |
| **5. Client Communication & Risk Reporting** | **Demonstrated** | `module-8/03-client-communication.md` | Executive proposal for COO, VP dialogue, 118-word bad-news memo, and next-gain narrative. |
| **6. Disciplined Agent Usage & Controls** | **Demonstrated** | `module-8/02-engagement-design.md` & `architecture/agent-responsibilities.md` | Hybrid split keeps human final judgment on primary verification; least privilege Entra ID RBAC. |
| **7. Identify Business Process Opportunity** | **Demonstrated** | `module-8/01-diagnosis.md` & `module-8/02-engagement-design.md` | Identified wait-time bottleneck (98.4% waiting); parallelized reference outreach & async committee. |
| **8. Field Evidence Supporting Readiness** | **Partial (Simulation Only)** | `module-8/05-self-assessment.md` | **Critical Distinction**: Simulation evidence demonstrates analytical proficiency but does not replace real field client experience. |

---

## Detailed Gap Analysis & Reflective Questions

### 1. Which readiness item is weakest?
**Item 8 (Field Evidence Supporting Readiness)** is the weakest. While this repository provides rigorous analytical, architectural, and communicative artifacts for Module 8, simulated engagement artifacts cannot replicate the unscripted organizational dynamics, legacy code surprises, and stakeholder political pressures encountered in live client environments.

### 2. Which Exercise 4 Defense question was answered worst?
**Question 2 (Physician Committee Resistance)** was initially the most challenging. Managing physician committee members who do not report to the COO represents an organizational authority constraint rather than a software problem. The initial draft leaned slightly too far on technical convenience. The final answer was corrected to offer asynchronous review as an optional lower-friction path while explicitly defining a pivot to other controllable constraints (reference outreach) if committee leadership rejects the workflow change.

### 3. Was that due to a framework gap, understanding gap, or experience gap?
It was an **experience gap**. In production engineering, technical constraints are straightforward to solve with code and architecture. Organizational power structures—such as independent medical committees—require stakeholder negotiation experience that pure software design frameworks do not automatically solve.

### 4. What would you do differently if running the scenario again tomorrow?
1. **Engage Committee Leadership Earlier**: Meet with the Credentialing Committee Chair during Week 1 to co-design the asynchronous review UI rather than presenting it as a finished proposal.
2. **Accelerate Baseline Data Extraction**: Build an automated SQL script for ServiceNow on Day 1 to extract distribution curves for elapsed time across individual provider specialties rather than relying on aggregated 34-day averages.
3. **Formalize Specialist Rule Extraction**: Establish structured 1-on-1 interview templates for the 11 verification specialists in Sprint 1 to map tacit exception rules into codified policy tables faster.
