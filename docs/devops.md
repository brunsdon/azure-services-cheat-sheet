# DevOps

## What It Is

Azure DevOps and GitHub Actions automate build, test, security validation,
infrastructure deployment, application release, and environment promotion.

For Azure architecture work, DevOps is not just "deployment." It is how teams
make environments repeatable, control risk, and keep production recoverable.

## When To Use It

- Use CI for every pull request.
- Use Bicep or Terraform for repeatable infrastructure.
- Use workload identity federation or Managed Identity where possible.
- Use environment approvals for production.
- Use immutable build artifacts promoted across environments.
- Use deployment slots or blue-green patterns for web workloads.

## When Not To Use It

- Do not deploy manually from a developer machine as the normal path.
- Do not store production secrets in repository variables.
- Do not let pipelines use broad subscription Owner access.
- Do not mix environment-specific configuration into source code.
- Do not skip rollback planning for database or schema changes.

## GitHub Actions Vs Azure DevOps

| Factor                | GitHub Actions                                      | Azure DevOps Pipelines           |
| --------------------- | --------------------------------------------------- | -------------------------------- |
| Best fit              | GitHub-native repos and open-source style workflows | Enterprise Azure DevOps delivery |
| YAML experience       | Strong                                              | Strong                           |
| Environment approvals | Supported                                           | Supported                        |
| Work item integration | GitHub Issues / Projects                            | Azure Boards                     |
| Azure integration     | Strong with `azure/login`                           | Strong with service connections  |
| Common choice         | Modern GitHub repos                                 | Existing Azure DevOps estates    |

## Environment Promotion

| Stage | Purpose                   | Typical checks                                   |
| ----- | ------------------------- | ------------------------------------------------ |
| Dev   | Fast engineering feedback | Build, unit tests, lint, Bicep validation.       |
| Test  | Integration confidence    | Integration tests, seeded config, smoke tests.   |
| UAT   | Business validation       | Approval, release notes, migration dry run.      |
| Prod  | Live service              | Approval, rollback plan, monitoring, smoke test. |

## Bicep Deployment Flow

```mermaid
flowchart LR
    pr[Pull Request] --> validate[Build and Validate]
    validate --> artifact[Build Artifact]
    artifact --> dev[Deploy Dev]
    dev --> test[Deploy Test]
    test --> approval[Production Approval]
    approval --> prod[Deploy Prod]
    prod --> smoke[Smoke Test and Monitor]
```

## Secret Handling

- Prefer workload identity federation for pipelines.
- Use Managed Identity for runtime workloads.
- Store runtime secrets in Key Vault.
- Use pipeline secrets only for bootstrap values that cannot be avoided.
- Never echo secrets in logs.
- Rotate service connections, app registration credentials, and certificates.
- Keep production secrets out of pull request validation workflows.

## CI/CD Checklist

- Build and test on pull request.
- Run markdown, code, and infrastructure lint checks.
- Validate Bicep with `what-if` where appropriate.
- Deploy infrastructure and application consistently.
- Promote immutable artifacts.
- Parameterize environment values.
- Capture deployment outputs.
- Smoke test after deployment.
- Alert on failed deployments.
- Keep rollback steps documented.

## Security Considerations

- Use least-privilege roles for deployment identities.
- Protect production environments with approvals.
- Require branch protection and status checks.
- Pin GitHub Actions to trusted versions where appropriate.
- Scan dependencies and infrastructure templates.
- Separate build, deploy, and runtime permissions.

## Scaling And Operations

- Use self-hosted agents only when private network access is required.
- Patch self-hosted agents and rotate their credentials.
- Keep pipeline logs long enough for audit and incident review.
- Use deployment slots for App Service warmup and safer release.
- Monitor deployment frequency, failure rate, and lead time.

## Cost Considerations

- Hosted build minutes and parallel jobs may have cost.
- Self-hosted agents need compute, patching, and operational ownership.
- Preview environments need automatic cleanup.
- Security scans can increase pipeline time but reduce delivery risk.

## Operational Gotchas

- Portal hotfixes create drift.
- Complete-mode infrastructure deployments can remove resources.
- App settings overwrites can delete values owned by another process.
- Slot swaps need slot-specific settings and warmup planning.
- Pipeline service principals often have more access than required.
- Failed deployments should page the owning team, not vanish into logs.

## GitHub Actions Example

```yaml
name: Build and Deploy

on:
  push:
    branches:
      - main

permissions:
  id-token: write
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: prod

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Azure login
        uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}

      - name: Deploy Bicep
        run: |
          az deployment group create \
            --resource-group rg-contoso-prod \
            --template-file infra/main.bicep \
            --parameters environmentName=prod
```

## Azure DevOps YAML Example

```yaml
trigger:
  branches:
    include:
      - main

stages:
  - stage: Validate
    jobs:
      - job: Build
        pool:
          vmImage: ubuntu-latest
        steps:
          - checkout: self
          - script: dotnet test
            displayName: Test

  - stage: Deploy
    dependsOn: Validate
    jobs:
      - deployment: DeployProd
        environment: prod
        strategy:
          runOnce:
            deploy:
              steps:
                - checkout: self
                - task: AzureCLI@2
                  inputs:
                    azureSubscription: sc-contoso-prod
                    scriptType: bash
                    scriptLocation: inlineScript
                    inlineScript: |
                      az deployment group create \
                        --resource-group rg-contoso-prod \
                        --template-file infra/main.bicep \
                        --parameters environmentName=prod
```

## Official Docs

- [Azure DevOps documentation](https://learn.microsoft.com/azure/devops/)
- [GitHub Actions documentation](https://docs.github.com/actions)
- [Bicep documentation](https://learn.microsoft.com/azure/azure-resource-manager/bicep/)
- [Azure login GitHub Action](https://github.com/Azure/login)
