
Best way to catch server error before any components get them is to use an HTTP Interceptor.Interceptors are used to intercept and modify HTTP requests and responses before they reach their destination. This allows you to perform tasks like adding authentication headers, handling errors, or logging requests. 

Example of error catching interceptor:

`export const errorInterceptor: HttpInterceptorFn = (req, next) => {`
	`const router = inject(Router);`
	`const toast = inject(ToastService);`
	`return next(req).pipe(`
		`catchError((error) => {`
			`if (error) {`
				`switch (error.status) {`
					`case 400:`
						`toast.error(error.error);`
						`break;`
					`case 401:`
						`console.error(error);`
						`toast.error('Unauthorized access');`
						`break;`
					`case 404:`
						`console.error(error);`
						`toast.error('Not found');`
						`break;`
					`case 500:`
						`console.error(error);`
						`const navigationExtras = { state: { error: error.error } };`
						`router.navigateByUrl('/server-error', navigationExtras);`
						`break;`
					`default:`
						`console.error(error);`
						`toast.error('Something unexpected went wrong');`
						`break;`
				`}`
			`}`
			`return throwError(() => new Error('Something bad happened; please try again later.'));`
		`})`
	`);`
`};`
To use an interceptor, we gotta add via app.config.ts:

provideHttpClient(withInterceptors([customInterceptr]))