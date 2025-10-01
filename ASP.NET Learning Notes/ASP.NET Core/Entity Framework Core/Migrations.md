
Migrations are like version control for databases generated via EF. To create them do:

dotnet ef migrations add InitialCreate

Each migration is a c# file with an Up() or Down() methods to apply or reverse changes. 

The db is created code first -> we write our classes as models and EF generates a DB Schema based off of that.

So if i add a new property to my model class, i can run dotnet migrations add 'commit message' and then dotnet ef database update to create a db

dotnet ef database update --startup-project ../../ProjectReferencePath -> useful in case of models being in a separate project