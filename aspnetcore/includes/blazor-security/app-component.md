<span data-ttu-id="33279-101">`App` bileşeni (*app. Razor*) Blazor Server uygulamalarında bulunan `App` bileşene benzerdir:</span><span class="sxs-lookup"><span data-stu-id="33279-101">The `App` component (*App.razor*) is similar to the `App` component found in Blazor Server apps:</span></span>

* <span data-ttu-id="33279-102">`CascadingAuthenticationState` bileşeni, `AuthenticationState` uygulamanın geri kalanında ortaya çıkarmayı yönetir.</span><span class="sxs-lookup"><span data-stu-id="33279-102">The `CascadingAuthenticationState` component manages exposing the `AuthenticationState` to the rest of the app.</span></span>
* <span data-ttu-id="33279-103">`AuthorizeRouteView` bileşeni, geçerli kullanıcının belirli bir sayfaya erişim yetkisi olduğundan emin olur veya başka bir şekilde `RedirectToLogin` bileşenini işler.</span><span class="sxs-lookup"><span data-stu-id="33279-103">The `AuthorizeRouteView` component makes sure that the current user is authorized to access a given page or otherwise renders the `RedirectToLogin` component.</span></span>
* <span data-ttu-id="33279-104">`RedirectToLogin` bileşeni, yetkisiz kullanıcıların oturum açma sayfasına yeniden yönlendirildiğini yönetir.</span><span class="sxs-lookup"><span data-stu-id="33279-104">The `RedirectToLogin` component manages redirecting unauthorized users to the login page.</span></span>

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
