appsettings.json: anything we place here can be accessed globally, including inside of view controllers by injecting IConfiguration object. This object maps all json props as hashmap pairs, so we can do config['mycustommessage']

In Program.cs we do configuration of data sources and services. Take a look at 'Add services to the container' part. Here we can specify a singleton data source as a service by doing builder.Services.AddSingleton<IRepository, RepositoryImpl>();



