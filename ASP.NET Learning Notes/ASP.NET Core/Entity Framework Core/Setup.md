
----------------------------
Install dotnet tool install -g dotnet-ef
-------------------------------

----------------------

Related NuGet packages
---------------------------------
Microsoft.EntityFrameworkCore.Design, Microsoft.EntityFrameworkCore.SqlServer or SQLite

We have to add db context in our program.cs as a service:

builder.Services.AddDbContextPool<OdeToFoodDbContext>(options => {
options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection"));
});

or newer way:

`builder.Services.AddDbContext<AppDbContext>(options =>`
`{`
`options.UseSqlite(builder.Configuration.GetConnectionString("DefaultConnection"));`
`});`


DbContextOptions object that's injected into custom db context classes contains the options above.


---------------------------------------
Connecting to a database
---------------------

Install mssql-cli tool (for arch follow
	sudo pacman -S python-pipx
	pipx install mssql-cli or consult with LLM linux is fucked with sql server)

To get SQL Server working on linux, we have to use Microsoft.EntityFrameworkCore.Sqlite nuget (i mean its easier, other way will involve docker and setting up spinning db instances)

************************************

Once the db is created, we need to go to appsettings.json and configure ConnectionStrings to point to the new db, like:

"ConnectionStrings": {
	"DefaultConnection": "Data Source=MyDB.db"
},

after this we run a new migration to generate everything
*********************************






