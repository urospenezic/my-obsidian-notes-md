
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


-------------------

**generate reusable components for form inputs with validation error display and everything.
--

<label class="floating-label w-full text-left">

<span class="label-text w-full">{{ label() }}</span>

<input

[type]="type()"

class="input w-full"

[formControl]="control"

[placeholder]="label()"

[class.input-error]="control.touched && control.invalid"

[class.input-success]="control.valid && control.invalid"

/>

@if( control.hasError('required') && control.touched ) {

<div class="text-error text-xs validation-hint">{{ label() }} is required</div>

} @if(control.hasError('email') && control.touched) {

<div class="text-error text-xs validation-hint">Please enter a valid email address</div>

} @if(control.hasError('minlength') && control.touched) {

<div class="text-error text-xs validation-hint">

{{ label() }} must be at least {{ control.getError('minlength').requiredLength }} characters

long (currently {{ control.getError('minlength').actualLength }})

</div>

} @if(control.hasError('passwordMissmatch') && control.touched) {

<div class="text-error text-xs validation-hint">Passwords do not match</div>

}

</label>



`@Component({`
`selector: 'app-text-input',`
`imports: [ReactiveFormsModule],`
`templateUrl: './text-input.html',`
`styleUrl: './text-input.css',`
`})`
`export class TextInput implements ControlValueAccessor {`
`//ControlValueAccessor tells angular this is a form control`
`label = input<string>('');`
`type = input<string>('text'); //text, password, email, etc`
`//@Self() tells the angular to only look up the ref we're injecting on the current element, not to seach it up the injector tree`
`//it guarntees that this control is unique for this text input we use inside of form`
`constructor(@Self() public ngControl: NgControl) {`
`ngControl.valueAccessor = this; //bind this control value accessor to the ngControl`
`}` 
`writeValue(value: any): void {}`
`registerOnChange(fn: any): void {}`
`registerOnTouched(fn: any): void {}`
`get control() {`
`return this.ngControl.control as FormControl;`
`}`
`}`


--------------------------------------------------

**ON LINUX CHANGE DEVELOPMENT ENVIRONMENT URL TO apiUrl: 'https://127.0.0.1:7241/api' INSTEAD OF LOCALHOST BECAUSE LINUX CANNOT AUTO SWITCH BETWEEN IPV4 AND V6

------------------

**SCROLL TO BOTTOM:

<div #messageEndRef></div> place a div like this at the bottom of the thread (chat thread, content thread that keeps updating, whatever)

Give it a reference in component ts:
  @ViewChild('messageEndRef') messageEndRef!: ElementRef;
  
And finally:

 scrollToBottom() {
    //set timeout to ensure that the current js call stack is cleared before scrolling
    setTimeout(() => {
      try {
        if (this.messageEndRef) {
          this.messageEndRef.nativeElement.scrollIntoView({ behavior: 'smooth' });
        }
      } catch (err) {
        console.error('Scroll to bottom failed:', err);
      }
    });
  }
  
  
  And we use this via effect:
  
    constructor() {
    effect(() => {
      const currentMessages = this.messages();
      if (currentMessages.length > 0) {
        this.scrollToBottom();
      }
    });
  }
 