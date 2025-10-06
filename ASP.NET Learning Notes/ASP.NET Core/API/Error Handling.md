
- Returning direct C# Exception inside of controller methods will result in 500 server error being returned. In case of developer mode running, there's a hidden middleware added by asp.net "app.UseDeveloperExceptionPage"
- in case of using prebuilt respones like Unauthorized() or NotFound() - a nicely formatted JSON object is returned with a status and a message
- BadRequest() prebuilt can accept a text which it returns. haven't checked the others
- For custom error responses, we can build a custom ExceptionMiddleware
- 