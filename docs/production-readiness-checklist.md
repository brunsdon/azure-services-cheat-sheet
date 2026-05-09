# Production Readiness Checklist

Use this checklist before promoting an Azure integration or application workload to production.

## Identity And Access

- [ ] Managed Identity is used where possible.
- [ ] Runtime identities are separate from deployment identities.
- [ ] RBAC least privilege is applied at the narrowest practical scope.
- [ ] Human privileged access is controlled and reviewed.
- [ ] App registrations have owners and credential expiry monitoring.
- [ ] Dataverse application users have least-privilege security roles.

## Secrets And Configuration

- [ ] Secrets are stored in Key Vault.
- [ ] No secrets are stored in source control.
- [ ] No secrets are stored in appsettings unless using secure references.
- [ ] Pipeline variables are not used for long-lived production secrets unless secured and
      unavoidable.
- [ ] Key Vault access has been reviewed.
- [ ] App Configuration is used for non-secret shared settings where useful.
- [ ] Secret rotation process is documented.

## Networking

- [ ] Public endpoints are intentional and documented.
- [ ] Private Endpoints are used where required.
- [ ] Private DNS resolution is tested from apps, agents, and support paths.
- [ ] Ingress uses API Management, Front Door, or Application Gateway when governance or WAF is
      required.
- [ ] Egress IPs are known where partners require allowlists.
- [ ] Firewall and IP restrictions do not block deployments or health probes.

## Messaging

- [ ] Service Bus queues, topics, and subscriptions have owners.
- [ ] Dead-letter monitoring is configured.
- [ ] Retry policies are documented and tested.
- [ ] Idempotency is considered for every message handler.
- [ ] Message ordering requirements are documented.
- [ ] Duplicate detection or idempotency keys are used where needed.
- [ ] Replay process is tested.

## APIs

- [ ] API contracts are documented.
- [ ] API versioning approach is defined.
- [ ] Authentication and authorization are tested.
- [ ] Rate limits and quotas are considered.
- [ ] Async APIs return appropriate status codes such as `202 Accepted`.
- [ ] Correlation IDs are accepted and returned.

## Storage

- [ ] Blob containers are private unless public access is intentional.
- [ ] Lifecycle policies match retention and replay requirements.
- [ ] Soft delete and versioning are configured where needed.
- [ ] Large payloads use claim-check patterns instead of message bodies.
- [ ] Backup and restore requirements are documented.

## Monitoring And Alerting

- [ ] Application Insights is configured.
- [ ] Correlation IDs are implemented across API, queue, worker, and database.
- [ ] Structured logging includes safe business keys.
- [ ] Alerts are configured for failure paths, not just infrastructure symptoms.
- [ ] Service Bus DLQ alerts are configured.
- [ ] Dependency failure alerts are configured.
- [ ] Dashboards or workbooks exist for support teams.
- [ ] Runbooks exist for common failures.

## CI/CD

- [ ] Infrastructure as Code is checked into source control.
- [ ] CI runs on pull requests.
- [ ] Bicep or Terraform validation runs before deployment.
- [ ] Environment promotion path is documented.
- [ ] Production deployments require approval where appropriate.
- [ ] Rollback or remediation process is documented.
- [ ] Pipeline permissions use least privilege.

## Resilience

- [ ] Timeout policies are explicit.
- [ ] Retry policies use backoff and avoid infinite retries.
- [ ] Circuit breaker or queue isolation is used for fragile dependencies.
- [ ] Poison message handling is documented.
- [ ] Downstream throttling behavior is tested.
- [ ] Disaster recovery and restore expectations are agreed.

## Cost Control

- [ ] Azure budgets or cost alerts are configured.
- [ ] Log Analytics retention is intentional.
- [ ] Verbose production logging is limited.
- [ ] Premium SKUs have documented justification.
- [ ] Idle test resources have cleanup rules.
- [ ] Storage lifecycle policies are configured where useful.

## Documentation And Support Handover

- [ ] Architecture diagram is current.
- [ ] Service owners are documented.
- [ ] Support runbooks are available.
- [ ] Common failure modes and replay steps are documented.
- [ ] Operational dashboards are linked.
- [ ] On-call or support escalation path is defined.
- [ ] Known limitations and technical debt are recorded.
