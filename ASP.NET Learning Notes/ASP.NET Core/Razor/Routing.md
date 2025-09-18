
We can route via query params as seen in Markup Attributes by specifying asp-route-propName. We can also go to cshtml of any page and change @page to @page {queryParam:type}. This way the page will be available at ./Page/param


To prevent exceptions in case users add a non existing param manually:

OnGet methods can return an IActionResult. There are many implementations of it, such as Page() or NotFound() or RedirectToPage(path)