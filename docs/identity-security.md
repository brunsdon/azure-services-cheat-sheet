# Identity & Security

## What It Is

Azure identity and security services control who can access resources, how workloads authenticate, how secrets are stored, and how private enterprise traffic reaches cloud services.

| Service | Best fit |
| --- | --- |
| Microsoft Entra ID | Users, groups, apps, OAuth 2.0, workload identity. |
| Managed Identity | Passwordless auth from Azure workloads to Azure resources. |
| Key Vault | Secrets, keys, certificates. |
| RBAC | Resource-level authorization. |
| Private Endpoints | Private network access to PaaS services. |
| App Registrations | OAuth clients, API permissions, application identities. |

## When To Use It

- Use Managed Identity for Azure-to-Azure authentication.
- Use Key Vault for secrets, certificates, signing keys, and rotation workflows.
- Use Entra app registrations for OAuth clients and APIs.
- Use RBAC for least-privilege access to Azure resources.
- Use Private Endpoints when traffic must stay on private IP space.

## When Not To Use It

- Do not use client secrets where Managed Identity is available.
- Do not assign broad roles such as Owner or Contributor to app identities by default.
- Do not store secrets in appsettings, source control, pipeline variables, or Dataverse plain text.
- Do not create private endpoints without planning DNS.

## Common Patterns

- Function App uses Managed Identity to read Key Vault and send to Service Bus.
- API Management validates JWTs issued by Entra ID.
- App registration exposes an API scope consumed by partner applications.
- Private Endpoint secures SQL, Storage, Key Vault, and Service Bus.
- RBAC groups map to platform, developer, reader, and deployment roles.

## Least Privilege Checklist

- Assign roles at the narrowest practical scope.
- Separate deploy-time permissions from runtime permissions.
- Use sender-only and receiver-only Service Bus roles where possible.
- Grant Key Vault Secrets User instead of broad administrator rights.
- Review app registration API permissions and admin consent.
- Rotate credentials that cannot be replaced with Managed Identity.
- Alert on privileged role assignment changes.

## Common Gotchas

- Managed Identity object IDs differ between environments.
- Private Endpoint DNS misconfiguration is a common outage source.
- Key Vault firewall rules can break CI/CD deployments.
- App registration secrets expire; certificates also need lifecycle management.
- RBAC propagation is not instant.

## Security Notes

- Use OAuth 2.0 authorization code flow for user-facing apps.
- Use client credentials only for service-to-service access when Managed Identity is unavailable.
- Use certificate credentials over long-lived client secrets for external workloads.
- Enable diagnostic logs for Key Vault and identity-sensitive resources.
- Treat connection strings as secrets even when RBAC is enabled.

## Cost Considerations

- Entra ID feature cost depends on tenant licensing.
- Key Vault charges for operations and protected key usage.
- Private Endpoints add hourly and data processing charges.
- Security logging can materially increase Log Analytics cost.

## Examples

```bash
az keyvault create \
  --name kv-cheatsheet-dev \
  --resource-group rg-cheatsheet-dev \
  --location australiaeast \
  --enable-rbac-authorization true
```

```csharp
var client = new SecretClient(
    new Uri("https://kv-cheatsheet-dev.vault.azure.net/"),
    new DefaultAzureCredential());

KeyVaultSecret secret = await client.GetSecretAsync("ServiceBusConnection");
```

## Official Docs

- [Microsoft identity platform documentation](https://learn.microsoft.com/entra/identity-platform/)
- [Managed identities documentation](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/)
- [Azure Key Vault documentation](https://learn.microsoft.com/azure/key-vault/)
- [Azure RBAC documentation](https://learn.microsoft.com/azure/role-based-access-control/)
- [Private Endpoint documentation](https://learn.microsoft.com/azure/private-link/private-endpoint-overview)
