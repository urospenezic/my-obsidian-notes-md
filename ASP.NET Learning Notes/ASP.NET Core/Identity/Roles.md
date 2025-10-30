
We can create a controller and guard requests that are for certain user roles and policies only like:

`public class AdminController : BaseApiController`
`{`
    `[Authorize(Policy = "RequireAdminRole")]`
    `[HttpGet("users-with-roles")]`
    `public IActionResult GetUsersWithRoles()`
    `{`
        `return Ok("This is a secret message only for admins");`
    `}`
`}`

**POLICIES:

Configure them in Program.cs like: 

`builder`
    `.Services.AddAuthorizationBuilder()`
    `.AddPolicy("RequireAdminRole", policy => policy.RequireRole("Admin"))`
    `.AddPolicy("ModeratePhotoRole", policy => policy.RequireRole("Admin", "Moderator"));;`
