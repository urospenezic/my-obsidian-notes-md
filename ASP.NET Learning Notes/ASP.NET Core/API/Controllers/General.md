
-------------------------
Async EF Stuff
--
ToListAsync() is available when fetching data from db. Code looks something like this:
`[HttpGet]`

`public async Task<ActionResult<IReadOnlyList<AppUser>>> GetMembers()`
`{`
`var users = await _context.Users.ToListAsync();`
`return Ok(users);`
`}`

`[HttpGet("{id}")] // localhost:5000/api/members/bob-id`
`public async Task<ActionResult<AppUser>> GetMember(string id)`
`{`
`var user = await _context.Users.FindAsync(id);`
`if (user == null)`
`{`
`return NotFound();`
`}`
`return Ok(user);`
`}`

