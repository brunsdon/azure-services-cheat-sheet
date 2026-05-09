# Diagrams

Mermaid diagrams used by the Azure Services Cheat Sheet.

The diagrams stay in Markdown so they render directly in GitHub, remain easy to review in pull
requests, and can evolve with the written guidance.

## API Management To Functions To Service Bus To SQL

```mermaid
flowchart LR
    client[Client or Partner] --> apim[API Management]
    apim --> function[HTTP Azure Function]
    function --> bus[Service Bus Queue]
    bus --> worker[Worker Function]
    worker --> sql[(Azure SQL)]
    function --> insights[Application Insights]
    worker --> insights
```

## Dataverse Integration Via Service Bus

```mermaid
flowchart LR
    dataverse[Dataverse / Dynamics 365] --> publisher[Plugin or Power Automate]
    publisher --> topic[Service Bus Topic]
    topic --> subscription[Filtered Subscription]
    subscription --> worker[Azure Function Worker]
    worker --> api[External API or ERP]
    worker --> store[(SQL or Blob Storage)]
    worker --> insights[Application Insights]
```

## Blob Claim-Check Pattern

```mermaid
flowchart LR
    producer[Producer] --> blob[(Blob Storage)]
    producer --> message[Service Bus Message with Blob Reference]
    message --> consumer[Consumer]
    consumer --> blob
```

## DMZ-Safe Document Upload Architecture

```mermaid
flowchart LR
    external[External Party] --> upload[Secure Upload API or Portal]
    upload --> landing[(Blob Landing Container)]
    landing --> event[Event Grid]
    event --> validate[Validate and Scan Function]
    validate --> clean[(Clean Blob Container)]
    validate --> queue[Service Bus Queue]
    queue --> worker[Internal Worker]
    worker --> internal[Dataverse / ERP / SQL]
```

## Monitoring And Distributed Tracing Flow

```mermaid
sequenceDiagram
    participant Client
    participant APIM as API Management
    participant API as Function API
    participant Bus as Service Bus
    participant Worker
    participant AI as Application Insights

    Client->>APIM: Request with correlation ID
    APIM->>API: Forward request
    API->>AI: Request telemetry
    API->>Bus: Message with CorrelationId
    Bus->>Worker: Deliver message
    Worker->>AI: Dependency, trace, and exception telemetry
```

## Event-Driven Microservice Architecture

```mermaid
flowchart LR
    orderApi[Order API] --> topic[Service Bus Topic]
    topic --> billingSub[Billing Subscription]
    topic --> crmSub[CRM Sync Subscription]
    topic --> reportingSub[Reporting Subscription]
    billingSub --> billing[Billing Service]
    crmSub --> crm[Dataverse Sync Worker]
    reportingSub --> reporting[Reporting Worker]
    billing --> appi[Application Insights]
    crm --> appi
    reporting --> appi
```

## Low-Cost Azure App Architecture

```mermaid
flowchart LR
    user[Users] --> app[App Service]
    app --> sql[(Azure SQL)]
    app --> blob[(Blob Storage)]
    timer[Timer Function] --> sql
    app --> keyvault[Key Vault]
    app --> insights[Application Insights]
    timer --> insights
```

## Enterprise Secure Integration Architecture

```mermaid
flowchart LR
    consumer[Consumer / Partner] --> waf[Front Door or App Gateway WAF]
    waf --> apim[API Management]
    apim --> api[Private API Backend]
    api --> bus[Service Bus Premium]
    bus --> worker[Worker Service]
    worker --> data[(SQL / Cosmos / Blob)]
    api --> keyvault[Key Vault]
    worker --> monitor[App Insights / Log Analytics]
```
