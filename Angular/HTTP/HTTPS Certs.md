
https://github.com/FiloSottile/mkcert - creates local certificate authority. That means that any certs created by mkcert are going to be automatically trusted.

Depends on the OS, for Arch: sudo pacman -Syu mkcert
MacOs is: brew install mkcert
Windows: choco install mkcert (if you have no choco, get it lolz)

To use it run:

mkcert "address" (for example mkcert localhost). That creates a cert.
Then in angular.json look for "serve". There add a "options":{ "ssl": true, "sslCert":"./ssl/localhost.pem", "sslKey":"./ssl/localhost-key.pem"}

With this cert we will be running on https. 

I will not be doing this for the demo projects because i tend to develop on 3 different machines and 3 different operating systems and it seems like a big hassle for development env at this point in time.



