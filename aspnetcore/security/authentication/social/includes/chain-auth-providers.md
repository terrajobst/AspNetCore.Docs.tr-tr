## <a name="multiple-authentication-providers"></a><span data-ttu-id="5b3d1-101">Birden çok kimlik doğrulama sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="5b3d1-101">Multiple authentication providers</span></span>

<span data-ttu-id="5b3d1-102">Uygulama birden çok sağlayıcı gerektirdiğinde arkasında sağlayıcısı genişletme yöntemleri zincir [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span><span class="sxs-lookup"><span data-stu-id="5b3d1-102">When the app requires multiple providers, chain the provider extension methods behind [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication):</span></span>

```csharp
services.AddAuthentication()
    .AddMicrosoftAccount(microsoftOptions => { ... })
    .AddGoogle(googleOptions => { ... })
    .AddTwitter(twitterOptions => { ... })
    .AddFacebook(facebookOptions => { ... });
```
