# .NET Snippets

## Packages

```bash
dotnet add package Azure.Identity
dotnet add package Azure.Messaging.ServiceBus
dotnet add package Azure.Security.KeyVault.Secrets
dotnet add package Microsoft.Azure.Functions.Worker
dotnet add package Microsoft.Azure.Functions.Worker.Extensions.Http
dotnet add package Microsoft.Azure.Functions.Worker.Extensions.ServiceBus
```

## Service Bus Sender With Managed Identity

```csharp
using Azure.Identity;
using Azure.Messaging.ServiceBus;

await using var client = new ServiceBusClient(
    "sbns-contoso-prod.servicebus.windows.net",
    new DefaultAzureCredential());

ServiceBusSender sender = client.CreateSender("topic-orders");

var message = new ServiceBusMessage(BinaryData.FromObjectAsJson(new
{
    OrderId = "SO-10042",
    Source = "Dataverse",
    SubmittedOnUtc = DateTimeOffset.UtcNow
}))
{
    MessageId = "orders/SO-10042",
    Subject = "OrderSubmitted",
    CorrelationId = "corr-123"
};

message.ApplicationProperties["sourceSystem"] = "dataverse";
message.ApplicationProperties["schemaVersion"] = "1.0";

await sender.SendMessageAsync(message);
```

## Service Bus Receiver With Explicit Completion

```csharp
using Azure.Identity;
using Azure.Messaging.ServiceBus;

await using var client = new ServiceBusClient(
    "sbns-contoso-prod.servicebus.windows.net",
    new DefaultAzureCredential());

ServiceBusProcessor processor = client.CreateProcessor(
    "topic-orders",
    "sub-order-worker",
    new ServiceBusProcessorOptions
    {
        AutoCompleteMessages = false,
        MaxConcurrentCalls = 8
    });

processor.ProcessMessageAsync += async args =>
{
    try
    {
        string body = args.Message.Body.ToString();
        string correlationId = args.Message.CorrelationId;

        await ProcessAsync(body, correlationId);
        await args.CompleteMessageAsync(args.Message);
    }
    catch (InvalidOperationException ex)
    {
        await args.DeadLetterMessageAsync(
            args.Message,
            "ValidationFailed",
            ex.Message);
    }
};

processor.ProcessErrorAsync += args =>
{
    Console.Error.WriteLine(args.Exception);
    return Task.CompletedTask;
};

await processor.StartProcessingAsync();
```

## Function App HTTP Trigger

```csharp
using System.Net;
using Azure.Messaging.ServiceBus;
using Microsoft.Azure.Functions.Worker;
using Microsoft.Azure.Functions.Worker.Http;

public class SubmitOrderFunction
{
    private readonly ServiceBusSender _sender;

    public SubmitOrderFunction(ServiceBusSender sender)
    {
        _sender = sender;
    }

    [Function("SubmitOrder")]
    public async Task<HttpResponseData> Run(
        [HttpTrigger(AuthorizationLevel.Function, "post")] HttpRequestData req)
    {
        string correlationId = req.Headers.TryGetValues("x-correlation-id", out var values)
            ? values.First()
            : Guid.NewGuid().ToString("N");

        using var reader = new StreamReader(req.Body);
        string body = await reader.ReadToEndAsync();

        var message = new ServiceBusMessage(body)
        {
            MessageId = correlationId,
            CorrelationId = correlationId,
            Subject = "OrderSubmitted"
        };

        await _sender.SendMessageAsync(message);

        HttpResponseData response = req.CreateResponse(HttpStatusCode.Accepted);
        response.Headers.Add("x-correlation-id", correlationId);
        await response.WriteStringAsync("Accepted");
        return response;
    }
}
```

## Service Bus Trigger Function

```csharp
using Microsoft.Azure.Functions.Worker;
using Microsoft.Extensions.Logging;

public class ProcessOrderFunction
{
    private readonly ILogger<ProcessOrderFunction> _logger;

    public ProcessOrderFunction(ILogger<ProcessOrderFunction> logger)
    {
        _logger = logger;
    }

    [Function("ProcessOrder")]
    public async Task Run(
        [ServiceBusTrigger("orders", Connection = "ServiceBus")]
        string message,
        FunctionContext context)
    {
        _logger.LogInformation("Processing order message");
        await Task.CompletedTask;
    }
}
```

## Key Vault Secret Retrieval

```csharp
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

var secretClient = new SecretClient(
    new Uri("https://kv-contoso-prod.vault.azure.net/"),
    new DefaultAzureCredential());

KeyVaultSecret secret = await secretClient.GetSecretAsync("ExternalApi--ClientId");
string clientId = secret.Value;
```

## Dependency Injection For Service Bus

```csharp
using Azure.Identity;
using Azure.Messaging.ServiceBus;
using Microsoft.Extensions.Azure;
using Microsoft.Extensions.Hosting;

var host = new HostBuilder()
    .ConfigureFunctionsWorkerDefaults()
    .ConfigureServices(services =>
    {
        services.AddAzureClients(builder =>
        {
            builder.UseCredential(new DefaultAzureCredential());
            builder.AddServiceBusClientWithNamespace(
                "sbns-contoso-prod.servicebus.windows.net");
            builder.AddClient<ServiceBusSender, ServiceBusClientOptions>(
                (_, _, provider) =>
                {
                    var client = provider.GetRequiredService<ServiceBusClient>();
                    return client.CreateSender("topic-orders");
                });
        });
    })
    .Build();

host.Run();
```
