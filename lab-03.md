# Try the template app

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

## The controller

ASP&#46;NET Core can use the class name of a controller to help construct a URL. Take a look at `ValuesController.cs`.

```csharp
[Route("api/[controller]")]
[ApiController]
public class ValuesController : ControllerBase
```

The `Route` attribute specifies the URL you can use to access this controller. The `[controller]` segment of the URL says to make use of the name of the class itself. For example, `ValuesController` is accessible at `/api/Values`.

Now you need to combine what you know about your app's base URL and your controller's route. Try navigating to <https://localhost:5001/api/values> in your web browser. Your browser might warn you about a self-signed security certificate - that's OK for local testing, so add an exception in your web browser.

## Controller actions

At <https://localhost:5001/api/values>, you should see `["value1","value2"]`. This comes straight from the `Get()` method in `ValuesController.cs`.

The methods in `ValuesController` have associated [attributes](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-2.1#attribute-routing-with-httpverb-attributes) such as `[HttpGet]`, `[HttpPost]`, and `[HttpDelete]` that limit the HTTP verbs that are allowed to trigger them. Navigating to an action in a web browser triggers an HTTP GET request.

## Using Postman

TODO

Self-signed SSL certificates are being blocked:
Fix this by turning off 'SSL certificate verification' in Settings > General

Try making a GET request to <https://localhost:5001/api/values/1>. You should see the word `value` as a response, which is hard-coded in the `Get(int id)` method. Let's change this to echo back the input you send it.

