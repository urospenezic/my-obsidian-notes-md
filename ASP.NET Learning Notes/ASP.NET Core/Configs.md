appsettings.json: anything we place here can be accessed globally, including inside of view controllers by injecting IConfiguration object. This object maps all json props as hashmap pairs, so we can do config['mycustommessage']

In Program.cs we do configuration of data sources and services. Take a look at 'Add services to the container' part. Here we can specify a singleton data source as a service by doing builder.Services.AddSingleton<IRepository, RepositoryImpl>();

Or for db context and EF Core use AddScoped instead of singleton

lauchSettings.json defines environments. iisSettings is for launching from VS, "ProjectName" settings are for launching from command line.

launchSettings is also responsible for setting up default ports and addresses for the project

appSettings.json is used to configure logging levels for example

**NOTE: for good practice -> remove the http profile from launchSettings and remove the http address in https profile. nobody runs http anymore


------------------------------

**in the csproj in vs code there's <ImplicitUsings> statement so that we dont need that much using boilerplate code (so using System is no longer needed, which is nice)

