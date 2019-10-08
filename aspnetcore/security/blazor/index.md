---
title: ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme
author: guardrex
description: Blazor kimlik doğrulaması ve yetkilendirme senaryoları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: security/blazor/index
ms.openlocfilehash: 1fcd54e954d09e66b8bb1c9a51ef56193f3acf93
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007430"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="1605b-103">ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="1605b-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="1605b-104">[Steve Sanderson](https://github.com/SteveSandersonMS) tarafından</span><span class="sxs-lookup"><span data-stu-id="1605b-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="1605b-105">ASP.NET Core, Blazor uygulamalarında güvenliğin yapılandırmasını ve yönetimini destekler.</span><span class="sxs-lookup"><span data-stu-id="1605b-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="1605b-106">Güvenlik senaryoları Blazor Server ve Blazor WebAssembly Apps arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="1605b-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="1605b-107">Blazor sunucu uygulamaları sunucuda çalıştığı için, yetkilendirme denetimleri şunları tespit edebilir:</span><span class="sxs-lookup"><span data-stu-id="1605b-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="1605b-108">Kullanıcıya sunulan kullanıcı ARABIRIMI seçenekleri (örneğin, bir kullanıcı için hangi menü girişlerinin kullanılabildiği).</span><span class="sxs-lookup"><span data-stu-id="1605b-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="1605b-109">Uygulama ve bileşenlerin bölgeleri için erişim kuralları.</span><span class="sxs-lookup"><span data-stu-id="1605b-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="1605b-110">Blazor WebAssembly Apps, istemcide çalışır.</span><span class="sxs-lookup"><span data-stu-id="1605b-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="1605b-111">Yetkilendirme *yalnızca* hangi kullanıcı arabirimi seçeneklerinin gösterileceğini belirlemede kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1605b-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="1605b-112">İstemci tarafı denetimleri bir kullanıcı tarafından değiştirililerek veya atlandığından, bir Blazor WebAssembly uygulaması yetkilendirme erişim kurallarını zorunlu kılamaz.</span><span class="sxs-lookup"><span data-stu-id="1605b-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="1605b-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="1605b-113">Authentication</span></span>

<span data-ttu-id="1605b-114">Blazor, kullanıcının kimliğini kurmak için mevcut ASP.NET Core kimlik doğrulama mekanizmalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="1605b-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="1605b-115">Tam mekanizma Blazor uygulamasının nasıl barındırıldığını, Blazor Server veya Blazor WebAssembly öğesine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="1605b-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="1605b-116">Blazor sunucusu kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="1605b-116">Blazor Server authentication</span></span>

<span data-ttu-id="1605b-117">Blazor Server uygulamaları, SignalR kullanılarak oluşturulan gerçek zamanlı bir bağlantı üzerinden çalışır.</span><span class="sxs-lookup"><span data-stu-id="1605b-117">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="1605b-118">[SignalR tabanlı uygulamalarda kimlik doğrulaması](xref:signalr/authn-and-authz) , bağlantı kurulduunda işlenir.</span><span class="sxs-lookup"><span data-stu-id="1605b-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="1605b-119">Kimlik doğrulaması, bir tanımlama bilgisine veya başka bir taşıyıcı belirtecine dayalı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="1605b-120">Blazor sunucusu proje şablonu, proje oluşturulduğunda kimlik doğrulamasını sizin için ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1605b-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1605b-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1605b-122">Kimlik doğrulama mekanizmasına sahip yeni bir Blazor Server projesi oluşturmak için <xref:blazor/get-started> makalesindeki Visual Studio kılavuzunu izleyin.</span><span class="sxs-lookup"><span data-stu-id="1605b-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="1605b-123">**Yeni ASP.NET Core Web uygulaması oluştur** Iletişim kutusunda **Blazor Server uygulama** şablonunu seçtikten sonra, **kimlik doğrulaması**altında **Değiştir** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1605b-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="1605b-124">Diğer ASP.NET Core projelerine yönelik aynı kimlik doğrulama mekanizması kümesini sunmak için bir iletişim kutusu açılır:</span><span class="sxs-lookup"><span data-stu-id="1605b-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="1605b-125">**Kimlik doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="1605b-125">**No Authentication**</span></span>
* <span data-ttu-id="1605b-126">@No__t **bireysel kullanıcı hesapları** , 1 Kullanıcı hesabı depolanabilir:</span><span class="sxs-lookup"><span data-stu-id="1605b-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="1605b-127">ASP.NET Core [kimlik](xref:security/authentication/identity) sistemini kullanarak uygulama içinde.</span><span class="sxs-lookup"><span data-stu-id="1605b-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="1605b-128">[Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="1605b-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="1605b-129">**İş veya okul hesapları**</span><span class="sxs-lookup"><span data-stu-id="1605b-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="1605b-130">**Windows kimlik doğrulaması**</span><span class="sxs-lookup"><span data-stu-id="1605b-130">**Windows Authentication**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1605b-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1605b-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1605b-132">Bir kimlik doğrulama mekanizması ile yeni bir Blazor Server projesi oluşturmak için <xref:blazor/get-started> makalesindeki Visual Studio Code kılavuzunu izleyin:</span><span class="sxs-lookup"><span data-stu-id="1605b-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="1605b-133">İzin verilen kimlik doğrulama değerleri (`{AUTHENTICATION}`) aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="1605b-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="1605b-134">Kimlik doğrulama mekanizması</span><span class="sxs-lookup"><span data-stu-id="1605b-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="1605b-135">`{AUTHENTICATION}` değeri</span><span class="sxs-lookup"><span data-stu-id="1605b-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="1605b-136">Kimlik doğrulaması yok</span><span class="sxs-lookup"><span data-stu-id="1605b-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="1605b-137">Ye</span><span class="sxs-lookup"><span data-stu-id="1605b-137">Individual</span></span><br><span data-ttu-id="1605b-138">Uygulamada ASP.NET Core kimlikle depolanan kullanıcılar.</span><span class="sxs-lookup"><span data-stu-id="1605b-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="1605b-139">Ye</span><span class="sxs-lookup"><span data-stu-id="1605b-139">Individual</span></span><br><span data-ttu-id="1605b-140">[Azure AD B2C](xref:security/authentication/azure-ad-b2c)' de depolanan kullanıcılar.</span><span class="sxs-lookup"><span data-stu-id="1605b-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="1605b-141">İş veya okul hesapları</span><span class="sxs-lookup"><span data-stu-id="1605b-141">Work or School Accounts</span></span><br><span data-ttu-id="1605b-142">Tek bir kiracı için kuruluş kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="1605b-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="1605b-143">İş veya okul hesapları</span><span class="sxs-lookup"><span data-stu-id="1605b-143">Work or School Accounts</span></span><br><span data-ttu-id="1605b-144">Birden çok kiracı için kuruluş kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="1605b-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="1605b-145">Windows Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="1605b-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="1605b-146">Komut, `{APP NAME}` yer tutucusu için belirtilen değere sahip adlı bir klasör oluşturur ve uygulamanın adı olarak klasör adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="1605b-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="1605b-147">Daha fazla bilgi için .NET Core kılavuzundaki [DotNet New](/dotnet/core/tools/dotnet-new) komutuna bakın.</span><span class="sxs-lookup"><span data-stu-id="1605b-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
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

### <a name="blazor-webassembly-authentication"></a><span data-ttu-id="1605b-148">Blazor WebAssembly kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="1605b-148">Blazor WebAssembly authentication</span></span>

<span data-ttu-id="1605b-149">Blazor WebAssembly uygulamalarında, tüm istemci tarafı kodlar kullanıcılar tarafından değiştirilemediği için kimlik doğrulama denetimleri atlanabilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="1605b-150">Aynı, JavaScript SPA çerçeveleri veya herhangi bir işletim sistemi için yerel uygulamalar dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1605b-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="1605b-151">Uygulamanın proje dosyasına [Microsoft. AspNetCore. components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) için bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1605b-151">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="1605b-152">Blazor WebAssembly uygulamaları için özel `AuthenticationStateProvider` hizmeti uygulaması aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1605b-152">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="1605b-153">AuthenticationStateProvider hizmeti</span><span class="sxs-lookup"><span data-stu-id="1605b-153">AuthenticationStateProvider service</span></span>

<span data-ttu-id="1605b-154">Blazor Server uygulamaları, ASP.NET Core `HttpContext.User` ' den kimlik doğrulama durumu verilerini alan yerleşik bir `AuthenticationStateProvider` hizmeti içerir.</span><span class="sxs-lookup"><span data-stu-id="1605b-154">Blazor Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="1605b-155">Kimlik doğrulama durumu, mevcut ASP.NET Core sunucu tarafı kimlik doğrulama mekanizmalarıyla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-155">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="1605b-156">`AuthenticationStateProvider`, kimlik doğrulama durumunu almak için `AuthorizeView` bileşeni ve `CascadingAuthenticationState` bileşeni tarafından kullanılan temel hizmettir.</span><span class="sxs-lookup"><span data-stu-id="1605b-156">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="1605b-157">Genellikle `AuthenticationStateProvider` ' i doğrudan kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="1605b-157">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="1605b-158">Bu makalenin ilerleyen kısımlarında açıklanan [Authorizeview bileşenini](#authorizeview-component) veya [Task @ no__t-2](#expose-the-authentication-state-as-a-cascading-parameter) yaklaşımlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="1605b-158">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="1605b-159">@No__t-0 kullanmanın ana dezavantajı, temeldeki kimlik doğrulama durumu verileri değişirse bileşen tarafından otomatik olarak bildirilmemektedir.</span><span class="sxs-lookup"><span data-stu-id="1605b-159">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="1605b-160">@No__t-0 hizmeti, aşağıdaki örnekte gösterildiği gibi geçerli kullanıcının <xref:System.Security.Claims.ClaimsPrincipal> verilerini sağlayabilir:</span><span class="sxs-lookup"><span data-stu-id="1605b-160">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```cshtml
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
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

<span data-ttu-id="1605b-161">@No__t-0 `true` ise ve Kullanıcı bir <xref:System.Security.Claims.ClaimsPrincipal> ise, talepler numaralandırılabilir ve değerlendirilen rollerde üyelik yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-161">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="1605b-162">Bağımlılık ekleme (dı) ve hizmetleri hakkında daha fazla bilgi için bkz. <xref:blazor/dependency-injection> ve <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="1605b-162">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="1605b-163">Özel bir AuthenticationStateProvider uygulama</span><span class="sxs-lookup"><span data-stu-id="1605b-163">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="1605b-164">Bir Blazor WebAssembly uygulaması oluşturuyorsanız veya uygulamanızın belirtimi kesinlikle özel bir sağlayıcı gerektiriyorsa, bir sağlayıcı uygulayın ve geçersiz kılın `GetAuthenticationStateAsync`:</span><span class="sxs-lookup"><span data-stu-id="1605b-164">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

namespace BlazorSample.Services
{
    public class CustomAuthStateProvider : AuthenticationStateProvider
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
}
```

<span data-ttu-id="1605b-165">@No__t-0 hizmeti `Startup.ConfigureServices` ' de kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="1605b-165">The `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Components.Authorization;
// using BlazorSample.Services;

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="1605b-166">@No__t-0 ' ı kullanarak, tüm kullanıcılar Kullanıcı adı `mrfibuli` ile doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="1605b-166">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="1605b-167">Kimlik doğrulama durumunu basamaklı bir parametre olarak kullanıma sunma</span><span class="sxs-lookup"><span data-stu-id="1605b-167">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="1605b-168">Kullanıcı tarafından tetiklenen bir eylem gerçekleştirirken olduğu gibi, yordamsal mantık için kimlik doğrulama durumu verileri gerekliyse, `Task<AuthenticationState>` türünde basamaklı bir parametre tanımlayarak kimlik doğrulama durumu verilerini alın:</span><span class="sxs-lookup"><span data-stu-id="1605b-168">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

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

> [!NOTE]
> <span data-ttu-id="1605b-169">Bir Blazor WebAssembly uygulama bileşeninde `Microsoft.AspNetCore.Components.Authorization` ad alanını (`@using Microsoft.AspNetCore.Components.Authorization`) ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1605b-169">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="1605b-170">@No__t-0 ' ı `true` ise, talepler, değerlendirilen rollere göre numaralandırılabilir ve üyelik yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-170">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="1605b-171">@No__t-1 ve `CascadingAuthenticationState` bileşenlerini kullanarak `Task<AuthenticationState>` basamaklı parametresini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="1605b-171">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

## <a name="authorization"></a><span data-ttu-id="1605b-172">Authorization</span><span class="sxs-lookup"><span data-stu-id="1605b-172">Authorization</span></span>

<span data-ttu-id="1605b-173">Bir kullanıcının kimliği doğrulandıktan sonra, kullanıcının neler yapabileceğini denetlemek için *Yetkilendirme* kuralları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="1605b-173">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="1605b-174">Erişim, genellikle aşağıdakileri yapıp verilmeksizin verilir veya reddedilir:</span><span class="sxs-lookup"><span data-stu-id="1605b-174">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="1605b-175">Bir kullanıcının kimliği doğrulanır (oturum açıldı).</span><span class="sxs-lookup"><span data-stu-id="1605b-175">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="1605b-176">Bir Kullanıcı bir *roldür*.</span><span class="sxs-lookup"><span data-stu-id="1605b-176">A user is in a *role*.</span></span>
* <span data-ttu-id="1605b-177">Bir kullanıcının *talebi*vardır.</span><span class="sxs-lookup"><span data-stu-id="1605b-177">A user has a *claim*.</span></span>
* <span data-ttu-id="1605b-178">Bir *ilke* karşılandı.</span><span class="sxs-lookup"><span data-stu-id="1605b-178">A *policy* is satisfied.</span></span>

<span data-ttu-id="1605b-179">Bu kavramların her biri, ASP.NET Core MVC veya Razor Pages uygulamasındaki ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="1605b-179">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="1605b-180">ASP.NET Core güvenliği hakkında daha fazla bilgi için, [ASP.NET Core güvenlik ve kimlik](xref:security/index)' in altındaki makalelere bakın.</span><span class="sxs-lookup"><span data-stu-id="1605b-180">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="1605b-181">AuthorizeView bileşeni</span><span class="sxs-lookup"><span data-stu-id="1605b-181">AuthorizeView component</span></span>

<span data-ttu-id="1605b-182">@No__t-0 bileşeni, kullanıcının onu görme yetkisine sahip olup olmadığına bağlı olarak Kullanıcı ARABIRIMINI seçmeli olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="1605b-182">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="1605b-183">Bu yaklaşım yalnızca Kullanıcı için veri *görüntülemesi* gerektiğinde ve Kullanıcı kimliğini yordamsal mantığda kullanmanıza gerek olmadığında yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="1605b-183">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="1605b-184">Bileşeni, oturum açmış kullanıcıyla ilgili bilgilere erişmek için kullanabileceğiniz `AuthenticationState` türünde `context` değişkeni kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="1605b-184">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="1605b-185">Kullanıcının kimliği doğrulanmadıysa, görüntülenmek üzere farklı içerikler de sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="1605b-185">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="1605b-186">@No__t-0 ve `<NotAuthorized>` etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-186">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="1605b-187">Kullanıcı arabirimi seçeneklerini veya erişimini denetleyen roller veya ilkeler gibi yetkilendirme koşulları, [Yetkilendirme](#authorization) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1605b-187">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="1605b-188">Yetkilendirme koşulları belirtilmemişse, `AuthorizeView` varsayılan bir ilke kullanır ve şu şekilde davranır:</span><span class="sxs-lookup"><span data-stu-id="1605b-188">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="1605b-189">Kimliği doğrulanmış (oturum açmış) kullanıcılar yetkili olarak.</span><span class="sxs-lookup"><span data-stu-id="1605b-189">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="1605b-190">Kimliği doğrulanmamış (oturumu açılmış) kullanıcılar yetkilendirilmemiş.</span><span class="sxs-lookup"><span data-stu-id="1605b-190">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="1605b-191">Rol tabanlı ve ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="1605b-191">Role-based and policy-based authorization</span></span>

<span data-ttu-id="1605b-192">@No__t-0 bileşeni *rol tabanlı* veya *ilke tabanlı* yetkilendirmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="1605b-192">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="1605b-193">Rol tabanlı yetkilendirme için `Roles` parametresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="1605b-193">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="1605b-194">Daha fazla bilgi için bkz. <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="1605b-194">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="1605b-195">İlke tabanlı yetkilendirme için `Policy` parametresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="1605b-195">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="1605b-196">Talep tabanlı yetkilendirme, ilke tabanlı yetkilendirme için özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="1605b-196">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="1605b-197">Örneğin, kullanıcıların belirli bir talebe sahip olmasını gerektiren bir ilke tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1605b-197">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="1605b-198">Daha fazla bilgi için bkz. <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="1605b-198">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="1605b-199">Bu API 'Ler, Blazor Server ya da Blazor WebAssembly uygulamalarında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-199">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="1605b-200">Ne `Roles` ne de `Policy` belirtilmemişse, `AuthorizeView` varsayılan ilkeyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="1605b-200">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="1605b-201">Zaman uyumsuz kimlik doğrulaması sırasında görünen içerik</span><span class="sxs-lookup"><span data-stu-id="1605b-201">Content displayed during asynchronous authentication</span></span>

<span data-ttu-id="1605b-202">Blazor, kimlik doğrulaması durumunun *zaman uyumsuz*olarak belirlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="1605b-202">Blazor allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="1605b-203">Bu yaklaşım için birincil senaryo, kimlik doğrulaması için bir dış uç noktaya istek yapan Blazor WebAssembly uygulamalarında bulunur.</span><span class="sxs-lookup"><span data-stu-id="1605b-203">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="1605b-204">Kimlik doğrulaması devam ederken, `AuthorizeView` varsayılan olarak içerik görüntülemez.</span><span class="sxs-lookup"><span data-stu-id="1605b-204">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="1605b-205">Kimlik doğrulama gerçekleştiğinde içeriği göstermek için `<Authorizing>` öğesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="1605b-205">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="1605b-206">Bu yaklaşım normalde Blazor Server uygulamaları için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="1605b-206">This approach isn't normally applicable to Blazor Server apps.</span></span> <span data-ttu-id="1605b-207">Blazor sunucu uygulamaları, durum belirlenir oluşturmaz kimlik doğrulama durumunu bilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-207">Blazor Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="1605b-208">`Authorizing` içeriği bir Blazor Server uygulamasının `AuthorizeView` bileşeninde bulunabilir, ancak içerik hiçbir şekilde gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="1605b-208">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="1605b-209">[Yetkilendir] özniteliği</span><span class="sxs-lookup"><span data-stu-id="1605b-209">[Authorize] attribute</span></span>

<span data-ttu-id="1605b-210">@No__t-0 özniteliği Razor bileşenlerinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="1605b-210">The `[Authorize]` attribute can be used in Razor components:</span></span>

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="1605b-211">Bir Blazor WebAssembly uygulama bileşeninde, bu bölümdeki örneklere `Microsoft.AspNetCore.Authorization` ad alanını (`@using Microsoft.AspNetCore.Authorization`) ekleyin.</span><span class="sxs-lookup"><span data-stu-id="1605b-211">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1605b-212">Blazor yönlendiricisi aracılığıyla yalnızca `@page` bileşenlerinde erişilen `[Authorize]` kullanın.</span><span class="sxs-lookup"><span data-stu-id="1605b-212">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="1605b-213">Yetkilendirme yalnızca, bir sayfada işlenen alt bileşenler için *değil* , yönlendirmenin bir yönü olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-213">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="1605b-214">Bir sayfa içindeki belirli bölümlerin görüntülenmesini yetkilendirmek için bunun yerine `AuthorizeView` kullanın.</span><span class="sxs-lookup"><span data-stu-id="1605b-214">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="1605b-215">@No__t-0 özniteliği rol tabanlı veya ilke tabanlı yetkilendirmeyi de destekler.</span><span class="sxs-lookup"><span data-stu-id="1605b-215">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="1605b-216">Rol tabanlı yetkilendirme için `Roles` parametresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="1605b-216">For role-based authorization, use the `Roles` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="1605b-217">İlke tabanlı yetkilendirme için `Policy` parametresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="1605b-217">For policy-based authorization, use the `Policy` parameter:</span></span>

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="1605b-218">@No__t-0 veya `Policy` belirtilmemişse, `[Authorize]` varsayılan ilkeyi kullanır, bu varsayılan olarak değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="1605b-218">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="1605b-219">Kimliği doğrulanmış (oturum açmış) kullanıcılar yetkili olarak.</span><span class="sxs-lookup"><span data-stu-id="1605b-219">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="1605b-220">Kimliği doğrulanmamış (oturumu açılmış) kullanıcılar yetkilendirilmemiş.</span><span class="sxs-lookup"><span data-stu-id="1605b-220">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="1605b-221">Yönlendirici bileşeniyle yetkisiz içeriği özelleştirme</span><span class="sxs-lookup"><span data-stu-id="1605b-221">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="1605b-222">@No__t-1 bileşeniyle birlikte `Router` bileşeni, uygulamanın şu durumlarda özel içerik belirlemesine izin verir:</span><span class="sxs-lookup"><span data-stu-id="1605b-222">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="1605b-223">İçerik bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="1605b-223">Content isn't found.</span></span>
* <span data-ttu-id="1605b-224">Kullanıcı, bileşene uygulanan `[Authorize]` koşulunu başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="1605b-224">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="1605b-225">@No__t-0 özniteliği [[Yetkilendir] öznitelik](#authorize-attribute) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="1605b-225">The `[Authorize]` attribute is covered in the [[Authorize] attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="1605b-226">Zaman uyumsuz kimlik doğrulama devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="1605b-226">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="1605b-227">Varsayılan Blazor Server proje şablonunda, *app. Razor* dosyası nasıl özel içerik ayarlanacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="1605b-227">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```cshtml
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

<span data-ttu-id="1605b-228">@No__t-0, `<NotAuthorized>` ve `<Authorizing>` etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-228">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="1605b-229">@No__t-0 öğesi belirtilmemişse, `AuthorizeRouteView` aşağıdaki geri dönüş iletisini kullanır:</span><span class="sxs-lookup"><span data-stu-id="1605b-229">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="1605b-230">Kimlik doğrulama durumu değişiklikleri hakkında bildirim</span><span class="sxs-lookup"><span data-stu-id="1605b-230">Notification about authentication state changes</span></span>

<span data-ttu-id="1605b-231">Uygulama, temeldeki kimlik doğrulama durumu verilerinin değiştiğini belirlerse (örneğin, Kullanıcı oturumu kapattığından veya başka bir kullanıcı rollerini değiştirse), özel bir `AuthenticationStateProvider` isteğe bağlı olarak `AuthenticationStateProvider` temel sınıfında `NotifyAuthenticationStateChanged` metodunu çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-231">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="1605b-232">Bu, yeni verileri kullanarak yeniden kimlik doğrulama durumu verilerinin (örneğin, `AuthorizeView`) bir şekilde yeniden kullanılması hakkında bilgilendirir.</span><span class="sxs-lookup"><span data-stu-id="1605b-232">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="1605b-233">Yordamsal mantık</span><span class="sxs-lookup"><span data-stu-id="1605b-233">Procedural logic</span></span>

<span data-ttu-id="1605b-234">Uygulama, yordamsal mantığın bir parçası olarak yetkilendirme kurallarını denetmek için gerekliyse, kullanıcının <xref:System.Security.Claims.ClaimsPrincipal> ' i elde etmek için `Task<AuthenticationState>` türünde basamaklı bir parametre kullanın.</span><span class="sxs-lookup"><span data-stu-id="1605b-234">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="1605b-235">`Task<AuthenticationState>`, ilkeleri değerlendirmek için `IAuthorizationService` gibi diğer hizmetlerle birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-235">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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

> [!NOTE]
> <span data-ttu-id="1605b-236">Blazor WebAssembly uygulama bileşeninde `Microsoft.AspNetCore.Authorization` ve `Microsoft.AspNetCore.Components.Authorization` ad alanlarını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1605b-236">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```cshtml
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-blazor-webassembly-apps"></a><span data-ttu-id="1605b-237">Blazor WebAssembly uygulamalarında yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="1605b-237">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="1605b-238">Blazor WebAssembly uygulamalarında, tüm istemci tarafı kodlar kullanıcılar tarafından değiştirilemediği için yetkilendirme denetimleri atlanabilir.</span><span class="sxs-lookup"><span data-stu-id="1605b-238">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="1605b-239">Aynı, JavaScript SPA çerçeveleri veya herhangi bir işletim sistemi için yerel uygulamalar dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1605b-239">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="1605b-240">**İstemci tarafı uygulamanız tarafından erişilen tüm API uç noktalarında sunucuda her zaman yetkilendirme denetimleri gerçekleştirin.**</span><span class="sxs-lookup"><span data-stu-id="1605b-240">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="1605b-241">Sorun giderme hataları</span><span class="sxs-lookup"><span data-stu-id="1605b-241">Troubleshoot errors</span></span>

<span data-ttu-id="1605b-242">Yaygın hatalar:</span><span class="sxs-lookup"><span data-stu-id="1605b-242">Common errors:</span></span>

* <span data-ttu-id="1605b-243">**Authorization, Task @ no__t-1AuthenticationState > türünde bir geçişli parametre gerektirir. Bunu sağlamak için basamaklı Dingauthenticationstate kullanmayı düşünün.**</span><span class="sxs-lookup"><span data-stu-id="1605b-243">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="1605b-244">**`null` değeri `authenticationStateTask` için alındı**</span><span class="sxs-lookup"><span data-stu-id="1605b-244">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="1605b-245">Projenin kimlik doğrulaması etkin bir Blazor sunucu şablonu kullanılarak oluşturulmamış olması olasıdır.</span><span class="sxs-lookup"><span data-stu-id="1605b-245">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="1605b-246">@No__t-0 ' ı, UI ağacının bazı bir bölümünde (örneğin, *app. Razor* ) aşağıdaki gibi sarmalayın:</span><span class="sxs-lookup"><span data-stu-id="1605b-246">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="1605b-247">@No__t-0, temel alınan @no__t 2 DI hizmetinden aldığı `Task<AuthenticationState>` geçişli parametre sağlar.</span><span class="sxs-lookup"><span data-stu-id="1605b-247">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1605b-248">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1605b-248">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
