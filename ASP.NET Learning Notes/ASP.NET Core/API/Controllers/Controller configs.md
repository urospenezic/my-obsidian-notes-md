
[Route("[controller]")] that sits on top of controller is a design by convention. It tells the ASP.NET to use the class name **MINUS THE CONTROLLER PART IN THE NAME** for controller discovery

[HttpGet(Name="GetRequestName")] -> self explanatory

[HttpGet("{id}")] // localhost:5000/api/controllerName/id


[FromForm] inside of controller endpoint params -> specifies that the endpoint only accepts param if it was passed inside of form-data object in body. In Angular we got FormData class to populate with form data.

[FromQuery] -> specifies that params are to be received via query params
