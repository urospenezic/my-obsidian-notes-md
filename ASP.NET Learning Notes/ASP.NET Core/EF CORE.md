Related NuGet packages: Microsoft.EntityFrameworkCore.Design, Microsoft.EntityFrameworkCore.SqlServer

We have to add db context in our program.cs as a service:

builder.Services.AddDbContextPool<OdeToFoodDbContext>(options => {
options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection"));
});


DbContextOptions object that's injected into custom db context classes contains the options above.

********************

Connecting to a database:

Install mssql-cli tool (for arch follow
	sudo pacman -S python-pipx
	pipx install mssql-cli or consult with LLM linux is fucked with sql server)

To get SQL Server working on linux, we have to use Microsoft.EntityFrameworkCore.Sqlite nuget (i mean its easier, other way will involve docker and setting up spinning db instances)

******Migrations*******

Migrations are like version control for databases generated via EF. To create them do:

dotnet ef migrations add InitialCreate

Each migration is a c# file with an Up() or Down() methods to apply or reverse changes. 

The db is created code first -> we write our classes as models and EF generates a DB Schema based off of that.

So if i add a new property to my model class, i can run dotnet migrations add 'commit message' and then dotnet ef database update

dotnet ef database update --startup-project ../../ProjectReferencePath -> useful in case of models being in a separate project


************************************

Once the db is created, we need to go to appsettings.json and configure ConnectionStrings to point to the new db, like:

"ConnectionStrings": {
	"DefaultConnection": "Data Source=MyDB.db"
},


*********************************






