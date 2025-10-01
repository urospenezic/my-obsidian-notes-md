
[Route("[controller]")] that sits on top of controller is a design by convention. It tells the ASP.NET to use the class name **MINUS THE CONTROLLER PART IN THE NAME** for controller discovery

[HttpGet(Name="GetRequestName")] -> self explanatory

