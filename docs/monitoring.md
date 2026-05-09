# Monitoring

## What It Is

Azure monitoring services collect metrics, logs, traces, dependencies, availability results, and alerts across applications and infrastructure.

| Service | Best fit |
| --- | --- |
| Application Insights | Application telemetry, traces, dependencies, exceptions, performance. |
| Log Analytics | Central KQL query store for logs. |
| Azure Monitor Metrics | Platform and resource metrics. |
| Alerts | Actionable notification and automation. |
| Workbooks / Dashboards | Operational views and reporting. |

## When To Use It

- Use Application Insights for APIs, Functions, workers, and distributed tracing.
- Use Log Analytics for cross-resource query and diagnostics.
- Use alerts for symptoms users care about, not every noisy metric.
- Use correlation IDs across APIs, queues, workers, and databases.

## When Not To Use It

- Do not log secrets, tokens, full message bodies, or personal data without a clear policy.
- Do not keep verbose logs forever.
- Do not rely only on infrastructure metrics for application health.
- Do not create alerts that no one owns.

## Common Patterns

- API receives request, sets correlation ID, sends Service Bus message with same ID.
- Worker logs processing outcome and dependency calls to Application Insights.
- Log Analytics query powers an alert for DLQ count or failed jobs.
- Availability tests monitor public APIs.

## Common Gotchas

- Sampling can hide individual traces during high volume.
- Log Analytics ingestion can become expensive quickly.
- Distributed tracing breaks when correlation context is not propagated.
- Alerts without runbooks create noise.
- Service Bus DLQs need explicit monitoring.

## Security Notes

- Treat telemetry as potentially sensitive.
- Use private ingestion where required.
- Restrict workspace access with RBAC.
- Apply retention policies aligned to compliance needs.

## Cost Considerations

- Ingestion volume, retention, and high-cardinality logs drive cost.
- Verbose debug logs in production can be expensive.
- Separate workspaces can help ownership but reduce cross-query simplicity.

## Examples

```kusto
traces
| where timestamp > ago(1h)
| where customDimensions.CorrelationId == "corr-123"
| order by timestamp asc
```

```csharp
logger.LogInformation(
    "Processed message {MessageId} for {EntityName}",
    message.MessageId,
    "account");
```

## Official Docs

- [Azure Monitor documentation](https://learn.microsoft.com/azure/azure-monitor/)
- [Application Insights documentation](https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview)
- [Log Analytics documentation](https://learn.microsoft.com/azure/azure-monitor/logs/log-analytics-overview)
