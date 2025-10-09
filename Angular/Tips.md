
In typescript we can throw someRandomArray.flat(); or any object for that matter. That will be the exception object being passed around. Seen it whilst doing a validation error http interceptor. It was used to collect all KVP pairs in an error (like 'email is required' 'password too short') and just return that error to components so that they can do anything they want with it. We can store these errors in our components where we use randomDataFetchingSerice.getRequest() -> in this observables error will be the array. We can then use that array to render a notification of a list of issues with validation

----------------------

Async pipes auto dispose observable subscriptions, so we can use them for quickly binding to some observable data in the UI instead of going for full code behind approach. Syntax is like:

protected propertyName$ : Obsevable<typeWeChoose/>;// the '/' is just here in obsidian 
constructor(){ this.propertyName$ = this.serviceForFetching.getRequest()}

we import AsyncPipe and we use it like: @for(item of propertyName$ | async; track by id) in HTML

-----------------------------------------------------------------------------------------------



**JSON TO TS websites offer auto generation of models from json responses

--------------

**EXTRA SPACING IN URLS IN MARKUP WILL TRIGGER ANGULARS SECURITY MECHANISM AND MARK THEM AS UNSAFE AND UNABLE TO DISPLAY. SO SRC="{{PHOTO.URL}}" IS FINE, BUT " {PHOTO.URL} " IS NOT