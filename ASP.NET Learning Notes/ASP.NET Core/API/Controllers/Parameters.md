
By default, any params inside of method adorned with [HttpSomething] automatically bind to query strings. This is kinda shitty if not running Razor so that we can redirect post requests. 

To avoid that, we use DTO (data transfer object).  We just create a class/record/readonly ref struct and use that object as a param inside of controller methods. Add Dto at the end of the class name for consistency. That way, the params will not be extracted from query strings, but from the actual body of HTTP request.

Validation adorners are viable in DTOs because the standard C# required keyword is not really enough here to forbid the client from sending empty data to populate DTOs. See validation in other notes.