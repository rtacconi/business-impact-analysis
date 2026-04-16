# Capacity Management & Observability — Overview

## Purpose

Establish a systematic approach to capacity management for \<company\>, ensuring the platform can meet current demand, forecast future needs, and surface scaling risks before they become incidents.

## Framework

```mermaid
flowchart TD
    A[Define Capacity Metrics] --> B[Instrument Systems]
    B --> C[Build Dashboards]
    C --> D[Set Thresholds & Alerts]
    D --> E[Identify Scaling Risks]
    E --> F[Plan & Execute Scaling]
    F --> G[Review & Iterate]
    G --> A
```

## Pillars

| Pillar | Document | What It Covers |
|--------|----------|----------------|
| 1. Capacity Metrics | [capacity-metrics.md](capacity-metrics.md) | Which metrics to track across compute, storage, network, and application layers |
| 2. Instrumentation | [instrumentation.md](instrumentation.md) | How to collect metrics: exporters, agents, custom telemetry |
| 3. Dashboards | [dashboards.md](dashboards.md) | Dashboard design, layout, and examples |
| 4. Thresholds | [thresholds.md](thresholds.md) | Setting static and dynamic thresholds, alert routing |
| 5. Scaling Risk Indicators | [scaling-risk.md](scaling-risk.md) | Early-warning signals and risk scoring |

## Observability Stack

```mermaid
flowchart LR
    subgraph Applications
        APP[App Services]
        WORKERS[Workers]
        DB[(Databases)]
    end

    subgraph Collection Layer
        OC[OpenTelemetry Collector]
        PE[Prometheus Exporters]
        CW[CloudWatch Agent]
    end

    subgraph Storage Layer
        PROM[Prometheus / Mimir]
        LOKI[Loki - Logs]
        TEMPO[Tempo - Traces]
    end

    subgraph Presentation Layer
        GRAF[Grafana]
        AM[Alertmanager]
        PD[PagerDuty]
        SLACK[Slack]
    end

    APP --> OC
    WORKERS --> OC
    DB --> PE
    APP --> CW

    OC --> PROM
    OC --> LOKI
    OC --> TEMPO
    PE --> PROM
    CW --> PROM

    PROM --> GRAF
    LOKI --> GRAF
    TEMPO --> GRAF
    PROM --> AM
    AM --> PD
    AM --> SLACK
```

## Roles & Responsibilities

| Role | Responsibility |
|------|----------------|
| SRE / Platform Team | Own observability stack, define SLOs, operate alerts |
| Service Owners | Instrument their services, define service-level capacity metrics |
| Engineering Managers | Capacity review participation, scaling budget requests |
| Finance / FP&A | Cloud cost forecasting, budget approval |
