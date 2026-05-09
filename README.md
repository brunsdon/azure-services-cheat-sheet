# Azure Services Cheat Sheet

[![Azure](https://img.shields.io/badge/Azure-Services-0078D4?logo=microsoftazure&logoColor=white)](https://learn.microsoft.com/azure/)
[![.NET](https://img.shields.io/badge/.NET-Integration-512BD4?logo=dotnet&logoColor=white)](https://learn.microsoft.com/dotnet/)
[![DevOps](https://img.shields.io/badge/DevOps-CI%2FCD-0A66C2?logo=azuredevops&logoColor=white)](docs/devops.md)
[![Architecture](https://img.shields.io/badge/Architecture-Patterns-2E7D32)](docs/architecture-patterns.md)
[![Markdown Check](https://github.com/brunsdon/azure-services-cheat-sheet/actions/workflows/markdown-check.yml/badge.svg)](https://github.com/brunsdon/azure-services-cheat-sheet/actions/workflows/markdown-check.yml)
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](CONTRIBUTING.md)

A practical Azure architecture field guide for choosing, using, and
integrating Azure services in real delivery work.

This repository is written for developers, integration specialists, solution
architects, technical leads, and Dynamics 365 / Power Platform professionals
who need clear service selection guidance without marketing noise.

Built from real-world delivery experience across Australian Federal Government,
enterprise integration projects, Dynamics 365 platforms, Dataverse
integrations, and Azure-native architectures.

## Why This Repo Exists

Azure has many overlapping services. The hard part is rarely finding a service
that can work. The hard part is choosing the service that fits the workload,
team, cost model, security posture, and operational reality.

This cheat sheet focuses on production-minded decisions:

- When to use a service.
- When not to use it.
- What usually goes wrong.
- How to secure and monitor it.
- How it fits into integration-heavy enterprise systems.
- What I would usually choose in 2026 for common architecture problems.

## Who This Is For

| Audience | What you get |
| --- | --- |
| Azure developers | Practical defaults for APIs, Functions, queues, storage, identity, and CI/CD. |
| Solution architects | Decision tables, trade-offs, diagrams, and architecture patterns. |
| Technical leads | Delivery checklists, production gotchas, and operational review points. |
| Dynamics 365 / Power Platform developers | Dataverse integration guidance using Service Bus, Functions, Logic Apps, and API Management. |
| Recruiters and managers | A quick view of hands-on Azure integration, architecture, and delivery judgement. |

## For Recruiters, Managers And Technical Leads

This repo demonstrates:

- Azure service selection judgement.
- Practical integration architecture.
- Secure cloud design awareness.
- Event-driven architecture understanding.
- DevOps and production-readiness thinking.
- Ability to explain complex services clearly.

It positions the author as a senior Azure integration developer, Dynamics 365 /
Power Platform integration specialist, .NET and Azure architecture practitioner,
and enterprise delivery-focused engineer.

## 🧭 Fast Path: Choosing The Right Azure Service

| Need | Start with | Consider instead when |
| --- | --- | --- |
| Run a web API | App Service | Use Functions for event handlers; Container Apps for container-first workloads. |
| Run event-driven code | Azure Functions | Use App Service or Container Apps for long-running services. |
| Buffer business work | Service Bus Queue | Use Event Grid for notification; Event Hubs for streaming telemetry. |
| Publish to multiple business consumers | Service Bus Topic | Use Event Grid when subscribers only need event notification. |
| Store files or payloads | Blob Storage | Use Data Lake Storage for analytics zones and big-data access. |
| Store relational data | Azure SQL Database | Use Cosmos DB for partitioned, globally distributed NoSQL workloads. |
| Store secrets | Key Vault | Use App Configuration for non-secret settings and feature flags. |
| Build connector-heavy workflow | Logic Apps | Use Durable Functions when orchestration needs code-first control. |
| Put a boundary in front of APIs | API Management | Use Front Door or Application Gateway for broader ingress/WAF scenarios. |
| Observe production behavior | Application Insights | Use Log Analytics for cross-resource KQL and diagnostic logs. |

See the full [Service Selection Guide](docs/service-selection-guide.md).

## Production Readiness

Use the [Production Readiness Checklist](docs/production-readiness-checklist.md)
before moving an Azure workload into production.

It covers identity, secrets, networking, messaging, APIs, storage, monitoring,
CI/CD, resilience, cost control, documentation, and support handover.

## Real-World Azure Integration Focus

This repo is intentionally biased toward the integration patterns that show up
in enterprise delivery:

- Azure Service Bus queues, topics, subscriptions, DLQs, sessions, retries, and
  duplicate detection.
- Azure Functions for HTTP APIs, timers, Service Bus processors, and Event Grid
  handlers.
- API Management for API governance, OAuth/JWT validation, throttling, and
  partner-facing contracts.
- Logic Apps for connector-heavy workflow, approvals, and enterprise
  orchestration.
- Blob Storage for document ingestion, export, archiving, and claim-check
  payloads.
- Key Vault and Managed Identity for secretless Azure-to-Azure authentication.
- Azure DevOps and GitHub Actions for build, validation, environment promotion,
  and repeatable Bicep deployments.
- Dataverse / Dynamics 365 integration using queues, webhooks, plugins,
  application users, and least-privilege access.
- Event-driven architecture and DMZ-safe upload patterns for secure enterprise
  boundaries.

## Common Enterprise Azure Stacks

| Scenario | Typical stack | Why it works |
| --- | --- | --- |
| Partner API integration | API Management, App Service or Functions, Service Bus, Key Vault, Application Insights | Separates API contract, processing, secrets, and async reliability. |
| Dataverse async integration | Dataverse plugin or Power Automate, Service Bus Topic, Azure Function worker, SQL or Blob Storage | Keeps Dataverse transactions short and gives Azure-side retry/monitoring. |
| Document ingestion | Blob Storage, Event Grid, Azure Function, Service Bus, worker service, Defender or malware scan | Creates a controlled landing zone before internal processing. |
| Workflow automation | Logic Apps, API Management, Key Vault, Service Bus, Function helpers | Uses connectors for workflow while keeping complex logic in code. |
| Containerized worker platform | Container Apps, Service Bus scaler, Managed Identity, Key Vault, Log Analytics | Good balance between container packaging and low platform overhead. |
| Enterprise API platform | Front Door or Application Gateway, API Management, App Service, Private Endpoints, App Insights | Provides ingress, governance, private backends, and telemetry. |

## Architecture Recipes

Practical implementation recipes live in [recipes](recipes/README.md).

Featured recipes:

- [Public API to Service Bus](recipes/public-api-to-service-bus.md)
- [Dataverse to Service Bus](recipes/dataverse-to-service-bus.md)
- [Blob Claim-Check Pattern](recipes/blob-claim-check-pattern.md)
- [DMZ-Safe Document Ingestion](recipes/dmz-safe-document-ingestion.md)
- [Enterprise Secure Integration Stack](recipes/enterprise-secure-integration-stack.md)

## Recruiter / Manager Quick Summary

This repository demonstrates practical knowledge across:

- Azure architecture and service selection.
- .NET APIs, Azure Functions, workers, and integration services.
- Azure Service Bus, Event Grid, Event Hubs, and event-driven design.
- API Management, Logic Apps, Blob Storage, Key Vault, Managed Identity, and
  Azure DevOps.
- Dynamics 365 / Dataverse integration and enterprise application integration.
- Security, cost, scalability, monitoring, deployment, and production support
  considerations.

## Production Lessons This Repo Emphasizes

- If message ordering matters, unrestricted parallel consumers will eventually
  create reconciliation issues unless sessions or another ordering strategy are
  used.
- If there is no DLQ alert, failed integration work is silently becoming support
  debt.
- If correlation IDs stop at the API boundary, production incidents become
  guesswork.
- If secrets live in appsettings or pipelines, rotation becomes a release
  project instead of an operational task.
- If private endpoints are added without DNS planning, the outage will look like
  an application problem.

## 🏗️ Featured Architecture Patterns

- [Queue-based load leveling](docs/architecture-patterns.md#queue-based-load-leveling)
- [API gateway pattern](docs/architecture-patterns.md#api-gateway-pattern)
- [Outbox pattern](docs/architecture-patterns.md#outbox-pattern)
- [Claim check pattern](docs/architecture-patterns.md#claim-check-pattern)
- [DMZ-safe document ingestion](docs/architecture-patterns.md#dmz-safe-document-ingestion)
- [Distributed tracing flow](docs/architecture-patterns.md#monitoring-and-distributed-tracing)
- [Event-driven microservice architecture](docs/architecture-patterns.md#event-driven-microservice-architecture)

## Typical Enterprise Integration

```mermaid
flowchart LR
    external[External App] --> apim[API Management]
    apim --> compute[Azure Function or App Service]
    compute --> bus[Service Bus Queue or Topic]
    bus --> worker[Worker Function or Container App]
    worker --> sql[(Azure SQL)]
    worker --> blob[(Blob Storage)]
    compute --> insights[Application Insights]
    worker --> insights
```

## Azure Service Map

| Category | Services | Use when |
| --- | --- | --- |
| Compute | App Service, Azure Functions, Container Apps, AKS, VMs | You need to run APIs, jobs, containers, event handlers, or long-running workloads. |
| Storage | Blob Storage, Data Lake Storage, Files, Queues, Tables | You need files, documents, data lake zones, simple queues, or low-cost object storage. |
| Integration | Logic Apps, Functions, API Management, Event Grid, Durable Functions | You need workflow, orchestration, API mediation, system integration, or event routing. |
| Messaging | Service Bus, Event Grid, Event Hubs, Storage Queues | You need async messaging, pub/sub, event notification, telemetry ingestion, or buffering. |
| Identity & Security | Entra ID, Managed Identity, Key Vault, RBAC, Private Link | You need auth, secret management, least privilege, private access, or workload identity. |
| Databases | Azure SQL, Cosmos DB, PostgreSQL, Redis, Table Storage | You need relational, globally distributed NoSQL, cache, or simple key-value storage. |
| Monitoring | Azure Monitor, Application Insights, Log Analytics, Alerts | You need telemetry, traces, logs, dashboards, alerting, and production diagnosis. |
| DevOps | Azure DevOps, GitHub Actions, Bicep, deployment slots | You need repeatable build, test, deploy, promotion, and infrastructure automation. |
| Networking | VNets, Private Endpoints, DNS, Load Balancer, Application Gateway, Front Door | You need secure connectivity, ingress, egress control, routing, and private service access. |

## Learning Roadmap

1. Start with the [Service Selection Guide](docs/service-selection-guide.md).
2. Learn the messaging split in [Messaging](docs/messaging.md).
3. Review integration trade-offs in [Integration](docs/integration.md).
4. Study production patterns in [Architecture Patterns](docs/architecture-patterns.md).
5. Add security depth with [Identity & Security](docs/identity-security.md).
6. Add delivery maturity with [DevOps](docs/devops.md).
7. Use [Gotchas](docs/gotchas.md) as a pre-production review checklist.
8. Use [Architecture Recipes](recipes/README.md) for real implementation shapes.
9. Use [Production Readiness](docs/production-readiness-checklist.md) before go-live.

## Docs

- [Docs Index](docs/README.md)
- [Compute](docs/compute.md)
- [Storage](docs/storage.md)
- [Integration](docs/integration.md)
- [Messaging](docs/messaging.md)
- [Identity & Security](docs/identity-security.md)
- [Networking](docs/networking.md)
- [Databases](docs/databases.md)
- [Monitoring](docs/monitoring.md)
- [DevOps](docs/devops.md)
- [Architecture Patterns](docs/architecture-patterns.md)
- [Service Selection Guide](docs/service-selection-guide.md)
- [Production Readiness Checklist](docs/production-readiness-checklist.md)
- [Promotion Notes](docs/promotion-notes.md)
- [Gotchas](docs/gotchas.md)
- [Diagrams](diagrams/README.md)
- [Architecture Recipes](recipes/README.md)

## Snippets

- [Snippets Index](snippets/README.md)
- [Azure CLI](snippets/azure-cli.md)
- [Bicep](snippets/bicep.md)
- [PowerShell](snippets/powershell.md)
- [.NET](snippets/dotnet.md)
- [DevOps YAML](snippets/devops-yaml.md)

## Contributing

Contributions are welcome when they keep the repo practical, concise, and
field-tested.

Good additions include:

- Real-world gotchas.
- Decision tables.
- Secure defaults.
- Production snippets.
- Links to official Microsoft documentation where they help.

See [CONTRIBUTING.md](CONTRIBUTING.md).

## Related Repositories

- [Dynamics 365 Developer Cheat Sheet](https://github.com/brunsdon/dynamics365-developer-cheat-sheet)
- [Power Platform Integration Patterns](https://github.com/brunsdon/power-platform-integration-patterns)
- [Power Pages Cheat Sheet](https://github.com/brunsdon/power-pages-cheat-sheet)
- [Dataverse Schema Design Guide](https://github.com/brunsdon/dataverse-schema-design-guide)

## Suggested GitHub Topics

`azure`, `azure-functions`, `azure-service-bus`, `azure-architecture`,
`cloud-architecture`, `dotnet`, `integration-patterns`, `azure-devops`,
`api-management`, `event-driven-architecture`, `service-bus`, `serverless`,
`cloud-native`, `microservices`, `power-platform`, `dynamics365`

## Author

Maintained by Matthew Brunsdon.

## License

MIT. See [LICENSE](LICENSE).
