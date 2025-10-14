```
public class MemberParams : PagingParams

{

public string? CurrentMemberId { get; set; }

public string? Gender { get; set; }

}



public async Task<PaginatedResult<Member>> GetMembersAsync(MemberParams memberParams)

{

var query = _context.Members.AsQueryable();

  

if (!string.IsNullOrEmpty(memberParams.CurrentMemberId))

{

query = query.Where(m => m.Id != memberParams.CurrentMemberId);

}

  

if (!string.IsNullOrEmpty(memberParams.Gender))

{

query = query.Where(m => m.Gender == memberParams.Gender);

}

  

return await PaginationHelper.CreateAsync(

query,

memberParams.PageNumber,

memberParams.PageSize

);

}



[HttpGet]

public async Task<ActionResult<IReadOnlyList<Member>>> GetMembers(

[FromQuery] MemberParams memberParams

)

{

memberParams.CurrentMemberId = User.GetMemberId();

var members = await memberRepository.GetMembersAsync(memberParams);

return Ok(members);

}
```