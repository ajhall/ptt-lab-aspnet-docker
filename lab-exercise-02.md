# 2. Add a POST route that accepts JSON

You don't have to manually translate JSON to objects - ASP&#46;NET will do that for you with its [model binding](https://exceptionnotfound.net/asp-net-core-demystified-model-binding-in-mvc/) feature. If you set up a simple "model" class and use it as a parameter to your action, ASP&#46;NET will analyze the values on the request and try to map them to the model you specified.

## Exercise 2

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

## Tips

* We use model binding in UX to map inputs to controller actions, so this might look familiar.
* In Postman, make sure you set your POST request's body type to **raw** -> **JSON (application/json)**.

## Verify

Use Postman to send POST requests to <https://localhost:5001/api/math/divide>.

* `{ "dividend": 10, "divisor": 5 }` should return `2`
* `{ "dividend": -5, "divisor": 2 }` should return `-2.5`