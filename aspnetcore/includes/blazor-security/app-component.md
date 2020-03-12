`App` bileşeni (*app. Razor*) Blazor Server uygulamalarında bulunan `App` bileşene benzerdir:

* `CascadingAuthenticationState` bileşeni, `AuthenticationState` uygulamanın geri kalanında ortaya çıkarmayı yönetir.
* `AuthorizeRouteView` bileşeni, geçerli kullanıcının belirli bir sayfaya erişim yetkisi olduğundan emin olur veya başka bir şekilde `RedirectToLogin` bileşenini işler.
* `RedirectToLogin` bileşeni, yetkisiz kullanıcıların oturum açma sayfasına yeniden yönlendirildiğini yönetir.

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" 
                DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    <RedirectToLogin />
                </NotAuthorized>
            </AuthorizeRouteView>
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```
