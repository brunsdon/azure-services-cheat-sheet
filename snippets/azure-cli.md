# Azure CLI Snippets

## Login And Subscription

```bash
az login
az account set --subscription "<subscription-id-or-name>"
az account show --output table
```

## Create Resource Group

```bash
az group create \
  --name rg-cheatsheet-dev \
  --location australiaeast
```

## Create Storage Account

```bash
az storage account create \
  --name stcheatsheetdev \
  --resource-group rg-cheatsheet-dev \
  --location australiaeast \
  --sku Standard_LRS \
  --kind StorageV2 \
  --https-only true \
  --allow-blob-public-access false
```

## Create Service Bus Namespace, Topic, And Subscription

```bash
az servicebus namespace create \
  --name sbns-cheatsheet-dev \
  --resource-group rg-cheatsheet-dev \
  --location australiaeast \
  --sku Standard

az servicebus topic create \
  --name topic-orders \
  --namespace-name sbns-cheatsheet-dev \
  --resource-group rg-cheatsheet-dev \
  --enable-duplicate-detection true \
  --duplicate-detection-history-time-window PT10M

az servicebus topic subscription create \
  --name sub-worker \
  --topic-name topic-orders \
  --namespace-name sbns-cheatsheet-dev \
  --resource-group rg-cheatsheet-dev \
  --max-delivery-count 10
```

## Deploy Bicep

```bash
az deployment group create \
  --resource-group rg-cheatsheet-dev \
  --template-file infra/main.bicep \
  --parameters environmentName=dev
```

## Create Function App

```bash
az functionapp create \
  --name func-cheatsheet-dev \
  --resource-group rg-cheatsheet-dev \
  --storage-account stcheatsheetdev \
  --consumption-plan-location australiaeast \
  --runtime dotnet-isolated \
  --functions-version 4
```

## Create Key Vault

```bash
az keyvault create \
  --name kv-cheatsheet-dev \
  --resource-group rg-cheatsheet-dev \
  --location australiaeast \
  --enable-rbac-authorization true
```
