# DevOps

## What It Is

Azure DevOps and GitHub Actions automate build, test, security validation, infrastructure deployment, and release promotion.

## When To Use It

- Use CI for every pull request.
- Use Bicep or Terraform for repeatable infrastructure.
- Use environments and approvals for controlled promotion.
- Use deployment slots or blue-green patterns for web apps.
- Use workload identity or service connections instead of static secrets where possible.

## When Not To Use It

- Do not deploy manually from a developer machine as the normal path.
- Do not store production secrets in repository variables.
- Do not let pipelines use broad subscription Owner access.
- Do not mix environment-specific configuration into source code.

## Common Patterns

- Build once, deploy the same artifact through dev, test, and prod.
- Bicep deployment creates infrastructure, app deployment publishes code.
- GitHub Actions or Azure DevOps pipeline uses federated credentials.
- Pull request checks run tests, lint, and infrastructure validation.
- Release gates require approval before production.

## Environment Promotion

| Stage | Purpose | Typical checks |
| --- | --- | --- |
| Dev | Fast feedback | Build, unit tests, lint, Bicep validation. |
| Test | Integration confidence | Integration tests, smoke tests, seeded config. |
| UAT | Business validation | Approval, release notes, migration dry run. |
| Prod | Live service | Approval, rollback plan, monitoring. |

## Secret Handling

- Prefer Managed Identity and workload identity federation.
- Store runtime secrets in Key Vault.
- Use pipeline secrets only for bootstrap values that cannot be avoided.
- Never echo secrets in logs.
- Rotate service connection credentials and app registration secrets.

## CI/CD Checklist

- Build and test on pull request.
- Validate infrastructure before deployment.
- Deploy infrastructure and app separately but consistently.
- Promote immutable artifacts.
- Parameterize environment values.
- Capture deployment outputs.
- Smoke test after deployment.
- Alert on failed deployments.

## Common Gotchas

- Pipeline service principals often have more access than needed.
- Environment drift appears when hotfixes are made in the portal.
- Bicep complete mode can delete resources if used carelessly.
- App settings overwrite can remove values owned by another process.
- Deployment slot swaps need warmup and configuration slot settings.

## Security Notes

- Use least-privilege roles for deployment identities.
- Protect production environments with approvals.
- Require branch protection and status checks.
- Scan dependencies and infrastructure templates.

## Cost Considerations

- Hosted build minutes and parallel jobs may have cost.
- Preview environments need lifecycle cleanup.
- Diagnostic and security scans can increase pipeline duration.

## Examples

```yaml
name: deploy

on:
  push:
    branches: [main]

permissions:
  id-token: write
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
      - run: az deployment group create --resource-group rg-cheatsheet-dev --template-file infra/main.bicep
```

## Official Docs

- [Azure DevOps documentation](https://learn.microsoft.com/azure/devops/)
- [GitHub Actions documentation](https://docs.github.com/actions)
- [Bicep documentation](https://learn.microsoft.com/azure/azure-resource-manager/bicep/)
- [Azure login GitHub Action](https://github.com/Azure/login)
