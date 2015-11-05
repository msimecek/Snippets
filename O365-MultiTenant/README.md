O365 - multitenant s kontrolou tenantu
===

**Startup.Auth.cs**:

```csharp
private static string authority = aadInstance + "common/";
```

```csharp
app.UseOpenIdConnectAuthentication(
	new OpenIdConnectAuthenticationOptions
	{
		ClientId = clientId,
		Authority = authority,
		PostLogoutRedirectUri = postLogoutRedirectUri,
		TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters()
		{
			ValidateIssuer = false
		},
		Notifications = new OpenIdConnectAuthenticationNotifications()
		{
			SecurityTokenValidated = (context) =>
			{
				string tenantId = context.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
				if (tenantId != "[tenantId]" && tenantId != "[tenant2Id]")
				{
					throw new SecurityTokenValidationException("Only microsoft.com or specific accounts are allowed to sign-in.");
				}
				return Task.FromResult(0);
			},
			AuthenticationFailed = (context) =>
			{
				context.OwinContext.Response.Redirect("Home/Error?mess=" + context.Exception.Message);
				context.HandleResponse();
				return Task.FromResult(0);
			}
		}
	});
```

**Azure Management portál**

![](Images/ad-multitenant.png)