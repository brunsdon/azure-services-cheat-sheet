# Service Selection Guide

## What It Is

Decision tables for common Azure architecture choices. Start here when the question is "which service should I use?"

## I Need To Run Code

| Need | Choose | Notes |
| --- | --- | --- |
| Web API or web app | App Service | Simple operations, good .NET fit, deployment slots. |
| Event handler or scheduled job | Azure Functions | Great for Service Bus, Event Grid, timers, HTTP handlers. |
| Containerized API or worker | Container Apps | KEDA scaling, revisions, Dapr option, less overhead than AKS. |
| Kubernetes platform | AKS | Use when you need Kubernetes control and can operate it. |
| Legacy server workload | VM | Best for migration or hard OS/runtime constraints. |

## I Need To Store Files

| Need | Choose | Notes |
| --- | --- | --- |
| Documents or binary files | Blob Storage | Default for app files and integration payloads. |
| Analytics lake | Data Lake Storage | Use hierarchical namespace and analytics tooling. |
| Shared file system | Azure Files | SMB/NFS file share. |
| Large message payload | Blob Storage + Service Bus | Claim-check pattern. |

## I Need Async Messaging

| Need | Choose | Notes |
| --- | --- | --- |
| Business command queue | Service Bus Queue | Retries, DLQ, lock handling. |
| Multiple business subscribers | Service Bus Topic | Durable pub/sub with subscriptions and filters. |
| Simple queue | Storage Queue | Lower feature set, often lower complexity. |

## I Need Pub/Sub

| Need | Choose | Notes |
| --- | --- | --- |
| Durable business pub/sub | Service Bus Topic | Each subscriber gets independent delivery state. |
| Event notification | Event Grid | Push notifications and filtering. |
| Streaming telemetry | Event Hubs | Partitioned ingestion at scale. |

## I Need Workflow Automation

| Need | Choose | Notes |
| --- | --- | --- |
| Connector-heavy workflow | Logic Apps | Strong for SaaS and enterprise connectors. |
| Code-first orchestration | Durable Functions | Better for custom logic and testability. |
| Human approvals | Power Automate or Logic Apps | Choose based on audience and licensing. |
| Lightweight scheduled task | Azure Functions timer | Simple and code-centric. |

## I Need Secrets

| Need | Choose | Notes |
| --- | --- | --- |
| Secrets, certs, keys | Key Vault | Default secret store. |
| Non-secret app settings | App Configuration | Feature flags and config centralization. |
| Azure-to-Azure auth | Managed Identity | Prefer over secrets. |
| Partner OAuth app | Entra app registration | Use certificates where practical. |

## I Need Observability

| Need | Choose | Notes |
| --- | --- | --- |
| App traces and dependencies | Application Insights | APIs, Functions, workers. |
| Central log queries | Log Analytics | KQL across resources. |
| Platform metrics | Azure Monitor Metrics | Resource health and capacity. |
| Actionable notifications | Azure Monitor Alerts | Own every alert. |

## I Need Database Storage

| Need | Choose | Notes |
| --- | --- | --- |
| Relational data | Azure SQL Database | Strong default for enterprise apps. |
| Globally distributed NoSQL | Cosmos DB | Requires careful partition design. |
| PostgreSQL app | Azure Database for PostgreSQL | Managed PostgreSQL. |
| Cache | Azure Cache for Redis | Do not use as source of truth. |
| Simple key-value | Table Storage | Cheap and simple, limited querying. |

## Official Docs

- [Azure products by category](https://azure.microsoft.com/products/)
- [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)
