All razor pages exist within a local private razor page called _Layout.cshtml. Here we can edit nav bar to include routes to other razor pages.

In terms of MVC - cshtml files are Views and cshtml.cs files can be considered controllers for those views.

Inside of cshtml.cs is OnGet method - that responds to HTTP GET requests. So we can add properties to this class and have OnGet alter them, which will happen any time GET request is called

Any property that cshtml.cs class has can be accessed in the view via @Model


