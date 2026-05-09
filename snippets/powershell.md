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
  -Name "rg-cheatsheet-dev" `
  -Location "australiaeast"
```

## Deploy Bicep

```powershell
New-AzResourceGroupDeployment `
  -ResourceGroupName "rg-cheatsheet-dev" `
  -TemplateFile "infra/main.bicep" `
  -environmentName "dev"
```

## Read Key Vault Secret

```powershell
$secret = Get-AzKeyVaultSecret `
  -VaultName "kv-cheatsheet-dev" `
  -Name "ServiceBusConnection" `
  -AsPlainText
```

## Check Service Bus Dead-Letter Count

```powershell
Get-AzServiceBusSubscription `
  -ResourceGroupName "rg-cheatsheet-dev" `
  -NamespaceName "sbns-cheatsheet-dev" `
  -TopicName "topic-orders" `
  -Name "sub-worker" |
  Select-Object Name, CountDetails
```
