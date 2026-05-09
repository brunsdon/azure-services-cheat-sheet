# Architecture Recipes

Practical Azure architecture recipes for common integration and application
patterns.

Each recipe explains the problem, recommended services, trade-offs, production
gotchas, monitoring, cost profile, and readiness checks.

## Featured Recipes

| Recipe                                                                        | Best for                         |
| ----------------------------------------------------------------------------- | -------------------------------- |
| [Public API to Service Bus](public-api-to-service-bus.md)                     | Async API processing.            |
| [Dataverse to Service Bus](dataverse-to-service-bus.md)                       | Dynamics / Dataverse decoupling. |
| [Blob Claim-Check Pattern](blob-claim-check-pattern.md)                       | Large payload messaging.         |
| [DMZ-Safe Document Ingestion](dmz-safe-document-ingestion.md)                 | Secure external uploads.         |
| [Event-Driven Microservice](event-driven-microservice.md)                     | Durable business pub/sub.        |
| [Low-Cost Startup Azure Stack](low-cost-startup-azure-stack.md)               | Low-cost first production stack. |
| [Enterprise Secure Integration Stack](enterprise-secure-integration-stack.md) | Governed enterprise integration. |

## How To Use These Recipes

- Start with the problem statement.
- Check when the pattern fits and when it does not.
- Review the operational gotchas before choosing the pattern.
- Use the checklist before production.
- Follow related repo docs for deeper service guidance.
