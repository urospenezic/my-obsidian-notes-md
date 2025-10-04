
HttpClient is used for sending https request. We inject it via inject(HttpClient);

post, get and all methods are generic, so post<User> is valid and we will not have to type check the response.