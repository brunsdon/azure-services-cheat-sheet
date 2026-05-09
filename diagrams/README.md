# Diagrams

This folder holds architecture diagrams used by the cheat sheet. Mermaid is preferred because it renders directly in GitHub markdown and stays reviewable in pull requests.

## Enterprise Integration

```mermaid
flowchart LR
    ext[External App] --> apim[API Management]
    apim --> api[Azure Function / App Service]
    api --> sb[Service Bus]
    sb --> worker[Worker]
    worker --> sql[(Azure SQL / Cosmos DB)]
    worker --> blob[(Blob Storage)]
    api --> insights[Application Insights]
    worker --> insights
```

## Claim Check

```mermaid
flowchart LR
    producer[Producer] --> blob[(Blob Storage)]
    producer --> bus[Service Bus Message]
    bus --> consumer[Consumer]
    consumer --> blob
```

## Dataverse Integration

```mermaid
flowchart LR
    d365[Dynamics 365 / Dataverse] --> plugin[Plugin / Power Automate]
    plugin --> topic[Service Bus Topic]
    topic --> fn[Azure Function]
    fn --> lob[Line-of-Business API]
    fn --> appi[Application Insights]
```
