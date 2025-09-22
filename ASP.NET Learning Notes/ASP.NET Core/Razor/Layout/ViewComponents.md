
ViewData object has several keys. ViewData["Title"] will set the title of the web page in the browser. Layout page does not know what to put there, so we place it manually inside of each @{} block for every page.


View Components use more of a MVC pattern than Razor Pages. View components are just c# classes that inherit from ViewComponent. What these do is just render some sort of View:

	public class RestaurantCountViewComponent(IRestaurantData restaurantData) : ViewComponent
	{
		private readonly IRestaurantData _restaurantData = restaurantData;
	
		public IViewComponentResult Invoke()
		{
			var count = _restaurantData.GetCountOfRestaurants();
		return View(count); // we can also specify the name of the view like "count":count
		}
	}


To render a View Component, we need to follow couple of rules:

1. In shared folder, add a new folder called Components
2. Inside of that folder, create a new folder with the same name as our view component class name. e.g. RestaurantCountViewComponent. Use -np or --no-pagemodel to generate a razor page without a model, so just a view with a name Default OR the name we specified in our Invoke() method. 
3. Setup a model for that view. In the case of our count component, we use @model int
4. Write a markup using that @Model
5. Go to ViewImports file and add a tag helper for the assembly, e.g. @addTagHelper *, our assembly name
6. Add it anywhere in any HTML by using: <vc:restaurant-count></vc:restaurant-count> **BE WARY OF SPELLING, WE HAVE TO USE THIS CASING









