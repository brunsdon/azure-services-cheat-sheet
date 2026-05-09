# Azure CLI Snippets

Production examples should be parameterized in scripts or pipelines. These snippets are
intentionally small so the command shape is easy to reuse.

## Login And Subscription

```bash
az login
az account set --subscription "<subscription-id-or-name>"
az account show --output table
```

## Create Resource Group

```bash
az group create \
  --name rg-contoso-dev \
  --location australiaeast \
  --tags workload=integration environment=dev owner=platform
```

## Create Storage Account

```bash
az storage account create \
  --name stcontosodev001 \
  --resource-group rg-contoso-dev \
  --location australiaeast \
  --sku Standard_LRS \
  --kind StorageV2 \
  --https-only true \
  --allow-blob-public-access false \
  --min-tls-version TLS1_2
```

## Create Blob Container

```bash
az storage container create \
  --name inbound-documents \
  --account-name stcontosodev001 \
  --auth-mode login \
  --public-access off
```

## Create Service Bus Namespace, Topic, And Subscription

```bash
az servicebus namespace create \
  --name sbns-contoso-dev \
  --resource-group rg-contoso-dev \
  --location australiaeast \
  --sku Standard

az servicebus topic create \
  --name topic-orders \
  --namespace-name sbns-contoso-dev \
  --resource-group rg-contoso-dev \
  --enable-duplicate-detection true \
  --duplicate-detection-history-time-window PT10M

az servicebus topic subscription create \
  --name sub-order-worker \
  --topic-name topic-orders \
  --namespace-name sbns-contoso-dev \
  --resource-group rg-contoso-dev \
  --max-delivery-count 10 \
  --dead-letter-on-filter-exceptions true
```

## Create Function App

```bash
az functionapp create \
  --name func-contoso-dev \
  --resource-group rg-contoso-dev \
  --storage-account stcontosodev001 \
  --consumption-plan-location australiaeast \
  --runtime dotnet-isolated \
  --functions-version 4
```

## Enable Managed Identity

```bash
principal_id=$(az functionapp identity assign \
  --name func-contoso-dev \
  --resource-group rg-contoso-dev \
  --query principalId \
  --output tsv)

echo "$principal_id"
```

## Assign Service Bus Sender Role

```bash
scope=$(az servicebus namespace show \
  --name sbns-contoso-dev \
  --resource-group rg-contoso-dev \
  --query id \
  --output tsv)

az role assignment create \
  --assignee "$principal_id" \
  --role "Azure Service Bus Data Sender" \
  --scope "$scope"
```

## Create Key Vault

```bash
az keyvault create \
  --name kv-contoso-dev \
  --resource-group rg-contoso-dev \
  --location australiaeast \
  --enable-rbac-authorization true
```

## Deploy Bicep

```bash
az deployment group create \
  --resource-group rg-contoso-dev \
  --template-file infra/main.bicep \
  --parameters environmentName=dev
```

## Run What-If

```bash
az deployment group what-if \
  --resource-group rg-contoso-dev \
  --template-file infra/main.bicep \
  --parameters environmentName=dev
```
