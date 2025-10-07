
AutoMapper went full SASS, so its a paid service now... MediatR same thing

There is no way to auto map by default into relational tables. So if we have a user that has a foreign key to their photos table, the photos will not automatically magically be returned by _context.Users.FindAsync()_ or any other EF methods. We have to say like .FindAsync(id).Include(x => x.Photos) to do the mapping

**[JsonIgnore] is used in entities and dto's to exclude certain properties from the returned json automatically. Use this on any id's or navigation properties you're not mapping