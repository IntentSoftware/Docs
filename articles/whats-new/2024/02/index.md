# What's new with Intent Architect (February 2024)

Welcome to the February 2024 edition of highlights of What's New with Intent Architect.

- Highlights
  - **[Smarter merging for C# files](#smarter-merging-for-c-files)** - Merge mode will now remove code which is no longer being generated by Intent. 
  - **[Model your Domain and persist it in a Redis Stack through Object Mapper](#model-your-domain-and-persist-it-in-a-redis-stack-through-object-mapper)** - Redis Stack is now available as a persistence option for Domain Modeling.
  - **[Send Integration Commands using MassTransit](#send-integration-commands-using-masstransit)** - Send a command to a specific recipient through a designated queue.
  - **[Model Request/Response interactions over message brokers with MassTransit](#model-requestresponse-interactions-over-message-brokers-with-masstransit)** - Use MassTransit to exchange commands/queries between applications, similar to HTTP calls, via a message broker.
  - **[ASP.NET Core Integration Tests module](#aspnet-core-integration-tests-module)** - Adds an integration testing project to your solution with support for containerized persistence.

- More updates
  - **[ASP.NET Core Integration Tests CRUD module](#aspnet-core-integration-tests-crud-module)** - Implements integration tests for CRUD style services.
  - **[Quartz.NET Scheduler](#quartznet-scheduler)** - Add scheduled jobs to your services design.
  - **[HTTP file upload / download support](#http-file-upload--download-support)** - Model ASP.NET Core file uploads / downloads in the `Services Designer`.
  - **[Windows Service Hosting](#windows-service-hosting)** - Application Template and patterns for modelling Windows Service applications.
  - **[Cosmos DB optimistic concurrency](#cosmos-db-optimistic-concurrency-through-etags)** - Our Cosmos DB patterns now implement optimistic concurrency through eTags.
  - **[DocumentDB Domain Designer key automation enhancement](#document-db-domain-designer-key-automation-enhancements)** - A new option to configure how the DocumentDB designer manages keys.
  - **[Oracle Database provider support for Entity Framework Core](#support-for-oracle-database-provider-for-entity-framework-core)** - Use Oracle Database with Entity Framework Core.
  - **[Visual Studio Solution Folder creation for static content template files](#visual-studio-solution-folder-creation-for-static-content-templates)** - Solution sub-folders will now be created for static content template files.

## Update details

### Smarter Merging for C# files

Previously, when `Merge` mode was used in C# files, the merging algorithm would add any content generated by the template which is missing in the existing file and no content would ever be removed.

As of Intent Architect 4.2, the Software Factory will now also store pre-code merging template output of all files allowing the code merging to intelligently know when content is no longer generated and should be removed.

This means now that for most templates, `Merge` mode is the ideal mode to operate in.

Available from:

- Intent Architect 4.2+ (presently in beta)
- Intent.OutputManager.RoslynWeaver 4.7.0 (currently in pre-release)

### Model your Domain and persist it in a Redis Stack through Object Mapper

![Redis Domain Modeling](images/redis-om-domain-modeling.png)

This module brings in a new `Document Database Provider`, `Redis OM`, allowing you to realize your Document DB paradigm Domain Models with a Redis Stack persistence layer. This module includes:

- Modeler customizations.
- Repositories using [Object Mapper](https://github.com/redis/redis-om-dotnet#readme).
- Unit of work pattern.

For more detail, refer to the [module documentation](https://github.com/IntentArchitect/Intent.Modules.NET/blob/development/Modules/Intent.Modules.Redis.Om.Repositories/README.md).

Available from:

- Intent.Redis.Om.Repositories 1.0.0

### Send Integration Commands using MassTransit

Send a command to a specific recipient through a designated queue.

![New Integration Command](images/new-integration-command.png)

For more detail, refer to the [module documentation](https://github.com/IntentArchitect/Intent.Modules.NET/blob/development/Modules/Intent.Modules.Eventing.MassTransit/README.md).

Available from:

- Intent.Eventing.MassTransit 6.0.2

### Model Request/Response interactions over message brokers with MassTransit

Use MassTransit to exchange commands/queries between applications, similar to HTTP calls, via a message broker.

![Request/Response proxies](images/sender-app-service-proxies-created.png)

For more detail, refer to the [module documentation](https://github.com/IntentArchitect/Intent.Modules.NET/blob/development/Modules/Intent.Modules.Eventing.MassTransit.RequestResponse/README.md).

Available from:

- Intent.Eventing.MassTransit.RequestResponse 1.0.0

### ASP.NET Core Integration Tests module

This module adds an xUnit testing project to your ASP.NET Core application with integrations tests which can be run to validate that your application is working end-to-end against containerized infrastructure, like databases, e.g. MS SQL Server, PostgreSQL or Cosmos DB Emulator. These tests do not replace Unit testing but rather compliment them ensuring the individually tested pieces work together correctly.

```csharp
[IntentManaged(Mode.Merge, Signature = Mode.Fully)]
[Collection("SharedContainer")]
public class MyCustomEndpointTests : BaseIntegrationTest
{
    public MyCustomEndpointTests(IntegrationTestWebAppFactory factory) : base(factory)
    {
    }

    [Fact]
    public async Task MyCustomEndpoint_ShouldDoX()
    {
        //Arrange
        var client = new MyCustomServiceHttpClient(CreateClient());

        //Act
        client.InvokeMyCustomEndpoint();

        //Assert
        ...
    }
}
```

See the [module documentation](https://github.com/IntentArchitect/Intent.Modules.NET/blob/master/Modules/Intent.Modules.AspNetCore.IntegrationTesting/README.md) for more details.

Available from:

- Intent.AspNetCore.IntegrationTesting 1.0.1

### ASP.NET Core Integration Tests CRUD module

This module extends the `Intent.AspNetCore.IntegrationTesting` module, offering test implementations for CRUD style services. It does this by consuming your `Service` Designer's `Service`s, `Command`s and `Query`s, generating an Integration test for each Endpoint.

For a service to be eligible is must implement at least:

- Create{Entity} - returning either the Primary Key or a DTO with a mapped Primary Key.
- Get{Entity}ById - returning an Entity based DTO taking a single parameter of the entity's primary key.

```csharp
[Fact]
public async Task CreateCustomer_ShouldCreateCustomer()
{
    //Arrange
    var client = new CustomersServiceHttpClient(CreateClient());

    var dataFactory = new TestDataFactory(WebAppFactory);
    await dataFactory.CreateCustomerDependencies();

    var command = dataFactory.CreateCommand<CustomerCreateDto>();

    //Act
    var customerId = await client.CreateCustomerAsync(command);

    //Assert
    var customer = await client.FindCustomerByIdAsync(customerId);
    Assert.NotNull(customer);
}
```

See the [module documentation](https://github.com/IntentArchitect/Intent.Modules.NET/blob/development/Modules/Intent.Modules.AspNetCore.IntegrationTests.CRUD/README.md) for more details.

Available from:

- Intent.AspNetCore.IntegrationTests.CRUD 1.0.1

### Quartz.NET Scheduler

This module allows you to model scheduled jobs in the Services Designer. These scheduled job are then realized using the [Quartz.NET Scheduler](https://www.quartz-scheduler.net/).

![Modeled scheduled jobs](images/modeled-scheduled-jobs.png)

See the [module documentation](https://github.com/IntentArchitect/Intent.Modules.NET/blob/master/Modules/Intent.Modules.QuartzScheduler/README.md) for more details.

Available from:

- Intent.QuartzScheduler 1.0.0

### HTTP file upload / download support

In `Service Designer` there are 2 new content menu options :

- `Expose as Http File Upload`, available on `Command`s and `Service Operation`s
- `Expose as Http File Download`, available on `Command`s, `Query`s and `Service Operation`s

![Content Menu](images/expose-file-transfer.png)

Leading to service endpoints as follows:

![Service Endpoint](images/file-transfer-endpoints.png)

These options will configure your service endpoint to Upload / Download a file.

Available from:

- Intent.AspNetCore.Controllers 6.0.3

### Windows Service Hosting

A new `Windows Service Host` Application Template is now available for designing Windows Service applications.

![Windows Service Application Template](images/windows-service-application-template.png)

Available from:

- Intent.WindowsServiceHost 1.0.0

### Cosmos DB optimistic concurrency through eTags

We have introduced [optimistic concurrency using eTags](https://learn.microsoft.com/azure/cosmos-db/nosql/database-transactions-optimistic-concurrency#optimistic-concurrency-control) into our Cosmos DB patterns, this pattern ensures documents which are read and written back, within a transaction, are not modified externally between the read and the write.

To this end we introduced a new setting to the Cosmos DB module:

- `Use Optimistic Concurrency`, this setting enables / disables optimistic concurrency through the use of eTags and is on by default.

![Cosmos DB Config](images/cosmosdb-setting-.png)

This is a behavioral change in the module, you can simply turn off this feature to have parity with what the module was doing previously.

Available from:

- Intent.CosmosDB 1.1.0

### Document DB Domain Designer key automation enhancements

The designers key automation is now configurable:

- `All` - The designer will manage keys on document / aggregate roots, as well as Compositional child collections.
- `Only on Documents` - The designer will only manage keys on documents / aggregate roots

`All` is how the designer previously worked and is the default, `Only on Documents` is a new option for teams who want to manage child keys more explicitly.

![Document DB Key Automation Options](images/documentdb-key-auitomation.png)

Available from:

- Intent.Metadata.DocumentDB 1.1.10

### Support for Oracle Database Provider for Entity Framework Core

"Oracle" is now an available option under "Database Provider" within "Database Settings" for which the correct configuration will now be generated.

Available from:

- Intent.EntityFrameworkCore 4.6.1

### Visual Studio Solution Folder creation for static content templates

When a static content template's output is set to a solution folder, it will now create corresponding solution sub-folders for sub-folders of the static content files.

Additionally, it is now possible to set an output folder prefix for the file system output path of the static content files.

For more information refer to the [](xref:module-building.templates-general.how-to-generate-static-files) article.

Available from:

- Intent.VisualStudio.Projects 3.6.0