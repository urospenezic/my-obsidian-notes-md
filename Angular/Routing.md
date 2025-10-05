https://angular.dev/guide/routing


top level routes are defined in app/app.routes.ts

Layout:

{ path: '**', component: Home }, -- page not found

{ path: 'someRoute/:query', component: component }, -> like baseUrl/someRoute/query


routes are accessed via the top level `<router-outlet>` usually residing within app.html. so the `<router-outlet>` is replaces with whatever content/component is described at a certain URL route in app.routes.ts

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

ng g guard [name]  ->  Creates a new route guard in your project. Route guards are used to control access to parts of your    application by checking certain conditions before a route is activated. This schematic generates a new guard with the specified name, type, and options.       





