
Read the docs for existing pipes like AsyncPipe, DatePipet, ways to format numbers etc. Pipes are basically WPF converters on steroids. From CLI: Pipes are used to transform data for display in templates. They take input values and apply a specific transformation, such as formatting dates, currency, or filtering arrays. 


**Creating custom pipe:

ng g p
 Example (converting a date of birth passed as string to number representing age):
 
 `@Pipe({`
`name: 'age',`
`})`
`export class AgePipe implements PipeTransform {transform(value: string): number {`
	`var today = new Date();`
	`var date = new Date(value);`
	`var age = today.getFullYear() - date.getFullYear();`
	`var m = today.getMonth() - date.getMonth();`
	`if (m < 0 || (m === 0 && today.getDate() < date.getDate())) {`
		`age--;`
	`}`
	`return age;`
	`}`
`}`

We using withg ' | ' syntax, so like {{user.joinDate | date:'long date'}}