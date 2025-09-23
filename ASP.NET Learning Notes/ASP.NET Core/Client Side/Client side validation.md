
asp-for that we setup for validation with the help of asp.net will in the background setup a jquery-validation tags to those html elements that will be served to the client. 

so the model adorners like:
[Required, StringLength(80)]
	 public string Name {get; set;}

will inject into the html that's delivered to the client attributes like "data-val = true" "data-val-length-max=80" etc. that way we actually get proper client side validation out of the box

Inside of shared folder is ValidationScripts.cshtml file that gets referenced by Layout.cshtml. That one has pointers to jquery-validation libraries

**Check the ClientRestaurants.cshtml to see a mockup of some client code calling our backend to fetch the restaurants.