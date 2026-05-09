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

## Service Bus Sender

```csharp
using Azure.Identity;
using Azure.Messaging.ServiceBus;

await using var client = new ServiceBusClient(
    "sbns-cheatsheet-dev.servicebus.windows.net",
    new DefaultAzureCredential());

ServiceBusSender sender = client.CreateSender("topic-orders");

var message = new ServiceBusMessage(BinaryData.FromObjectAsJson(new
{
    OrderId = "SO-10042",
    Source = "Dataverse"
}))
{
    MessageId = "SO-10042",
    Subject = "OrderSubmitted",
    CorrelationId = "corr-123"
};

await sender.SendMessageAsync(message);
```

## Service Bus Receiver

```csharp
using Azure.Identity;
using Azure.Messaging.ServiceBus;

await using var client = new ServiceBusClient(
    "sbns-cheatsheet-dev.servicebus.windows.net",
    new DefaultAzureCredential());

ServiceBusProcessor processor = client.CreateProcessor(
    "topic-orders",
    "sub-worker",
    new ServiceBusProcessorOptions
    {
        AutoCompleteMessages = false,
        MaxConcurrentCalls = 5
    });

processor.ProcessMessageAsync += async args =>
{
    Console.WriteLine(args.Message.Body.ToString());
    await args.CompleteMessageAsync(args.Message);
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
using Microsoft.Azure.Functions.Worker;
using Microsoft.Azure.Functions.Worker.Http;

public class SubmitOrderFunction
{
    [Function("SubmitOrder")]
    public async Task<HttpResponseData> Run(
        [HttpTrigger(AuthorizationLevel.Function, "post")] HttpRequestData req)
    {
        using var reader = new StreamReader(req.Body);
        string body = await reader.ReadToEndAsync();

        HttpResponseData response = req.CreateResponse(HttpStatusCode.Accepted);
        await response.WriteStringAsync(body);
        return response;
    }
}
```

## Key Vault Secret Retrieval

```csharp
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

var secretClient = new SecretClient(
    new Uri("https://kv-cheatsheet-dev.vault.azure.net/"),
    new DefaultAzureCredential());

KeyVaultSecret secret = await secretClient.GetSecretAsync("ServiceBusConnection");
string value = secret.Value;
```
