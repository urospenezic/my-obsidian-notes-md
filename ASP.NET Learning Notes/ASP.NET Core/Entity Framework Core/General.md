
DbSet will translate to a table, so a name given to DbSet prop will be the table name. EF will then go into the Entity of <T> and create columns based off of property names in the entity class.

Updating items:

public void Update(Member user)
{
	_context.Entry(user).State = EntityState.Modified;
}

Do not forget to hook up repositories in services: app.builder.Services.AddScoped<IRepository, RepositoryImpl>(); so that we can inject it in controllers