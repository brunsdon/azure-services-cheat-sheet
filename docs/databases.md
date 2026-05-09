# Databases

## What It Is

Azure database services store operational, analytical, relational, document, cache, and key-value
data.

| Service                       | Best fit                                                        |
| ----------------------------- | --------------------------------------------------------------- |
| Azure SQL Database            | Relational data, transactions, reporting-friendly schemas.      |
| Cosmos DB                     | Globally distributed NoSQL, low latency, partitioned workloads. |
| Azure Database for PostgreSQL | Open-source relational apps and PostgreSQL ecosystem.           |
| Azure Cache for Redis         | Low-latency cache, session state, distributed locks.            |
| Table Storage                 | Simple key-value access with low cost.                          |

## When To Use It

- Use Azure SQL for relational systems, transactional consistency, joins, and familiar SQL tooling.
- Use Cosmos DB when partitioning, latency, global distribution, and flexible schema are core
  requirements.
- Use PostgreSQL when your app expects PostgreSQL features or portability.
- Use Redis to cache expensive reads or share transient state.

## When Not To Use It

- Do not choose Cosmos DB to avoid schema design; partition design is critical.
- Do not put reporting-heavy relational workloads into document storage by default.
- Do not use Redis as the system of record.
- Do not use Table Storage for complex query patterns.

## Common Patterns

- API writes to SQL and publishes an outbox message.
- Cosmos DB change feed triggers downstream processors.
- Redis cache-aside for reference data and API responses.
- Blob Storage for large documents with database metadata.

## Common Gotchas

- Cosmos DB partition keys are hard to change later.
- SQL DTU/vCore sizing must consider peak workload and maintenance.
- Database firewall and private endpoint rules affect deployments.
- Redis eviction policy can remove data under memory pressure.
- Cross-region replication has consistency and cost trade-offs.

## Security Notes

- Use Managed Identity where supported.
- Prefer private endpoints for production databases.
- Enable auditing and threat protection where appropriate.
- Avoid putting secrets or tokens in application data tables.
- Encrypt sensitive fields at the application layer when policy requires it.

## Cost Considerations

- SQL cost depends on compute tier, storage, backup retention, and replicas.
- Cosmos DB cost is driven by RU/s or serverless request units, storage, regions, and indexing.
- Redis cost depends heavily on tier and memory size.
- Over-indexing increases write cost and storage.

## Examples

```bash
az sql db create \
  --resource-group rg-cheatsheet-dev \
  --server sql-cheatsheet-dev \
  --name sqldb-cheatsheet-dev \
  --service-objective GP_Gen5_2
```

## Official Docs

- [Azure SQL documentation](https://learn.microsoft.com/azure/azure-sql/)
- [Azure Cosmos DB documentation](https://learn.microsoft.com/azure/cosmos-db/)
- [Azure Database for PostgreSQL documentation](https://learn.microsoft.com/azure/postgresql/)
- [Azure Cache for Redis documentation](https://learn.microsoft.com/azure/azure-cache-for-redis/)
