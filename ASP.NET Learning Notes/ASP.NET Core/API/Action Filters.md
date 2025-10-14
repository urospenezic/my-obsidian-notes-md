
Allows us to take action after or before the request.
Note: since c# doesnt support DI inside of functions like typescript, we "inject" using a service-locator-pattern  like:
var dbContext =
resultContext.HttpContext.RequestServices.GetRequiredService<AppDbContext>();