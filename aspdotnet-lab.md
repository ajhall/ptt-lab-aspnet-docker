# ASP.NET Core and Docker Lab

## Goals

* Build a simple Web API without any behind-the-scenes help from Visual Studio
* Keep things simple
  * Rely on `dotnet` CLI tools - no build scripts
  * Understand what every file in your project is
  * Stick with the defaults until there's a reason not to
* Make a simple API and use Postman to interact with it

### Bonus goal

* Put your app in a Docker container and interact with it some more
* Try a multi-stage Docker build

## Prerequisites

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

## Getting started

### Environment prep

Make sure you have installed all the software from the **Prerequisites** section above.

Pick an empty folder that you can use as your work area. For example, you might want to use `C:\Dev\Labs\WebAPI\`.

Open a command line (I recommend PowerShell) and change to your work folder. Keep your command line handy, as you'll be using it throughout the lab.

```powershell
cd C:\Dev\Labs\WebAPI\
```

### Try the `dotnet` tool

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
  pack              Create a NuGet package.
  publish           Publish a .NET project for deployment.
  run               Build and run a .NET project output.
  ...

Run 'dotnet [command] --help' for more information on a command.
```

### Create a new project

You're starting a new project, so the `dotnet new` command is what you want. Use `dotnet new --help` to see how to use it and a list of all the templates available.

You can also use `dotnet help new` to open the [help page for `dotnet new`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new?tabs=netcore21) with more thorough documentation.

In the documentation, you'll see that you need to run `dotnet new <TEMPLATE>` to generate a new project. Let's make a web api with the ASP.NET Core Web API template (`webapi`). Make sure you're in your work folder and then run `dotnet new webapi`.

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

### Look around

Open your work folder in Visual Studio Code to see your whole project. Before you open a new window to find your project, though, try this command-line trick.

You can open VS Code with the `code` command, which takes a file or folder path as its first argument. On the command line, you can specify the current folder by just typing a dot (`.`). That means you can run `code .` to quickly open your project in VS Code.

```powershell
code .
```

Look around in your project to see the files that were created by this template.

#### What are these files?

Your folder structure should look something like this.

```text
│   appsettings.Development.json
│   appsettings.json
│   Program.cs
│   Startup.cs
│   WebAPI.csproj
│
├───Controllers
│       ValuesController.cs
│
├───obj
│       project.assets.json
│       WebAPI.csproj.nuget.cache
│       WebAPI.csproj.nuget.g.props
│       WebAPI.csproj.nuget.g.targets
│
├───Properties
│       launchSettings.json
│
└───wwwroot
```

##### `appsettings.json` and `appsettings.Development.json`

Configuration files that your app can read from. This lets you change your configuration easily without relying on code changes or the Windows registry.

ASP.NET Core provides [classes to read from these files automatically](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1#json-configuration-provider). You can provide multiple configuration files named as `appsettings.<Environment>.json` to load a specific one based on your current.

The `<Environment>` value is read from the [`ASPNETCORE_ENVIRONMENT` environment variable](https://andrewlock.net/how-to-set-the-hosting-environment-in-asp-net-core/). If you don't specify your environment explicitly, it will default to "Production".

##### `Program.cs`

The entry point to your app. It starts the ASP.NET Core web server and tells it to use your `Startup` class for configuration.

You can configure your web host in detail if you choose to, but the [defaults provided by CreateDefaultBuilder](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/host/web-host?view=aspnetcore-2.1#set-up-a-host) provide a solid set of best practices.

It may look like there's not much going on in `Program.cs`, but this single line of code applies the default configuration and hides quite a bit of complexity.

```csharp
WebHost.CreateDefaultBuilder(args)
```

##### `Startup.cs`

Contains two methods (`Configure` and `ConfigureServices`) that get called automatically by ASP.NET Core when your app starts up. These configure your app's built-in web server.

##### `WebAPI.csproj`

Defines metadata about your project and how the project gets compiled and published.

##### `Controllers/ValuesController.cs`

A basic shell of an [ASP.NET Model-View-Controller (MVC) Controller](https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs). Once your app is started up, you'll be able to trigger these routes by going to the path based on the controller name and function name.

The `[Route("api/[controller]")]` attribute at the top of the class specifies that the route to this controller is `/api/Values`, so you can trigger these routes by accessing `/api/Values`, `/api/Values/1`, and so on.

##### `obj/*`

These are automatically generated by the `dotnet` tool. You don't need to edit these, and they can be added to your `.gitignore` file so they won't be included in source control.

##### `Properties/launchSettings.json`

This sets up different launch configurations for your app. By default, `dotnet run` will launch your application using the first profile it finds in this file that has `"commandName": "Project"` specified.

##### `wwwroot`

This folder holds any static web content you might need to serve with your app. We won't be using it in this lab.

### Try the template app

The template app is fully usable out of the box. Try it out!

Go back to the command line and use `dotnet run` to start the project.

```powershell
> dotnet run
Using launch settings from C:\Dev\Labs\WebAPI\Properties\launchSettings.json...
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\Andy\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
Hosting environment: Development
Content root path: C:\Dev\Labs\WebAPI
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

You can see in the output that the app started up locally at the addresses <https://localhost:5001> (HTTPS) and <http://localhost:5000> (HTTP, but it automatically redirects to the HTTPS address). These are the base URLs for your app.

#### The controller

ASP.NET Core can use the class name of a controller to help construct a URL. Take a look at `ValuesController.cs`.

```csharp
[Route("api/[controller]")]
[ApiController]
public class ValuesController : ControllerBase
```

The `Route` attribute specifies the URL you can use to access this controller. The `[controller]` segment of the URL says to make use of the name of the class itself. For example, `ValuesController` is accessible at `/api/Values`.

Now you need to combine what you know about your app's base URL and your controller's route. Try navigating to <https://localhost:5001/api/values> in your web browser. Your browser might warn you about a self-signed security certificate - that's OK for local testing, so add an exception in your web browser.

#### Controller actions

At <https://localhost:5001/api/values>, you should see `["value1","value2"]`. This comes straight from the `Get()` method in `ValuesController.cs`.

The methods in `ValuesController` have associated [attributes](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-2.1#attribute-routing-with-httpverb-attributes) such as `[HttpGet]`, `[HttpPost]`, and `[HttpDelete]` that limit the HTTP verbs that are allowed to trigger them. Navigating to an action in a web browser triggers an HTTP GET request.

#### Using Postman

TODO

Self-signed SSL certificates are being blocked:
Fix this by turning off 'SSL certificate verification' in Settings > General

Try making a GET request to <https://localhost:5001/api/values/1>. You should see the word `value` as a response, which is hard-coded in the `Get(int id)` method. Let's change this to echo back the input you send it.

#### Make some changes

Instead of returning the hard-coded string `"value"` from `Get(int id)`, change it to return the number you passed in.

When you're done changing the code, go back to the command line. Your app should still be running here, with some logs shown based on your interactions with it. Press <kbd>CTRL</kbd>+<kbd>C</kbd> to stop your app, then press the up arrow to find `dotnet run` in your command-line history. Start your app up, then try a GET request to <https://localhost:5001/api/values/1> again. You should see `1` as a result now, and it should change to any number you enter as a parameter to `/values/{id}`.

You're probably thinking that each time you make a code change you'll have to go back to the command line, stop your app, and start it back up again. *There Has To Be A Better Way!*, you say. How did you know?!

### .NET Core file watcher

Press <kbd>CTRL</kbd>+<kbd>C</kbd> to stop your app again. Then, run `dotnet watch --help` to learn about the `dotnet watch` command.

Run `dotnet watch run` to start your app again in watch mode. Go make another code change and keep an eye on the console window where your app is running. You'll see your app automatically exit and start up with your new code changes in place.

### Logging

ASP.NET Core provides [built-in dependency injection](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection). This isn't set up to be fully automatic like it is in our UX framework, though. To use this with your own classes, you'll need to add each class to the `services` collection in the `ConfigureServices` class of `Startup.cs`. Don't worry about that for now, though.

`ILogger` is a special case because it's provided by default by the ASP.NET Core framework. To get a logger object, just request one in the parameters of a controller's constructor.

```csharp
public class MyController : ControllerBase
{
  private readonly ILogger<MyController> _logger;

  public MyController(ILogger<MathController> logger)
  {
    _logger = logger;
  }

  [HttpGet]
  public ActionResult<IEnumerable<string>> Get()
  {
    _logger.LogWarning("Warning!");
    return "Got";
  }
}
```

You can use this pattern to get a `_logger` object to help you write contextual info from your app as you work through the exercises.

## Exercises

### 1. Add a new controller

The following are the [essential pieces](https://docs.microsoft.com/en-us/aspnet/core/web-api/) you'll need to create a controller:

* [Inherit from `ControllerBase`](https://docs.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-2.1#derive-class-from-controllerbase) - This provides the properties and methods that ASP.NET Core needs to treat the class as a controller.
* [`[ApiController]` attribute](https://docs.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-2.1#annotate-class-with-apicontrollerattribute) - This isn't strictly necessary, but it provides common functionality to automatically make your app act like a web app.
* [`[Route]` attributes](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-2.1#attribute-routing) on the class and/or on actions - These define the paths that should route to actions on this controller.
  * When the `[ApiController]` attribute is used, the `[Route]` attribute **must** be specified.
  * If the `[Route]` attribute is specified on the entire class (as it is in the `ValuesController.cs` example), it applies to all methods in the class.
  * If a `[Route]` attribute is on an individual action, the `[Route]` specified on the class acts as a base URL that is extended by the route defined on the action.

#### Exercise 1

Create a new file in your `Controllers` folder named `MathController.cs`. Use `ValuesController.cs` as a guideline to set up `MathController` as a new controller.

Set up one action on your new `MathController` class:

* URL: `https://localhost:5001/api/math/negative/{number}`
* Responds to HTTP GET requests
* Accepts one integer as input
* Returns the negation of the input number

#### Tips

* See the [routing documentation](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-2.1#route-name) for help defining routes.

#### Verify

* <https://localhost:5001/api/math/negative/100> should return -100.
* <https://localhost:5001/api/math/negative/-55> should return 55.

---

### 2. Add a POST route that accepts JSON

You don't have to manually translate JSON to objects - ASP.NET will do that for you with its [model binding](https://exceptionnotfound.net/asp-net-core-demystified-model-binding-in-mvc/) feature. If you set up a simple "model" class and use it as a parameter to your action, ASP.NET will analyze the values on the request and try to map them to the model you specified.

#### Exercise 2

Use model binding to set up the next action on your `MathController` class.

* URL: <https://localhost:5001/api/math/divide>
* Responds to HTTP POST requests
* Accepts a JSON object in the following format
  ```json
  {
    "dividend": 123,
    "divisor": 234
  }
  ```
* Uses model binding to parse the input
* Returns the quotient of the dividend divided by the divisor

#### Tips

* We use model binding in UX to map inputs to controller actions, so this might look familiar.
* In Postman, make sure you set your POST request's body type to **raw** -> **JSON (application/json)**.

#### Verify

Use Postman to send POST requests to <https://localhost:5001/api/math/divide>.

* `{ "dividend": 10, "divisor": 5 }` should return `2`
* `{ "dividend": -5, "divisor": 2 }` should return `-2.5`

---

### 3. Add and use a NuGet package

NuGet packages included in your project are specified in your `csproj` file. Instead of manually editing the list of packages and versions, you can [use the `dotnet` tool](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-add-package) to make the changes for you.

#### Exercise 3

* Use the `dotnet` tool to install [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/) version 3.0.0.
* Follow steps 1-5 of the [Swashbuckle Getting Started guide](https://github.com/domaindrivendev/Swashbuckle.AspNetCore#getting-started) to create automatic API documentation for your app.

#### Verify

Open `WebAPI.csproj` and look for the following `PackageReference`.

```xml
<PackageReference Include="Swashbuckle.AspNetCore" Version="3.0.0" />
```

Open <https://localhost:5001/swagger/> in a web browser to see documentation for your API.

---

### 4. Read from configuration files

ASP.NET Core apps can read configuration from [a variety of sources](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/) including files, environment variables, command-line parameters, and more.

Because your web server is built using `WebHost.CreateDefaultBuilder`, [application configuration is loaded from `appsettings.json` and `appsettings.<Environment>.json`](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/host/web-host?view=aspnetcore-2.1#set-up-a-host) with no additional setup required.

#### Setup

Let's make use of these settings files. Add a new "Math" section to `appsettings.Development.json`. Inside it, add a `Multiplier` property and set it to `10`.

```json
{
  "Logging": {
    ...
  },

  "Math": {
    "Multiplier": 10
  }
}
```

Create a new file named `appsettings.Production.json`. Big numbers are always more impressive to customers, so let's multiply everything we show them by 1000.

```json
{
  "Math" {
    "Multiplier": 1000
  }
}
```

Note that you don't have to specify all of your settings in `appsettings.Production.json` or `appsettings.Development.json`. The environment-specific `appsettings.<Environment>.json` files act as overrides for your base set in `appsettings.json`. The full set of settings applied in an environment will be a combination of the two files, plus any overrides that might come from [other configuration sources](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1#providers).

**Important:** If you use `dotnet run` or `dotnet watch run` to start your app, the default profile (in `Properties\launchSettings.json`) will override the `ASPNETCORE_ENVIRONMENT` variable. Delete these sections from `launchSettings.json` or run your app's executable file directly from the `bin\Debug\netcoreapp2.1` folder to avoid this.

Near the top of the output after you start your app, you can see the environment that the server thinks it's running in.

```text
Hosting environment: Development
```

Based on this, you can tell which settings file your app will use. Now, make use of your settings in your app.

#### Exercise 4

* Change the code in `MathController` so that all outputs are multiplied by the multiplier you specify.
* Settings are read on-the-fly. Update the settings file and make another request to your API to see the new multiplier take effect without restarting your app.
* [Change the `ASPNETCORE_ENVIRONMENT` environment variable](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/environments?view=aspnetcore-2.1#windows) to `Development` or `Production` and restart your app to read from the specified settings file.

#### Tips

* Use dependency injection to get an `IConfiguration` object in your constructor and use that to [read settings](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1#getvalue).
* There are multiple ways of reading settings - you can bind them to a class, read a whole section, or read one at a time. Do whatever makes sense with the [hierarchy of your settings](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1#hierarchical-configuration-data).

#### Verify

* With your environment set to Development
  * <https://localhost:5001/api/math/divide>
    * POSTing `{ "dividend": -5, "divisor": 2 }` should return `-25`
  * <https://localhost:5001/api/math/negative/5>
    * GET should return `-50`
* With your environment set to Production
  * <https://localhost:5001/api/math/divide>
    * POSTing `{ "dividend": -5, "divisor": 2 }` should return `-2500`
  * <https://localhost:5001/api/math/negative/5>
    * GET should return `-5000`
* Changing the multiplier should immediately affect the resulting output

---

### 5. Create a release for deployment

When your app is ready to share, you'll want to get it to its users (in our case, usually the Ops team) in a ready-to-use form. The [`dotnet publish`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish) command compiles your app and drops the compiled output into a folder with all its dependencies. You can then take this folder and copy it to another computer to share your app.

If you run `dotnet publish` without any extra parameters, it'll make a few assumptions about what you want to do.

First of all, it'll publish to `bin\Release\netcoreapp2.1`. This isn't a big deal, but you might want to send your output somewhere easier to find.

Second, it'll use the `Debug` build configuration. This will produce binaries with extra debugging options enabled, which runs somewhat slower than if you publish in `Release` mode instead.

Third, it'll only publish the `.dll` files that make up your application, without any self-starting executable file. To run your app from this output, you'll need to run `dotnet WebAPI.dll` from your output folder. This means that your app will be portable across operating systems, but somewhat inconvenient to start up.

Run `dotnet publish --help` or check the [online documentation](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish) to learn how to specify your output directory, build configuration, and target runtime.

#### Exercise 5

* Use `dotnet publish` to generate an executable version of your app
  * Set the build configuration to `Release`
  * Set the runtime to `win-x64`
  * Set the output directory to anything you want

#### Tips

* You can also specify a `<RuntimeIdentifier>` in your `csproj` file to automatically target a certain runtime.
* You can build binaries for any platform supported by .NET Core (Windows, Mac, or Linux) from any OS by specifying a target runtime.

#### Verify

* Open the output folder you specified and find `WebAPI.exe`.
* Stop any copies of your app that you might already have running.
* Run `WebAPI.exe` and use Postman to try making some requests to it.

---

## BONUS ZONE

Now you know how to build a ASP.NET Core app, run it on your computer, and prepare a build for your users.

You're still at the mercy of the environment your user chooses to run it in, though. Maybe you want to use a brand-new version of .NET Core that hasn't been installed on the server you're running on, for example, and your app just won't start up unless they install the new runtime.

Sounds like a job for Docker!

### 6. Make a Docker image

To make an effective Docker image for your application, start by picking the environment you want to run your app in. You built your app with ASP.NET Core 2.1, so you'll need an environment that can run that framework.

Microsoft provides [`dotnet` Docker images](https://hub.docker.com/r/microsoft/dotnet/) on Docker Hub that you can use as a base. These images give you a minimal Linux environment that's fully prepared to run .NET Core apps.

Many other maintainers provide similar images for their own applications, too. For example, you might use an [nginx](https://hub.docker.com/_/nginx/) image to start from the base of a webserver that's ready to go, [MySQL](https://hub.docker.com/_/mysql/) for a database, or [golang](https://hub.docker.com/_/golang/) to build apps with the Go programming language.

Take a look at the common tags listed on the [`dotnet` repository](https://hub.docker.com/r/microsoft/dotnet/). One of them is `2.1-aspnetcore-runtime`, which is exactly what you'll need to run your ASP.NET Core app. To specify a specific image by its tag, add a colon after the repository name (`microsoft/dotnet:2.1-aspnetcore-runtime`). If you don't specify a tag, Docker will automatically try to use the `latest` tag.

Now that you've picked `microsoft/dotnet:2.1-aspnetcore-runtime` as your base image, you can start preparing a Dockerfile.

#### The Dockerfile

A Dockerfile is a set of instructions to prepare a Docker image. It starts from a base, follows a set of commands, and then takes a snapshot of the environment after the commands are run. The result of this process is the Docker image. When you run an image, it "wakes up" the snapshot, turns it into a container, and starts running.

To build a Docker image for your app, you'll want to:

1. Build your project with Linux as your target runtime.
2. Copy the output of your build into a Docker image.
3. Configure the image to run your app when it starts up.

The following is an example of a Dockerfile that builds a Python project. Use it as reference to help you build an image for your ASP.NET Core project. The official [Dockerfile reference](https://docs.docker.com/engine/reference/builder) page is also helpful.

```Dockerfile
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Copy the current directory contents into the container at /pythonapp
ADD . /pythonapp

# Define an environment variable
ENV PYTHON_VERSION 2.7

# Set the working directory to /pythonapp
WORKDIR /pythonapp

# Run app.py when the container launches
ENTRYPOINT ["python", "app.py"]
```

Add a new file at the root of your project named `Dockerfile`. Follow the above example, but use `microsoft/dotnet:2.1-aspnetcore-runtime` as a base and build a set of instructions to copy your app into the image and run it.

**Note:** When your app starts up, it searches the **current working directory** for `appsettings.json` files - **not** the directory the app lives in. Make sure you set the right working directory in your Dockerfile so your app will know where to look for its configuration files.

#### Building your image

Run [`docker build .`](https://docs.docker.com/engine/reference/commandline/build/) from the folder containing your Dockerfile to build your Docker image. You can (and should) add the [`--tag` parameter](https://docs.docker.com/engine/reference/commandline/build/#tag-an-image--t) (often shortened to `-t`) to name your image.

```powershell
docker build . --tag mywebapi
```

When the build starts, you'll see it go through your Dockerfile line by line, displaying each instruction as it progressively builds the image. When it succeeds, you'll see `Successfully built xxxxxxxxx` and `Successfully tagged mywebapi` in the output.

#### Finding your image

After you build an image, you can see it (and any other Docker images on your machine) by running [`docker image ls`](https://docs.docker.com/engine/reference/commandline/image_ls/). The newest images will show at the top of the list, so the one you just built should be at the very top with the tag you specified displayed in the `REPOSITORY` column.

You can also filter the list of images to your specific image by adding the repository name as an extra parameter to the `docker image ls` command.

```powershell
docker image ls mywebapi
```

#### Exercise 6

* Use `dotnet publish` to generate an executable Linux version of your app
  * Set the build configuration to `Release`
  * Set the runtime to `linux-x64`
  * Set the output directory to `./output`
* Create a Dockerfile that:
  * Starts from `microsoft/dotnet:2.1-aspnetcore-runtime`
  * Copies the content of `./output` into the `/app` folder inside the image
  * Sets the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`
  * Sets the working directory to `/app` so the app will be able to find its configuration files
  * Starts running your app, which should be located at `/app/WebAPI` inside the image
* Use the Dockerfile to build an image

#### Tips

* Use the [Dockerfile reference page](https://docs.docker.com/engine/reference/builder) to help make sense of the different Dockerfile instructions.

#### Verify

* Run `docker image ls` and find the image you created.
* Make sure the image is tagged as `webapi`.

---

### 7. Run your Docker image

After your image builds successfully, you can run the container with [`docker run`](https://docs.docker.com/engine/reference/commandline/run/).

You can use `docker run --help` to see a list of options, but it's a long and unfiltered list. For now, you just need to know the basics.

#### Running inside a box

The first parameter to `docker run` is the name of your image. This is the name shown in the `REPOSITORY` column in the output of `docker image ls`, with the value from the `TAG` column appended after a colon if the tag is anything other than `latest`.

For your purposes, you'll just need to use:

```powershell
docker run mywebapi
```

Run this command and you'll see output from your app running inside the container you built. It should also specify that it's listening on port 80. This is different from port 5000 and port 5001 that it ran on locally - long story short, the `microsoft/dotnet:2.1-aspnetcore-runtime` image is configured to run the server on port 80 by default.

Because it's in a container, you can't actually reach it at port 80 just yet. Press <kbd>CTRL</kbd>+<kbd>C</kbd> to break out of the container and get back to the command line.

#### Cleaning up

One thing before you move on. Run [`docker ps`](https://docs.docker.com/engine/reference/commandline/ps/) to see all the containers you have running on your computer. If you ran `docker run` and exited with <kbd>CTRL</kbd>+<kbd>C</kbd>, you should see at least one item on this list.

Find the container you started up and stop it with [`docker stop [CONTAINER ID]`](https://docs.docker.com/engine/reference/commandline/stop/). You'll see the container ID printed as confirmation that it stopped.

```powershell
> docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
56c78de88c88        mywebapi            "/app/WebAPI --publi…"   4 minutes ago       Up 4 minutes        80/tcp              hardcore_volhard

> docker stop 56c78de88c88
56c78de88c88
```

You can also specify a container by its name (from the `NAMES` column, not the image name) or by a partial container ID.

```powershell
> docker stop hardcore_volhard
hardcore_volhard

> docker stop 56c
56c
```

Then, use [`docker rm`](https://docs.docker.com/engine/reference/commandline/rm/) to get rid of the stopped container completely. If you need to see the name or ID of a stopped container, run `docker ps --all` first.

```powershell
> docker ps --all
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS                          PORTS               NAMES
56c78de88c88        mywebapi            "/app/WebAPI --publi…"   4 minutes ago        Exited (0) About a minute ago                       hardcore_volhard

> docker rm 56c78de88c88
56c78de88c88
```

Or take a shortcut and use [`docker rm --force`](https://docs.docker.com/engine/reference/commandline/rm/#force-remove-a-running-container) to stop and remove the running container all in one step.

```powershell
> docker rm --force 56c78de88c88
56c78de88c88
```

#### Poking a hole

To get to port 80 inside the container, you'll need to use the [`--publish` option](https://docs.docker.com/engine/reference/commandline/run/#publish-or-expose-port--p---expose) (often shortened to `-p`) of `docker run`. This maps from a port on your local PC to a port inside the container, letting it communicate freely on that port. To specify the port mapping, provide a port number on your local PC followed by a port number inside the container, separated by a colon.

Let's map local port `5000` to container port `80`. Make sure you specify your `docker run` options **before** the image name, or else they'll get passed through as parameters to the app running inside the container!

```powershell
docker run --publish 5000:80 mywebapi
```

Now you can communicate with your API at <http://localhost:5000>.

#### Exercise 7

* Run your app with `docker run`
  * Map local port `5000` to container port `80`

#### Verify

* Use Postman to interact with your API at <http://localhost:5000/>.

---

### 8. Build inside a Docker container

The above process works fine if you know the details of the environment in which your code is being built - you can compile your code on your developer PC, copy it into a container, and go on your merry way. But what if your build is running on a build server (maybe VSTS or Bamboo) where you don't control which version of the .NET Core SDK is installed?

To solve this, you can take advantage of Docker's ability to conjure up a pre-prepared environment and compile your code inside the `docker build` process itself. Let's try out a feature called [multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/#use-multi-stage-builds).

#### Multi-stage Dockerfile

A multi-stage Dockerfile uses the `FROM` instruction multiple times, allowing you to choose the right environment for each step of the job. This lets you end up with a minimal image containing exactly the output you want.

The following is an example of a Dockerfile that builds a Go project in one environment and then copies the compiled output into another environment. The final stage in the Dockerfile is the only one that gets captured in the resulting image.

```Dockerfile
# Start from an official Golang image, naming it 'builder'
FROM golang:1.7.3 as builder

# Set the working directory
WORKDIR /go/src/fakegit.com/go-app/

# Copy all .go files to the working directory
COPY *.go .

# Build the code in the working directory and output a binary named 'compiled-go-app'
RUN go build -o compiled-go-app .



# Start a new stage, this time using the Alpine image
FROM alpine:latest

# Set the working directory
WORKDIR /go-app/

# Copy the 'compiled-go-app' file from the builder image to the working directory in the Alpine image
COPY --from=builder /go/src/fakegit.com/go-app/compiled-go-app .

# Start the compiled program
CMD ["./compiled-go-app"]
```

Notice the line that says `COPY --from=builder`? This allows you to move files between images during the build process, where `builder` is the name specified on the `FROM golang:1.7.3 as builder` line. You can name each stage anything you want.

Use this example to help you design a Dockerfile that builds your app in one environment and adds the compiled output to an output image.

#### Exercise 8

Create a multi-stage Dockerfile for your ASP.NET Core app.

* Use `microsoft/dotnet:2.1-sdk` as a build environment
* Use `microsoft/dotnet:2.1-aspnetcore-runtime` as the environment for your output image
* Only copy across the output of the build, not the source code

#### Tips

* Build and run the image the same way you did in sections 6 and 7.

#### Verify

* Use Postman to interact with your API at <http://localhost:5000/>.
