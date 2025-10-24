
Same as LINQ def execution. Basically any query that does not go through select stage (like select statement or toList() or anything that will force the select to go off) is stored in IQueryable<T/>. We can pass this around and execute when needed 

ExecuteUpdateAsync - newer method. Asynchronously updates database rows for the entity instances which match the LINQ query from the database. This operation executes immediately against the database, rather than being deferred until DbContext.SaveChanges() is called. It is also useful because it matches a LINQ query, so it won't actually retrieve from db (in case below members). example of usage below:

```
public class LogUserActivity : IAsyncActionFilter

{

public async Task OnActionExecutionAsync(

ActionExecutingContext context,

ActionExecutionDelegate next

)

{

//next is the action executing inside of our controller

//context gives us access to the http context

var resultContext = await next();

  

// no reason to continue if unauthorized

if (context.HttpContext.User.Identity?.IsAuthenticated != true)

return;

  

var memberId = resultContext.HttpContext.User.GetMemberId();

  

var dbContext =

resultContext.HttpContext.RequestServices.GetRequiredService<AppDbContext>();

  

await dbContext

.Members.Where(u => u.Id == memberId)

.ExecuteUpdateAsync(setters =>

setters.SetProperty(member => member.LastActive, DateTime.UtcNow)

);

}

}
```

Ofc, we register these as services as well:
builder.Services.AddScoped<API.Helpers.LogUserActivity>();

We then hook it up to any controller to be executed before/after requests (much like angular interceptor):

[ServiceFilter(typeof(LogUserActivity))]
[Route("api/[controller]")]
[ApiController]
public class BaseApiController : ControllerBase { }