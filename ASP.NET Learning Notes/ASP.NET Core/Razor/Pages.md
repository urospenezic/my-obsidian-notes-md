All razor pages exist within a local private razor page called _Layout.cshtml. Here we can edit nav bar to include routes to other razor pages.

In terms of MVC - cshtml files are Views and cshtml.cs files can be considered controllers for those views.

Inside of cshtml.cs is OnGet method - that responds to HTTP GET requests. So we can add properties to this class and have OnGet alter them, which will happen any time GET request is called

Similarly to this, OnPost method will respond to any POST requests.

Any property that cshtml.cs class has can be accessed in the view via @Model


We switch from HTML to C# via @, like: 
<tbody>
	@foreach(var stuff in stuffCollection){
		<tr>
			<td>@stuff.Name</td>
		</tr>
	}
</tbody>


There is a special helper called _IHtmlHelper that we can inject into our controllers. What this can do, for example, is transform an enum into an html <select> option list.