# Compute

## What It Is

Azure compute services run application code: web APIs, scheduled jobs, event handlers, containers, background workers, and full virtual machines.

| Service | Best fit |
| --- | --- |
| App Service | Hosted web apps, .NET APIs, REST services, background WebJobs. |
| Azure Functions | Event-driven code, HTTP triggers, queues, timers, lightweight integration logic. |
| Container Apps | Containerized APIs, workers, event-driven jobs, KEDA-based scaling. |
| AKS | Kubernetes platforms with strong operations ownership. |
| Virtual Machines | Legacy workloads, special OS needs, installed software, migration stepping stones. |

## When To Use It

- Use App Service for stable HTTP APIs and web apps that need predictable hosting.
- Use Functions for small event handlers, queue processors, timers, and serverless integration.
- Use Container Apps when packaging, sidecars, scale-to-zero, or container parity matters.
- Use AKS when you need Kubernetes APIs, custom networking, service mesh, or platform-level control.

## When Not To Use It

- Do not use Functions as a dumping ground for large domain applications.
- Do not use AKS just because the app is containerized.
- Avoid VMs for new cloud-native apps unless there is a hard dependency.
- Avoid Consumption Functions for workloads needing stable warm latency or long execution windows.

## Common Patterns

- API on App Service behind API Management.
- Queue-triggered Function processing Service Bus messages.
- Container Apps worker scaled by Service Bus queue length.
- Durable Functions for stateful orchestration and fan-out/fan-in.
- App Service deployment slots for blue-green style releases.

## Common Gotchas

- Cold starts can matter for latency-sensitive Functions.
- Function timeout limits depend on hosting plan.
- App Service plan sizing affects every app in the plan.
- Container Apps revisions can surprise teams if traffic weights are not understood.
- AKS shifts a lot of operational responsibility to your team.

## Security Notes

- Prefer Managed Identity over connection strings.
- Use Private Endpoints for sensitive backends.
- Put public APIs behind API Management, Front Door, or Application Gateway where appropriate.
- Disable FTP/basic publishing unless required.
- Store secrets in Key Vault, not application settings.

## Cost Considerations

- Functions Consumption can be cheap for bursty workloads.
- Premium Functions reduce cold starts but add baseline cost.
- App Service cost is tied to the plan, not request count.
- Container Apps charges for allocated CPU/memory and requests.
- AKS needs node, networking, logging, and operations cost planning.

## Examples

```bash
az functionapp create \
  --resource-group rg-cheatsheet-dev \
  --consumption-plan-location australiaeast \
  --runtime dotnet-isolated \
  --functions-version 4 \
  --name func-cheatsheet-dev \
  --storage-account stcheatsheetdev
```

```bicep
resource plan 'Microsoft.Web/serverfarms@2023-12-01' = {
  name: 'asp-cheatsheet-dev'
  location: resourceGroup().location
  sku: {
    name: 'P0v3'
    tier: 'PremiumV3'
  }
}
```

## Official Docs

- [Azure App Service documentation](https://learn.microsoft.com/azure/app-service/)
- [Azure Functions documentation](https://learn.microsoft.com/azure/azure-functions/)
- [Azure Container Apps documentation](https://learn.microsoft.com/azure/container-apps/)
- [AKS documentation](https://learn.microsoft.com/azure/aks/)
