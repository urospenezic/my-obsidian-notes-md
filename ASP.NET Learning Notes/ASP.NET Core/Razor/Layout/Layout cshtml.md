
Layout page with the leading underscore is not a razor page, it is a razor view. That means that that page cannot respond to HTTP request, but is just a plain organizing view spec. 

Convention is that any ASP.NET page that is meant to be global/shared is placed within the shared folder with a leading underscore in its name.

The view layout page controls structure of UI: what scripts and css files get included with each page, copyright footer etc. Anything shared.

The layout page has the following div (expand it to see html in obsidian):

<div class="container">

<main role="main" class="pb-3">

@RenderBody()

</main>

</div>

That render body will get replaced with concrete razor page currently active.

One hacky thing we can do is this:

add  @RenderSection(nameOfSection, required:true/false) anywhere in layout page. Each razor page can then within its own view specify:

@section sameNameAsRenderSection {
	html code
}

that piece of html will render wherever we specified render section. so razor pages can effectively add stuff to footer for example from their own cshtml even though they have no idea how the structure of the main layout view looks like.

**We can create a razor page without a layout by setting Layout = null inside of @{} at the beginning of the page

If we do this: we lose any css bootstrap sheets that a layout page passes to rendered pages. 

**WE CAN HAVE MULTIPLE LAYOUT PAGES, WE JUST CREATE OUR OWN AND PLACE THEM IN SHARED FOLDER, ASP.NET WILL AUTO LOOK IN THAT FOLDER FOR THEM IF WE SPECIFY THEM IN ANY OF OUR PAGES

