
Name of the game is a data structure named TempData. It is a bit like a dictionary of key-value pairs. 

TempData["MyCustomMessage"] = "Hello from temp data!"

Any razor page controller can then bind to this temp data like so:

[TempData]
public string MyCustomMessage {get;set;}

this will auto fill when temp data has data with the key of propName.

if markup we can do:

@if(Model.MyCustomMessage is not null){
	<div class="alert alert-message">@Model.MyCustomMessage</div>
}



This data will dissappear as soon as the user refreshes the page.