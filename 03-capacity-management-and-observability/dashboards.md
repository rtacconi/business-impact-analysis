# Capacity Dashboards

## Dashboard Hierarchy

```mermaid
flowchart TD
    L0[Executive Summary] --> L1A[Compute Capacity]
    L0 --> L1B[Storage Capacity]
    L0 --> L1C[Network & Connectivity]
    L0 --> L1D[Application Capacity]

    L1A --> L2A[Per-Service Compute Detail]
    L1B --> L2B[Per-Database / Bucket Detail]
    L1C --> L2C[Per-Endpoint Connectivity]
    L1D --> L2D[Per-Service Application Detail]
```

| Level | Audience | Refresh Rate | Retention |
|-------|----------|--------------|-----------|
| L0 — Executive Summary | VP Eng, CTO, Finance | 5 min | 24 months |
| L1 — Domain Overviews | Team leads, SRE | 1 min | 13 months |
| L2 — Service Detail | Service owners, on-call | 30 s | 6 months |

## L0 — Executive Summary Dashboard

**Purpose:** Single-pane view of platform capacity health for leadership.

### Layout

```mermaid
block-beta
    columns 4
    block:header:4
        H1["Overall Capacity Health: 🟢 OK"]
    end
    block:kpis:4
        K1["Total Compute\nUtilisation: 62%"]
        K2["Total Storage\nUsed: 4.2 TB"]
        K3["Monthly Cloud\nCost: £28,400"]
        K4["Days to\nCapacity Limit: 94"]
    end
    block:charts:4
        C1["Cost Trend (12 months) — line chart"]
        C2["Utilisation by Layer — stacked bar"]
        C3["Growth Forecast — projection line"]
        C4["Active Alerts — count by severity"]
    end
```

### Key Panels

| Panel | Metric | Visualisation |
|-------|--------|---------------|
| Capacity Health | Worst-case utilisation across all layers | Stat (colour-coded) |
| Compute Utilisation | Avg CPU across all services | Gauge |
| Storage Used | Sum of all tier sizes | Stat + trend sparkline |
| Monthly Cost | AWS Cost Explorer data | Time series |
| Days to Limit | Linear projection to nearest hard limit | Stat |
| Active Alerts | Count of firing capacity alerts | Stat by severity |

## L1 — Compute Capacity Dashboard

### Layout

```mermaid
block-beta
    columns 3
    block:row1:3
        A["Cluster CPU Utilisation (time series)"]
        B["Cluster Memory Utilisation (time series)"]
        C["Node Count Over Time (bar)"]
    end
    block:row2:3
        D["Pod CPU Requests vs Limits vs Usage (multi-line)"]
        E["Pod Memory Working Set vs Limit (multi-line)"]
        F["CPU Throttle % by Service (heatmap)"]
    end
    block:row3:3
        G["HPA Replica Count (time series)"]
        H["Pending Pods (time series)"]
        I["OOM Kills (event list)"]
    end
```

## L1 — Storage Capacity Dashboard

### Layout

```mermaid
block-beta
    columns 3
    block:row1:3
        A["Storage by Tier (pie chart)"]
        B["Storage Growth 30/60/90d (line)"]
        C["Cost per Tier (stacked bar)"]
    end
    block:row2:3
        D["PostgreSQL Database Sizes (bar + trend)"]
        E["Top 10 Tables by Size (horizontal bar)"]
        F["Table Bloat Ratio (heatmap)"]
    end
    block:row3:3
        G["S3 Bucket Size & Object Count (table)"]
        H["Redis Memory vs Max (gauge per instance)"]
        I["Disk Utilisation by Volume (gauge grid)"]
    end
```

## L1 — Application Capacity Dashboard

### Layout

```mermaid
block-beta
    columns 3
    block:row1:3
        A["Request Rate by Service (time series)"]
        B["p99 Latency by Service (time series)"]
        C["Error Rate by Service (time series)"]
    end
    block:row2:3
        D["DB Connection Pool Usage (gauge per service)"]
        E["Queue Depth (time series per queue)"]
        F["Cache Hit Ratio (gauge per service)"]
    end
    block:row3:3
        G["Active WebSocket Connections (time series)"]
        H["Background Job Throughput (bar)"]
        I["Rate Limit Hits (time series)"]
    end
```

## Dashboard-as-Code

All dashboards are version-controlled as Grafana JSON models or Grafonnet (Jsonnet) and deployed via CI/CD.

```
dashboards/
├── L0-executive-summary.jsonnet
├── L1-compute.jsonnet
├── L1-storage.jsonnet
├── L1-network.jsonnet
├── L1-application.jsonnet
└── L2/
    ├── auth-service.jsonnet
    ├── payment-service.jsonnet
    └── ...
```

### Deployment Pipeline

```mermaid
flowchart LR
    A[Engineer edits .jsonnet] --> B[PR Review]
    B --> C[CI: jsonnet lint + render]
    C --> D[Merge to main]
    D --> E[CD: grizzly apply to Grafana]
    E --> F[Dashboard live in Grafana]
```

## Dashboard Standards

1. **Consistent time ranges** — Default to "Last 6 hours" with quick selectors for 1h, 24h, 7d, 30d.
2. **Annotations** — Overlay deployments, incidents, and scaling events.
3. **Variables** — Use Grafana template variables for cluster, namespace, service.
4. **Thresholds** — Colour panels green / amber / red aligned with alert thresholds.
5. **Links** — Every panel links to the relevant L2 drill-down dashboard.
6. **Documentation** — Each dashboard includes a description panel explaining purpose and ownership.
