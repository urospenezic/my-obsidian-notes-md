Before model binding, we had to pull query params through HttpContext.Request.Query in our OnGet method, or whichever method is serving the current HTTP request. 

Model binding simplifies that and makes it a bit more flexible.

To do model binding: 

Give form inputs a name. Use that name as a parameter to OnGet, OnPost.. http handler methods. ASP.NET will automatically link the 2 together. If that param is not found -> in case of a reference type it will pass in null. In case of value types it will throw an exception.


***** **TAG HELPERS** *****

One other way to get Model Binding in effect is to use BindProperty attribute. Simply create a property, add decorative attribute [BindProperty(SupportsGet = true)] and use the same name of that property as the name of any input field in the markup.

To bind our value of input field in HTML, we can use asp-for as a special attribute. Set that to our property name and we're good to go e.g. <input type="search" name="SearchTerm" class="form-control" placeholder="Search for text..." asp-for=SearchTerm/>


So, asp-for does a two-way binding to the given property. 

