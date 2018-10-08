# 3. Add and use a NuGet package

NuGet packages included in your project are specified in your `csproj` file. Instead of manually editing the list of packages and versions, you can [use the `dotnet` tool](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-add-package) to make the changes for you.

[Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is a package that automatically produces documentation for your API. Let's install that and take a look at the documentation site it produces.

## Exercise 3

* Use the `dotnet` tool to install [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/) version 3.0.0.
* Follow steps 1-5 of the [Swashbuckle Getting Started guide](https://github.com/domaindrivendev/Swashbuckle.AspNetCore#getting-started) to create automatic API documentation for your app.

## Verify

Open `WebAPI.csproj` and look for the following `PackageReference`.

```xml
<PackageReference Include="Swashbuckle.AspNetCore" Version="3.0.0" />
```

Open <https://localhost:5001/swagger/> in a web browser to see documentation for your API.
