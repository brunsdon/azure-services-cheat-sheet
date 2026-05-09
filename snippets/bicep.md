# Bicep Snippets

## Resource Group Scope

```bicep
targetScope = 'resourceGroup'

param location string = resourceGroup().location
param environmentName string
param workloadName string = 'contoso'

var suffix = '${workloadName}-${environmentName}'
```

## Storage Account

```bicep
resource storage 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'st${workloadName}${environmentName}001'
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    supportsHttpsTrafficOnly: true
    allowBlobPublicAccess: false
    minimumTlsVersion: 'TLS1_2'
    allowSharedKeyAccess: false
  }
}

resource inboundContainer 'Microsoft.Storage/storageAccounts/blobServices/containers@2023-05-01' = {
  name: '${storage.name}/default/inbound-documents'
  properties: {
    publicAccess: 'None'
  }
}
```

## Service Bus Topic And Subscription

```bicep
resource serviceBus 'Microsoft.ServiceBus/namespaces@2024-01-01' = {
  name: 'sbns-${suffix}'
  location: location
  sku: {
    name: 'Standard'
    tier: 'Standard'
  }
  properties: {
    minimumTlsVersion: '1.2'
    publicNetworkAccess: 'Enabled'
  }
}

resource topic 'Microsoft.ServiceBus/namespaces/topics@2024-01-01' = {
  parent: serviceBus
  name: 'topic-orders'
  properties: {
    requiresDuplicateDetection: true
    duplicateDetectionHistoryTimeWindow: 'PT10M'
    defaultMessageTimeToLive: 'P14D'
  }
}

resource subscription 'Microsoft.ServiceBus/namespaces/topics/subscriptions@2024-01-01' = {
  parent: topic
  name: 'sub-order-worker'
  properties: {
    maxDeliveryCount: 10
    lockDuration: 'PT1M'
    deadLetteringOnMessageExpiration: true
  }
}
```

## Function App With Managed Identity

```bicep
resource plan 'Microsoft.Web/serverfarms@2023-12-01' = {
  name: 'asp-${suffix}'
  location: location
  sku: {
    name: 'Y1'
    tier: 'Dynamic'
  }
}

resource functionApp 'Microsoft.Web/sites@2023-12-01' = {
  name: 'func-${suffix}'
  location: location
  kind: 'functionapp'
  identity: {
    type: 'SystemAssigned'
  }
  properties: {
    serverFarmId: plan.id
    httpsOnly: true
    siteConfig: {
      minTlsVersion: '1.2'
      ftpsState: 'Disabled'
      appSettings: [
        {
          name: 'FUNCTIONS_EXTENSION_VERSION'
          value: '~4'
        }
        {
          name: 'FUNCTIONS_WORKER_RUNTIME'
          value: 'dotnet-isolated'
        }
        {
          name: 'ServiceBus__fullyQualifiedNamespace'
          value: '${serviceBus.name}.servicebus.windows.net'
        }
      ]
    }
  }
}
```

## Key Vault With RBAC

```bicep
resource keyVault 'Microsoft.KeyVault/vaults@2023-07-01' = {
  name: 'kv-${suffix}'
  location: location
  properties: {
    tenantId: tenant().tenantId
    sku: {
      family: 'A'
      name: 'standard'
    }
    enableRbacAuthorization: true
    enableSoftDelete: true
    publicNetworkAccess: 'Enabled'
  }
}
```

## Assign Service Bus Receiver Role

```bicep
resource receiverRole 'Microsoft.Authorization/roleAssignments@2022-04-01' = {
  name: guid(serviceBus.id, functionApp.id, 'receiver')
  scope: serviceBus
  properties: {
    principalId: functionApp.identity.principalId
    principalType: 'ServicePrincipal'
    roleDefinitionId: subscriptionResourceId(
      'Microsoft.Authorization/roleDefinitions',
      '4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0'
    )
  }
}
```

## Diagnostic Settings To Log Analytics

```bicep
resource workspace 'Microsoft.OperationalInsights/workspaces@2023-09-01' existing = {
  name: 'law-${suffix}'
}

resource serviceBusDiagnostics 'Microsoft.Insights/diagnosticSettings@2021-05-01-preview' = {
  name: 'diag-servicebus'
  scope: serviceBus
  properties: {
    workspaceId: workspace.id
    logs: [
      {
        category: 'OperationalLogs'
        enabled: true
      }
    ]
    metrics: [
      {
        category: 'AllMetrics'
        enabled: true
      }
    ]
  }
}
```
