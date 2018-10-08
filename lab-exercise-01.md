# 1. Add a new controller

The following are the [essential pieces](https://docs.microsoft.com/en-us/aspnet/core/web-api/) you'll need to create a controller:

* [Inherit from `ControllerBase`](https://docs.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-2.1#derive-class-from-controllerbase) - This provides the properties and methods that ASP&#46;NET Core needs to treat the class as a controller.
* [`[ApiController]` attribute](https://docs.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-2.1#annotate-class-with-apicontrollerattribute) - This isn't strictly necessary, but it provides common functionality to automatically make your app act like a web app.
* [`[Route]` attributes](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-2.1#attribute-routing) on the class and/or on actions - These define the paths that should route to actions on this controller.
  * When the `[ApiController]` attribute is used, the `[Route]` attribute **must** be specified.
  * If the `[Route]` attribute is specified on the entire class (as it is in the `ValuesController.cs` example), it applies to all methods in the class.
  * If a `[Route]` attribute is on an individual action, the `[Route]` specified on the class acts as a base URL that is extended by the route defined on the action.

## Exercise 1

Create a new file in your `Controllers` folder named `MathController.cs`. Use `ValuesController.cs` as a guideline to set up `MathController` as a new controller.

Set up one action on your new `MathController` class:

* URL: `https://localhost:5001/api/math/negative/{number}`
* Responds to HTTP GET requests
* Accepts one integer as input
* Returns the negation of the input number

## Tips

* See the [routing documentation](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-2.1#route-name) for help defining routes.

## Verify

* <https://localhost:5001/api/math/negative/100> should return -100.
* <https://localhost:5001/api/math/negative/-55> should return 55.
