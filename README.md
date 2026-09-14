# Taller Module 8 — Meridian Health Services Engagement Simulation

This repository contains the complete analysis, engagement design, client communications, technical architecture, and defense artifacts for **Taller Module 8: Full Engagement Simulation** at **Meridian Health Services**.

## Overview

Meridian Health Services is a mid-sized healthcare administration company (~2,400 employees, founded in 1998) processing claims and managing provider networks. After 18 months and ~$3.4M in internal AI initiatives yielded zero measurable operational improvements, COO Diane Okafor engaged Taller as the Frontier Engineering firm to deliver a verifiable business outcome.

Rather than implementing a generic chatbot or broad document ingestion, Taller diagnosed Meridian's core operational bottleneck in **Provider Credentialing**—a process plagued by a **1.57% flow efficiency** (770 minutes of active work spread over a 34-day average / 41-day P90 turnaround, where 98.43% of elapsed time is waiting between tasks).

## Key Analytical Insights

1. **Flow vs. Task Productivity**:
   - Total active working time across 9 steps: 770 minutes (~12.8 hours).
   - Average elapsed duration: 34 days (48,960 minutes).
   - Baseline Flow Efficiency: `770 / 48,960 ≈ 1.57%`.
   - **Driveshaft Failure**: A 50% reduction in working time saves only ~385 minutes (0.27 days), reducing average elapsed time to ~33.7 days without impacting the 40% contractual SLA miss rate.
   - **Systemic Redesign**: Parallelizing reference outreach, asynchronous committee reviews, and scoped shared context targets a reduction of average elapsed time to 20–24 days and P90 elapsed time to **<= 28 days within 90 days**.

2. **Diagnosis of Past AI Failures**:
   - **Policy Chatbot (4% usage)**: Isolated from natural worker context; solved no high-value workflow step.
   - **GitHub Copilot**: Improved local developer speed, but deployment/governance constraints kept release cadence at 2 weeks.
   - **Claims Classification (91% test accuracy)**: Stalled before production due to missing accountable execution and untraceable exception paths.
   - **3 Fragmented RAG Systems**: Rebuilt retrieval infrastructure independently without shared governance or workflow integration.

3. **Three Conditions with Existing Technology**:
   - **Shared Context**: Scoped credentialing knowledge model using SharePoint (source policy), Oracle (provider data), ServiceNow (case state), and Azure Blob/SQL (evidence & decision storage). No Echo required.
   - **Identity**: Entra ID Service Principals with RBAC least-privilege scoping per agent (`Intake Agent`, `Verification Assistant`, `Reference Outreach Agent`). No Chiron required.
   - **Accountable Execution**: Immutable trace logging across ServiceNow and Azure Log Analytics linking case IDs, agent identities, source documents, human review checkpoints, and final decisions.

## Repository Structure

```text
.
├── README.md
├── skill.md
├── module-8/
│   ├── 01-diagnosis.md            # Flow analysis, past initiative post-mortems, three conditions, tacit knowledge
│   ├── 02-engagement-design.md    # 90-day scope, outcome metrics, workflow redesign, tech architecture, hybrid split
│   ├── 03-client-communication.md # COO proposal memo, VP Engineering dialogue, week 6 bad-news memo, next gain
│   ├── 04-defense.md              # 10 direct defense Q&As for COO, VP Engineering, and Chief Security Officer
│   ├── 05-self-assessment.md      # 8 readiness evidence evaluations, gap analysis, future adjustments
│   └── 06-return-to-doubt.md      # Re-evaluating core thesis doubt & field evidence requirements
├── architecture/
│   ├── workflow-current.md        # Step-by-step baseline process & bottleneck breakdown
│   ├── workflow-redesign.md       # Target state parallelized workflow blueprint
│   ├── agent-responsibilities.md  # Agent catalog, permission boundaries, human judgment checkpoints
│   ├── identity-and-permissions.md# Entra ID & Azure RBAC technical security mapping
│   ├── accountable-execution.md   # Six-month auditability and trace logging architecture
│   └── context-model.md           # Scoped credentialing context storage & maintenance schema
└── references/
    └── assumptions-and-evidence.md# Explicit boundary of scenario facts vs. design assumptions
```

## Taller Core Principles Applied

1. **AI informs. People decide.** (Consequential credentialing determinations remain with human specialists).
2. **Understanding before intelligence.** (Redesigning the waiting workflow before applying model automation).
3. **Trust is designed.** (Separate managed identities, least privilege, explicit decision provenance).
4. **Build for outcomes, not adoption.** (Primary metric: P90 turnaround time <= 28 days, guarded by zero quality degradation).
5. **Human capability grows with AI.** (Transitioning verification specialists from manual data chasing to rules governance and exception handling).
