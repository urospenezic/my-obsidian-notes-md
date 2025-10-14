
ORM and databases dont really support UTC signatures with the appended 'Z' at the end of datetime, and likely never will. But the browsers expect it if we want them to interpret dates as UTC, otherwise they will treat it as local time. In order to avoid that, we override model creation of EF Core main DbContext like:

```
//override for UTC date time return since DBs dont support it natively

protected override void OnModelCreating(ModelBuilder modelBuilder)

{

base.OnModelCreating(modelBuilder);

  

var dateTimeConverter = new ValueConverter<DateTime, DateTime>(

v => v.ToUniversalTime(),

v => DateTime.SpecifyKind(v, DateTimeKind.Utc)

);

  

foreach (var entityType in modelBuilder.Model.GetEntityTypes())

{

foreach (var property in entityType.GetProperties())

{

if (property.ClrType == typeof(DateTime) || property.ClrType == typeof(DateTime?))

{

property.SetValueConverter(dateTimeConverter);

}

}

}

}
```