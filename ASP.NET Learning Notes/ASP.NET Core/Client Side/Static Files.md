
Inside of wwwroot is all of the client code that will be served via get requests for pages. Anything not in that folder will be invisible to the outside world


Inside of lib folder that's inside of wwwroot are the js and css libraries like jquery, bootstrap..

These libraries are imported inside of Layout.cshtml file at the <environment> element. Notice that this element also has "include"  tag to specify Development vs Production mode. We can add custom environments like QA, Staging etc. Typical example of development env is serving jquery and other big libraries from the local machine, whilst in production we wanna use content delivery network like aspnetcdn.com

launchSettings.json is where we specify which environment runs in which mode. IIS Express is local server


