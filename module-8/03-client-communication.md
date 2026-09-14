# Exercise 3 — Client Communication

## 3a. Executive Proposal for Diane Okafor (COO)

**MEMORANDUM**

**TO**: Diane Okafor, Chief Operating Officer  
**FROM**: Frontier Engineering Team, Taller  
**DATE**: September 11, 2026  
**SUBJECT**: Redesigning Provider Credentialing to Eliminate Operational Waiting Time  

---

### 1. What We Found

Meridian’s provider credentialing process takes an average of **34 days** (and **41 days** for 90% of cases), causing approximately **40% of applications to miss your 30-day contractual commitment**. Each missed deadline triggers financial penalties and drives healthcare providers to join competing networks.

However, our operational flow analysis revealed a surprising fact: **your team performs only 13 hours (770 minutes) of actual work per case**. 

This means credentialing cases spend **98.4% of their lifespan sitting idle in queues**, waiting for manual handoffs, reference responses, and bi-weekly committee meetings. The primary business problem is not employee task speed; it is structural waiting time between tasks.

---

### 2. Why Previous Initiatives Did Not Produce Results

Over the past 18 months, Meridian invested approximately $3.4M in AI initiatives that did not move operational metrics:

1. **Internal Policy Chatbot (4% adoption)**: Built as a standalone tool outside daily applications. Staff had to leave their workflow to ask questions, creating extra steps rather than saving time.
2. **Developer Copilot**: Accelerated code writing for engineers, but overall system releases remained capped at every two weeks due to testing and deployment bottlenecks.
3. **Claims Classification Pilot**: Achieved 91% accuracy in testing but stalled before launch because compliance could not audit or explain incorrect classifications.
4. **Three Independent Search Tools**: Separate teams built redundant document search engines, creating fragmented information without changing how work gets done.

Previous efforts focused on introducing technology in isolation. To move operational numbers, technology must be integrated directly into redesigned workflows with strict governance.

---

### 3. What We Propose

We propose restructuring the provider credentialing workflow around three core changes:

1. **Parallel Reference Verification**: Trigger automated reference outreach immediately upon application intake, running follow-ups concurrently with primary background checks rather than waiting 11 days to begin.
2. **Asynchronous Committee Approvals**: Transition routine, low-risk credentialing approvals to a continuous 24-hour digital sign-off queue, reserving bi-weekly committee meetings exclusively for complex exception cases.
3. **Unified Case Workspaces**: Provide verification specialists with a single, auto-populated case context containing all policy rules and retrieved evidence, eliminating manual data re-gathering.

---

### 4. What It Will Take

This engagement requires no new software licenses and no changes to your core mainframe or database infrastructure. We will utilize your existing Azure environment, ServiceNow system, and identity management tools.

The primary requirement is operational alignment:
- Codifying the implicit decision rules currently held by your 11 senior verification specialists.
- Shifting committee review procedures for routine cases to digital sign-offs.
- Dedicating a core engineering team for 90 days to build and govern the workflow.

---

### 5. How We Will Know It Worked

We measure success by business outcomes, not software usage:

- **Primary Metric**: Reduce 90th percentile (P90) credentialing completion time from **41 days to 28 days or less** within 90 days.
- **Early Milestone (Day 14)**: Deploy an automated reference tracking slice into production to prove workflow integration.
- **Quality Guardrail**: Maintain credentialing rework and compliance audit exception rates below **2.0%**.

If P90 turnaround time does not reach 28 days or if quality metrics degrade, the initiative will be evaluated against strict pre-defined falsification criteria.

---

### 6. What We Need From Diane Okafor

1. Approval to reallocate routine committee approvals to asynchronous digital sign-off.
2. Authorization for your 11 verification specialists to dedicate 4 hours per week to rule codification.
3. Executive sponsorship to align engineering and operations behind this single outcome metric for the next 90 days.

---

## 3b. Dialogue with the VP of Engineering

**Setting**: Conference room at Meridian Health Services headquarters.

**Participants**:
- **Frontier Engineer** (Taller)
- **VP of Engineering** (Meridian Health Services)

---

### Exchange 1

**Frontier Engineer**:  
"Thanks for taking the time to meet. We've reviewed the existing tech stack and your team's Copilot deployment. We agree with your assessment that technical debt in the release pipeline is a major bottleneck—it explains why developer code acceleration hasn't translated into faster release cadences. We aren't here to push new platforms or ask your team to rebuild core systems. Our goal is to address a specific operational constraint in provider credentialing by building on your existing Azure tenancy and ServiceNow infrastructure."

**VP of Engineering**:  
"Look, every consultant who comes through that door wants to add another layer of software for my team to maintain. We have 27 years of legacy code, a mainframe handling eligibility, and a release pipeline held together by shell scripts. Copilot made my engineers faster at writing code, but it didn't change our two-week release window because QA and compliance testing are manual. Now you want to introduce AI agents into credentialing. Who is going to fix the pipeline when these agents break?"

---

### Exchange 2

**Frontier Engineer**:  
"You're completely right about the risk of adding maintenance overhead. If we built custom agent frameworks that required your engineers to debug model prompts at 2 AM, that would be a failure. Instead, we are scoping the technical footprint tightly: we will use your existing Entra ID for agent permissions, Azure Log Analytics for trace logging, and standard REST APIs into ServiceNow. We will write automated regression tests for every agent integration step. Furthermore, by automating the verification documentation pipeline, we will actually reduce the volume of manual ad-hoc script requests your team receives from the verification group."

**VP of Engineering**:  
"That sounds fine in theory, but my engineers are already drowning. If you need them to write custom integrations for Oracle and the mainframe, or maintain detailed decision records for every automated step, they won't do it. Voluntary diligence never works here."

---

### Exchange 3

**Frontier Engineer**:  
"We agree—relying on voluntary developer diligence is a guaranteed point of failure. We will not ask your team to write manual decision records or build new database connectors. The decision logging will be generated automatically as part of the execution runtime into Azure storage whenever an agent or specialist performs an action. All schema definitions, Entra ID managed identity setups, and deployment scripts will be delivered, documented, and tested by us. Your team will retain administrative oversight through standard Azure RBAC without taking on operational maintenance debt."

**VP of Engineering**:  
"If you can deliver it inside our Azure tenant, use our existing Entra ID roles, and prove it doesn't increase our deployment queue, I won't block it. But I want to see the exact security blast radius before anything touches production."

**Frontier Engineer**:  
"Fair deal. We will deliver the full security and identity specification for your review before deploying the first slice."

---

## 3c. Week 6 Bad-News Memo (Under 200 Words)

**TO**: Diane Okafor, COO  
**FROM**: Frontier Engineering Team  
**DATE**: October 23, 2026  
**SUBJECT**: Credentialing Reference Outreach Response Rate Drop  

Diane,

During initial testing of automated reference outreach in Sprint 3, reference response rates dropped by 20% compared to manual calls. Recipients are filtering automated emails as spam or ignoring templated requests.

**Impact on Outcome**: If unaddressed, this delay will add 4 days to reference collection, threatening our 28-day P90 target.

**Immediate Action**: We are testing two adjustments this week:
1. Sending communications from verified provider-relations domain addresses with personalized practitioner context.
2. Implementing automated SMS follow-up prompts for non-responsive referees after 48 hours.

**Request**: We need approval from Provider Relations to issue outreach under their authenticated domain headers.

We will provide updated response rate data at our standing Thursday review.

---

## 3d. Communicating the Next Productivity Gain

> "Following the successful reduction of credentialing turnaround to 26 days, our operational data revealed that verification specialists still spend an average of 45 minutes per case manually cross-referencing state licensing board databases. By expanding our scoped verification assistant to automate license status extraction across top 5 regional state portals, we can eliminate an additional 35 minutes of manual verification work per case. This will free approximately 120 hours of specialist capacity per week, allowing the team to handle projected 15% provider network growth without adding headcount."
