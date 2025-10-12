

**angular.dev/guide/forms

Template driven and reactive. Check the maximillian project on forms to see the difference. TLDR: template driven are easier to manage and offer two-way data binding. quick to set up. reactive are a lot more robust and offer tons of features, but require 5k lines of code for a simple filtering form


**Template-driven forms
--

They use # to assign names to elements which are then usable in code behind. <form #loginForm="ngForm"> will be able to be referenced in code behind

[(ngModel)]="creds.email" -> two way binding syntax

 #emailCtl="ngModel" is syntax supported by ngModel directive that tells angular that we wanna store this control created by angular into a variable called emailCtl-->

**class bindings for template forms
we can disable elements for example if form is not dirty like: [disabled]="!editForm.dirty"


**Reactive forms
--
Forms based on models that change over time using observable streams. A lot more customization and a lot more powerful than template-driven forms, but require in some cases a lot setup.

Example:
//import ReactiveFormsModule
`protected registerForm: FormGroup = new FormGroup({});`

`ngOnInit(): void {`
	`this.initializeForm();`
`}`

//register inputs
`initializeForm() {`
	`this.registerForm = new FormGroup({`
		`email: new FormControl('', [Validators.required, Validators.email]),//in html we bind to these via formsControlName="nameProvidedhere"`
		`displayName: new FormControl('', [Validators.required]),`
		`password: new FormControl('', [Validators.required, Validators.minLength(8)]),`
		`confirmPassword: new FormControl('', [Validators.required]),`
	`});`
`}`

**or via forms builder service:

`initializeForm() {`
	`this.registerForm = this.formBuilder.group({`
		`email: ['', [Validators.required, Validators.email]],`
		`displayName: ['', [Validators.required]],`
		`password: ['', [Validators.required, Validators.minLength(8)]],`
		`confirmPassword: ['', [Validators.required, this.matchValues('password')]],`
	`});`
	`this.registerForm.controls['password'].valueChanges.subscribe(() => {`
		`this.registerForm.controls['confirmPassword'].updateValueAndValidity();`
	`});`
`}`

**Custom validation:

change confirm password setup:
`confirmPassword: new FormControl('', [Validators.required, this.matchValues('password')]),`

below in initializeForm do:
`this.registerForm.controls['password'].valueChanges.subscribe(() => {`
	`this.registerForm.controls['confirmPassword'].updateValueAndValidity();`
`});`

this will effectively subscribe to value changes (remember, reactive forms utilize observables, so we can listen to input changes) and prop validators when the password input value changes

Finally, custom validator:

`matchValues(matchTo: string): ValidatorFn {`
	`return (control: AbstractControl): ValidationErrors | null => {`
		`//all angular field inputs derive from AbstractControl`
		`const parent = control.parent as FormGroup;`
		`if (!parent) return null;`
		`const matchValue = parent.get(matchTo)?.value;`
		`if (control.value === matchValue) return null;`
		`return { passwordMissmatch: true };`
	`};`
`}`

matchTo is the value of input form field where the validator is setup

**Displaying validation errors:

<label class="floating-label w-full text-left">

<span class="label-text w-full">Display Name</span>

<input

type="text"

class="input w-full"

formControlName="displayName"

placeholder="Display Name"

[class.input-error]="

registerForm.controls['displayName'].touched &&

registerForm.controls['displayName'].invalid

"

/>

@if( registerForm.controls['displayName'].touched &&

registerForm.controls['displayName'].invalid ) {

<div class="text-error text-sm validation-hint">Display Name is required</div>

}

</label>

