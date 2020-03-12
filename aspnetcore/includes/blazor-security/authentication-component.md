<span data-ttu-id="d991b-101">`Authentication` bileşeni tarafından üretilen sayfa (*Sayfalar/Authentication. Razor*), farklı kimlik doğrulama aşamalarını işlemek için gereken yolları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="d991b-101">The page produced by the `Authentication` component (*Pages/Authentication.razor*) defines the routes required for handling different authentication stages.</span></span>

<span data-ttu-id="d991b-102">`RemoteAuthenticatorView` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="d991b-102">The `RemoteAuthenticatorView` component:</span></span>

* <span data-ttu-id="d991b-103">`Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketi tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d991b-103">Is provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span>
* <span data-ttu-id="d991b-104">Her kimlik doğrulama aşamasında uygun eylemlerin gerçekleştirilmesini yönetir.</span><span class="sxs-lookup"><span data-stu-id="d991b-104">Manages performing the appropriate actions at each stage of authentication.</span></span>

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
