# Snippets

Small, reusable examples for common Azure implementation tasks.

## Index

| Snippet file                  | Includes                                                                                         |
| ----------------------------- | ------------------------------------------------------------------------------------------------ |
| [Azure CLI](azure-cli.md)     | Resource groups, Storage, Service Bus, Functions, Managed Identity, Key Vault, Bicep deployment. |
| [Bicep](bicep.md)             | Storage, Service Bus, Function Apps, Key Vault, RBAC, diagnostics.                               |
| [PowerShell](powershell.md)   | Resource groups, Bicep deployment, Key Vault, Service Bus checks.                                |
| [.NET](dotnet.md)             | Service Bus sender/receiver, Functions, Key Vault, dependency injection.                         |
| [DevOps YAML](devops-yaml.md) | GitHub Actions and Azure DevOps pipeline examples.                                               |

## Notes

- Replace sample names with your environment naming standard.
- Prefer Managed Identity over connection strings for Azure-hosted workloads.
- Keep secrets in Key Vault, not in snippets, YAML, or appsettings files.
- Treat snippets as starting points, not complete production modules.
