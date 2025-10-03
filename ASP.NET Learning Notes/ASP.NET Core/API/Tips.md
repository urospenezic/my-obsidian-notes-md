
Remove API.http file. Its used for outdated testing, Postman is king.

When starting development of api, change the appsettings.Developmnet.json logging to information for everything

when removing http project config from launchSettings, check dotnet dev-certs https --check. ##that will tell us whether or not the cert is working

**add appsettings or any config file that might have public API keys into git ignore for the love of god


**EF has issues with string comparison in a sense that AnyAsync(x=> x.Email.ToLower() == email.ToLower()) works, but x=> String.Compare(x.Email, email, StringComparison.OrdinalIgnoreCase) does not.

