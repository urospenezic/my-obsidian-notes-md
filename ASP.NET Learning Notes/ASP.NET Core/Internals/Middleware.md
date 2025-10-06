
Processes HTTP messages example:

http request -> middleware 1 (Logger - POST) -> middleware 2 (Authorizer POST) -> middleware 3 (Router POST)-> generates response -> back to middleware 2 -> back to middleware 1 -> endpoint

Middleware pipeline is bidirectional.

Middlewares can be found in Program.cs. app.UseDeveloperExceptionPage() or UseRouting() etc.

There is a fuck ton of pre built middleware, like UseCors() etc. https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/?view=aspnetcore-9.0


**Custom middleware
--

A proper full detailed middleware requires construction of special classes. 

A simple way is to use app.Use() method and pass in a function that is going to get executed as requests come in. These functions accept a RequestDelegate object as args.

I mid tier easy way to add is to create a custom class without implementing IMiddleware interface. **If using this approach, we have to make sure to have a method with a signature = "public async Task InvokeAsync(HttpContext context)"

Any time we calle next() method, the request is passed on to the next middleware in a sequence. So a exception catching middleware that formats errors for return looks like this:

`public class ExceptionMiddleware(`
`RequestDelegate next,`
`ILogger<ExceptionMiddleware> logger,`
`IHostEnvironment env`
`)`
`{`
`public async Task InvokeAsync(HttpContext context)`
`{`
	`try`
	`{`
		`await next(context);`
	`}`
	`catch (Exception ex)`
	`{`
		`logger.LogError(ex, "{message}", ex.Message);`
		`context.Response.ContentType = "application/json";`
		`context.Response.StatusCode = StatusCodes.Status500InternalServerError;`
		`var response = env.IsDevelopment() ? new ApiException(`
			`context.Response.StatusCode,`
			`ex.Message,`
			`ex.StackTrace?.ToString()`
		`) : new ApiException(context.Response.StatusCode, "Internal Server Error", null);`

		`var options = new JsonSerializerOptions`
		`{`
			`PropertyNamingPolicy = JsonNamingPolicy.CamelCase,`
		`};`
		`var json = JsonSerializer.Serialize(response, options);`

		`await context.Response.WriteAsync(json);`
	`}`
`}`
`}`

**TO ADD THIS MIDDLEWARE INTO OUR MIDDLEWARE PIPELINE, WE DO APP.USEMIDDLEWARE<OURCUSTOMMIDDLEWARECLASS>() IN PROGRAM.CS WHERE NEEDED

**NOTE!!! 
Middleware goes through from top to bottom as we write it in Program.cs. So if we use SayHelloMiddleware (see asp.net core project from Scott Allen) before UseMvc() middleware, any request will just display "Hello from middleware". So order is important.  If any middleware before this produces a response, it will never get to it. So it's like a motherboard bus type of thing. Request comes in, middlewares are a bus stops, they take the request, perform something, put it back on the track. Once a middleware is found that actually produces a response, we stop and return that response. So, middleware further down the track will not get the chance to execute if some middleware returned a response.


