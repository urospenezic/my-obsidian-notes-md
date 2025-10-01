
Publish -> before deployment. Puts files into a specific location in preparation for deployment

dotnet publish is a CLI command for publish. it has a --self-contained option for building self containing apps. without this the app runs on the framework version on our machine. with self contained the framework elements needed are included with the publish. the drawback is that security patches will not get auto installed, we have to re-deploy to get the new stuff within the framework. 

To use --self-contained, we have to specify the exact runtime via -r. https://docs.microsoft.com/en-us/dotnet/core/rid-catalog (runtime identifiers)

To execute instruction for npm install (so that the end user can auto get node_modules without including them) -> we usi MSBuild, which has been around since dotnet beggining:

1. Inside of csproj file for the startup project
2. include :
 %%  <ItemGroup>
    <Content Include="node_modules\**" CopyToPublishDirectory="PreserveNewest" />
  </ItemGroup> %%



--------------------------

**Deploying to a Web Server

https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy
https://learn.microsoft.com/en-us/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-9.0&tabs=linux-ubuntu
https://nginx.org/en/docs/


Ofc, we prolly wanna use a docker container running nginx either way, not IIS Windows bs. The course goes through steps of IIS deployment.

______________________________

**Using database on a web server

dotnet ef migrations script -s "StartuProject path" -> generates a script that EF uses to generate a database based on our model. We can use that script to build a database on project startup. (Or we can have it run with docker files i guess, maybe even github action for deployment?)

To set that up we do this:

`var app = builder.Build();`

`MigrateDatabase();`

`void MigrateDatabase()`
`{`
    `using var scope = app.Services.CreateScope();`
    `var dbContext = scope.ServiceProvider.GetRequiredService<DbContextReference>();`
    `dbContext.Database.Migrate();`

`}`

In Program.cs


________________________

**Connecting to a db

Generate a login and connection string to connect to that db. On windows we can use windows integrated security.

Imma skip this part since we prolly need something else these days. This course is atm 6 years old.






