# DevOps YAML Snippets

## GitHub Actions: Validate Markdown

```yaml
name: Markdown Check

on:
  pull_request:
  push:
    branches:
      - main

jobs:
  markdown:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Markdown lint
        uses: DavidAnson/markdownlint-cli2-action@v17
        with:
          globs: "**/*.md"
```

## GitHub Actions: Deploy Bicep With Federated Identity

```yaml
name: Deploy Azure

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

      - name: What-if
        run: |
          az deployment group what-if \
            --resource-group rg-contoso-prod \
            --template-file infra/main.bicep \
            --parameters environmentName=prod

      - name: Deploy
        run: |
          az deployment group create \
            --resource-group rg-contoso-prod \
            --template-file infra/main.bicep \
            --parameters environmentName=prod
```

## Azure DevOps: Build And Deploy

```yaml
trigger:
  branches:
    include:
      - main

variables:
  vmImage: ubuntu-latest

stages:
  - stage: Validate
    displayName: Validate
    jobs:
      - job: Build
        pool:
          vmImage: $(vmImage)
        steps:
          - checkout: self
          - script: dotnet test
            displayName: Run tests

  - stage: DeployProd
    displayName: Deploy Prod
    dependsOn: Validate
    jobs:
      - deployment: Deploy
        environment: prod
        pool:
          vmImage: $(vmImage)
        strategy:
          runOnce:
            deploy:
              steps:
                - checkout: self
                - task: AzureCLI@2
                  displayName: Deploy Bicep
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
