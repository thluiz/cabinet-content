---
created: 2024-09-09T10:25:14 (UTC -03:00)
tags: []
source: https://github.com/Giorgi/EntityFramework.Exceptions
author: 
---

# Giorgi/EntityFramework.Exceptions: Handle database errors easily when working with Entity Framework Core. Supports SQLServer, PostgreSQL, SQLite, Oracle and MySql.

> ## Excerpt
> Handle database errors easily when working with Entity Framework Core. Supports SQLServer, PostgreSQL, SQLite, Oracle and  MySql. - Giorgi/EntityFramework.Exceptions

---
[![EntityFramework.Exceptions](https://raw.githubusercontent.com/Giorgi/EntityFramework.Exceptions/main/Icon.png "EntityFramework.Exceptions")](https://raw.githubusercontent.com/Giorgi/EntityFramework.Exceptions/main/Icon.png)

## EntityFramework.Exceptions

Handle database errors easily when working with Entity Framework Core. Supports SQLServer, PostgreSQL, SQLite, Oracle and MySql

[![License](https://camo.githubusercontent.com/0cc5972c6fd68d116a2c18af407098c376aaaf933d95ec273e2566e8128d98b5/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4c6963656e73652d417061636865253230322e302d626c75652e7376673f7374796c653d666c61742d737175617265266c6f676f3d417061636865)](https://github.com/Giorgi/EntityFramework.Exceptions/blob/main/License.md) [![Target](https://camo.githubusercontent.com/c91c499536d8b2db59d039fe1e28523ce0fc4de6a5b660b9542c7902c5c3f2b5/68747470733a2f2f696d672e736869656c64732e696f2f7374617469632f76313f6c6162656c3d746172676574266d6573736167653d6e6574362e303b6e6574382e3026636f6c6f723d353132626434266c6f676f3d2e6e6574267374796c653d666c61742d737175617265)](https://dotnet.microsoft.com/en-us/) [![GitHub Actions Workflow Status](https://camo.githubusercontent.com/dde46b04ff4be49c26a07c9251e577828aabeee6a15b30556b39f2417fc8cd1e/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f616374696f6e732f776f726b666c6f772f7374617475732f47696f7267692f456e746974794672616d65776f726b2e457863657074696f6e732f646f746e65742e796d6c)](https://camo.githubusercontent.com/dde46b04ff4be49c26a07c9251e577828aabeee6a15b30556b39f2417fc8cd1e/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f616374696f6e732f776f726b666c6f772f7374617475732f47696f7267692f456e746974794672616d65776f726b2e457863657074696f6e732f646f746e65742e796d6c) [![Coverage Status](https://camo.githubusercontent.com/073515e1d48baa2890e56ed5fa1ec9cafbfb871cec240172129618ff33cda1f7/68747470733a2f2f696d672e736869656c64732e696f2f636f766572616c6c732f6769746875622f47696f7267692f456e746974794672616d65776f726b2e457863657074696f6e733f6c6f676f3d636f766572616c6c73267374796c653d666c61742d737175617265)](https://coveralls.io/github/Giorgi/EntityFramework.Exceptions) [![Ko-Fi](https://camo.githubusercontent.com/2c55b863c7c82493ef0e260d10f6f8709ec7fe9639f08c6b964e7b47af8a24b6/68747470733a2f2f696d672e736869656c64732e696f2f7374617469632f76313f7374796c653d666c61742d737175617265266d6573736167653d537570706f727425323074686525323050726f6a65637426636f6c6f723d73756363657373267374796c653d706c6173746963266c6f676f3d6b6f2d6669266c6162656c3d2424)](https://ko-fi.com/U6U81LHU8)

[![Maintainability Rating](https://camo.githubusercontent.com/78a57e70f19b6adcca64e9b8cabaf75707883bf873b07642970913582f4bed1e/68747470733a2f2f736f6e6172636c6f75642e696f2f6170692f70726f6a6563745f6261646765732f6d6561737572653f70726f6a6563743d456e746974794672616d65776f726b2e457863657074696f6e73266d65747269633d7371616c655f726174696e67)](https://sonarcloud.io/dashboard?id=EntityFramework.Exceptions) [![Vulnerabilities](https://camo.githubusercontent.com/4342732ca99f8878774cb86af4041b02c992cd859857175c448c4b50fb181a87/68747470733a2f2f736f6e6172636c6f75642e696f2f6170692f70726f6a6563745f6261646765732f6d6561737572653f70726f6a6563743d456e746974794672616d65776f726b2e457863657074696f6e73266d65747269633d76756c6e65726162696c6974696573)](https://sonarcloud.io/dashboard?id=EntityFramework.Exceptions) [![Bugs](https://camo.githubusercontent.com/581078ee328fde80d38df7af37c1447fe67bf5eeddeda9a05e6a45f92aac28cf/68747470733a2f2f736f6e6172636c6f75642e696f2f6170692f70726f6a6563745f6261646765732f6d6561737572653f70726f6a6563743d456e746974794672616d65776f726b2e457863657074696f6e73266d65747269633d62756773)](https://sonarcloud.io/dashboard?id=EntityFramework.Exceptions) [![Code Smells](https://camo.githubusercontent.com/00991a24138df64a98795ff5b838f066136a107b2a71a6ff7866842a45fd5ac2/68747470733a2f2f736f6e6172636c6f75642e696f2f6170692f70726f6a6563745f6261646765732f6d6561737572653f70726f6a6563743d456e746974794672616d65776f726b2e457863657074696f6e73266d65747269633d636f64655f736d656c6c73)](https://sonarcloud.io/dashboard?id=EntityFramework.Exceptions) [![Duplicated Lines (%)](https://camo.githubusercontent.com/ae91db7c14f2b273339ae45068ac0613d67ece1241e1fb2fdceb40d265bd0081/68747470733a2f2f736f6e6172636c6f75642e696f2f6170692f70726f6a6563745f6261646765732f6d6561737572653f70726f6a6563743d456e746974794672616d65776f726b2e457863657074696f6e73266d65747269633d6475706c6963617465645f6c696e65735f64656e73697479)](https://sonarcloud.io/dashboard?id=EntityFramework.Exceptions) [![Coverage](https://camo.githubusercontent.com/d4d29523ec2359a3cb8bd16eae815b270fb7c6bf8a2ea469cdadcaf95efee7da/68747470733a2f2f736f6e6172636c6f75642e696f2f6170692f70726f6a6563745f6261646765732f6d6561737572653f70726f6a6563743d456e746974794672616d65776f726b2e457863657074696f6e73266d65747269633d636f766572616765)](https://sonarcloud.io/dashboard?id=EntityFramework.Exceptions)

[![](https://camo.githubusercontent.com/5c2e827f369d6dcc21ea1b696db00de5ccbe30e202bee86c0d5b7b93fd398cff/68747470733a2f2f696d672e736869656c64732e696f2f6e756765742f64742f456e746974794672616d65776f726b436f72652e457863657074696f6e732e53716c5365727665722e7376673f6c6162656c3d456e746974794672616d65776f726b436f72652e457863657074696f6e732e53716c536572766572267374796c653d666c61742d737175617265266c6f676f3d4d6963726f736f66742d53716c2d536572766572)](https://www.nuget.org/packages/EntityFrameworkCore.Exceptions.SqlServer/) [![](https://camo.githubusercontent.com/949509c4e97420e3f59aa8f6cecbfbeb29486ac940d75ee79784f3428fdfddbe/68747470733a2f2f696d672e736869656c64732e696f2f6e756765742f64742f456e746974794672616d65776f726b436f72652e457863657074696f6e732e506f737467726553514c2e7376673f6c6162656c3d456e746974794672616d65776f726b436f72652e457863657074696f6e732e506f737467726553514c267374796c653d666c61742d737175617265266c6f676f3d506f737467726553514c)](https://www.nuget.org/packages/EntityFrameworkCore.Exceptions.PostgreSQL/) [![](https://camo.githubusercontent.com/c57934597171581b88d330fcf722ae7ccb795e02003c233a54055f2cd8a2a663/68747470733a2f2f696d672e736869656c64732e696f2f6e756765742f64742f456e746974794672616d65776f726b436f72652e457863657074696f6e732e4d7953514c2e7376673f6c6162656c3d456e746974794672616d65776f726b436f72652e457863657074696f6e732e4d7953514c267374796c653d666c61742d737175617265266c6f676f3d4d7953514c266c6f676f436f6c6f723d7768697465)](https://www.nuget.org/packages/EntityFrameworkCore.Exceptions.MySQL/) [![](https://camo.githubusercontent.com/1d3a1ed5e7d2ebfb1c207a4c012d88415aa3c0199319ce88b5f21f6bddafc1e4/68747470733a2f2f696d672e736869656c64732e696f2f6e756765742f64742f456e746974794672616d65776f726b436f72652e457863657074696f6e732e4d7953514c2e506f6d656c6f2e7376673f6c6162656c3d456e746974794672616d65776f726b436f72652e457863657074696f6e732e4d7953514c2e506f6d656c6f267374796c653d666c61742d737175617265266c6f676f3d4d7953514c266c6f676f436f6c6f723d7768697465)](https://www.nuget.org/packages/EntityFrameworkCore.Exceptions.MySQL.Pomelo/) [![](https://camo.githubusercontent.com/78ec43aef3954fc93e76147c2bf9f779909c1568665456446bc532ab35a4088b/68747470733a2f2f696d672e736869656c64732e696f2f6e756765742f64742f456e746974794672616d65776f726b436f72652e457863657074696f6e732e53716c6974652e7376673f6c6162656c3d456e746974794672616d65776f726b436f72652e457863657074696f6e732e53716c697465267374796c653d666c61742d737175617265266c6f676f3d53716c697465)](https://www.nuget.org/packages/EntityFrameworkCore.Exceptions.Sqlite/) [![](https://camo.githubusercontent.com/33062ce0adb196d1cbf8b751beb058d6513550854f7856a33504ab1856031d9b/68747470733a2f2f696d672e736869656c64732e696f2f6e756765742f64742f456e746974794672616d65776f726b436f72652e457863657074696f6e732e4f7261636c652e7376673f6c6162656c3d456e746974794672616d65776f726b436f72652e457863657074696f6e732e4f7261636c65267374796c653d666c61742d737175617265266c6f676f3d4f7261636c65)](https://www.nuget.org/packages/EntityFrameworkCore.Exceptions.Oracle/)

### Entity Framework Community Standup Live Show

[![Entity Framework Community Standup - Typed Exceptions for Entity Framework Core](https://camo.githubusercontent.com/b7a2345501d2ba55a887b826cfbdfc9b2c64b465d08d118b1abe4b9be27aa421/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f61556c35516673774e55342f302e6a7067)](https://www.youtube.com/watch?v=aUl5QfswNU4)

### Nick Chapsas - The Smart Way to Handle EF Core Exceptions

[![Nick Chapsas - The Smart Way to Handle EF Core Exceptions](https://camo.githubusercontent.com/62049d5988f802cb6a48301563791fa38f54853d9dd0fcd396bf6555be62e2cc/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f514b775a6c577666682d6f2f302e6a7067)](https://www.youtube.com/watch?v=QKwZlWvfh-o)

### What does EntityFramework.Exceptions do?

When using Entity Framework Core for data access all database exceptions are wrapped in `DbUpdateException`. If you need to find whether the exception was caused by a unique constraint, value being too long or value missing for a required column you need to dig into the concrete `DbException` subclass instance and check the error code to determine the exact cause.

EntityFramework.Exceptions simplifies this by handling all the database specific details and throwing different exceptions. All you have to do is to configure `DbContext` by calling `UseExceptionProcessor` and handle the exception(s) you need, such as `UniqueConstraintException`, `CannotInsertNullException`, `MaxLengthExceededException`, `NumericOverflowException`, or `ReferenceConstraintException`.

In case of `UniqueConstraintException` and `ReferenceConstraintException` you can get the name of the associated constraint with **`ConstraintName`** property. The **`ConstraintProperties`** will contain the properties that are part of the constraint.

Warning

`ConstraintName` and `ConstraintProperties` will be populated only if the index is defined in the Entity Framework Model. These properties will not be populated if the index exists in the database but isn't part of the model or if the index is added with [MigrationBuilder.Sql](https://learn.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.migrations.migrationbuilder.sql?view=efcore-8.0) method.

Warning

`ConstraintName` and `ConstraintProperties` will not be populated when using SQLite.

All these exceptions inherit from `DbUpdateException` for backwards compatibility.

### How do I get started?

First, install the package corresponding to your database:

```
dotnet add package EntityFrameworkCore.Exceptions.SqlServer
```

```
dotnet add package EntityFrameworkCore.Exceptions.MySql
```

```
dotnet add package EntityFrameworkCore.Exceptions.MySql.Pomelo
```

```
dotnet add package EntityFrameworkCore.Exceptions.PostgreSQL
```

```
dotnet add package EntityFrameworkCore.Exceptions.Sqlite
```

```
dotnet add package EntityFrameworkCore.Exceptions.Oracle
```

Next, in your DbContext `OnConfiguring` method call `UseExceptionProcessor` extension method:

```cs
class DemoContext : DbContext
{
    public DbSet<Product> Products { get; set; }
    public DbSet<ProductSale> ProductSale { get; set; }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        builder.Entity<Product>().HasIndex(u => u.Name).IsUnique();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseExceptionProcessor();
    }
}
```

You will now start getting different exception for different database error. For example, when a unique constraints fails you will get `UniqueConstraintException` exception:

```cs
using (var demoContext = new DemoContext())
{
    demoContext.Products.Add(new Product
    {
        Name = "demo",
        Price = 10
    });

    demoContext.Products.Add(new Product
    {
        Name = "demo",
        Price = 100
    });

    try
    {
        demoContext.SaveChanges();
    }
    catch (UniqueConstraintException e)
    {
        //Handle exception here
        Console.WriteLine($"Unique constraint {e.ConstraintName} violated. Duplicate value for {e.ConstraintProperties[0]}");
    }
}
```

### Usage with DbContext pooling

Instead of calling `UseExceptionProcessor` in the `OnConfiguring` method, add it where you add your `DbContextPool`:

```cs
// Replace UseNpgsql with the sql flavor you're using
builder.Services.AddDbContextPool<DemoContext>(options => options
    .UseNpgsql(config.GetConnectionString("DemoConnection"))
    .UseExceptionProcessor());
```
