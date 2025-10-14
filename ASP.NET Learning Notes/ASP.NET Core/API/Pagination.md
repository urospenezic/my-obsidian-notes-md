
From Bottom up we got:

**Setting up required params:

```
public class PagingParams
{
private const int _maxPageSize = 50;

public int PageNumber { get; set; } = 1;
  

private int _pageSize = 10;
public int PageSize
{
get => _pageSize;
set => _pageSize = Math.Min(value, _maxPageSize);
}
}

public class PaginationMetadata

{
public int TotalCount { get; set; }

public int PageSize { get; set; }

public int CurrentPage { get; set; }

public int TotalPages { get; set; }
};

public class PaginatedResult<T>

{
public PaginationMetadata Metadata { get; set; } = default!;

public List<T> Items { get; set; } = default!;
};
```

**We then use these to build a query using linq to return an amount of data back:

```
public static class PaginationHelper

{

public static async Task<PaginatedResult<T>> CreateAsync<T>(

IQueryable<T> query,

int pageNumber,

int pageSize

)

{

var count = await query.CountAsync();

var items = await query.Skip((pageNumber - 1) * pageSize).Take(pageSize).ToListAsync();

  

return new PaginatedResult<T>

{

Metadata = new PaginationMetadata

{

TotalCount = count,

PageSize = pageSize,

CurrentPage = pageNumber,

TotalPages = (int)Math.Ceiling(count / (double)pageSize),

},

Items = items,

};

}

}
```

**Then we use this in our repository:

```
public async Task<PaginatedResult<Member>> GetMembersAsync(PagingParams pagingParams)

{

return await PaginationHelper.CreateAsync(

_context.Members.AsQueryable(),

pagingParams.PageNumber,

pagingParams.PageSize

);

}
```

**And finally in the controller endpoint:

```
[HttpGet]

public async Task<ActionResult<IReadOnlyList<Member>>> GetMembers(

[FromQuery] PagingParams pagingParams

)

{

var members = await memberRepository.GetMembersAsync(pagingParams);

return Ok(members);

}
```

