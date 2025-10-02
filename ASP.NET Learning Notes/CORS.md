Cross origin resource sharing -> standard that a browser enforces that forces an API and Frontend to have a handshake deal via HTTP headers. A special CORS header has to be returned and accepted by both parties in order to tell the browser "hey this is my FE and it is talking to my Backend". 


To include CORS in our API -> add a CORS service via builder.Services.AddCors() in Program.cs

We also gotta include CORS in our middleware, so app.UseCors(options delegate)

app.UseCors(policy =>

    policy.WithOrigins("http://localhost:4200/", "https://localhost:4200/")

          .AllowAnyHeader()

          .AllowAnyMethod());

without with origins will just accept anything and return the appropriate header.  the origin should always be specified and should always be the default address of our frontend.

CORS was initially implemented to combat injection js scripts from sneaking into the FE side and being posted to the server.