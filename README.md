# ASP&#46;NET Core and Docker Lab

## Goals

* Build a Web API without any behind-the-scenes help from Visual Studio
* Keep things simple
  * Rely on `dotnet` CLI tools - no build scripts
  * Understand what every file in your project is
  * Stick with the defaults until there's a reason not to
* Use Postman to interact with your API

### Bonus goal

* Put your app in a Docker container and interact with it some more
* Try a multi-stage Docker build

## Prerequisites

### Quick install with Chocolatey

```powershell
choco install dotnetcore-sdk vscode postman docker-for-windows
code --install-extension ms-vscode.csharp --install-extension peterjausovec.vscode-docker
```

### Required

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)
* [Visual Studio Code](https://code.visualstudio.com/), or your lightweight editor of choice
  * We will explicitly **not** be using Visual Studio
* [C# extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) - Recommended for a better C# editing experience
* [Postman](https://www.getpostman.com/) - To interact with the API you'll build

### Optional

* [Docker](https://www.docker.com/products/docker-desktop) - If you choose to do the bonus exercises at the end of the lab
* [Docker extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) - Recommended for a better Dockerfile editing experience

## Helpful links

* [.NET Core CLI documentation](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
* [Create a Web API with ASP.NET Core and Visual Studio Code](https://docs.microsoft.com/en-us/aspnet/core/tutorials/web-api-vsc?view=aspnetcore-2.1)
* [Dockerize a .NET Core application](https://docs.docker.com/engine/examples/dotnetcore/)

## Basic terms

* [.NET](https://www.microsoft.com/net/learn/what-is-dotnet) - Microsoft's set of standard libraries used to build software, widely used with C#.

* [.NET Framework](https://docs.microsoft.com/en-us/dotnet/framework/) - Microsoft's historical software framework. It provides a huge library of classes that you can use to easily build software. .NET Framework exists mostly as an all-in-one, all-or-nothing inclusion in your project - you can't pick and choose which components of it you want. It only natively supports Windows.

* [.NET Core](https://docs.microsoft.com/en-us/dotnet/core/) - Microsoft's modern software framework. It provides many of the features of .NET Framework, but is designed to be open-source, cross-platform, and modular.

* [.NET Standard](https://docs.microsoft.com/en-us/dotnet/standard/net-standard) - A set of APIs that all implementations of .NET share.

* [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/) - A framework that extends .NET Core with features to build web APIs and web apps.