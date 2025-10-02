
Multiple lifecycle interfaces are available: https://angular.dev/guide/components/lifecycle

The general principle is: you implement an interface that forces you to implement a certain function that gets called every time something happens. So, there are a ton of these interfaces, so that they don't force you to react to every little angular bullshit.

For good practice always remember to unsub from observables, at least do a cleanup in OnDestroy. There's also a nifty little shortcut like a local function we can invoke inside of any function to do local cleanup on destroy. It's somewhere in my Angular projects (DestroyRef). 

We can alternatively use a property pattern for tracking observables by converting them to raw Promises:

async getRequest(){ try{ return lastValueFrom(this.httpClient.get(...))}} catch{}}

this way we can do:

async ngOnInit(){
	this.signalProp.set(await this.getMembers());
}

this way we no longer need to track subscriptions. i do like observables though, they provide nicer architecture.

I like this approach the best:

private readonly destroyRef = inject(DestroyRef);
ngOnInit() {

    //this.members.set(await this.getMembers() as any[]);

    const request =this.httpClient.get('https://localhost:7241/api/members').subscribe({

      next: (response) => this.members.set(response as any[]),

      error: (err) => console.log(err),

      complete: () => console.log('Request completed'),

    });

  

    this.destroyRef.onDestroy(() => {

      request.unsubscribe();

    });

  }


