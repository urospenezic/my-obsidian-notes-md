
Observable -> like a Task in .NET. Used to manage async data via subscriber-publisher pattern.


it has .subscribe() .pipe() methods. check the doc. subscribe observable has next, error and complete props where we can specify what happens when:

ngOnInit(): void {

    this.httpClient.get('https://localhost:7241/api/members').subscribe({

      next: (members) => console.log(members),

      error: (err) => console.log(err),

    });

  }

map() can be used along pipe to transform data

**ASYNC PIPE: <li *ngFor='let response of service.getRequest() | async'>{{response.Property}}/> automatically subs and unsubs from observables as the component gets destroyed, but hard to debug


tap() => does something with the data without modifying it. pipe(tap({})) is valid because pipe still returns an observable. tap invokes when there an error or response. whatever we do with that response will not change the structure of response being sent to other subscribers to observable. so pipe() to notify observable to let us know when the data is coming in. tap to do something with the response. because pipe predicted the tap, we will for sure be the first to get notified before subscribers waiting for the actual result.

