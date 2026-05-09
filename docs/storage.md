# Storage

## What It Is

Azure Storage provides durable cloud storage for files, documents, blobs,
queues, tables, and analytics workloads.

| Service           | Best fit                                                                 |
| ----------------- | ------------------------------------------------------------------------ |
| Blob Storage      | Documents, images, exports, payloads, backups, claim-check bodies.       |
| Data Lake Storage | Analytics zones, hierarchical namespace, Spark/Synapse/Fabric workloads. |
| Azure Files       | SMB/NFS file shares and lift-and-shift file dependencies.                |
| Queue Storage     | Simple queueing when Service Bus features are not needed.                |
| Table Storage     | Simple key-value storage with low operational overhead.                  |

## When To Use It

- Store large payloads outside messages using Blob Storage.
- Use Data Lake Storage for analytics-oriented storage zones.
- Use lifecycle management for archive/cool tier transitions.
- Use private containers by default and grant access through managed identity or
  short-lived SAS.

## When Not To Use It

- Do not put sensitive files in public containers.
- Do not use Blob Storage as a relational database.
- Do not use Storage Queues when you need sessions, duplicate detection,
  transactions, or topics.
- Do not use SAS tokens as long-lived shared credentials.

## Common Patterns

- Claim check: send metadata on Service Bus, store large body in Blob Storage.
- Document ingestion: upload blob, trigger Function, validate, then publish
  message.
- Export/archive: write daily files to blob and move to cool/archive tier.
- Landing/raw/curated zones in Data Lake.

## Common Gotchas

- Blob triggers can be slower and less predictable than Event Grid triggers.
- Archive tier data must be rehydrated before access.
- Hot partitions can throttle high-volume workloads.
- Soft delete and versioning protect data but can increase cost.
- SAS expiry and clock skew can cause intermittent access failures.

## Security Notes

- Disable public blob access unless explicitly required.
- Use Azure RBAC with Managed Identity.
- Prefer private endpoints for enterprise workloads.
- Enable soft delete, versioning, and Defender for Storage where appropriate.
- Use customer-managed keys only when policy requires them.

## Cost Considerations

- Access tier matters: hot is cheap to read, archive is cheap to store.
- Transactions, reads, writes, and data egress can be material.
- Versioning and soft delete retain extra data.
- Data Lake hierarchical namespace changes some behaviors and pricing.

## Examples

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

```bicep
resource storage 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: 'stcheatsheetdev'
  location: resourceGroup().location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    supportsHttpsTrafficOnly: true
    allowBlobPublicAccess: false
    minimumTlsVersion: 'TLS1_2'
  }
}
```

## Official Docs

- [Azure Blob Storage documentation](https://learn.microsoft.com/azure/storage/blobs/)
- [Azure Data Lake Storage][data-lake-docs]
- [Azure Files documentation](https://learn.microsoft.com/azure/storage/files/)

[data-lake-docs]: https://learn.microsoft.com/azure/storage/blobs/data-lake-storage-introduction
