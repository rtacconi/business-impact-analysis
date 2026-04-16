# Business Impact Analysis — Overview

## Purpose

This Business Impact Analysis (BIA) identifies and evaluates the potential effects of disruptions to \<company\>'s critical business functions. It provides the foundation for continuity planning, disaster recovery investment, and risk-informed decision-making.

## Scope

| In Scope | Out of Scope |
|---|---|
| Customer-facing web and mobile applications | Physical office continuity |
| API platform and integrations | HR / people-related risks |
| Data stores (relational, document, cache) | Legal / regulatory compliance audit |
| Payment processing pipeline | Third-party vendor deep-dive audits |
| Background job infrastructure | |
| CDN and static asset delivery | |

## Methodology

```mermaid
flowchart LR
    A[Identify Business Processes] --> B[Map Dependencies]
    B --> C[Assess Impact]
    C --> D[Define Recovery Objectives]
    D --> E[Prioritise & Recommend]
    E --> F[Review Cycle]
    F -->|Quarterly| A
```

1. **Identify** — Enumerate all business-critical processes and their owners.
2. **Map** — Document upstream and downstream dependencies (services, data, people).
3. **Assess** — Quantify financial, operational, reputational, and regulatory impact of downtime at 1 h, 4 h, 24 h, and 72 h intervals.
4. **Define** — Set Recovery Time Objective (RTO) and Recovery Point Objective (RPO) per process.
5. **Prioritise** — Rank processes by impact severity and assign continuity investment.
6. **Review** — Re-run quarterly or after significant architectural changes.

## Stakeholders

| Role | Responsibility |
|---|---|
| CTO / VP Engineering | Sponsor, final sign-off |
| SRE / Platform Team | Technical dependency mapping, recovery testing |
| Product Management | Business process identification and prioritisation |
| Finance | Impact quantification |
| Security | Threat and risk overlay |

## Key Definitions

| Term | Definition |
|---|---|
| **RTO** | Maximum acceptable time to restore a process after disruption |
| **RPO** | Maximum acceptable data loss measured in time |
| **MTTR** | Mean Time to Recover — average observed recovery duration |
| **MTBF** | Mean Time Between Failures — average uptime between incidents |
| **BCP** | Business Continuity Plan — actionable procedures to maintain operations |
