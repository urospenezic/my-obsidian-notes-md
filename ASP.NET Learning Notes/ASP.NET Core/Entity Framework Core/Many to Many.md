
Not really super suported, we gotta get around with:

```
//Join entity for many-to-many relationship between Member and Member (self-referencing)

// public List<MemberLike> LikedByMembers { get; set; } = [];

// public List<MemberLike> LikedMembers { get; set; } = [];
//!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
public class MemberLike

{

public required string SourceMemberId { get; set; }

public Member SourceMember { get; set; } = null!;

public required string TargetMemberId { get; set; }

public Member TargetMember { get; set; } = null!;

}

------------------------------------------------------------

public class Member

{

//Reason why we're keeping some same properties between AppUser and Member is so that we do not have

//to do a join query to get the data we need for members list

//Also, Member is not used for authentication, only for displaying data

public string Id { get; set; } = null!;

public DateOnly DateOfBirth { get; set; }

public string? ImageUrl { get; set; }

public required string DisplayName { get; set; }

public DateTime Created { get; set; } = DateTime.UtcNow;

public DateTime LastActive { get; set; } = DateTime.UtcNow;

public required string Gender { get; set; }

public string? Description { get; set; }

public required string City { get; set; }

public required string Country { get; set; }

//!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

[JsonIgnore]

public List<MemberLike> LikedByMembers { get; set; } = [];

  

[JsonIgnore]

public List<MemberLike> LikedMembers { get; set; } = [];

  

//Nav property

[ForeignKey(nameof(Id))]

[JsonIgnore]

public AppUser AppUser { get; set; } = null!;

  

[JsonIgnore]

public List<Photo> Photos { get; set; } = [];

}

------------------------------------------------------------

public class AppDbContext(DbContextOptions<AppDbContext> options) : DbContext(options)

{

public DbSet<AppUser> Users { get; set; }

public DbSet<Member> Members { get; set; }

public DbSet<Photo> Photos { get; set; }

public DbSet<MemberLike> Likes { get; set; }

  

//override for UTC date time return since DBs dont support it natively

protected override void OnModelCreating(ModelBuilder modelBuilder)

{

base.OnModelCreating(modelBuilder);

//!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
//since memberlike does not have id which ef will look for by default, we need to specify composite key
modelBuilder
.Entity<MemberLike>()
.HasKey(ml => new { ml.SourceMemberId, ml.TargetMemberId });

//setup both sides of many to many
modelBuilder
.Entity<MemberLike>()
.HasOne(ml => ml.SourceMember)
.WithMany(m => m.LikedMembers)
.HasForeignKey(ml => ml.SourceMemberId)
.OnDelete(DeleteBehavior.Cascade);

modelBuilder
.Entity<MemberLike>()
.HasOne(ml => ml.TargetMember)
.WithMany(m => m.LikedByMembers)
.HasForeignKey(ml => ml.TargetMemberId)
.OnDelete(DeleteBehavior.Cascade);
  

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

}

--------------------------------------------------------

public class LikesRepository(AppDbContext context) : ILikesRepository

{

public async void AddLike(MemberLike like)

{

await context.Likes.AddAsync(like);

}

  

public void DeleteLike(MemberLike like)

{

context.Likes.Remove(like);

}

  

public async Task<IReadOnlyList<string>> GetCurrentMemberLikeIds(string memberId)

{

return await context

.Likes.Where(l => l.SourceMemberId == memberId)

.Select(l => l.TargetMemberId)

.ToListAsync();

}

  

public async Task<MemberLike?> GetMemberLike(string sourceMemberId, string targetMemberId)

{

return await context.Likes.FindAsync(sourceMemberId, targetMemberId);

}

  

public async Task<IReadOnlyList<Member>> GetMemberLikes(string predicate, string memberId)

{

var query = context.Likes.AsQueryable();

switch (predicate)

{

case "liked":

return await query

.Where(l => l.SourceMemberId == memberId)

.Select(l => l.TargetMember)

.ToListAsync();

case "likedBy":

return await query

.Where(l => l.TargetMemberId == memberId)

.Select(l => l.SourceMember)

.ToListAsync();

default:

var likeIds = await GetCurrentMemberLikeIds(memberId);

return await query

.Where(m => m.TargetMemberId == memberId && likeIds.Contains(m.SourceMemberId))

.Select(l => l.SourceMember)

.ToListAsync();

}

}

  

public async Task<bool> SaveAllAsync()

{

return await context.SaveChangesAsync() > 0;

}

}
------------------------------------------------------------

public class LikesController(ILikesRepository likesRepository) : BaseApiController

{

[HttpPost("{targetMemberId}")]

public async Task<ActionResult> ToggleLike(string targetMemberId)

{

var sourceMemberId = User.GetMemberId();

if (sourceMemberId == targetMemberId)

return BadRequest("You cannot like yourself.");

  

var existingLike = await likesRepository.GetMemberLike(sourceMemberId, targetMemberId);

if (existingLike == null)

{

var like = new MemberLike

{

SourceMemberId = sourceMemberId,

TargetMemberId = targetMemberId,

};

likesRepository.AddLike(like);

}

else

{

likesRepository.DeleteLike(existingLike);

}

  

if (await likesRepository.SaveAllAsync())

return Ok();

return BadRequest("Failed to update like.");

}

  

[HttpGet("list")]

public async Task<ActionResult<IReadOnlyList<string>>> GetCurrentMemberLikeIds()

{

var memberId = User.GetMemberId();

var likes = await likesRepository.GetCurrentMemberLikeIds(memberId);

return Ok(likes);

}

  

[HttpGet]

public async Task<ActionResult<IReadOnlyList<Member>>> GetMemberLikes(string predicate)

{

var memberId = User.GetMemberId();

var members = await likesRepository.GetMemberLikes(predicate, memberId);

return Ok(members);

}

}
```