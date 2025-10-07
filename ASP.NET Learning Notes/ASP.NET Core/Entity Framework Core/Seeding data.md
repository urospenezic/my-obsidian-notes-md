
Generate seeding data via random ai given entity properties. Best to seed via json and make sure that json properties match to our c# entities by name for serialization.

In case we keep seeding data json object as blobs, we should create a DTO that accepts all the properties at once via json.deserialize and then use the data to generate actual entities we need.

Seeding is just reading from a file, generating and filling entities and then commiting those changes into a local db

**AFTER WE'VE DONE SETTING UP READING FROM FILE AND ADDING DATA TO CONTEXT, WE DO THIS PRIOR TO APP.RUN():

`sing var scope = app.Services.CreateScope();`
`var services = scope.ServiceProvider;`
`try {`
	`var context = services.GetRequiredService<AppDbContext>();`
	`await context.Database.MigrateAsync();`
	`await Seed.SeedUsers(context);`
`} catch (Exception ex) {`
	`var logger = services.GetRequiredService<ILogger<Program>>();`
	`logger.LogError(ex, "An error occurred during migration");`
`}`

