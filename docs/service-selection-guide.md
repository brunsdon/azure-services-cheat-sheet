# Service Selection Guide

## What It Is

Decision tables for common Azure architecture choices. Use this page when the question is "which
service should I use?"

## Quick Decision Rules

- Start simple, but leave room for production operations.
- Use Service Bus for business messages.
- Use Event Grid for notifications.
- Use Event Hubs for telemetry streams.
- Use Blob Storage for files and large payloads.
- Use Key Vault for secrets and App Configuration for non-secret settings.
- Use Managed Identity whenever the workload runs in Azure.

## What I Would Usually Choose

| Scenario                                       | Usual choice                                     |
| ---------------------------------------------- | ------------------------------------------------ |
| Business-critical asynchronous commands        | Service Bus                                      |
| Lightweight event notification                 | Event Grid                                       |
| High-volume telemetry stream                   | Event Hubs                                       |
| Simple HTTP integration workflow               | Logic Apps                                       |
| Custom code integration                        | Azure Functions or App Service                   |
| Public enterprise API boundary                 | API Management                                   |
| Relational business data                       | Azure SQL                                        |
| Globally distributed low-latency document data | Cosmos DB                                        |
| Secrets                                        | Key Vault                                        |
| App feature/config values                      | App Configuration                                |
| Large files or integration payloads            | Blob Storage                                     |
| DMZ-safe document upload                       | Blob Storage, Event Grid, Functions, Service Bus |

## Decision Scars

- Do not choose Cosmos DB just because it is cloud-native. Partitioning and RU cost need to be
  understood first.
- Do not choose Functions for long-running stateful workflows unless Durable Functions is the
  intended model.
- Do not choose Service Bus for high-volume telemetry streams. Use Event Hubs.
- Do not choose Event Grid when guaranteed ordered processing is required.
- Do not choose Logic Apps when complex custom code and testability are central.
- Do not choose AKS unless the team is ready to operate Kubernetes.

## I Need To Run Code

| Need                            | Choose          | Why                                                        |
| ------------------------------- | --------------- | ---------------------------------------------------------- |
| Web API or web app              | App Service     | Stable hosting, simple deployment, good .NET fit.          |
| Event handler or scheduled task | Azure Functions | Native triggers for HTTP, timers, Service Bus, Event Grid. |
| Containerized API or worker     | Container Apps  | Container-first, KEDA scaling, revisions, scale-to-zero.   |
| Kubernetes platform             | AKS             | Full Kubernetes control when the team can operate it.      |
| Legacy server workload          | VM              | Useful for migration or hard OS/runtime constraints.       |

## Functions Vs App Service Vs Container Apps

| Factor               | Azure Functions                | App Service                  | Container Apps                         |
| -------------------- | ------------------------------ | ---------------------------- | -------------------------------------- |
| Best default         | Event-driven handlers          | Web APIs and web apps        | Containerized services and workers     |
| Scaling              | Trigger and plan based         | App Service plan based       | KEDA and revision based                |
| Cold starts          | Possible on Consumption        | No typical cold start        | Possible with scale-to-zero            |
| Long-running work    | Better on Premium              | Good                         | Good                                   |
| Local dev            | Good                           | Excellent                    | Good                                   |
| Operational overhead | Low                            | Low                          | Medium                                 |
| Avoid when           | App is a large domain monolith | Work is mostly trigger-based | Team does not want container ownership |

## I Need Async Messaging

| Need                          | Choose            | Why                                          |
| ----------------------------- | ----------------- | -------------------------------------------- |
| Business command queue        | Service Bus Queue | Durable delivery, retries, DLQ, locks.       |
| Multiple business subscribers | Service Bus Topic | Independent subscriptions and filters.       |
| Event notification            | Event Grid        | Lightweight event routing.                   |
| Telemetry ingestion           | Event Hubs        | Partitioned high-throughput stream.          |
| Basic low-cost queue          | Storage Queue     | Simple queue with fewer enterprise features. |

## Service Bus Vs Event Grid Vs Event Hubs

| Need                        | Service Bus | Event Grid | Event Hubs                  |
| --------------------------- | ----------- | ---------- | --------------------------- |
| Business command            | Strong fit  | Poor fit   | Poor fit                    |
| Resource event notification | Sometimes   | Strong fit | Poor fit                    |
| High-volume telemetry       | Poor fit    | Poor fit   | Strong fit                  |
| Durable pub/sub             | Strong fit  | Medium     | Consumer-managed            |
| Replay stream               | Limited     | No         | Strong fit within retention |
| Ordered processing          | Sessions    | No         | Per partition               |

## I Need To Store Files

| Need                          | Choose                     | Why                                             |
| ----------------------------- | -------------------------- | ----------------------------------------------- |
| App files, documents, exports | Blob Storage               | Durable object storage and simple access model. |
| Analytics lake                | Data Lake Storage          | Hierarchical namespace and analytics tooling.   |
| Shared file system            | Azure Files                | SMB/NFS file share.                             |
| Large message payload         | Blob Storage + Service Bus | Claim-check pattern.                            |

## Blob Storage Vs Data Lake

| Factor         | Blob Storage                 | Data Lake Storage                      |
| -------------- | ---------------------------- | -------------------------------------- |
| Best for       | App files and object storage | Analytics and lakehouse zones          |
| Namespace      | Flat by default              | Hierarchical namespace                 |
| Access pattern | App read/write               | Analytics engines and batch processing |
| Complexity     | Lower                        | Higher                                 |
| Common users   | Apps and integrations        | Data engineering and analytics teams   |

## I Need Database Storage

| Need                       | Choose                        | Why                                                    |
| -------------------------- | ----------------------------- | ------------------------------------------------------ |
| Relational data            | Azure SQL Database            | Transactions, SQL, joins, reporting-friendly schema.   |
| Globally distributed NoSQL | Cosmos DB                     | Low latency, partitioned storage, global distribution. |
| PostgreSQL workload        | Azure Database for PostgreSQL | Managed PostgreSQL ecosystem.                          |
| Cache                      | Azure Cache for Redis         | Low-latency cache and transient state.                 |
| Simple key-value           | Table Storage                 | Cheap and simple for limited query patterns.           |

## Azure SQL Vs Cosmos DB

| Factor       | Azure SQL                       | Cosmos DB                                      |
| ------------ | ------------------------------- | ---------------------------------------------- |
| Data model   | Relational                      | Document, key-value, graph, column-family APIs |
| Transactions | Strong relational transactions  | Best within logical partition                  |
| Query style  | SQL joins and reporting         | Partition-aware queries                        |
| Scaling      | Vertical and read replicas      | Horizontal partitioning                        |
| Best for     | Enterprise apps and reporting   | Global low-latency NoSQL workloads             |
| Avoid when   | Schema is massively distributed | Partition key is unclear                       |

## I Need Workflow Automation

| Need                       | Choose                       | Why                                        |
| -------------------------- | ---------------------------- | ------------------------------------------ |
| Connector-heavy workflow   | Logic Apps                   | Strong managed connectors and run history. |
| Code-first orchestration   | Durable Functions            | Testable orchestration in code.            |
| Human approvals            | Power Automate or Logic Apps | Depends on audience and licensing.         |
| Lightweight scheduled task | Azure Functions timer        | Simple and code-centric.                   |

## Logic Apps Vs Durable Functions

| Factor         | Logic Apps                          | Durable Functions        |
| -------------- | ----------------------------------- | ------------------------ |
| Best for       | Connectors and workflow visibility  | Code-first orchestration |
| Complex logic  | Can become hard to maintain         | Better fit               |
| Source control | Workflow JSON can be noisy          | Normal code review       |
| Support view   | Excellent run history               | Requires good telemetry  |
| Team fit       | Integration analysts and developers | Developers               |

## I Need Secrets Or Configuration

| Need                        | Choose            | Why                                    |
| --------------------------- | ----------------- | -------------------------------------- |
| Secrets, keys, certificates | Key Vault         | Purpose-built secure store.            |
| Feature flags               | App Configuration | Runtime config and feature management. |
| Azure-to-Azure auth         | Managed Identity  | Avoids stored credentials.             |
| External OAuth client       | App Registration  | Standards-based identity.              |

## Key Vault Vs App Configuration

| Factor               | Key Vault               | App Configuration                |
| -------------------- | ----------------------- | -------------------------------- |
| Stores secrets       | Yes                     | No                               |
| Stores feature flags | No                      | Yes                              |
| Certificate support  | Yes                     | No                               |
| App settings         | Possible but not ideal  | Strong fit                       |
| Access pattern       | Secure secret retrieval | Centralized configuration        |
| Common pairing       | Referenced by apps      | References Key Vault for secrets |

## I Need Observability

| Need                        | Choose                | Why                                    |
| --------------------------- | --------------------- | -------------------------------------- |
| App traces and dependencies | Application Insights  | Best fit for APIs, Functions, workers. |
| Central log queries         | Log Analytics         | KQL across resources.                  |
| Platform metrics            | Azure Monitor Metrics | Resource health and capacity.          |
| Actionable notifications    | Azure Monitor Alerts  | Owned alerts and response.             |
| Dashboards                  | Workbooks             | Operational views.                     |

## I Need Secure Networking

| Need                        | Choose              | Why                                    |
| --------------------------- | ------------------- | -------------------------------------- |
| Private PaaS access         | Private Endpoint    | Keeps traffic on private IP space.     |
| Global HTTP ingress         | Front Door          | Edge routing, acceleration, WAF.       |
| Regional HTTP ingress       | Application Gateway | Layer 7 regional gateway and WAF.      |
| Predictable outbound IP     | NAT Gateway         | Stable egress for allowlists.          |
| Hybrid private connectivity | VPN or ExpressRoute | Connect Azure to on-premises networks. |

## Official Docs

- [Azure product documentation](https://learn.microsoft.com/azure/)
- [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)
