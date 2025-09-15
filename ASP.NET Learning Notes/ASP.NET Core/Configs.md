appsettings.json: anything we place here can be accessed globally, including inside of view controllers by injecting IConfiguration object. This object maps all json props as hashmap pairs, so we can do config['mycustommessage']

