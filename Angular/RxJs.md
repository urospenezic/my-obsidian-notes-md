
Observable -> like a Task in .NET. Used to manage async data via subscriber-publisher pattern.


it has .subscribe() .pipe() methods. check the doc. subscribe observable has next, error and complete props where we can specify what happens when:

ngOnInit(): void {

    this.httpClient.get('https://localhost:7241/api/members').subscribe({

      next: (members) => console.log(members),

      error: (err) => console.log(err),

    });

  }


