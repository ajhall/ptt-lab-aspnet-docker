# Getting started

## Environment prep

Make sure you have installed all the software from the **Prerequisites** section.

Pick an empty folder that you can use as your work area. For example, you might want to use `C:\Dev\Labs\WebAPI\`.

Open a command line (I recommend PowerShell) and change to your work folder. Keep your command line handy, as you'll be using it throughout the lab.

```powershell
cd C:\Dev\Labs\WebAPI\
```

## Try the `dotnet` tool

Try out the `dotnet` command-line tool. Type `dotnet` and press <kbd>ENTER</kbd> to see a short summary of its options.

```powershell
> dotnet

Usage: dotnet [options]
Usage: dotnet [path-to-application]

Options:
  -h|--help         Display help.
  --info            Display .NET Core information.
  --list-sdks       Display the installed SDKs.
  --list-runtimes   Display the installed runtimes

path-to-application:
  The path to an application .dll file to execute.
```

Try running `dotnet --help` to see a list of everything the tool can do. For this lab, we'll focus on the **Execute a .NET Core SDK command** section.

```powershell
> dotnet --help

...

Usage: dotnet [sdk-options] [command] [command-options] [arguments]

Execute a .NET Core SDK command.

...

SDK commands:
  add               Add a package or reference to a .NET project.
  ...
  help              Show command line help.
  ...
  new               Create a new .NET project or file.
  ...
  publish           Publish a .NET project for deployment.
  run               Build and run a .NET project output.
  ...

Run 'dotnet [command] --help' for more information on a command.
```

## Create a new project

You're starting a new project, so the `dotnet new` command is what you want. Use `dotnet new --help` to see how to use it and a list of all the templates available.

You can also use `dotnet help new` to open the [help page for `dotnet new`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new?tabs=netcore21) with more thorough documentation.

In the documentation, you'll see that you need to run `dotnet new <TEMPLATE>` to generate a new project. Let's make a web API with the ASP&#46;NET Core Web API template (`webapi`). Make sure you're in your work folder and then run `dotnet new webapi`.

```powershell
> dotnet new webapi
The template "ASP.NET Core Web API" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on C:\Dev\Labs\WebAPI\WebAPI.csproj...
  Restoring packages for C:\Dev\Labs\WebAPI\WebAPI.csproj...
  Generating MSBuild file C:\Dev\Labs\WebAPI\obj\WebAPI.csproj.nuget.g.props.
  Generating MSBuild file C:\Dev\Labs\WebAPI\obj\WebAPI.csproj.nuget.g.targets.
  Restore completed in 1.07 sec for C:\Dev\Labs\WebAPI\WebAPI.csproj.

Restore succeeded.
```

This command created a basic project named after the folder you created it in.