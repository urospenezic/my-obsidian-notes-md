Instead of doing the login the manual way with password hashing, salting, token generation etc, we derive our user entity from IdentityUser class

Since using the entity framework core identity package, AppDbContext can also derive from IdentityDbContext in which case the Users table will be auto generated, so we don't need that

**Configuration:

Program.cs -> 
`builder`
    `.Services.AddIdentityCore<API.Entities.AppUser>(options => {`
    `//setup options accordingly })`
    `.AddRoles<IdentityRole>()//optional`
    `.AddEntityFrameworkStores<AppDbContext>();`

Note: AddIdentityApiEndpoints is less flexible, but adds decent ammount of functionality upfront

**Seeding:

We no longer use db context for seeding, we use the new UserManager<UserEntity> class which comes with a bunch of functionalities. Check the doc for UserManager, has stuff like ChangePassword, CreateAsync, CheckPasswordAsync, everything you might need for role based user management. Check Seed.cs in DatingApp in course-project repo for ref.






