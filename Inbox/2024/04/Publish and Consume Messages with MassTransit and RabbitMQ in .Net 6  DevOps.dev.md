---
created: 2024-03-30T01:27:19 (UTC -03:00)
tags: []
source: https://blog.devops.dev/mastering-distributed-messaging-with-net-6-and-masstransit-building-a-robust-product-api-b914befc65d1
author: Waqas Ahmed
---

# Publish and Consume Messages with MassTransit and RabbitMQ in .Net 6 | DevOps.dev

> ## Excerpt
> MassTransit is an open-source distributed application framework for building message-based systems in the .NET ecosystem. It provides a high-level abstraction for sending and receiving messages…

---
## Mastering Distributed Messaging with .NET 6 and MassTransit: Building a Robust Product API

[

![Waqas Ahmed](https://miro.medium.com/v2/da:true/resize:fill:88:88/0*h5MvjXv4IaG7JWpW)



](https://waqasahmeddev.medium.com/?source=post_page-----b914befc65d1--------------------------------)[

![DevOps.dev](https://miro.medium.com/v2/resize:fill:48:48/1*3SZtM9yhQyfh-dfWk5gGFw.jpeg)



](https://blog.devops.dev/?source=post_page-----b914befc65d1--------------------------------)

![Building Real-Time Product Services with .NET 6 and MassTransit: A Complete Guide](https://miro.medium.com/v2/resize:fit:875/1*ph9s2onO4dYCFOTqFM5unw.png)

## What is MassTransit?

MassTransit is an open-source distributed application framework for building message-based systems in the .NET ecosystem. It provides a high-level abstraction for sending and receiving messages between components, allowing developers to build decoupled, scalable, and maintainable applications.

MassTransit facilitates communication between different components of an application using a messaging infrastructure, typically based on message queues. It supports various message transport technologies, including popular ones like RabbitMQ, ActiveMQ, Azure Service Bus, and Amazon Simple Queue Service (SQS).

By leveraging the messaging pattern, MassTransit enables loose coupling between different parts of a system. It promotes the use of messages as the primary means of communication, rather than direct method invocations or shared databases. This approach enhances scalability, fault tolerance, and maintainability.

Key features of MassTransit include:

1.  Message-based communication: MassTransit encourages the use of messages as the fundamental unit of communication between components. Messages are strongly typed and define the data exchanged between different parts of the system.
2.  Publish/Subscribe model: MassTransit supports a publish/subscribe pattern, allowing components to publish messages to a specific topic or event. Subscribed components receive relevant messages without requiring direct knowledge of the publishers.
3.  Request/Response model: MassTransit facilitates the request/response pattern, where one component sends a request message and another component responds with a corresponding response message. This pattern enables synchronous communication between components.
4.  Saga/state machine support: MassTransit provides a saga/state machine framework, which helps manage long-running, multi-step processes in distributed systems. Sagas allow you to coordinate actions across multiple services while maintaining data consistency and reliability.
5.  Fault tolerance and retries: MassTransit includes built-in mechanisms for handling message failures and performing retries. It provides configurable retry policies, dead-letter queues, and error handling strategies to ensure reliable message processing.
6.  Extensibility and pluggability: MassTransit is designed to be highly extensible and allows developers to integrate custom transports, serializers, and middleware into the messaging pipeline. This flexibility enables customization and integration with various technologies.

## Step 1:

Create a new ASP.NET Core Web API project in Visual Studio or your preferred development environment.

## Step 2:

Install the necessary NuGet packages. In the NuGet Package Manager Console, run the following commands:

```
<span id="b54c" data-selectable-paragraph="">Install-Package Microsoft.AspNetCore.Mvc -Version 6.0.0<br>Install-Package MassTransit -Version 7.1.12<br>Install-Package MassTransit.AspNetCore -Version 7.1.12<br>Install-Package MassTransit.RabbitMQ -Version 7.1.12</span>
```

## Step 3:

**Create a new folder called “Messages” in the project. Inside the “Messages” folder, create a class called “ProductCreatedMessage.cs” with the following content:**

```
<span id="649c" data-selectable-paragraph=""><span>public</span> <span>class</span> <span>ProductCreatedMessage</span><br>{<br>    <span>public</span> <span>int</span> ProductId { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>string</span> Name { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>decimal</span> Price { <span>get</span>; <span>set</span>; }<br>}</span>
```

## Step 4:

Open the Startup.cs file and add the necessary code for configuring DI and MassTransit. Modify the ConfigureServices method as follows:

```
<span id="2cb9" data-selectable-paragraph=""><span>using</span> MassTransit;<br><br><span><span>public</span> <span>void</span> <span>ConfigureServices</span>(<span>IServiceCollection services</span>)</span><br>{<br>    services.AddControllers();<br><br>    services.AddMassTransit(x =&gt;<br>    {<br>        x.UsingRabbitMq((context, cfg) =&gt;<br>        {<br>            cfg.Host(<span>"rabbitmq://localhost"</span>); <br><br>            cfg.ConfigureEndpoints(context);<br>        });<br>    });<br><br>    services.AddMassTransitHostedService();<br>}</span>
```

## Step 5:

Create a new folder called “Controllers” in the project. Inside the “Controllers” folder, create a class called “ProductController.cs” with the following content:

```
<span id="fb55" data-selectable-paragraph=""><span>using</span> System;<br><span>using</span> System.Threading.Tasks;<br><span>using</span> MassTransit;<br><span>using</span> Microsoft.AspNetCore.Mvc;<br><br>[<span>ApiController</span>]<br>[<span>Route(<span>"[controller]"</span>)</span>]<br><span>public</span> <span>class</span> <span>ProductController</span> : <span>ControllerBase</span><br>{<br>    <span>private</span> <span>readonly</span> IPublishEndpoint _publishEndpoint;<br><br>    <span><span>public</span> <span>ProductController</span>(<span>IPublishEndpoint publishEndpoint</span>)</span><br>    {<br>        _publishEndpoint = publishEndpoint;<br>    }<br><br>    [<span>HttpPost</span>]<br>    <span><span>public</span> <span>async</span> Task&lt;IActionResult&gt; <span>Create</span>(<span>ProductCreateRequest request</span>)</span><br>    {<br>        <br><br>        <br>        <span>await</span> _publishEndpoint.Publish(<span>new</span> ProductCreatedMessage<br>        {<br>            ProductId = <span>1</span>, <br>            Name = request.Name,<br>            Price = request.Price<br>        });<br><br>        <span>return</span> Ok();<br>    }<br>}</span>
```

```
<span id="5cce" data-selectable-paragraph=""><span>public</span> <span>class</span> <span>ProductCreateRequest</span><br>{<br>    <span>public</span> <span>string</span> Name { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>decimal</span> Price { <span>get</span>; <span>set</span>; }<br>}</span>
```

## Step 6:

Modify the HomeController.cs file to remove the default API template code.

## Step 7:

Build and run the application. You can use a tool like Postman to send a POST request to `http://localhost:<port>/product` with the following JSON payload:

```
<span id="548c" data-selectable-paragraph=""><span>{</span><br>    <span>"name"</span><span>:</span> <span>"Example Product"</span><span>,</span><br>    <span>"price"</span><span>:</span> <span>9.99</span><br><span>}</span></span>
```

## Step 8:

To consume the published messages, you can create a consumer. Create a new folder called “Consumers” in the project. Inside the “Consumers” folder, create a class called “ProductCreatedConsumer.cs” with the following content:

```
<span id="f682" data-selectable-paragraph=""><span>using</span> System.Threading.Tasks;<br><span>using</span> MassTransit;<br><span>using</span> Microsoft.Extensions.Logging;<br><br><span>public</span> <span>class</span> <span>ProductCreatedConsumer</span> : <span>IConsumer</span>&lt;<span>ProductCreatedMessage</span>&gt;<br>{<br>    <span>private</span> <span>readonly</span> ILogger&lt;ProductCreatedConsumer&gt; _logger;<br><br>    <span><span>public</span> <span>ProductCreatedConsumer</span>(<span>ILogger&lt;ProductCreatedConsumer&gt; logger</span>)</span><br>    {<br>        _logger = logger;<br>    }<br><br>    <span><span>public</span> <span>async</span> Task <span>Consume</span>(<span>ConsumeContext&lt;ProductCreatedMessage&gt; context</span>)</span><br>    {<br>        <br>        _logger.LogInformation(<span>$"Product created - ID: <span>{context.Message.ProductId}</span>, Name: <span>{context.Message.Name}</span>, Price: <span>{context.Message.Price}</span>"</span>);<br><br>        <span>await</span> Task.CompletedTask;<br>    }<br>}</span>
```

## Step 9:

Register the consumer in the ConfigureServices method of the Startup.cs file:

```
<span id="c155" data-selectable-paragraph=""><span>using</span> MassTransit;<br><span>using</span> Microsoft.Extensions.DependencyInjection;<br><br><span><span>public</span> <span>void</span> <span>ConfigureServices</span><span>(IServiceCollection services)</span><br></span>{<br>    <br><br>    services.<span>AddMassTransit</span>(x =&gt;<br>    {<br>        <br><br>        x.<span>AddConsumer</span>&lt;ProductCreatedConsumer&gt;();<br>    });<br><br>    services.<span>AddScoped</span>&lt;ProductCreatedConsumer&gt;();<br><br>    <br>}</span>
```

## Step 10:

Modify the ConfigureServices method to include the consumer endpoint configuration:

```
<span id="f563" data-selectable-paragraph=""><span>using</span> MassTransit;<br><br><span><span>public</span> <span>void</span> <span>ConfigureServices</span>(<span>IServiceCollection services</span>)</span><br>{<br>    <br><br>    services.AddMassTransit(x =&gt;<br>    {<br>        x.UsingRabbitMq((context, cfg) =&gt;<br>        {<br>            <br><br>            cfg.ConfigureEndpoints(context);<br>        });<br>    });<br><br>    services.AddMassTransitHostedService();<br><br>    <br>}</span>
```

Define the message types that will be exchanged between the publisher and subscribers. For example, let’s create a simple event message called `OrderCompletedEvent`:

```
<span id="25da" data-selectable-paragraph=""><span>public</span> <span>class</span> <span>OrderCompletedEvent</span><br>{<br>    <span>public</span> <span>int</span> OrderId { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>string</span> CustomerName { <span>get</span>; <span>set</span>; }<br>}</span>
```

Set up MassTransit and configure the message endpoints. In your `Program.cs` file, add the following code:

```
<span id="a5aa" data-selectable-paragraph=""><span>using</span> MassTransit;<br><br><span>var</span> bus = Bus.Factory.CreateUsingInMemory(configure =&gt;<br>{<br>    configure.ReceiveEndpoint(<span>"order_completed_queue"</span>, endpoint =&gt;<br>    {<br>        endpoint.Handler&lt;OrderCompletedEvent&gt;(context =&gt;<br>        {<br>            <span>var</span> orderCompletedEvent = context.Message;<br>            Console.WriteLine(<span>$"Order completed: OrderId=<span>{orderCompletedEvent.OrderId}</span>, CustomerName=<span>{orderCompletedEvent.CustomerName}</span>"</span>);<br>            <span>return</span> Task.CompletedTask;<br>        });<br>    });<br>});<br><br><span>await</span> bus.StartAsync();<br><br>Console.WriteLine(<span>"Publishing events. Press any key to exit."</span>);<br>Console.ReadKey();<br><br><span>await</span> bus.StopAsync();</span>
```

In the above code, we create an in-memory bus using `CreateUsingInMemory` and configure a receive endpoint called "order\_completed\_queue" that handles messages of type `OrderCompletedEvent`. Inside the handler, we simply output the received event to the console.

```
<span id="5f84" data-selectable-paragraph=""><span>var</span> orderCompletedEvent = <span>new</span> OrderCompletedEvent<br>{<br>    OrderId = <span>1</span>,<br>    CustomerName = <span>"John Doe"</span><br>};<br><br><span>await</span> bus.Publish(orderCompletedEvent);</span>
```

**Error Handling:** To implement error handling in MassTransit, you can configure retry policies and error queues. Here’s an example of configuring a retry policy with exponential backoff and moving failed messages to an error queue:

```
<span id="385d" data-selectable-paragraph=""><span>services</span><span>.AddMassTransit</span>(x =&gt;<br>{<br>    <span>x</span><span>.UsingRabbitMq</span>((context, cfg) =&gt;<br>    {<br>        <br><br>        <span>cfg</span><span>.UseMessageRetry</span>(r =&gt;<br>        {<br>            <span>r</span><span>.Exponential</span>(<span>5</span>, TimeSpan.<span>FromSeconds</span>(<span>1</span>), TimeSpan.<span>FromSeconds</span>(<span>10</span>), TimeSpan.<span>FromSeconds</span>(<span>2</span>));<br>        });<br><br>        <span>cfg</span><span>.UseInMemoryOutbox</span>(); <br><br>        <span>cfg</span><span>.ConfigureEndpoints</span>(context);<br><br>        <span>cfg</span><span>.UseScheduledRedelivery</span>(r =&gt;<br>        {<br>            <span>r</span><span>.Intervals</span>(TimeSpan.<span>FromSeconds</span>(<span>5</span>), TimeSpan.<span>FromSeconds</span>(<span>10</span>), TimeSpan.<span>FromSeconds</span>(<span>30</span>));<br>        });<br>    });<br>});</span>
```

**Message Serialization:** To customize the message serialization format, you can use different serializers. Here’s an example of configuring MassTransit to use Protobuf serialization:

```
<span id="8399" data-selectable-paragraph="">services.AddMassTransit(<span>x =&gt;</span><br>{<br>    x.UsingRabbitM<span>q((context, cfg)</span> =&gt;<br>    {<br>        <span>//</span> ...<br><br>        cfg.UseMessageDataSerialization();<br><br>        cfg.UseJsonSerializer();<br><br>        <span>//</span> <span>or</span><br><br>        cfg.UseProtoBufSerializer();<br>        <br>        <span>//</span> ...<br>    });<br>});</span>
```

**Message Versioning:** To handle message versioning, you can include versioning information in the message contract and handle different versions in the consumer. Here’s an example:

```
<span id="e3c8" data-selectable-paragraph=""><br><span>public</span> <span>class</span> <span>ProductCreatedMessageV1</span><br>{<br>    <span>public</span> <span>int</span> ProductId { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>string</span> Name { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>decimal</span> Price { <span>get</span>; <span>set</span>; }<br>}<br><br><br><span>public</span> <span>class</span> <span>ProductCreatedMessageV2</span><br>{<br>    <span>public</span> <span>int</span> ProductId { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>string</span> Name { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>decimal</span> Price { <span>get</span>; <span>set</span>; }<br>    <span>public</span> DateTime CreatedAt { <span>get</span>; <span>set</span>; }<br>}<br><br><br><span>public</span> <span>class</span> <span>ProductCreatedConsumer</span> : <span>IConsumer</span>&lt;<span>ProductCreatedMessageV1</span>&gt;, <span>IConsumer</span>&lt;<span>ProductCreatedMessageV2</span>&gt;<br>{<br>    <span><span>public</span> Task <span>Consume</span>(<span>ConsumeContext&lt;ProductCreatedMessageV1&gt; context</span>)</span><br>    {<br>        <br>        <span>return</span> Task.CompletedTask;<br>    }<br><br>    <span><span>public</span> Task <span>Consume</span>(<span>ConsumeContext&lt;ProductCreatedMessageV2&gt; context</span>)</span><br>    {<br>        <br>        <span>return</span> Task.CompletedTask;<br>    }<br>}</span>
```

**Message Validation:** To validate incoming messages, you can use a validation library like FluentValidation. Here’s an example of using FluentValidation for message validation:

```
<span id="279f" data-selectable-paragraph=""><br><span>public</span> <span>class</span> <span>ProductCreatedMessage</span><br>{<br>    <span>public</span> <span>int</span> ProductId { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>string</span> Name { <span>get</span>; <span>set</span>; }<br>    <span>public</span> <span>decimal</span> Price { <span>get</span>; <span>set</span>; }<br>}<br><br><br><span>public</span> <span>class</span> <span>ProductCreatedMessageValidator</span> : <span>AbstractValidator</span>&lt;<span>ProductCreatedMessage</span>&gt;<br>{<br>    <span><span>public</span> <span>ProductCreatedMessageValidator</span>()</span><br>    {<br>        RuleFor(x =&gt; x.ProductId).NotEmpty();<br>        RuleFor(x =&gt; x.Name).NotEmpty().Length(<span>1</span>, <span>100</span>);<br>        RuleFor(x =&gt; x.Price).GreaterThan(<span>0</span>);<br>    }<br>}<br><br><br><span>public</span> <span>class</span> <span>ProductCreatedConsumer</span> : <span>IConsumer</span>&lt;<span>ProductCreatedMessage</span>&gt;<br>{<br>    <span><span>public</span> <span>async</span> Task <span>Consume</span>(<span>ConsumeContext&lt;ProductCreatedMessage&gt; context</span>)</span><br>    {<br>        <span>var</span> validationResult = <span>await</span> <span>new</span> ProductCreatedMessageValidator().ValidateAsync(context.Message);<br>        <span>if</span> (!validationResult.IsValid)<br>        {<br>            <br>            <span>return</span>;<br>        }<br><br>        <br>    }<br>}</span>
```

**Logging and Monitoring:** To enhance logging and monitoring capabilities in your application, you can integrate a logging framework like Serilog. Here’s an example of configuring Serilog for logging MassTransit messages:

```
<span id="10cd" data-selectable-paragraph=""><span>using</span> Serilog;<br><span>using</span> Serilog.Events;<br><span>using</span> MassTransit;<br><span>using</span> MassTransit.SerilogIntegration;<br><br><br>services.AddMassTransit(x =&gt;<br>{<br>    x.UsingRabbitMq((context, cfg) =&gt;<br>    {<br>        <br><br>        cfg.ConfigureEndpoints(context);<br><br>        cfg.UseSerilog();<br><br>        <br>    });<br>});</span>
```

Make sure you’ve set up Serilog properly in your application and have configured it to write log events to your desired logging sink.

**Security:** To implement security measures for your messaging infrastructure, you can enable transport-level security and authentication. Here’s an example of configuring TLS encryption and setting a username/password for RabbitMQ:

```
<span id="9416" data-selectable-paragraph="">services.AddMassTransit(x =&gt;<br>{<br>    x.UsingRabbitMq((context, cfg) =&gt;<br>    {<br>        cfg.Host(<span>"rabbitmq://localhost"</span>, host =&gt;<br>        {<br>            host.Username(<span>"guest"</span>);<br>            host.Password(<span>"guest"</span>);<br>            host.UseSsl(ssl =&gt;<br>            {<br>                ssl.Protocol = System.Security.Authentication.SslProtocols.Tls12;<br>            });<br>        });<br><br>        cfg.ConfigureEndpoints(context);<br><br>        <br>    });<br>});</span>
```

Make sure to replace the username and password with your actual RabbitMQ credentials.

**Scalability and Performance:** To optimize scalability and performance, you can choose a different transport technology based on your requirements. Here’s an example of using Azure Service Bus as the transport:

```
<span id="250c" data-selectable-paragraph=""><span>using</span> MassTransit.Azure.ServiceBus.Core;<br><br>services.AddMassTransit(x =&gt;<br>{<br>    x.UsingAzureServiceBus((context, cfg) =&gt;<br>    {<br>        cfg.Host(<span>"your_connection_string"</span>);<br><br>        cfg.ConfigureEndpoints(context);<br><br>        <br>    });<br>});</span>
```

Replace “your\_connection\_string” with the actual connection string for your Azure Service Bus instance.

**Unit Testing and Integration Testing:** For unit testing and integration testing, you can use frameworks like xUnit or NUnit. Here’s an example of writing a unit test for a consumer using xUnit:

```
<span id="c0eb" data-selectable-paragraph=""><span>using</span> Xunit;<br><span>using</span> Moq;<br><span>using</span> MassTransit;<br><span>using</span> Microsoft.Extensions.Logging;<br><br><span>public</span> <span>class</span> <span>ProductCreatedConsumerTests</span><br>{<br>    [<span>Fact</span>]<br>    <span><span>public</span> <span>async</span> Task <span>Consume_ProductCreatedMessage_LoggerCalled</span>()</span><br>    {<br>        <br>        <span>var</span> loggerMock = <span>new</span> Mock&lt;ILogger&lt;ProductCreatedConsumer&gt;&gt;();<br>        <span>var</span> consumer = <span>new</span> ProductCreatedConsumer(loggerMock.Object);<br>        <span>var</span> contextMock = <span>new</span> Mock&lt;ConsumeContext&lt;ProductCreatedMessage&gt;&gt;();<br>        <span>var</span> message = <span>new</span> ProductCreatedMessage { ProductId = <span>1</span>, Name = <span>"Test"</span>, Price = <span>9.99</span>m };<br>        contextMock.Setup(x =&gt; x.Message).Returns(message);<br><br>        <br>        <span>await</span> consumer.Consume(contextMock.Object);<br><br>        <br>        loggerMock.Verify(<br>            x =&gt; x.LogInformation(<br>                <span>$"Product created - ID: <span>{message.ProductId}</span>, Name: <span>{message.Name}</span>, Price: <span>{message.Price}</span>"</span><br>            ),<br>            Times.Once<br>        );<br>    }<br>}</span>
```

In this example, we’re using the Moq library to create a mock logger and mock consume context, allowing us to test the behavior of the consumer.

Remember to adjust and adapt these code examples according to your specific needs and testing frameworks.
