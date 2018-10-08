# Logging

ASP&#46;NET Core provides [built-in dependency injection](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection). This isn't set up to be fully automatic like it is in our UX framework, though. To use this with your own classes, you'll need to add each class to the `services` collection in the `ConfigureServices` class of `Startup.cs`. Don't worry about that for now, though.

`ILogger` is a special case because it's provided by default by the ASP&#46;NET Core framework. To get a logger object, just request one in the parameters of a controller's constructor.

```csharp
public class MyController : ControllerBase
{
  private readonly ILogger<MyController> _logger;

  public MyController(ILogger<MathController> logger)
  {
    _logger = logger;
  }

  [HttpGet]
  public ActionResult<string> Get()
  {
    _logger.LogWarning("Warning!");
    return "Got";
  }
}
```

You can use this pattern to get a `_logger` object to help you write contextual info from your app as you work through the exercises.
