
**addin services:

we can build a local service similar to angulars services. we can then inject that service into our apps. before we do that, we gotta notify asp.net that we intend to use a service and how. there are couple of ways to do that, all of which reside in Program.cs:

1. builder.Services.AddSingleton -> generates a global singleton ref to a service
2. builder.Services.AddTransient -> new instance of service for every request
3. builder.Services.AddScoped -> scoped to the level of http request. created when request comes in and disposed as the request leaves.

