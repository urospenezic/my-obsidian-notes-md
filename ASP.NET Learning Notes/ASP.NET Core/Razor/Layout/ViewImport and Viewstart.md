Asp will auto look for ViewStart cshtml file when launching. If we place a view here, that will be the default inherent view for any page we create. 

**ViewImports

Add using statements that will auto added for other pages, like: @using NameSpace

here we also add tag helpers that will be included with any page, like:
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers

We can add custom tag helpers, those have to imported here. 

