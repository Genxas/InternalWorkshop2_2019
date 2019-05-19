# Internal Workshop 2/2019

## Guideline

1. Register your application with our Azure AD
```http://apps.dev.microsoft.com/```

2. Create a .NET Core Project with Azure AD Authentication
```dotnet new mvc --auth SingleOrg --tenant-id <TENANT-ID> --client-id <CLIENT ID or APP ID>```

3. Connect you application with databse
Install a NuGet package.
```Install-Package Microsoft.EntityFrameworkCore -Version 2.2.4```
or
```dotnet add package Microsoft.EntityFrameworkCore --version 2.2.4```

then `dotnet restore`

EF Core support wide rage of database providers, you can check it from `https://docs.microsoft.com/en-us/ef/core/providers/`

#### Get started
1. Create a Database Context Class in `\Data\ApplicationDbContext.cs`
```
public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    { }
}
```
2. In configuration services `Startup.cs` you have to add a DB Context
```
services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
```
3. You might think that ther's no configuration of the database server? Now, it's time!
Every created .NET Core project will have 2 application setting files.
    1. appSettings.json
    2. appSettings.Development.json

It will read only one file at a time based on your environment setting.
To check your environment, you have to run the commnad `dotnet run` and take a look at `Hosting environment: Development || Production`
Then add the following code in appSettings.<ENV>.json
```
"ConnectionStrings": {
    "DefaultConnection": "Server=tcp:<LOCAL DB SERVER>;Initial Catalog=Workshop_2019_Development;Persist Security Info=False;User ID=<USER ID>;Password=<PASSWORD>;"
}
```

4. Now it's a time to create your first simple model all the data model should locate in `Models\Data\...`. By this way, you can seperate a data model with other model.