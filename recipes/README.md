# Architecture Recipes

Practical Azure architecture recipes for common integration and application
patterns.

Each recipe explains the problem, recommended services, trade-offs, production
gotchas, monitoring, cost profile, and readiness checks.

## Featured Recipes

| Recipe | Best for |
| --- | --- |
| [Public API to Service Bus](public-api-to-service-bus.md) | Accepting API requests quickly and processing work asynchronously. |
| [Dataverse to Service Bus](dataverse-to-service-bus.md) | Decoupling Dynamics 365 / Dataverse from Azure-side processing. |
| [Blob Claim-Check Pattern](blob-claim-check-pattern.md) | Moving large payloads through messaging workflows safely. |
| [DMZ-Safe Document Ingestion](dmz-safe-document-ingestion.md) | Secure external document upload and controlled internal processing. |
| [Event-Driven Microservice](event-driven-microservice.md) | Durable business pub/sub across independent services. |
| [Low-Cost Startup Azure Stack](low-cost-startup-azure-stack.md) | Starting small without painting the architecture into a corner. |
| [Enterprise Secure Integration Stack](enterprise-secure-integration-stack.md) | Secure, monitored, governed enterprise integration architecture. |

## How To Use These Recipes

- Start with the problem statement.
- Check when the pattern fits and when it does not.
- Review the operational gotchas before choosing the pattern.
- Use the checklist before production.
- Follow related repo docs for deeper service guidance.
