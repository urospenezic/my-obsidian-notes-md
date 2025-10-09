https://angular.dev/guide/routing


top level routes are defined in app/app.routes.ts

Layout:

{ path: '**', component: Home }, -- page not found

{ path: 'someRoute/:query', component: component }, -> like baseUrl/someRoute/query


routes are accessed via the top level `<router-outlet>` usually residing within app.html. so the `<router-outlet>` is replaces with whatever content/component is described at a certain URL route in app.routes.ts These outlets are used to acces child routes as well (so we can display a custom child route that can even take a part of the parent component, not the full thing)

 `protected router = inject(Router); with imported RouterOutlet`


**NOTE!!!!! NgClass is a module?? to be imported for conditional CSS class append. So, a div with [ngClass]="{'mt-24' : !(router.url === '/')}" is valid syntax to say "apply this style if the current router url is not a home page"

**NOTE 2!!!! IN THE STYLE GUIDES, THE ANGULAR DEVS SPECIFY NOT TO USE [ngClass] BUT RATHER A NEW SYNTAX, WHICH LOOKS LIKE: [class.mt-24]="router.url !== '/'". I assume we have to copy paste this attribute a gazzillion times... for each class... yep... 

`<div`
  `[class.mt-24]="router.url !== '/'"`
  `[class.container]="router.url === '/'"`
  `[class.mx-auto]="router.url === '/'"`
`>`
  `<router-outlet />`
`</div>`

i don't like this at the moment. leaning more towards the ngClass. I would be inclined to use the styling guides approach if CSS was a bit more OOP.



-------------------------------------------
Defining route links
--


We gotta have a nav bar component ofc. Inside of that component there is hopefully a list of some anchor tags or something.

To set up a route, we can add an attribute to an anchor like: `<a routerLink = "/users">`. We have to import RouterLink in the ts file of the nav component ofc.

**routerLinkActive is a directive to specify a css class when a certain anchor for a route is active. so routerLinkActive="text-accent" for example


-----------------------------------------

Routing in code behind
--

In any component that needs to redirect at some point -> import a Router. Then simply do a this.router.navigateByUrl('relativePath');

**WE CAN USE THIS INSIDE OF INTERCEPTORS

If state is passed to a route, we can access it in components constructor:
const navigation = this.router.currentNavigation();
this.error.set(navigation?.extras?.state?.['key']);

-----------------------------------------

**Adding toasts
--


Toasts are like a little notifications that are less on the nose than typical JS alerts. 

ngx-toastr is a 3p lib to display toasts with js baked in to cancel it. daisy ui also has them, but with just pure css, so no js baked in (so no button to dismiss it). **just use daisy ui with custom js, fuck 3p libs (unless you switch jobs every year so that 3p libs don't bite you in the ass). generate a toast service with pure js DOM manipulation to manipulate the toasts:

`import { Injectable } from '@angular/core';`
`@Injectable({`
  `providedIn: 'root',`
`})`
`export class ToastService {`
  `constructor() {`
    `this.createToastContainer();`
  `}`
  `private createToastContainer() {`
    `if (!document.getElementById('toast-container')) {`
      `const container = document.createElement('div');`
      `container.id = 'toast-container';`
      `container.className = 'toast toast-bottom toast-end';`
      `document.body.appendChild(container);`
    `}`
  `}`
  `private createToastElement(message: string, alertClass: string, duration = 5000) {`
    `const toast = document.getElementById('toast-container');`
    `if (!toast) return;`
    `const toastElement = document.createElement('div');`
    `toastElement.classList.add(alert, alertClass, shadow-lg);`
    `toastElement.innerHTML = `
      `<div>`

        `<span>${message}</span>`

        `<button class="ml-4 btn btn-sm btn-ghost"x</button>`

      `</div>`

    ``;  toastElement.querySelector('button')?.addEventListener('click', () => {`
      `toastElement.remove();`
    `});`
    `toast.append(toastElement);`
    `setTimeout(() => {`
      `if (toast.contains(toastElement)) {`
        `toast.removeChild(toastElement);`
      `}`
    `}, duration);`
  `}`
  `success(message: string, duration?: number) {`
    `this.createToastElement(message, 'alert-success', duration);`
  `}`
  `error(message: string, duration?: number) {`
    `this.createToastElement(message, 'alert-error', duration);`
  `}`
  `warning(message: string, duration?: number) {`
    `this.createToastElement(message, 'alert-warning', duration);`
  `}`
  `info(message: string, duration?: number) {`
    `this.createToastElement(message, 'alert-info', duration);`
  `}`
`}`


To use this we inject ToastService and go like this.toastService.error('message');


**DAISY UI HAS A THEME GENERATOR???? daisyui.com/theme-generator



------------------------------------------

Route Guards
--

ng g g[name]  ->  Creates a new route guard in your project. Route guards are used to control access to parts of your    application by checking certain conditions before a route is activated. This schematic generates a new guard with the specified name, type, and options.      

CLI will ask us to select which action to guard from

Guards moved away from OOP to more of a functional programming, so all guards are now essentially funcions

**Do not forget that we can inject into functions as well in angular!!!!

To add a route guard to a route, got to wherever the route is specified and add canDoSomething (DoSomething is replaces by Activate, ActivateChild, Match...) and add a array of guards like: canActivate=[authGuard]
**to avoid copy pasting the same guard to multiple routes, consider creating dummy routes that contain those routes as children (but do not alter the URL navigation links):

`export const routes: Routes = [`
`{ path: '', component: Home },`
`{`
`path: '',`
`canActivateChild: [authGuard],`
`children: [`
`{ path: 'members', component: MemberList },`
`{ path: 'members/:id', component: MemberDetail },`
`{ path: 'lists', component: Lists },`
`{ path: 'messages', component: Messages },`
`],`
`},`
`{ path: '**', component: Home },`
`];`


OR:

`export const routes: Routes = [`
`{ path: '', component: Home },`
`{`
`path: '',`
`runGuardsAndResolvers: 'always',`
`canActivate: [authGuard],`
`children: [`
`{ path: 'members', component: MemberList },`
`{ path: 'members/:id', component: MemberDetail },`
`{ path: 'lists', component: Lists },`
`{ path: 'messages', component: Messages },`
`],`
`},`
`{ path: '**', component: Home },`
`];`


**APP INITIALIZATION
--

A mechanism for loading up things before route guards and other stuff happens. So if anything has to be initialized before the app starts rendering stuff:

`export class InitService {`
`private readonly accountService = inject(AccountService);`
`init() {`
`const user = localStorage.getItem('user');`
`if (user) {`
`this.accountService.setCurrentUser(JSON.parse(user));`
`}`
`return of(true);`
`}`

**WE CAN THEN USE THIS IS app.config.ts by adding a new provider (READ MORE ABOUT PROVIDERS IN ANGULAR COURSE PROJECTS):

`provideAppInitializer(async () => {`
	`const initService = new InitService();`
	`try {`
		`await lastValueFrom(initService.init());`
	`} catch (error) {`
		`console.error('Error during app initialization:', error);`
	`} finally {`
		`const splash = document.getElementById('initial-splash');`
		`if (splash) {`
			`splash.remove();`
		`}`
	`}`
`}),`

So we can fetch some data like configs for the app from the server. In Index.html we generate a 'initial-splash' div or whatever, a spinning thing. That will be showing until we fetch everything that the app needs in order to function. After the initializing service returns data, angular will auto display <app-root>. So in index we'll have like `<spinningStuff/>` and below that `<app-root/>`

**Route animations
--

change the provideRouter provider to have withViewTransitions. That thing is customizible, but by default gives a fading animation. so provideRouter(routes, withViewTransitions() )


**ROUTES FROM HTML:

<button routerLink="/someRoute"/>

OR WE CAN inject(Location) which contains routing history:

private location = inject(Location);

goBack() {
this.location.back();
}

and just use that on click

---------------------------------------------------------

**We can inject(ActivtedRoute) for the route info in code behind of active navigation, like route.shapshots.paramMap.get('key') property which has value that is picked up by url (like users/4 -> get('id') it will contain 4)


**ROUTERLINKACTIVE in HTML binds to a css class that will activate if the specified route is active. Ofc required routerLink=path as well

*NESTED ROUTING*
--

{

path: 'members/:id',

component: MemberDetail,

children: [

{ path: '', redirectTo: 'profile', pathMatch: 'full' },

{ path: 'profile', component: MemberProfile, title: 'Profile' },

{ path: 'photos', component: MemberPhotos, title: 'Photos' },

{ path: 'messages', component: MemberMessages, title: 'Messages' },

],

},

the first empty route is used as fallback a default/fallback. pathMatch full means that router will not stop checking the url from left to right and stop at the first accurance of passed segment.


**ROUTER EVENTS (OBSERVABLES)
We can listen to router events like: Router.events() : 
ngOnInit(): void {
this.member$ = this.loadMember();
this.title.set(this.route.firstChild?.snapshot.title|| 'Profile');
this.router.events.pipe(filter((event) => event instanceof NavigationEnd)).subscribe(() => {
this.title.set(this.route.firstChild?.snapshot.title || 'Profile');
});
}



** documentation for other router events

---------------------------------------------------------------------------

**ROUTE RESOLVERS

A way to pass data to child routes, or any routes? It executes just before the route loads up. Can be made by CLI using "ng g r". From CLI help:
Resolvers are used to pre-fetch data before a route is activated, ensuring that the necessary data is available before the component is displayed. This can improve the user experience by preventing delays and loading states. *IT APPEND -resolver TO THE NAME*. 
It's a function that looks similar to route guards except it returns ResolveFn<T>. We can actually fetch data inside of these (or go to cache).

To activate a router, we go to routes definitions and add {resolve: {nameOfThingToPass: routeResolver}} to any route definition. We can also add runGuardsAndResolvers: always. read on that in docs

These resolvers return an Observable basically, so we use them in any component like:

ActivatedRoute.parent.data.subscribe({
next: data => this.propertyOfComponent = data['nameOfThingToPass]
})