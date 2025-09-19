
We can route via query params as seen in Markup Attributes by specifying asp-route-propName. We can also go to cshtml of any page and change @page to @page {queryParam:type}. This way the page will be available at ./Page/param

We can also choose to make the param optional by placing a question mark at the end like {queryParam:type?}. This is useful to make dual mode pages. For example, edit and create usually have mostly the same form markup. So why not have the same page handle both?


To prevent exceptions in case users add a non existing param manually:

OnGet methods can return an IActionResult. There are many implementations of it, such as Page() or NotFound() or RedirectToPage(path)