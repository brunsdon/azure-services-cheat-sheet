# Gotchas

## What It Is

Short notes on Azure issues that often show up during delivery, production
support, or architecture reviews.

## Identity

- RBAC assignments can take time to propagate.
- Managed Identity object IDs change per environment.
- App registration secrets and certificates expire.
- Key Vault firewall rules can block deployments and local debugging.

## Messaging

- Service Bus messages can be delivered more than once; handlers must be
  idempotent.
- Lock duration must match processing time or be renewed.
- DLQs do not fix themselves; monitor and triage them.
- Sessions are useful for ordering but reduce concurrency.
- Large payloads belong in Blob Storage, not queue messages.

## Functions And App Services

- Cold starts can affect Consumption Functions.
- App settings are environment config, not a secret strategy.
- Deployment slots need slot-specific settings.
- Scaling out can expose missing idempotency and concurrency bugs.

## Storage

- Public blob access should be disabled by default.
- Archive tier requires rehydration before reads.
- Soft delete and versioning increase retained storage.
- Blob trigger timing may not be suitable for every workflow; Event Grid is
  often cleaner.

## Networking

- Private Endpoint DNS is easy to misconfigure.
- App Service VNet integration affects outbound traffic only.
- NAT and egress IP planning matter for partner allowlists.
- Firewalls and IP restrictions can block deployment agents.

## Databases

- Cosmos DB partition keys are foundational design decisions.
- SQL performance issues often come from missing indexes, chatty access, or
  under-sized compute.
- Redis eviction can remove data if memory policy is not understood.

## DevOps

- Portal hotfixes create drift.
- Pipelines often have excessive permissions.
- Complete-mode infrastructure deployments can remove resources.
- Logging secrets by accident is easier than most teams expect.

## Monitoring

- No correlation ID means painful incident diagnosis.
- Sampling can hide individual traces.
- Log ingestion can become a top cost driver.
- Alerts without owners become ignored noise.
