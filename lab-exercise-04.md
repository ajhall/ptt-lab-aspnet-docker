# 4. Read from configuration files

ASP&#46;NET Core apps can read configuration from [a variety of sources](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/) including files, environment variables, command-line parameters, and more.

Because your web server is built using `WebHost.CreateDefaultBuilder`, [application configuration is loaded from `appsettings.json` and `appsettings.<Environment>.json`](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/host/web-host?view=aspnetcore-2.1#set-up-a-host) with no additional setup required.

## Setup

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
  "Math": {
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

## Exercise 4

* Change the code in `MathController` so that all outputs are multiplied by the multiplier you specify.
* Settings are read on-the-fly. Update the settings file and make another request to your API to see the new multiplier take effect without restarting your app.
* [Change the `ASPNETCORE_ENVIRONMENT` environment variable](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/environments?view=aspnetcore-2.1#windows) to `Development` or `Production` and restart your app to read from the specified settings file.
  * **Note:** Changing an environment variable from the command line only affects your current command line session. Make sure you restart your app from the same command line session that you used to change the environment variable. Alternatively, set the value globally. See the linked documentation for details.

## Tips

* Use dependency injection to get an `IConfiguration` object in your constructor and use that to [read settings](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1#getvalue).
* There are multiple ways of reading settings - you can bind them to a class, read a whole section, or read one at a time. Do whatever makes sense with the [hierarchy of your settings](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.1#hierarchical-configuration-data).

## Verify

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