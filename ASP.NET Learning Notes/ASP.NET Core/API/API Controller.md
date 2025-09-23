In order for any API to work, we need to add the controller into services and map controllers. This is done in Program.cs by adding:

builder.Services.AddControllers()

and at the bottom before Run(): app.MapControllers()

----------------------------

"dotnet aspnet-codegenerator controller -name NameOfApiController -async -api -m Model -dc DBContext -outDir directoryName" will auto scaffold a api using entity framework.

Inside of Visual Studio this is done through the interface of Add Scaffold. 


----------------------------------

npm init + npm install to get npm into asp.net project. Make sure to install in the project where the pages and web content is.

**we can force asp.net to serve local node module files by doing: 

// Serve static files from node_modules in development

`if (app.Environment.IsDevelopment())`
`{`
	`app.UseStaticFiles(`
		`new StaticFileOptions`
			`{`
				`FileProvider = new Microsoft.Extensions.FileProviders.PhysicalFileProvider(` `Path.Combine(Directory.GetCurrentDirectory(), "node_modules")``),`
			`RequestPath = "/node_modules",`
			`}`
	`);`
`}`

**OR USE ODETOCODE NuGet that allows for app.UseNodeModules() middleware


