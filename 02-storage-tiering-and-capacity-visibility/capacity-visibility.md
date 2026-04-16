# Capacity Visibility

## Objective

Provide real-time and trend-based visibility into storage consumption across all tiers so that engineering and finance teams can forecast growth, control costs, and prevent capacity-related outages.

## Visibility Architecture

```mermaid
flowchart TD
    subgraph Data Sources
        PG[(PostgreSQL)]
        REDIS[(Redis)]
        S3[(S3 Buckets)]
        ES[(Elasticsearch)]
    end

    subgraph Collection
        CW[CloudWatch Metrics]
        PROM[Prometheus Exporters]
        CUSTOM[Custom Capacity Agent]
    end

    subgraph Aggregation
        MIMIR[Mimir / Thanos Long-Term Store]
    end

    subgraph Visualisation
        GRAFANA[Grafana Dashboards]
        ALERTS[Alertmanager]
        REPORTS[Weekly Capacity Report]
    end

    PG --> PROM
    REDIS --> PROM
    ES --> PROM
    S3 --> CW
    CW --> MIMIR
    PROM --> MIMIR
    CUSTOM --> MIMIR
    MIMIR --> GRAFANA
    MIMIR --> ALERTS
    MIMIR --> REPORTS
```

## Key Visibility Metrics

| Metric | Source | Granularity | Retention |
|--------|--------|-------------|-----------|
| `pg_database_size_bytes` | postgres_exporter | Per database, 60 s | 13 months |
| `pg_table_size_bytes` | postgres_exporter | Per table, 5 min | 13 months |
| `redis_used_memory_bytes` | redis_exporter | Per instance, 30 s | 6 months |
| `s3_bucket_size_bytes` | CloudWatch | Per bucket, daily | 24 months |
| `s3_number_of_objects` | CloudWatch | Per bucket, daily | 24 months |
| `es_index_store_size_bytes` | elasticsearch_exporter | Per index, 60 s | 6 months |
| `disk_used_percent` | node_exporter | Per volume, 30 s | 6 months |

## Dashboard Layout

```mermaid
block-beta
    columns 3
    block:row1:3
        A["Total Storage by Tier (pie chart)"]
        B["Growth Trend — 30/60/90d (line chart)"]
        C["Cost by Tier (stacked bar)"]
    end
    block:row2:3
        D["PostgreSQL DB Size (table + sparkline)"]
        E["Top 10 Tables by Size (bar chart)"]
        F["Redis Memory Utilisation (gauge)"]
    end
    block:row3:3
        G["S3 Bucket Growth (line chart)"]
        H["Elasticsearch Index Size (heatmap)"]
        I["Disk Utilisation by Volume (gauge grid)"]
    end
```

## Alerting Rules

| Alert | Condition | Severity | Action |
|-------|-----------|----------|--------|
| DiskSpaceCritical | `disk_used_percent > 90` for 5 min | P1 | Page on-call, expand volume |
| DiskSpaceWarning | `disk_used_percent > 80` for 15 min | P2 | Ticket for capacity review |
| PostgreSQLGrowthAnomaly | Growth rate > 2× 30-day average | P2 | Investigate unexpected writes |
| RedisMemoryHigh | `redis_used_memory / redis_maxmemory > 0.85` | P1 | Review eviction policy, scale |
| S3BucketCostSpike | Daily cost > 1.5× 7-day average | P3 | Review lifecycle policies |

## Weekly Capacity Report

Automatically generated every Monday and posted to `#platform-capacity` Slack channel.

**Contents:**

1. Total storage footprint by tier (current vs. previous week).
2. Month-over-month growth rate per data store.
3. Projected capacity exhaustion date (if trending toward limits).
4. Top 5 fastest-growing tables / buckets / indices.
5. Cost delta vs. budget.

## Capacity Review Process

```mermaid
flowchart LR
    A[Weekly Report Generated] --> B{Growth within forecast?}
    B -->|Yes| C[No action — file report]
    B -->|No| D[Capacity Review Meeting]
    D --> E{Root cause identified?}
    E -->|Data bloat| F[Optimise schema / lifecycle]
    E -->|Organic growth| G[Plan scaling / budget adjustment]
    E -->|Incident| H[Incident follow-up]
    F --> I[Update forecast]
    G --> I
    H --> I
```
