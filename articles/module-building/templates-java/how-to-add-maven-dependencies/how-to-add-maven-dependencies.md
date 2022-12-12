---
uid: module-building.templates-java.how-to-add-nuget-dependencies-csharp
---
# How to add Maven dependencies

Often source code generated by Templates will require that certain Maven dependencies are added to a project's `pom.xml` file. Templates allow you to define such dependencies by using the `AddDependency` method.

First, create a static class (if not already created) that will contain the `JavaDependency`s. By having them defined in a central class, multiple Templates can make use of the same dependencies and the versions can later be updated in a central location.

```csharp
using Intent.Modules.Common.Java.Templates;

namespace MyIntentModule;

public static class JavaDependencies
{
    public static JavaDependency SpringBootStarter = new JavaDependency("org.springframework.boot", "spring-boot-starter");
}
```

Then in your Template you can use the `AddDependency` method:

```csharp
[IntentManaged(Mode.Merge, Signature = Mode.Fully)]
partial class MyTemplate : JavaTemplateBase<object>
{
    [IntentManaged(Mode.Fully)]
    public const string TemplateId = "MyIntentModule.MyTemplate";

    [IntentManaged(Mode.Merge, Signature = Mode.Fully)]
    public MyTemplate(IOutputTarget outputTarget, object model = null) : base(TemplateId, outputTarget, model)
    {
        AddDependency(JavaDependencies.SpringBootStarter);
    }
...
```

> [!IMPORTANT]
> The `AddDependency` method call must be invoked in either the `Constructor` or in the overridden `BeforeTemplateExecution` method or the updates may not be applied to the `pom.xml`.

Once the Template executes in the Software Factory, it will introduce a `<dependency />` in the `pom.xml` file:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
```

## Additional options

### Version

To specify that a particular version of the dependency is used, use an overload which has a `version` parameter:

```csharp
new JavaDependency("org.springframework.boot", "spring-boot-starter", "2.7.5");

// Or using a named argument:
new JavaDependency("org.springframework.boot", "spring-boot-starter", version: "2.7.5");
```

Which will generate the following:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.7.5</version>
</dependency>
```

### Optional

To specify that a dependency is optional, use an overload which has an `optional` parameter and pass it the value `true`:

```csharp
new JavaDependency("org.springframework.boot", "spring-boot-starter", null, true);

// Or using a named argument:
new JavaDependency("org.springframework.boot", "spring-boot-starter", optional: true);
```

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <optional>true</optional>
</dependency>
```