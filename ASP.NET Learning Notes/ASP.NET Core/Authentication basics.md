
Storing password via hash:
1. is a one way street, so we cannot generate a password text from the hash code, which is good
2. problem is that the same hash is applied to same strings, so if 2 users have the same password, hackers may figure out a way to instantly have access to multiple accounts

Hashing and salting the password:
1. better than just hashing
2. salting is a hashing algo that scrambles the hash

ASP.NET Core Identity:

1. the go to way which is ultra secure with best practices



For very basic auth that has no security we can use HMACSHA512 to salt. The instance of hmac has computeHash function that generates a hash. The salt is Key property of hmac.

-------------------------------------------------------------------
JWT
--

To authenticate to an API, we typically use JsonWebTokens. A simple HTTP API does not track any sessions, so JWT is small enough to be ok to pass in with every request.

JWT is an Industry Standard for tokens (RFC 7519). it's self contained and can contain: credentials, claims, other information.

**JWT STRUCTURE:

1. it's a long string separated into 3 parts via a period
2. first part is the header that specifies an algorithm and token type used to encrypt the signature in third part, so something like { "alg": "HS512", "typ": "JWT"}
3. second part is the actual payload, so something like {"nameId": "lola", "role":"member", "nbf":1293891, "exp": 21389849289, "iat": 2983924891} (exp- expirationm, nbf- valid until, iat - issued at)
4. third part is the verify signature, so : HMACSHA512(base64UrlEncode(header)+"."+base64Encode(payload), your-256-bit-secret) secret base64 encoded


The token signature is encrypted by the server using a secure key that never leaves the server.

Typical auth workflow via a token looks like:
1. user sends username and password
2. server validates credentials and returns JWT that the client keeps
3. the client sends the JWT with each further request
4. server verifies JWT and sends responses

Benefits of JWT are:
1. No need for session tracking since tokens are very lightweight
2. Portable -> single token can be used for multiple backends
3. No cookies required -> mobile friendly, not just web
4. Performance -> once a token is issued, there is no need to make a database request to verify a user authentication


**IMPORTANT!! WE GOTTA USE MICROSOFT.IDENTITYMODEL.TOKENS AND SYSTEM.IDENTITYMODEL.TOKENS.JWT NUGETS FOR THIS. EXAMPLE OF A TOKEN SERVICE USING HMACSHA512 (MOST OF THIS IS BOILERPLATE YOU WRITE FOR EVERY TOKEN SERVICE):

`public class TokenService(IConfiguration config) : ITokenService`
`{`
    `public string CreateToken(AppUser user)`
    `{`
        `var tokenKey = config["tokenKey"] ?? throw new Exception("Token key not found");`
        `if (tokenKey.Length < 64) //since we are using HmacSha512`
        `{`
            `throw new Exception("Token key must be at least 64 characters long");`
        `}`
        `//requires nuget package Microsoft.IdentityModel.Tokens`
        `var key = new SymmetricSecurityKey(System.Text.Encoding.UTF8.GetBytes(tokenKey));`
        `var claims = new List<Claim>`
        `{`
            `new(ClaimTypes.Email, user.Email),`
            `new(ClaimTypes.NameIdentifier, user.Id),`
        `};`

        `var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha512Signature);`

        `var tokenDescriptor = new SecurityTokenDescriptor`
        `{`
            `Subject = new ClaimsIdentity(claims),`
            `Expires = DateTime.Now.AddDays(7),`
            `SigningCredentials = creds,`
        `};`

        `var tokenHandler = new JwtSecurityTokenHandler(); //class that actually creates the token`
        `var token = tokenHandler.CreateToken(tokenDescriptor);`
        `return tokenHandler.WriteToken(token);`
    `}`
`}`

TokenKey lives inside of config appsettings like: "TokenKey": "super secret key that is at least sixty four characters long for security reasons and stuff like that"


Read about SymmetricSecurityKey. It uses the same object to encrypt and decrypt. We also got Asymmetric one which is mostly used for certificates.


**CLAIMS are info we know about the user using the token. So a claim like an email, nameidentifier. ClaimTypes are predefined, we can define custom claim types by doing: new Claim("customWhatever", "customThing"). Once we have a token for a User, we have this User global property we can access inside our controllers like User.FindFirstValue(ClaimTypes.NameIdentifier) to get id. we set those up in token service.



**JWT.IO - Json Web Token Debugger

---------------------------------------------------
Auth middleware and validation
--

[Authorize] is a adorner we can use on any controller that makes any request that is not authenticated return Unauthorized.  This can also adorn a method, so we don't have to [Authorize] the entire controller and then [AllowAnonymous] -> we can just flag any requests that require authentication.
[AllowAnonymous] is used to bypass [Authorize] on certain requests of the controller. The other way around does not work.

Adding middleware 

**REQUIRES MICROSOFT.ASPNETCORE.AUTHENTICATION.JWTBEAER NUGET

To add authentication as a service, we go:

1. builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
				.AddJwtBeare(options => {
					var tokenKey = builder.Configuration["TokenKey"];
					options.TokenValidationParameters = new() {
						ValidateIssuerSigningKey = true;
						IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(tokenKey)),
						ValidateIssuer = false,
						ValidateAudience = false,
					}
				});
2. TokenValidationParams is a config on how do we want to validate a token passed through middleware
3. ValidateIssuerSigningKey = true -> dont allow any token, check with 4
4. Check with this
5. ValidateIssuer if true will check who issued the token, which is an extra layer of protection because we can sign tokens on issue and check the issuer


We can now add middleware to our pipeline. Ordering is important, so we go after Cors, but pretty high up the foodchain:

app.UseAuthentication();
app.UseAuthorization();


