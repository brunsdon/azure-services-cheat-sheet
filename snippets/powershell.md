# PowerShell Snippets

## Login And Subscription

```powershell
Connect-AzAccount
Set-AzContext -Subscription "<subscription-id-or-name>"
Get-AzContext
```

## Create Resource Group

```powershell
New-AzResourceGroup `
  -Name "rg-contoso-dev" `
  -Location "australiaeast" `
  -Tag @{
    workload = "integration"
    environment = "dev"
  }
```

## Deploy Bicep

```powershell
New-AzResourceGroupDeployment `
  -ResourceGroupName "rg-contoso-dev" `
  -TemplateFile "infra/main.bicep" `
  -environmentName "dev"
```

## Run What-If

```powershell
New-AzResourceGroupDeployment `
  -ResourceGroupName "rg-contoso-dev" `
  -TemplateFile "infra/main.bicep" `
  -environmentName "dev" `
  -WhatIf
```

## Read Key Vault Secret

```powershell
$secret = Get-AzKeyVaultSecret `
  -VaultName "kv-contoso-dev" `
  -Name "ExternalApi--ClientId" `
  -AsPlainText
```

## Check Service Bus Subscription Counts

```powershell
Get-AzServiceBusSubscription `
  -ResourceGroupName "rg-contoso-dev" `
  -NamespaceName "sbns-contoso-dev" `
  -TopicName "topic-orders" `
  -Name "sub-order-worker" |
  Select-Object Name, CountDetails
```

## Get Function Managed Identity Principal ID

```powershell
$app = Get-AzWebApp `
  -ResourceGroupName "rg-contoso-dev" `
  -Name "func-contoso-dev"

$app.Identity.PrincipalId
```
