Reusing parts of markup to encapsulate UI so that we can handle complex UI more robustly. 

We just generate a new page without a model, remove the @page and everything else. So plain view. 

Add @model for a data structure this time, like say Restaurant. We have to include a using statement here:

@using pathToModelRepo
@model Restaurant

We can then add reusable markup:
<div class="card mb-3">

<div class="card-body">

<div class="card header mb-2">

<img src="https://picsum.photos/600/200" class="card-img-top" alt="Restaurant Image">

</div>

<div class="card body">

<h5 class="card-title">@Model.Name</h5>

<p class="card-text">

<strong>Cuisine:</strong> @Model.Cuisine <br />

<strong>Location:</strong> @Model.Location

</p>

</div>

<div class="card-footer">

  

<a asp-page="./Details" asp-route-restaurantId="@Model.Id" class="card-link">

<i class="bi bi-search"></i></a>

<a asp-page="./Edit" asp-route-restaurantId="@Model.Id" class="card-link"> <i class="bi bi-pencil"></i></a>

<a asp-page="./Delete" asp-route-restaurantId="@Model.Id" class="card-link text-danger"> <i

class="bi bi-trash"></i></a>

</div>

</div>

</div>



We can then use this view in any page like:

<partial name="_PartialViewName" model="property"/>



