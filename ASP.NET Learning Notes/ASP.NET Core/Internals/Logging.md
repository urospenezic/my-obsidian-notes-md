
appSettings.json is where logging options can be configured.

we can inject ILogger<T> into any razor page data model. From here we can use _logger.LogInformation()_ methods to log any OnGet/OnPost etc.. requests
