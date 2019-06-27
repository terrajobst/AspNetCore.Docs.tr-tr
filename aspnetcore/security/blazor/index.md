---
title: ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme
author: guardrex
description: Blazor kimlik doğrulama ve yetkilendirme senaryoları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/26/2019
uid: security/blazor/index
ms.openlocfilehash: b3bca26e7088a8353084a065f9b9593c9d8e08e6
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406201"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="5c721-103">ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="5c721-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="5c721-104">Tarafından [Steve Sanderson](https://github.com/SteveSandersonMS)</span><span class="sxs-lookup"><span data-stu-id="5c721-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

<span data-ttu-id="5c721-105">ASP.NET Core güvenlik yönetimi ve yapılandırmayı Blazor uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="5c721-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="5c721-106">Güvenlik senaryoları Blazor sunucu tarafı ve istemci tarafı uygulamalar arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="5c721-106">Security scenarios differ between Blazor server-side and client-side apps.</span></span> <span data-ttu-id="5c721-107">Blazor sunucu tarafı uygulamalar, sunucu üzerinde çalıştığından, yetkilendirme denetimleri belirlemek kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5c721-107">Because Blazor server-side apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="5c721-108">Bir kullanıcı (örneğin, bir kullanıcı tarafından hangi menü girişlerini kullanılabilir) için sunulan kullanıcı Arabirimi seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="5c721-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="5c721-109">Erişim kuralları uygulama ve bileşenlerinin alanları.</span><span class="sxs-lookup"><span data-stu-id="5c721-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="5c721-110">İstemcide Blazor istemci tarafı uygulamalar çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5c721-110">Blazor client-side apps run on the client.</span></span> <span data-ttu-id="5c721-111">Yetkilendirmesi *yalnızca* hangi göstermek için kullanıcı Arabirimi seçenekleri belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5c721-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="5c721-112">İstemci tarafı denetimleri değişiklik veya kullanıcı tarafından atlanır Blazor istemci-tarafı uygulaması yetkilendirme erişim kuralları zorunlu kılamaz.</span><span class="sxs-lookup"><span data-stu-id="5c721-112">Since client-side checks can be modified or bypassed by a user, a Blazor client-side app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="5c721-113">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="5c721-113">Authentication</span></span>

<span data-ttu-id="5c721-114">Blazor, kullanıcının kimliğini oluşturmak için mevcut bir ASP.NET Core kimlik doğrulaması mekanizmalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="5c721-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="5c721-115">Tam mekanizması Blazor uygulamayı nasıl barındırılıyorsa, sunucu tarafı veya istemci tarafı üzerinde bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="5c721-115">The exact mechanism depends on how the Blazor app is hosted, server-side or client-side.</span></span>

### <a name="blazor-server-side-authentication"></a><span data-ttu-id="5c721-116">Blazor sunucu tarafı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="5c721-116">Blazor server-side authentication</span></span>

<span data-ttu-id="5c721-117">Sunucu tarafı uygulamalar Blazor oluşturulan SignalR kullanarak gerçek zamanlı bir bağlantı üzerinden çalışır.</span><span class="sxs-lookup"><span data-stu-id="5c721-117">Blazor server-side apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="5c721-118">[SignalR tabanlı uygulamalarda kimlik doğrulaması](xref:signalr/authn-and-authz) bağlantı kurulduğunda ele alınır.</span><span class="sxs-lookup"><span data-stu-id="5c721-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="5c721-119">Kimlik doğrulama tanımlama bilgisi veya diğer bir taşıyıcı belirteç temel alabilir.</span><span class="sxs-lookup"><span data-stu-id="5c721-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="5c721-120">Proje oluşturulduğunda kimlik doğrulaması Blazor sunucu tarafı proje şablonu ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5c721-120">The Blazor server-side project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c721-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c721-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5c721-122">Visual Studio yönergeleri <xref:blazor/get-started> makalenin bir kimlik doğrulama mekanizması ile yeni bir Blazor sunucu tarafı projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5c721-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism.</span></span>

<span data-ttu-id="5c721-123">Seçtikten sonra **Blazor (sunucu tarafı)** şablonunda **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda **değişiklik** altında **kimlik doğrulaması** .</span><span class="sxs-lookup"><span data-stu-id="5c721-123">After choosing the **Blazor (server-side)** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="5c721-124">Diğer ASP.NET Core kimlik doğrulaması mekanizmalarını aynı dizi projeleri sunmak için bir iletişim kutusu açılır:</span><span class="sxs-lookup"><span data-stu-id="5c721-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="5c721-125">**Kimlik doğrulama yok**</span><span class="sxs-lookup"><span data-stu-id="5c721-125">**No Authentication**</span></span>
* <span data-ttu-id="5c721-126">**Bireysel kullanıcı hesapları** &ndash; kullanıcı hesaplarını depolanabilir:</span><span class="sxs-lookup"><span data-stu-id="5c721-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="5c721-127">ASP.NET Core'nın içinde uygulamanın kullanarak [kimlik](xref:security/authentication/identity) sistem.</span><span class="sxs-lookup"><span data-stu-id="5c721-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="5c721-128">İle [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="5c721-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="5c721-129">**İş veya Okul hesapları**</span><span class="sxs-lookup"><span data-stu-id="5c721-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="5c721-130">**Windows kimlik doğrulaması**</span><span class="sxs-lookup"><span data-stu-id="5c721-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5c721-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5c721-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="5c721-132">Visual Studio Code yönergeleri <xref:blazor/get-started> makale ile kimlik doğrulama mekanizması yeni Blazor sunucu tarafı projesi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="5c721-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:</span></span>

```console
dotnet new blazorserverside -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="5c721-133">İzin verilen kimlik doğrulama değerlerini (`{AUTHENTICATION}`) aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="5c721-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="5c721-134">Kimlik doğrulama mekanizması</span><span class="sxs-lookup"><span data-stu-id="5c721-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="5c721-135">`{AUTHENTICATION}` Değer</span><span class="sxs-lookup"><span data-stu-id="5c721-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="5c721-136">Kimlik doğrulama yok</span><span class="sxs-lookup"><span data-stu-id="5c721-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="5c721-137">Kişi</span><span class="sxs-lookup"><span data-stu-id="5c721-137">Individual</span></span><br><span data-ttu-id="5c721-138">ASP.NET Core kimliği uygulamayla saklanan kullanıcı.</span><span class="sxs-lookup"><span data-stu-id="5c721-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="5c721-139">Kişi</span><span class="sxs-lookup"><span data-stu-id="5c721-139">Individual</span></span><br><span data-ttu-id="5c721-140">Depolanan kullanıcılar [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="5c721-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="5c721-141">İş veya Okul hesapları</span><span class="sxs-lookup"><span data-stu-id="5c721-141">Work or School Accounts</span></span><br><span data-ttu-id="5c721-142">Tek bir kiracı için kuruluş kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="5c721-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="5c721-143">İş veya Okul hesapları</span><span class="sxs-lookup"><span data-stu-id="5c721-143">Work or School Accounts</span></span><br><span data-ttu-id="5c721-144">Birden fazla Kiracı için kuruluş kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="5c721-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="5c721-145">Windows Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="5c721-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="5c721-146">Komut için sağlanan değer ile adlı bir klasör oluşturur `{APP NAME}` yer tutucu ve klasör adı uygulamanın adı kullanır.</span><span class="sxs-lookup"><span data-stu-id="5c721-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="5c721-147">Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new) .NET Core Kılavuzu'nda komutu.</span><span class="sxs-lookup"><span data-stu-id="5c721-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor server-side project with an authentication mechanism:

```console
dotnet new blazorserverside -o {APP NAME} -au {AUTHENTICATION}
```

Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.

| Authentication mechanism                                                                 | `{AUTHENTICATION}` value |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| No Authentication                                                                        | `None`                   |
| Individual<br>Users stored in the app with ASP.NET Core Identity.                        | `Individual`             |
| Individual<br>Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Work or School Accounts<br>Organizational authentication for a single tenant.            | `SingleOrg`              |
| Work or School Accounts<br>Organizational authentication for multiple tenants.           | `MultiOrg`               |
| Windows Authentication                                                                   | `Windows`                |

The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name. For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.

-->

---

### <a name="blazor-client-side-authentication"></a><span data-ttu-id="5c721-148">Blazor istemci-tarafı kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="5c721-148">Blazor client-side authentication</span></span>

<span data-ttu-id="5c721-149">Tüm istemci tarafı kod, kullanıcılar tarafından değiştirilebilir çünkü Blazor istemci tarafı uygulamalar, kimlik doğrulama denetimleri atlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5c721-149">In Blazor client-side apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="5c721-150">Aynı, JavaScript SPA'ya çerçeveleri ya da tüm işletim sistemlerine yönelik yerel uygulamaları dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5c721-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="5c721-151">Özel uygulanışı `AuthenticationStateProvider` hizmet Blazor istemci tarafı uygulamalar için aşağıdaki bölümlerde ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5c721-151">Implementation of a custom `AuthenticationStateProvider` service for Blazor client-side apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="5c721-152">AuthenticationStateProvider hizmeti</span><span class="sxs-lookup"><span data-stu-id="5c721-152">AuthenticationStateProvider service</span></span>

<span data-ttu-id="5c721-153">Blazor sunucu tarafı uygulamalar dahil yerleşik `AuthenticationStateProvider` ASP.NET çekirdek ait kimlik doğrulama durumu verilerini alan hizmet `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="5c721-153">Blazor server-side apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="5c721-154">Kimlik doğrulama durumu mevcut ASP.NET Core sunucu tarafı kimlik doğrulama mekanizmaları ile nasıl tümleştirildiğini budur.</span><span class="sxs-lookup"><span data-stu-id="5c721-154">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="5c721-155">`AuthenticationStateProvider` tarafından kullanılan temel alınan hizmet `AuthorizeView` bileşeni ve `CascadingAuthenticationState` kimlik doğrulama durumu almak için bileşen.</span><span class="sxs-lookup"><span data-stu-id="5c721-155">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="5c721-156">Genellikle kullanmayın `AuthenticationStateProvider` doğrudan.</span><span class="sxs-lookup"><span data-stu-id="5c721-156">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="5c721-157">Kullanım [AuthorizeView bileşen](#authorizeview-component) veya [görev<AuthenticationState> ](#expose-the-authentication-state-as-a-cascading-parameter) bu makalenin sonraki bölümlerinde açıklanan yaklaşımlardan.</span><span class="sxs-lookup"><span data-stu-id="5c721-157">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="5c721-158">Kullanmanın asıl sakıncası `AuthenticationStateProvider` doğrudan veri değişikliklerini temel kimlik doğrulama durumunu, bileşen otomatik olarak bildirilmez olduğu.</span><span class="sxs-lookup"><span data-stu-id="5c721-158">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="5c721-159">`AuthenticationStateProvider` Hizmet, geçerli kullanıcının sağlayabilir <xref:System.Security.Claims.ClaimsPrincipal> aşağıdaki örnekte gösterildiği gibi veri:</span><span class="sxs-lookup"><span data-stu-id="5c721-159">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```cshtml
@page "/"
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="@LogUsername">Write user info to console</button>

@code {
    private async Task LogUsername()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

<span data-ttu-id="5c721-160">Varsa `user.Identity.IsAuthenticated` olduğu `true` ve kullanıcı bir <xref:System.Security.Claims.ClaimsPrincipal>, talep numaralandırılmış ve rol üyelikleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="5c721-160">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="5c721-161">Bağımlılık ekleme (dı) ve Hizmetleri hakkında daha fazla bilgi için bkz. <xref:blazor/dependency-injection> ve <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="5c721-161">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="5c721-162">Özel AuthenticationStateProvider uygulayın</span><span class="sxs-lookup"><span data-stu-id="5c721-162">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="5c721-163">Bir Blazor istemci-tarafı uygulaması oluştururken veya uygulamanızın belirtimi kesinlikle özel bir sağlayıcı gerektiriyorsa, sağlayıcıyı uygulama ve geçersiz kılma `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="5c721-163">If you're building a Blazor client-side app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
class CustomAuthStateProvider : AuthenticationStateProvider
{
    public override Task<AuthenticationState> GetAuthenticationStateAsync()
    {
        var identity = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, "mrfibuli"),
        }, "Fake authentication type");

        var user = new ClaimsPrincipal(identity);

        return Task.FromResult(new AuthenticationState(user));
    }
}
```

<span data-ttu-id="5c721-164">`CustomAuthStateProvider` Hizmet kayıtlı `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5c721-164">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

<span data-ttu-id="5c721-165">Kullanarak `CustomAuthStateProvider`, tüm kullanıcıların kullanıcı adıyla kimlik doğrulamalarının `mrfibuli`.</span><span class="sxs-lookup"><span data-stu-id="5c721-165">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="5c721-166">Kimlik doğrulama durumu basamaklı bir parametre olarak kullanıma sunma</span><span class="sxs-lookup"><span data-stu-id="5c721-166">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="5c721-167">Kimlik doğrulama durumu verilerinin ne zaman kullanıcı tarafından tetiklenen eylemi gerçekleştirme gibi yordam mantığı, gerekiyorsa kimlik doğrulama durumu verileri türü basamaklı bir parametre tanımlayarak almak `Task<AuthenticationState>`:</span><span class="sxs-lookup"><span data-stu-id="5c721-167">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```cshtml
@page "/"

<button @onclick="@LogUsername">Log username</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

<span data-ttu-id="5c721-168">Varsa `user.Identity.IsAuthenticated` olduğu `true`, talep numaralandırılmış ve rol üyelikleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="5c721-168">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="5c721-169">Ayarlanan `Task<AuthenticationState>` basamaklı parametresini kullanarak `CascadingAuthenticationState` bileşeni:</span><span class="sxs-lookup"><span data-stu-id="5c721-169">Set up the `Task<AuthenticationState>` cascading parameter using the `CascadingAuthenticationState` component:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        <NotFoundContent>
            <h1>Sorry</h1>
            <p>Sorry, there's nothing at this address.</p>
        </NotFoundContent>
    </Router>
</CascadingAuthenticationState>
```

## <a name="authorization"></a><span data-ttu-id="5c721-170">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="5c721-170">Authorization</span></span>

<span data-ttu-id="5c721-171">Bir kullanıcının kimliği doğrulandıktan sonra *yetkilendirme* kullanıcı neler yapabileceğinizi denetlemek için kurallar uygulanır.</span><span class="sxs-lookup"><span data-stu-id="5c721-171">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="5c721-172">Erişim genellikle verilen veya reddedilen bağlı:</span><span class="sxs-lookup"><span data-stu-id="5c721-172">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="5c721-173">Bir kullanıcı (açık) doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="5c721-173">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="5c721-174">Bir kullanıcının bulunduğu bir *rol*.</span><span class="sxs-lookup"><span data-stu-id="5c721-174">A user is in a *role*.</span></span>
* <span data-ttu-id="5c721-175">Bir kullanıcının bir *talep*.</span><span class="sxs-lookup"><span data-stu-id="5c721-175">A user has a *claim*.</span></span>
* <span data-ttu-id="5c721-176">A *ilke* karşılanmadı.</span><span class="sxs-lookup"><span data-stu-id="5c721-176">A *policy* is satisfied.</span></span>

<span data-ttu-id="5c721-177">Bu kavramlar her bir ASP.NET Core MVC veya Razor sayfaları uygulama olduğu gibi aynı şeydir.</span><span class="sxs-lookup"><span data-stu-id="5c721-177">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="5c721-178">Makalelerin altında ASP.NET Core güvenlik hakkında daha fazla bilgi için bkz. [ASP.NET Core güvenlik ve kimlik](xref:security/index).</span><span class="sxs-lookup"><span data-stu-id="5c721-178">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="5c721-179">AuthorizeView bileşeni</span><span class="sxs-lookup"><span data-stu-id="5c721-179">AuthorizeView component</span></span>

<span data-ttu-id="5c721-180">`AuthorizeView` Bileşen seçmeli olarak olup kullanıcı görmek için yetkili bağlı olarak kullanıcı Arabirimi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5c721-180">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="5c721-181">Bu yaklaşım için yalnızca ihtiyacınız olduğunda yararlıdır *görüntüleme* kullanıcı verileri ve kullanıcının kimliğini yordam mantığı kullanmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5c721-181">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="5c721-182">Bileşen sunan bir `context` türündeki değişken `AuthenticationState`, oturum açmış kullanıcı hakkında bilgilere erişmek için kullanabileceğiniz:</span><span class="sxs-lookup"><span data-stu-id="5c721-182">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="5c721-183">Kullanıcı kimliği değil, ayrıca görüntülemek için farklı içerik belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5c721-183">You can also supply different content for display if the user isn't authenticated:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

<span data-ttu-id="5c721-184">İçeriği `<Authorized>` ve `<NotAuthorized>` etkileşimli diğer bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="5c721-184">The content of `<Authorized>` and `<NotAuthorized>` can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="5c721-185">Roller veya kullanıcı Arabirimi seçenekleri veya erişim denetimi ilkeleri gibi yetkilendirme koşulları içinde ele alınmıştır [yetkilendirme](#authorization) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5c721-185">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="5c721-186">Belirtilen yetkilendirme koşulları olmayan, `AuthorizeView` varsayılan ilkeyi kullanır ve algılar:</span><span class="sxs-lookup"><span data-stu-id="5c721-186">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="5c721-187">Yetkili olarak kimlik doğrulaması (oturum açma) kullanıcılar.</span><span class="sxs-lookup"><span data-stu-id="5c721-187">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="5c721-188">Kimliği doğrulanmamış (imzalı genişletme) kullanıcıların yetkisiz olarak.</span><span class="sxs-lookup"><span data-stu-id="5c721-188">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="5c721-189">Rol tabanlı ve ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="5c721-189">Role-based and policy-based authorization</span></span>

<span data-ttu-id="5c721-190">`AuthorizeView` Bileşeni destekler *rol tabanlı* veya *ilke tabanlı* yetkilendirme.</span><span class="sxs-lookup"><span data-stu-id="5c721-190">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="5c721-191">Rol tabanlı yetkilendirme için kullanan `Roles` parametresi:</span><span class="sxs-lookup"><span data-stu-id="5c721-191">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="5c721-192">Daha fazla bilgi için bkz. <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="5c721-192">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="5c721-193">İlke tabanlı yetkilendirme için kullanan `Policy` parametresi:</span><span class="sxs-lookup"><span data-stu-id="5c721-193">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="5c721-194">Beyana dayalı yetkilendirme, ilke tabanlı yetkilendirme özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="5c721-194">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="5c721-195">Örneğin, kullanıcıların belirli bir talep sahip olmasını gerektiren bir ilke tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5c721-195">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="5c721-196">Daha fazla bilgi için bkz. <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="5c721-196">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="5c721-197">Bu API'ler, Blazor sunucu tarafı ya da Blazor istemci-tarafı uygulamaları içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5c721-197">These APIs can be used in either Blazor server-side or Blazor client-side apps.</span></span>

<span data-ttu-id="5c721-198">Kullanılmazsa `Roles` ya da `Policy` belirtilen `AuthorizeView` varsayılan ilkesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="5c721-198">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="5c721-199">Zaman uyumsuz kimlik doğrulaması sırasında görüntülenen içerik</span><span class="sxs-lookup"><span data-stu-id="5c721-199">Content displayed during asynchronous authentication</span></span>

<span data-ttu-id="5c721-200">Blazor izin verir, kimlik doğrulama durumu belirlenecek *zaman uyumsuz olarak*.</span><span class="sxs-lookup"><span data-stu-id="5c721-200">Blazor allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="5c721-201">Birincil bu yaklaşım için kimlik doğrulaması için dış uç noktası için bir istekte bulunmak Blazor istemci-tarafı uygulamalarında senaryodur.</span><span class="sxs-lookup"><span data-stu-id="5c721-201">The primary scenario for this approach is in Blazor client-side apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="5c721-202">Kimlik doğrulaması, devam ederken `AuthorizeView` içerik varsayılan olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5c721-202">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="5c721-203">Kimlik doğrulaması gerçekleşirken içeriği görüntülemek için kullanın `<Authorizing>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="5c721-203">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```cshtml
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

<span data-ttu-id="5c721-204">Bu yaklaşım, normalde Blazor sunucu tarafı uygulamalar için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="5c721-204">This approach isn't normally applicable to Blazor server-side apps.</span></span> <span data-ttu-id="5c721-205">Durum oluşturulduktan hemen sonra Blazor sunucu tarafı uygulamalar, kimlik doğrulama durumu bildirin.</span><span class="sxs-lookup"><span data-stu-id="5c721-205">Blazor server-side apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="5c721-206">`Authorizing` içeriği bir Blazor sunucu-tarafı uygulaması'nın sağlanabilir `AuthorizeView` bileşeni, ancak içerik hiçbir zaman görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="5c721-206">`Authorizing` content can be provided in a Blazor server-side app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="5c721-207">[Özniteliği yetkilendirmek]</span><span class="sxs-lookup"><span data-stu-id="5c721-207">[Authorize] attribute</span></span>

<span data-ttu-id="5c721-208">Yalnızca bir uygulama kullanabilirsiniz gibi `[Authorize]` bir MVC denetleyicisi ya da bir Razor sayfası `[Authorize]` Razor bileşenleri ile de kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="5c721-208">Just like an app can use `[Authorize]` with an MVC controller or Razor page, `[Authorize]` can also be used with Razor Components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="5c721-209">Yalnızca `[Authorize]` üzerinde `@page` bileşenleri Blazor yönlendirici erişildi.</span><span class="sxs-lookup"><span data-stu-id="5c721-209">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="5c721-210">Yetkilendirme yalnızca Yönlendirme bir özelliği gerçekleştirilir ve *değil* içinde bir sayfa alt bileşenler için.</span><span class="sxs-lookup"><span data-stu-id="5c721-210">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="5c721-211">Bir sayfa içinde belirli bölümlerinin görüntülenmesini yetkilendirmek için `AuthorizeView` yerine.</span><span class="sxs-lookup"><span data-stu-id="5c721-211">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="5c721-212">Eklemeniz gerekebilir `@using Microsoft.AspNetCore.Authorization` bileşeni için veya çok *_Imports.razor* derlemek Bileşen dosyası.</span><span class="sxs-lookup"><span data-stu-id="5c721-212">You may need to add `@using Microsoft.AspNetCore.Authorization` either to the component or to the *_Imports.razor* file in order for the component to compile.</span></span>

<span data-ttu-id="5c721-213">`[Authorize]` Özniteliği, rol tabanlı veya ilke tabanlı yetkilendirme da destekler.</span><span class="sxs-lookup"><span data-stu-id="5c721-213">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="5c721-214">Rol tabanlı yetkilendirme için kullanan `Roles` parametresi:</span><span class="sxs-lookup"><span data-stu-id="5c721-214">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="5c721-215">İlke tabanlı yetkilendirme için kullanan `Policy` parametresi:</span><span class="sxs-lookup"><span data-stu-id="5c721-215">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="5c721-216">Kullanılmazsa `Roles` ya da `Policy` belirtilen `[Authorize]` varsayılan olarak değerlendirilecek olan varsayılan ilkeyi kullanır:</span><span class="sxs-lookup"><span data-stu-id="5c721-216">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="5c721-217">Yetkili olarak kimlik doğrulaması (oturum açma) kullanıcılar.</span><span class="sxs-lookup"><span data-stu-id="5c721-217">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="5c721-218">Kimliği doğrulanmamış (imzalı genişletme) kullanıcıların yetkisiz olarak.</span><span class="sxs-lookup"><span data-stu-id="5c721-218">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="5c721-219">Yetkisiz içerik yönlendirici bileşeni ile özelleştirme</span><span class="sxs-lookup"><span data-stu-id="5c721-219">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="5c721-220">`Router` Bileşeni, özel içerik belirtmek için uygulamayı sağlar:</span><span class="sxs-lookup"><span data-stu-id="5c721-220">The `Router` component allows the app to specify custom content if:</span></span>

* <span data-ttu-id="5c721-221">İçerik bulunamadığında.</span><span class="sxs-lookup"><span data-stu-id="5c721-221">Content isn't found.</span></span>
* <span data-ttu-id="5c721-222">Kullanıcı bir `[Authorize]` bileşenine uygulanmış koşul.</span><span class="sxs-lookup"><span data-stu-id="5c721-222">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="5c721-223">`[Authorize]` Özniteliği alınmıştır [[Authorize] özniteliği](#authorize-attribute) bölümü.</span><span class="sxs-lookup"><span data-stu-id="5c721-223">The `[Authorize]` attribute is covered in the [[Authorize] attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="5c721-224">Zaman uyumsuz bir kimlik doğrulama işlemi devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="5c721-224">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="5c721-225">Varsayılan Blazor sunucu tarafı proje şablonunda *App.razor* dosyası özel içerik ayarlama işlemini gösterir:</span><span class="sxs-lookup"><span data-stu-id="5c721-225">In the default Blazor server-side project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        <NotFoundContent>
            <h1>Sorry</h1>
            <p>Sorry, there's nothing at this address.</p>
        </NotFoundContent>
        <NotAuthorizedContent>
            <h1>Sorry</h1>
            <p>You're not authorized to reach this page.</p>
            <p>You may need to log in as a different user.</p>
        </NotAuthorizedContent>
        <AuthorizingContent>
            <h1>Authentication in progress</h1>
            <p>Only visible while authentication is in progress.</p>
        </AuthorizingContent>
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="5c721-226">İçeriği `<NotFoundContent>`, `<NotAuthorizedContent>`, ve `<AuthorizingContent>` etkileşimli diğer bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="5c721-226">The content of `<NotFoundContent>`, `<NotAuthorizedContent>`, and `<AuthorizingContent>` can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="5c721-227">Varsa `<NotAuthorizedContent>` belirtilmediyse, yönlendirici, aşağıdaki geri dönüş iletisi kullanır:</span><span class="sxs-lookup"><span data-stu-id="5c721-227">If `<NotAuthorizedContent>` isn't specified, the router uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="5c721-228">Kimlik doğrulaması durum değişiklikleri hakkında bildirim</span><span class="sxs-lookup"><span data-stu-id="5c721-228">Notification about authentication state changes</span></span>

<span data-ttu-id="5c721-229">Uygulama (örneğin, oturumunuz kullanıcı veya başka bir kullanıcı rollerinin değiştiğinden) temel kimlik doğrulama durumu verilerini değiştiğini, belirlerse özel `AuthenticationStateProvider` isteğe bağlı olarak yöntem çağırabilirsiniz `NotifyAuthenticationStateChanged` üzerinde `AuthenticationStateProvider` temel sınıf.</span><span class="sxs-lookup"><span data-stu-id="5c721-229">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="5c721-230">Bu kimlik doğrulama durumu veri tüketicilerinin bildirir (örneğin, `AuthorizeView`) yeni verileri kullanarak rerender için.</span><span class="sxs-lookup"><span data-stu-id="5c721-230">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="5c721-231">Yordam mantığı</span><span class="sxs-lookup"><span data-stu-id="5c721-231">Procedural logic</span></span>

<span data-ttu-id="5c721-232">Yetkilendirme kuralları yordam mantıksal bir parçası olarak denetlemek için uygulamanın gerekiyorsa art arda parametre türü kullanmak `Task<AuthenticationState>` kullanıcının edinme <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="5c721-232">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="5c721-233">`Task<AuthenticationState>` diğer hizmetlerle gibi birleştirilebilir `IAuthorizationService`ilkelerini değerlendirme için.</span><span class="sxs-lookup"><span data-stu-id="5c721-233">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```cshtml
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

## <a name="authorization-in-blazor-client-side-apps"></a><span data-ttu-id="5c721-234">Yetkilendirme Blazor istemci tarafı uygulamalar</span><span class="sxs-lookup"><span data-stu-id="5c721-234">Authorization in Blazor client-side apps</span></span>

<span data-ttu-id="5c721-235">Tüm istemci tarafı kod, kullanıcılar tarafından değiştirilebilir çünkü Blazor istemci tarafı uygulamalar, yetkilendirme denetimleri atlanabilir.</span><span class="sxs-lookup"><span data-stu-id="5c721-235">In Blazor client-side apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="5c721-236">Aynı, JavaScript SPA'ya çerçeveleri ya da tüm işletim sistemlerine yönelik yerel uygulamaları dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5c721-236">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="5c721-237">**Her zaman sunucu, istemci tarafı uygulamanız tarafından erişilen herhangi bir API uç noktalarını içinde yetkilendirme denetimleri gerçekleştirin.**</span><span class="sxs-lookup"><span data-stu-id="5c721-237">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="5c721-238">Hatalarını giderme</span><span class="sxs-lookup"><span data-stu-id="5c721-238">Troubleshoot errors</span></span>

<span data-ttu-id="5c721-239">Sık karşılaşılan hatalar:</span><span class="sxs-lookup"><span data-stu-id="5c721-239">Common errors:</span></span>

* <span data-ttu-id="5c721-240">**Yetkilendirme gerektirir basamaklı bir parametre türü görev\<AuthenticationState >. Bunu sağlamak için CascadingAuthenticationState kullanmayı düşünün.**</span><span class="sxs-lookup"><span data-stu-id="5c721-240">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="5c721-241">**`null` için değeri alındı `authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="5c721-241">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="5c721-242">Kimlik doğrulaması etkin Blazor sunucu tarafı şablon kullanarak proje oluşturulmadıysa olasıdır.</span><span class="sxs-lookup"><span data-stu-id="5c721-242">It's likely that the project wasn't created using a Blazor server-side template with authentication enabled.</span></span> <span data-ttu-id="5c721-243">Kaydırma bir `<CascadingAuthenticationState>` bazı kısımları örneğin UI ağacı etrafında *App.razor* gibi:</span><span class="sxs-lookup"><span data-stu-id="5c721-243">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="5c721-244">`CascadingAuthenticationState` Sağlayan `Task<AuthenticationState>` sırayla temel aldığı basamaklı parametresi `AuthenticationStateProvider` DI hizmeti.</span><span class="sxs-lookup"><span data-stu-id="5c721-244">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c721-245">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5c721-245">Additional resources</span></span>

* <xref:security/index>
* <xref:security/authentication/windowsauth>
