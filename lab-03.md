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

The methods in `ValuesController` have associated [attributes](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-2.1#attribute-routing-with-httpverb-attributes) such as `[HttpGet]`, `[HttpPost]`, and `[HttpDelete]` that limit the [HTTP verbs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) that are allowed to trigger them. Navigating to an action in a web browser triggers an HTTP GET request.

## Making HTTP requests

It's easy to make HTTP GET requests with your web browser - that's what happens by default whenever you type something into your address bar and press enter. There are many other [HTTP verbs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods), though, that take a little more configuration to use.

### Postman

[Postman](https://www.getpostman.com/) is a tool to build and test HTTP requests with more flexibility than a web browser.

Open the Postman app, then open a new tab in the main section of the UI (don't use the "New" button at the top left, and don't worry about the "Collections" section for now).

Click the dropdown menu that says "GET" to see a list of all the HTTP verbs you have available. For this lab, we'll only need GET and POST.

### Required setup

Open the settings (File > Settings) and turn off "SSL certificate verification" in the General tab. This will let you work with your local API even though its identity hasn't been verified by a certificate authority.

### Make a request

To make a request, just put a URL into the "Enter request URL" box, set the HTTP verb you want to use, and click Send. If you switch to the POST verb, you'll see the "Body" tab light up under the URL bar. This is where you'll define what data gets sent with your POST request.

To start, try making a GET request to <https://localhost:5001/api/values/1>. You should see the word `value` as a response, which is hard-coded in the `Get(int id)` method. In the next step, we'll change this to echo back the input you send it.
