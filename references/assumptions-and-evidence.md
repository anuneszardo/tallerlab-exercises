# Assumptions, Hypotheses & Source Evidence Log

## 1. Fixed Scenario Facts (Source of Truth)

These facts are explicitly established by `skill.md` and the Module 8 scenario material and are treated as immutable constraints:

- **Meridian Health Services**: Mid-sized healthcare administration company (~2,400 employees, founded in 1998, 27-year legacy systems, mainframe eligibility).
- **Executive Sponsor**: Diane Okafor, COO ($3.4M spent on AI over 2 years with zero operational movement; CEO reporting due in November).
- **VP of Engineering**: Focused on technical debt, fragile release pipelines, and 2-week release cadence.
- **Previous AI Initiatives**:
  - Internal Policy Chatbot: Deployed, ~4% weekly usage.
  - Developer Copilot: 140 licenses deployed, developers faster, release cadence unchanged at 2 weeks.
  - Claims Classification: 91% test accuracy, never reached production due to compliance block on misclassifications.
  - 3 Independent RAG Systems: Built independently by separate teams over policy docs.
- **Provider Credentialing Baseline**:
  - Total active working time: 770 minutes (~12.83 hours).
  - Average elapsed duration: 34 days (48,960 minutes).
  - P90 elapsed duration: 41 days.
  - Contractual commitment: 30 days (~40% miss rate).
  - Three longest waits: Reference outreach (11d), Committee decision (7d), Primary source verification (5d).
  - Verification team: 11 specialists, average tenure 9 years.
- **Technology Environment**: SharePoint (~400k files, no consistent taxonomy), Oracle, mainframe, ServiceNow, Azure tenancy, identity provider. No budget for new platform licenses. Echo and Chiron are NOT available.

---

## 2. Explicit Design Hypotheses & Proposed Targets

These elements are non-provided proposals formulated to solve the business outcome. They are explicitly labeled as testable hypotheses:

- **Target Metric Hypothesis**: Reducing P90 credentialing elapsed time from 41 days to $\le$ 28 days within 90 days.
- **Redesigned Flow Hypothesis**: Restructuring workflow into parallel streams will reduce average elapsed duration to 18–22 days.
- **First Shipment Slice**: Automated Reference Outreach & Tracking deployed by Day 14.
- **Quality Guardrail**: Maintaining credentialing rework/exception rates $\le$ 2.0%.
- **Technical Architecture Design**: Using Azure Entra ID Managed Identities, Azure Log Analytics, Azure SQL, and ServiceNow REST APIs.
