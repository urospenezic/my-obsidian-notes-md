validation can be set at attributes on model classes, like: 

public class Model{
	[Required, StringLength(80)]
	 public string Name {get; set;}
}

There is also a IValidatableObject interface that can be implemented for more custom validation code.

To run this validation, we can access the object called "ModelState". We can use it in various ways, like ModelState["propName"].Errors or ModelState["propName"].AttemptedValue (attempts the user tried) to see potential error. The most common is ModelState.IsValid -> true if all the check succeeded. 

asp-validation-for attribute will tell the user of any failed validations. added typically in spans of form inputs.

