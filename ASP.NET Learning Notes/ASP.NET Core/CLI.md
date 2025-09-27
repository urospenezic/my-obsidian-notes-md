dotnet aspnet-codegenerator (could be obsolete, i know that dotnet new page -n pageName generates a razor page)

with this code generator, we can specify a lot of things to be auto generated, such as: db context that should be used, default layout, templated view like empty, create, edit, delete, details, list

adding reference: dotnet add reference pathToCsProj


**ENTITY FRAMEWORK CORE 

Installing it: 
	1. dotnet tool install --global dotnet-ef
	2. export PATH="$PATH:pathToDotnet/.dotnet/tools"
	3. dotnet ef --version

dotnet ef scaffold -> can generate ef db context based on an existing database

dotnet ef dbcontext info -> good for checking that EF can see our db context implementations

dotnet ef migrations add "nameOfMigration" -> creates a new migration

otnet ef database update --startup-project ../../ProjectReferencePath -> generates a db based off of migration to a specified startup project path (in case there are multiple projects)

- `-np, --no-pagemodel`: Creates a Razor Page without a corresponding PageModel file. This is useful for simple, static pages.

**SCAFFOLDING ENTIRE CRUD

We need to have a model class in place, as well as a derivative of DbContext. From there we can:
1. If Visual Studio -> add -> Scaffolding Item  -> razor pages using entity framework
2. If on VS Code -> CLI -> dotnet aspnet-codegenerator razorpage -m ModelName -dc DbContextName -udl -outDir Pages/Folder where we wanna place pages --referenceScriptLibraries

Breakdown of commands:

- `dotnet aspnet-codegenerator razorpage` - The scaffolding command for Razor Pages
- `-m - Model class name (your entity)
- `-dc - DbContext class name
- `-udl` - Use default layout
- `-outDir  - Output directory for generated pages
- `--referenceScriptLibraries` - Include validation scripts



-------------------------

dotnet publish -> for publishing