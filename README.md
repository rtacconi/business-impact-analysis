# Business Impact Analysis — \<company\>

This repository contains the Business Impact Analysis (BIA) documentation for **\<company\>**, covering critical business processes, recovery objectives, storage strategy, and capacity management.

## Index

- [README](README.md) — Introduction, index, and context (this document)

### 01 — Business impact analysis

- [Overview](01-business-impact-analysis/overview.md) — BIA scope, objectives, methodology
- [Critical processes](01-business-impact-analysis/critical-processes.md) — Identification and ranking of business processes
- [Impact assessment](01-business-impact-analysis/impact-assessment.md) — Financial, operational, and reputational impact
- [Recovery objectives](01-business-impact-analysis/recovery-objectives.md) — RTO, RPO, and continuity strategies

### 02 — Storage tiering and capacity visibility

- [Overview](02-storage-tiering-and-capacity-visibility/overview.md) — Storage strategy and tiering model
- [Tiering model](02-storage-tiering-and-capacity-visibility/tiering-model.md) — Hot / warm / cold / archive tiers
- [Capacity visibility](02-storage-tiering-and-capacity-visibility/capacity-visibility.md) — Visibility tooling and reporting

### 03 — Capacity management and observability

- [Overview](03-capacity-management-and-observability/overview.md) — Capacity management framework
- [Capacity metrics](03-capacity-management-and-observability/capacity-metrics.md) — Defining capacity metrics
- [Instrumentation](03-capacity-management-and-observability/instrumentation.md) — Instrumenting systems for visibility
- [Dashboards](03-capacity-management-and-observability/dashboards.md) — Dashboard design and examples
- [Thresholds](03-capacity-management-and-observability/thresholds.md) — Defining capacity thresholds and alerts
- [Scaling risk](03-capacity-management-and-observability/scaling-risk.md) — Identifying scaling risk indicators

## How to Use

Each section is self-contained Markdown with **Mermaid** diagrams for visual clarity. Render them in any Mermaid-compatible viewer (GitHub, GitLab, VS Code with the Mermaid extension, etc.).

## Context

\<company\> is a SaaS platform. This BIA assumes a cloud-native architecture with typical e-commerce / SaaS components: web front-end, API layer, relational and document databases, object storage, CDN, and background workers.
