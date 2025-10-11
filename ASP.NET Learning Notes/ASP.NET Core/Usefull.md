
ILogger<T> is good to inject everywhere, it's prebuilt logger
IHostEnvironemnt can be injected to check which env we're on

[FromForm] inside of controller endpoint params -> specifies that the endpoint only accepts param if it was passed inside of form-data object in body. In Angular we got FormData class to populate with form data.