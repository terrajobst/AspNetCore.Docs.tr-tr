---
title: ASP.NET Core Blazor Weelsembly ek güvenlik senaryoları
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: ccb512392341e3eea33f4ab45740b7900f7b63f9
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989477"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a><span data-ttu-id="ce310-102">ASP.NET Core Blazor WebAssembly ek güvenlik senaryoları</span><span class="sxs-lookup"><span data-stu-id="ce310-102">ASP.NET Core Blazor WebAssembly additional security scenarios</span></span>

<span data-ttu-id="ce310-103">Sağlayan [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="ce310-103">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a><span data-ttu-id="ce310-104">Tanıtıcı belirteci isteği hataları</span><span class="sxs-lookup"><span data-stu-id="ce310-104">Handle token request errors</span></span>

<span data-ttu-id="ce310-105">Tek sayfalı uygulama (SPA), açma KIMLIĞI Connect (OıDC) kullanarak bir kullanıcının kimliğini doğruladığında, kimlik doğrulama durumu yerel olarak SPA 'da ve kimlik sağlayıcısı 'nda (IP) Kullanıcı tarafından sağlanan Credentials.</span><span class="sxs-lookup"><span data-stu-id="ce310-105">When a Single Page Application (SPA) authenticates a user using Open ID Connect (OIDC), the authentication state is maintained locally within the SPA and in the Identity Provider (IP) in the form of a session cookie that's set as a result of the user providing their credentials.</span></span>

<span data-ttu-id="ce310-106">IP 'nin Kullanıcı için yaydığı belirteçler genellikle kısa süreler boyunca geçerlidir. bu nedenle, istemci uygulamanın düzenli olarak yeni belirteçler getirmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ce310-106">The tokens that the IP emits for the user typically are valid for short periods of time, about one hour normally, so the client app must regularly fetch new tokens.</span></span> <span data-ttu-id="ce310-107">Aksi takdirde, Kullanıcı, verilen belirteçlerin süre dolduktan sonra günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-107">Otherwise, the user would be logged-out after the granted tokens expire.</span></span> <span data-ttu-id="ce310-108">Çoğu durumda, OıDC istemcileri kullanıcının kimlik doğrulaması durumunda veya IP içinde tutulan "oturum" için yeniden kimlik doğrulamasından geçmesini gerektirmeden yeni belirteçler sağlayabiliyor.</span><span class="sxs-lookup"><span data-stu-id="ce310-108">In most cases, OIDC clients are able to provision new tokens without requiring the user to authenticate again thanks to the authentication state or "session" that is kept within the IP.</span></span>

<span data-ttu-id="ce310-109">İstemcinin kullanıcı etkileşimi olmadan bir belirteç edinmedikleri bazı durumlar vardır, örneğin, kullanıcının IP 'den açık bir şekilde oturumu açtığı bir nedenden dolayı.</span><span class="sxs-lookup"><span data-stu-id="ce310-109">There are some cases in which the client can't get a token without user interaction, for example, when for some reason the user explicitly logs out from the IP.</span></span> <span data-ttu-id="ce310-110">Bu senaryo, bir Kullanıcı `https://login.microsoftonline.com` ve oturumu kapattığında oluşur. Bu senaryolarda, uygulama kullanıcının oturum açtığı hemen haberdar değildir. İstemcinin tuttuğu belirteç artık geçerli olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-110">This scenario occurs if a user visits `https://login.microsoftonline.com` and logs out. In these scenarios, the app doesn't know immediately that the user has logged out. Any token that the client holds might no longer be valid.</span></span> <span data-ttu-id="ce310-111">Ayrıca, istemci, geçerli belirtecin süresi dolduktan sonra Kullanıcı etkileşimi olmadan yeni bir belirteç sağlayamaz.</span><span class="sxs-lookup"><span data-stu-id="ce310-111">Also, the client isn't able to provision a new token without user interaction after the current token expires.</span></span>

<span data-ttu-id="ce310-112">Bu senaryolar belirteç tabanlı kimlik doğrulamasına özgü değildir.</span><span class="sxs-lookup"><span data-stu-id="ce310-112">These scenarios aren't specific to token-based authentication.</span></span> <span data-ttu-id="ce310-113">Bunlar, maça doğaları 'nın bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="ce310-113">They are part of the nature of SPAs.</span></span> <span data-ttu-id="ce310-114">Kimlik doğrulama tanımlama bilgisi kaldırılırsa, tanımlama bilgilerini kullanan bir SPA da sunucu API 'sini çağıramaz.</span><span class="sxs-lookup"><span data-stu-id="ce310-114">An SPA using cookies also fails to call a server API if the authentication cookie is removed.</span></span>

<span data-ttu-id="ce310-115">Bir uygulama, korumalı kaynaklara yönelik API çağrıları gerçekleştirdiğinde, aşağıdakilerin farkında olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="ce310-115">When an app performs API calls to protected resources, you must be aware of the following:</span></span>

* <span data-ttu-id="ce310-116">API 'yi çağırmak için yeni bir erişim belirteci sağlamak üzere kullanıcının yeniden kimlik doğrulaması yapması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-116">To provision a new access token to call the API, the user might be required to authenticate again.</span></span>
* <span data-ttu-id="ce310-117">İstemcinin geçerli gibi görünen bir belirteci olsa da, belirteç Kullanıcı tarafından iptal edildiğinden sunucu çağrısı başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-117">Even if the client has a token that seems to be valid, the call to the server might fail because the token was revoked by the user.</span></span>

<span data-ttu-id="ce310-118">Uygulama bir belirteç istediğinde, olası iki sonuç vardır:</span><span class="sxs-lookup"><span data-stu-id="ce310-118">When the app requests a token, there are two possible outcomes:</span></span>

* <span data-ttu-id="ce310-119">İstek başarılı olur ve uygulamanın geçerli bir belirteci vardır.</span><span class="sxs-lookup"><span data-stu-id="ce310-119">The request succeeds, and the app has a valid token.</span></span>
* <span data-ttu-id="ce310-120">İstek başarısız olur ve yeni bir belirteç almak için uygulamanın kullanıcının kimliğini doğrulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ce310-120">The request fails, and the app must authenticate the user again to obtain a new token.</span></span>

<span data-ttu-id="ce310-121">Bir belirteç isteği başarısız olduğunda, yeniden yönlendirme gerçekleştirmeden önce geçerli durumu kaydetmek isteyip istemediğinize karar vermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ce310-121">When a token request fails, you need to decide whether you want to save any current state before you perform a redirection.</span></span> <span data-ttu-id="ce310-122">Artan karmaşıklık düzeylerinde birkaç yaklaşım mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="ce310-122">Several approaches exist with increasing levels of complexity:</span></span>

* <span data-ttu-id="ce310-123">Geçerli sayfa durumunu oturum depolama alanında depolayın.</span><span class="sxs-lookup"><span data-stu-id="ce310-123">Store the current page state in session storage.</span></span> <span data-ttu-id="ce310-124">`OnInitializeAsync`sırasında, devam etmeden önce durumun geri yüklenebildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="ce310-124">During `OnInitializeAsync`, check if state can be restored before continuing.</span></span>
* <span data-ttu-id="ce310-125">Bir sorgu dizesi parametresi ekleyin ve bunu, uygulamayı daha önce kaydedilen durumu yeniden doldurma ihtiyacı olduğunu bildirmek için bir yol olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="ce310-125">Add a query string parameter and use that as a way to signal the app that it needs to re-hydrate the previously saved state.</span></span>
* <span data-ttu-id="ce310-126">Bir sorgu dizesi parametresini, verileri oturum depolamada diğer öğelerle riskli olmayan bir şekilde depolamak için benzersiz bir tanımlayıcıya ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ce310-126">Add a query string parameter with a unique identifier to store data in session storage without risking collisions with other items.</span></span>

<span data-ttu-id="ce310-127">Aşağıdaki örnekte gösterildiği nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="ce310-127">The following example shows how to:</span></span>

* <span data-ttu-id="ce310-128">Oturum açma sayfasına yönlendirmeden önce durumu koru.</span><span class="sxs-lookup"><span data-stu-id="ce310-128">Preserve state before redirecting to the login page.</span></span>
* <span data-ttu-id="ce310-129">Önceki durumu daha sonra sorgu dizesi parametresini kullanarak kimlik doğrulamasını kurtarın.</span><span class="sxs-lookup"><span data-stu-id="ce310-129">Recover the previous state afterward authentication using the query string parameter.</span></span>

```razor
<EditForm Model="User" @onsubmit="OnSaveAsync">
    <label>User
        <InputText @bind-Value="User.Name" />
    </label>
    <label>Last name
        <InputText @bind-Value="User.LastName" />
    </label>
</EditForm>

@code {
    public class Profile
    {
        public string Name { get; set; }
        public string LastName { get; set; }
    }

    public Profile User { get; set; } = new Profile();

    protected async override Task OnInitializedAsync()
    {
        var currentQuery = new Uri(Navigation.Uri).Query;

        if (currentQuery.Contains("state=resumeSavingProfile"))
        {
            User = await JS.InvokeAsync<Profile>("sessionStorage.getState", 
                "resumeSavingProfile");
        }
    }

    public async Task OnSaveAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var resumeUri = Navigation.Uri + $"?state=resumeSavingProfile";

        var tokenResult = await AuthenticationService.RequestAccessToken(
            new AccessTokenRequestOptions
            {
                ReturnUrl = resumeUri
            });

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            await httpClient.PostJsonAsync("Save", User);
        }
        else
        {
            await JS.InvokeVoidAsync("sessionStorage.setState", 
                "resumeSavingProfile", User);
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }
    }
}
```

## <a name="save-app-state-before-an-authentication-operation"></a><span data-ttu-id="ce310-130">Bir kimlik doğrulama işleminden önce uygulama durumunu Kaydet</span><span class="sxs-lookup"><span data-stu-id="ce310-130">Save app state before an authentication operation</span></span>

<span data-ttu-id="ce310-131">Bir kimlik doğrulama işlemi sırasında, tarayıcı IP 'ye yönlendirilmeden önce uygulama durumunu kaydetmek istediğiniz durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="ce310-131">During an authentication operation, there are cases where you want to save the app state before the browser is redirected to the IP.</span></span> <span data-ttu-id="ce310-132">Durum kapsayıcısı gibi bir şey kullandığınızda ve kimlik doğrulaması başarılı olduktan sonra durumu geri yüklemek istediğinizde bu durum oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-132">This can be the case when you are using something like a state container and you want to restore the state after the authentication succeeds.</span></span> <span data-ttu-id="ce310-133">Uygulamaya özgü durumu veya bir başvuruyu korumak ve kimlik doğrulama işlemi başarıyla tamamlandıktan sonra bu durumu geri yüklemek için özel bir kimlik doğrulama durumu nesnesi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce310-133">You can use a custom authentication state object to preserve app-specific state or a reference to it and restore that state once the authentication operation successfully completes.</span></span>

<span data-ttu-id="ce310-134">`Authentication` bileşeni (*Sayfalar/Authentication. Razor*):</span><span class="sxs-lookup"><span data-stu-id="ce310-134">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/authentication/{action}"
@inject JSRuntime JS
@inject StateContainer State
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorViewCore Action="@Action" 
    AuthenticationState="AuthenticationState" OnLoginSucceded="RestoreState" 
    OnLogoutSucceded="RestoreState" />

@code {
    [Parameter]
    public string Action { get; set; }

    public class ApplicationAuthenticationState : RemoteAuthenticationState
    {
        public string Id { get; set; }
    }

    protected async override Task OnInitializedAsync()
    {
        if (RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogIn, 
            Action))
        {
            AuthenticationState.Id = Guid.NewGuid().ToString();
            await JS.InvokeVoidAsync("sessionStorage.setKey", 
                AuthenticationState.Id, State.Store());
        }
    }

    public async Task RestoreState(ApplicationAuthenticationState state)
    {
        var stored = await JS.InvokeAsync<string>("sessionStorage.getKey", 
            state.Id);
        State.FromStore(stored);
    }

    public ApplicationAuthenticationState AuthenticationState { get; set; } = 
        new ApplicationAuthenticationState();
}
```

## <a name="request-additional-access-tokens"></a><span data-ttu-id="ce310-135">Ek erişim belirteçleri isteyin</span><span class="sxs-lookup"><span data-stu-id="ce310-135">Request additional access tokens</span></span>

<span data-ttu-id="ce310-136">Çoğu uygulama yalnızca kullandıkları korunan kaynaklarla etkileşim kurmak için bir erişim belirteci gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ce310-136">Most apps only require an access token to interact with the protected resources that they use.</span></span> <span data-ttu-id="ce310-137">Bazı senaryolarda, bir uygulama iki veya daha fazla kaynakla etkileşim kurmak için birden fazla belirteç gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-137">In some scenarios, an app might require more than one token in order to interact with two or more resources.</span></span> <span data-ttu-id="ce310-138">`IAccessTokenProvider.RequestToken` yöntemi, bir uygulamanın aşağıdaki örnekte görüldüğü gibi, belirli bir kapsam kümesiyle belirteç sağlamasını sağlayan bir aşırı yükleme sağlar:</span><span class="sxs-lookup"><span data-stu-id="ce310-138">The `IAccessTokenProvider.RequestToken` method provides an overload that allows an app to provision a token with a given set of scopes, as seen in the following example:</span></span>

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a><span data-ttu-id="ce310-139">Uygulama yollarını özelleştirme</span><span class="sxs-lookup"><span data-stu-id="ce310-139">Customize app routes</span></span>

<span data-ttu-id="ce310-140">Varsayılan olarak, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` kitaplığı, farklı kimlik doğrulama durumlarını temsil etmek için aşağıdaki tabloda gösterilen yolları kullanır.</span><span class="sxs-lookup"><span data-stu-id="ce310-140">By default, the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library uses the routes shown in the following table for representing different authentication states.</span></span>

| <span data-ttu-id="ce310-141">Yol</span><span class="sxs-lookup"><span data-stu-id="ce310-141">Route</span></span>                            | <span data-ttu-id="ce310-142">Amaç</span><span class="sxs-lookup"><span data-stu-id="ce310-142">Purpose</span></span> |
| -------------------------------- | ------- |
| `authentication/login`           | <span data-ttu-id="ce310-143">Bir oturum açma işlemini tetikler.</span><span class="sxs-lookup"><span data-stu-id="ce310-143">Triggers a sign-in operation.</span></span> |
| `authentication/login-callback`  | <span data-ttu-id="ce310-144">Herhangi bir oturum açma işleminin sonucunu işler.</span><span class="sxs-lookup"><span data-stu-id="ce310-144">Handles the result of any sign-in operation.</span></span> |
| `authentication/login-failed`    | <span data-ttu-id="ce310-145">Bazı nedenlerle oturum açma işlemi başarısız olduğunda hata iletilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ce310-145">Displays error messages when the sign-in operation fails for some reason.</span></span> |
| `authentication/logout`          | <span data-ttu-id="ce310-146">Bir oturum kapatma işlemi tetikler.</span><span class="sxs-lookup"><span data-stu-id="ce310-146">Triggers a sign-out operation.</span></span> |
| `authentication/logout-callback` | <span data-ttu-id="ce310-147">Bir oturum kapatma işleminin sonucunu işler.</span><span class="sxs-lookup"><span data-stu-id="ce310-147">Handles the result of a sign-out operation.</span></span> |
| `authentication/logout-failed`   | <span data-ttu-id="ce310-148">Bir nedenden dolayı oturum kapatma işlemi başarısız olduğunda hata iletilerini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ce310-148">Displays error messages when the sign-out operation fails for some reason.</span></span> |
| `authentication/logged-out`      | <span data-ttu-id="ce310-149">Kullanıcının oturumu başarıyla sonlandırdığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="ce310-149">Indicates that the user has successfully logout.</span></span> |
| `authentication/profile`         | <span data-ttu-id="ce310-150">Kullanıcı profilini düzenlemek için bir işlem tetikler.</span><span class="sxs-lookup"><span data-stu-id="ce310-150">Triggers an operation to edit the user profile.</span></span> |
| `authentication/register`        | <span data-ttu-id="ce310-151">Yeni bir kullanıcıyı kaydetmek için bir işlem tetikler.</span><span class="sxs-lookup"><span data-stu-id="ce310-151">Triggers an operation to register a new user.</span></span> |

<span data-ttu-id="ce310-152">Yukarıdaki tabloda gösterilen yollar `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`aracılığıyla yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-152">The routes shown in the preceding table are configurable via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span></span> <span data-ttu-id="ce310-153">Özel yollar sağlamak için seçenekleri ayarlarken, uygulamanın her yolu işleyen bir yolu olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ce310-153">When setting options to provide custom routes, confirm that the app has a route that handles each path.</span></span>

<span data-ttu-id="ce310-154">Aşağıdaki örnekte, tüm yollara `/security`ön eki eklenir.</span><span class="sxs-lookup"><span data-stu-id="ce310-154">In the following example, all the paths are prefixed with `/security`.</span></span>

<span data-ttu-id="ce310-155">`Authentication` bileşeni (*Sayfalar/Authentication. Razor*):</span><span class="sxs-lookup"><span data-stu-id="ce310-155">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="ce310-156">`Program.Main` (*program.cs*):</span><span class="sxs-lookup"><span data-stu-id="ce310-156">`Program.Main` (*Program.cs*):</span></span>

```csharp
builder.Services.AddApiAuthorization(options => { 
    options.AuthenticationPaths.LogInPath = "security/login";
    options.AuthenticationPaths.LogInCallbackPath = "security/login-callback";
    options.AuthenticationPaths.LogInFailedPath = "security/login-failed";
    options.AuthenticationPaths.LogOutPath = "security/logout";
    options.AuthenticationPaths.LogOutCallbackPath = "security/logout-callback";
    options.AuthenticationPaths.LogOutFailedPath = "security/logout-failed";
    options.AuthenticationPaths.LogOutSucceededPath = "security/logged-out";
    options.AuthenticationPaths.ProfilePath = "security/profile";
    options.AuthenticationPaths.RegisterPath = "security/register";
});
```

<span data-ttu-id="ce310-157">Gereksinim tamamen farklı yollar çağırırsa, daha önce açıklandığı gibi yolları ayarlayın ve `RemoteAuthenticatorView` açık bir eylem parametresiyle işlenir:</span><span class="sxs-lookup"><span data-stu-id="ce310-157">If the requirement calls for completely different paths, set the routes as described previously and render the `RemoteAuthenticatorView` with an explicit action parameter:</span></span>

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

<span data-ttu-id="ce310-158">Bunu seçerseniz, Kullanıcı arabirimini farklı sayfalara bölmek için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-158">You're allowed to break the UI into different pages if you choose to do so.</span></span>

## <a name="customize-the-authentication-user-interface"></a><span data-ttu-id="ce310-159">Kimlik doğrulama kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="ce310-159">Customize the authentication user interface</span></span>

<span data-ttu-id="ce310-160">`RemoteAuthenticatorView` her bir kimlik doğrulama durumu için varsayılan bir UI parçaları kümesi içerir.</span><span class="sxs-lookup"><span data-stu-id="ce310-160">`RemoteAuthenticatorView` includes a default set of UI pieces for each authentication state.</span></span> <span data-ttu-id="ce310-161">Her durum, özel bir `RenderFragment`geçirerek özelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-161">Each state can be customized by passing in a custom `RenderFragment`.</span></span> <span data-ttu-id="ce310-162">İlk oturum açma işlemi sırasında görüntülenecek metni özelleştirmek için `RemoteAuthenticatorView` aşağıdaki gibi değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="ce310-162">To customize the displayed text during the initial login process, can change the `RemoteAuthenticatorView` as follows.</span></span>

<span data-ttu-id="ce310-163">`Authentication` bileşeni (*Sayfalar/Authentication. Razor*):</span><span class="sxs-lookup"><span data-stu-id="ce310-163">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action">
    <LoggingIn>
        You are about to be redirected to https://login.microsoftonline.com.
    </LoggingIn>
</RemoteAuthenticatorView>

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="ce310-164">`RemoteAuthenticatorView`, aşağıdaki tabloda gösterilen her kimlik doğrulama yolu için kullanılabilecek bir parçaya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ce310-164">The `RemoteAuthenticatorView` has one fragment that can be used per authentication route shown in the following table.</span></span>

| <span data-ttu-id="ce310-165">Yol</span><span class="sxs-lookup"><span data-stu-id="ce310-165">Route</span></span>                            | <span data-ttu-id="ce310-166">Parça</span><span class="sxs-lookup"><span data-stu-id="ce310-166">Fragment</span></span>                |
| -------------------------------- | ----------------------- |
| `authentication/login`           | `<LoggingIn>`           |
| `authentication/login-callback`  | `<CompletingLoggingIn>` |
| `authentication/login-failed`    | `<LogInFailed>`         |
| `authentication/logout`          | `<LogOut>`              |
| `authentication/logout-callback` | `<CompletingLogOut>`    |
| `authentication/logout-failed`   | `<LogOutFailed>`        |
| `authentication/logged-out`      | `<LogOutSucceeded>`     |
| `authentication/profile`         | `<UserProfile>`         |
| `authentication/register`        | `<Registering>`         |
