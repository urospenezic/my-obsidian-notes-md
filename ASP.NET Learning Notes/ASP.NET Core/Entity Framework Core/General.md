
DbSet will translate to a table, so a name given to DbSet prop will be the table name. EF will then go into the Entity of <T> and create columns based off of property names in the entity class.