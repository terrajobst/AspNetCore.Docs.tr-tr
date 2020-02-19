---
title: ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme
author: guardrex
description: Blazor kimlik doğrulaması ve yetkilendirme senaryoları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: c07ffdbd5df58d6b3d19a5d75ce224d830101eac
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447431"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="031a7-103">ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="031a7-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="031a7-104">[Steve Sanderson](https://github.com/SteveSandersonMS) tarafından</span><span class="sxs-lookup"><span data-stu-id="031a7-104">By [Steve Sanderson](https://github.com/SteveSandersonMS)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="031a7-105">ASP.NET Core, Blazor uygulamalarında güvenliğin yapılandırmasını ve yönetimini destekler.</span><span class="sxs-lookup"><span data-stu-id="031a7-105">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="031a7-106">Güvenlik senaryoları Blazor Server ve Blazor WebAssembly Apps arasında farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="031a7-106">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="031a7-107">Blazor sunucu uygulamaları sunucuda çalıştığı için, yetkilendirme denetimleri şunları tespit edebilir:</span><span class="sxs-lookup"><span data-stu-id="031a7-107">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="031a7-108">Kullanıcıya sunulan kullanıcı ARABIRIMI seçenekleri (örneğin, bir kullanıcı için hangi menü girişlerinin kullanılabildiği).</span><span class="sxs-lookup"><span data-stu-id="031a7-108">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="031a7-109">Uygulama ve bileşenlerin bölgeleri için erişim kuralları.</span><span class="sxs-lookup"><span data-stu-id="031a7-109">Access rules for areas of the app and components.</span></span>

<span data-ttu-id="031a7-110">Blazor WebAssembly Apps, istemcide çalışır.</span><span class="sxs-lookup"><span data-stu-id="031a7-110">Blazor WebAssembly apps run on the client.</span></span> <span data-ttu-id="031a7-111">Yetkilendirme *yalnızca* hangi kullanıcı arabirimi seçeneklerinin gösterileceğini belirlemede kullanılır.</span><span class="sxs-lookup"><span data-stu-id="031a7-111">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="031a7-112">İstemci tarafı denetimleri bir kullanıcı tarafından değiştirililerek veya atlandığından, bir Blazor WebAssembly uygulaması yetkilendirme erişim kurallarını zorunlu kılamaz.</span><span class="sxs-lookup"><span data-stu-id="031a7-112">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

## <a name="authentication"></a><span data-ttu-id="031a7-113">Kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="031a7-113">Authentication</span></span>

<span data-ttu-id="031a7-114">Blazor, kullanıcının kimliğini kurmak için mevcut ASP.NET Core kimlik doğrulama mekanizmalarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="031a7-114">Blazor uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="031a7-115">Tam mekanizma Blazor uygulamasının nasıl barındırıldığını, Blazor Server veya Blazor WebAssembly öğesine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="031a7-115">The exact mechanism depends on how the Blazor app is hosted, Blazor Server or Blazor WebAssembly.</span></span>

### <a name="blazor-server-authentication"></a><span data-ttu-id="031a7-116">Blazor sunucusu kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="031a7-116">Blazor Server authentication</span></span>

<span data-ttu-id="031a7-117">Blazor Server uygulamaları, SignalR kullanılarak oluşturulan gerçek zamanlı bir bağlantı üzerinden çalışır.</span><span class="sxs-lookup"><span data-stu-id="031a7-117">Blazor Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="031a7-118">[SignalR tabanlı uygulamalarda kimlik doğrulaması](xref:signalr/authn-and-authz) , bağlantı kurulduunda işlenir.</span><span class="sxs-lookup"><span data-stu-id="031a7-118">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="031a7-119">Kimlik doğrulaması, bir tanımlama bilgisine veya başka bir taşıyıcı belirtecine dayalı olabilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-119">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="031a7-120">Blazor sunucusu proje şablonu, proje oluşturulduğunda kimlik doğrulamasını sizin için ayarlayabilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-120">The Blazor Server project template can set up authentication for you when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="031a7-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="031a7-121">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="031a7-122">Kimlik doğrulama mekanizmasına sahip yeni bir Blazor Server projesi oluşturmak için <xref:blazor/get-started> makalesindeki Visual Studio kılavuzunu izleyin.</span><span class="sxs-lookup"><span data-stu-id="031a7-122">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="031a7-123">**Yeni ASP.NET Core Web uygulaması oluştur** Iletişim kutusunda **Blazor Server uygulama** şablonunu seçtikten sonra, **kimlik doğrulaması**altında **Değiştir** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="031a7-123">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="031a7-124">Diğer ASP.NET Core projelerine yönelik aynı kimlik doğrulama mekanizması kümesini sunmak için bir iletişim kutusu açılır:</span><span class="sxs-lookup"><span data-stu-id="031a7-124">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="031a7-125">**Kimlik doğrulaması yok**</span><span class="sxs-lookup"><span data-stu-id="031a7-125">**No Authentication**</span></span>
* <span data-ttu-id="031a7-126">Kullanıcı hesaplarının &ndash; **bireysel kullanıcı hesapları** depolanabilir:</span><span class="sxs-lookup"><span data-stu-id="031a7-126">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="031a7-127">ASP.NET Core [kimlik](xref:security/authentication/identity) sistemini kullanarak uygulama içinde.</span><span class="sxs-lookup"><span data-stu-id="031a7-127">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="031a7-128">[Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span><span class="sxs-lookup"><span data-stu-id="031a7-128">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="031a7-129">**İş veya okul hesapları**</span><span class="sxs-lookup"><span data-stu-id="031a7-129">**Work or School Accounts**</span></span>
* <span data-ttu-id="031a7-130">**Windows Kimlik Doğrulaması**</span><span class="sxs-lookup"><span data-stu-id="031a7-130">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="031a7-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="031a7-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="031a7-132">Kimlik doğrulama mekanizması ile yeni bir Blazor Server projesi oluşturmak için <xref:blazor/get-started> makalesindeki Visual Studio Code kılavuzunu izleyin:</span><span class="sxs-lookup"><span data-stu-id="031a7-132">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="031a7-133">İzin verilen kimlik doğrulama değerleri (`{AUTHENTICATION}`) aşağıdaki tabloda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="031a7-133">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="031a7-134">Kimlik doğrulama mekanizması</span><span class="sxs-lookup"><span data-stu-id="031a7-134">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="031a7-135">`{AUTHENTICATION}` değeri</span><span class="sxs-lookup"><span data-stu-id="031a7-135">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="031a7-136">Kimlik Doğrulaması Yok</span><span class="sxs-lookup"><span data-stu-id="031a7-136">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="031a7-137">Ye</span><span class="sxs-lookup"><span data-stu-id="031a7-137">Individual</span></span><br><span data-ttu-id="031a7-138">Uygulamada ASP.NET Core kimlikle depolanan kullanıcılar.</span><span class="sxs-lookup"><span data-stu-id="031a7-138">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="031a7-139">Ye</span><span class="sxs-lookup"><span data-stu-id="031a7-139">Individual</span></span><br><span data-ttu-id="031a7-140">[Azure AD B2C](xref:security/authentication/azure-ad-b2c)' de depolanan kullanıcılar.</span><span class="sxs-lookup"><span data-stu-id="031a7-140">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="031a7-141">İş veya okul hesapları</span><span class="sxs-lookup"><span data-stu-id="031a7-141">Work or School Accounts</span></span><br><span data-ttu-id="031a7-142">Tek bir kiracı için kuruluş kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="031a7-142">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="031a7-143">İş veya okul hesapları</span><span class="sxs-lookup"><span data-stu-id="031a7-143">Work or School Accounts</span></span><br><span data-ttu-id="031a7-144">Birden çok kiracı için kuruluş kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="031a7-144">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="031a7-145">Windows Kimlik Doğrulaması</span><span class="sxs-lookup"><span data-stu-id="031a7-145">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="031a7-146">Komut, `{APP NAME}` yer tutucusu için belirtilen değere sahip adlı bir klasör oluşturur ve uygulamanın adı olarak klasör adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="031a7-146">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="031a7-147">Daha fazla bilgi için .NET Core kılavuzundaki [DotNet New](/dotnet/core/tools/dotnet-new) komutuna bakın.</span><span class="sxs-lookup"><span data-stu-id="031a7-147">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

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

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor<span data-ttu-id="031a7-148"> WebAssembly kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="031a7-148"> WebAssembly authentication</span></span>

<span data-ttu-id="031a7-149">Blazor WebAssembly uygulamalarında, tüm istemci tarafı kodlar kullanıcılar tarafından değiştirilemediği için kimlik doğrulama denetimleri atlanabilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-149">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="031a7-150">Aynı, JavaScript SPA çerçeveleri veya herhangi bir işletim sistemi için yerel uygulamalar dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="031a7-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="031a7-151">Uygulamanın proje dosyasına [Microsoft. AspNetCore. components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) için bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="031a7-151">Add a package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>

<span data-ttu-id="031a7-152">Blazor WebAssembly uygulamaları için özel bir `AuthenticationStateProvider` hizmeti uygulaması aşağıdaki bölümlerde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="031a7-152">Implementation of a custom `AuthenticationStateProvider` service for Blazor WebAssembly apps is covered in the following sections.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="031a7-153">AuthenticationStateProvider hizmeti</span><span class="sxs-lookup"><span data-stu-id="031a7-153">AuthenticationStateProvider service</span></span>

Blazor<span data-ttu-id="031a7-154"> Server uygulamaları ASP.NET Core `HttpContext.User`kimlik doğrulama durumu verilerini alan yerleşik bir `AuthenticationStateProvider` hizmeti içerir.</span><span class="sxs-lookup"><span data-stu-id="031a7-154"> Server apps include a built-in `AuthenticationStateProvider` service that obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="031a7-155">Kimlik doğrulama durumu, mevcut ASP.NET Core sunucu tarafı kimlik doğrulama mekanizmalarıyla tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-155">This is how authentication state integrates with existing ASP.NET Core server-side authentication mechanisms.</span></span>

<span data-ttu-id="031a7-156">`AuthenticationStateProvider`, kimlik doğrulama durumunu almak için `AuthorizeView` bileşeni ve `CascadingAuthenticationState` bileşeni tarafından kullanılan temel hizmettir.</span><span class="sxs-lookup"><span data-stu-id="031a7-156">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="031a7-157">Genellikle `AuthenticationStateProvider` doğrudan kullanmazsınız.</span><span class="sxs-lookup"><span data-stu-id="031a7-157">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="031a7-158">Bu makalenin ilerleyen kısımlarında açıklanan [Authorizeview bileşenini](#authorizeview-component) veya [görev<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) yaklaşımlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="031a7-158">Use the [AuthorizeView component](#authorizeview-component) or [Task<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="031a7-159">`AuthenticationStateProvider` doğrudan kullanmanın ana dezavantajı, temeldeki kimlik doğrulama durumu verileri değişirse bileşen tarafından otomatik olarak bildirilmemektedir.</span><span class="sxs-lookup"><span data-stu-id="031a7-159">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="031a7-160">`AuthenticationStateProvider` hizmeti, aşağıdaki örnekte gösterildiği gibi geçerli kullanıcının <xref:System.Security.Claims.ClaimsPrincipal> verilerini sağlayabilir:</span><span class="sxs-lookup"><span data-stu-id="031a7-160">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="LogUsername">Write user info to console</button>

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

<span data-ttu-id="031a7-161">`user.Identity.IsAuthenticated` `true` ve Kullanıcı bir <xref:System.Security.Claims.ClaimsPrincipal>olduğundan, talepler, değerlendirilen rollerde numaralandırılabilir ve üyelik yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-161">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="031a7-162">Bağımlılık ekleme (dı) ve hizmetleri hakkında daha fazla bilgi için bkz. <xref:blazor/dependency-injection> ve <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="031a7-162">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="031a7-163">Özel bir AuthenticationStateProvider uygulama</span><span class="sxs-lookup"><span data-stu-id="031a7-163">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="031a7-164">Blazor WebAssembly uygulaması oluşturuyorsanız veya uygulamanızın belirtimi kesinlikle özel bir sağlayıcı gerektiriyorsa, bir sağlayıcı uygulayın ve `GetAuthenticationStateAsync`geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="031a7-164">If you're building a Blazor WebAssembly app or if your app's specification absolutely requires a custom provider, implement a provider and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="031a7-165">Blazor WebAssembly uygulamasında `CustomAuthStateProvider` hizmeti, *Program.cs*`Main` kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="031a7-165">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Blazor.Hosting;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.Extensions.DependencyInjection;
using BlazorSample.Services;

public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddScoped<AuthenticationStateProvider, 
            CustomAuthStateProvider>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

<span data-ttu-id="031a7-166">`CustomAuthStateProvider`kullanarak, tüm kullanıcıların Kullanıcı adı `mrfibuli`kimlik doğrulaması yapılır.</span><span class="sxs-lookup"><span data-stu-id="031a7-166">Using the `CustomAuthStateProvider`, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="031a7-167">Kimlik doğrulama durumunu basamaklı bir parametre olarak kullanıma sunma</span><span class="sxs-lookup"><span data-stu-id="031a7-167">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="031a7-168">Kullanıcı tarafından tetiklenen bir eylem gerçekleştirirken olduğu gibi, yordamsal mantık için kimlik doğrulama durumu verileri gerekliyse, `Task<AuthenticationState>`türünde bir geçişli parametre tanımlayarak kimlik doğrulama durumu verilerini alın:</span><span class="sxs-lookup"><span data-stu-id="031a7-168">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```razor
@page "/"

<button @onclick="LogUsername">Log username</button>

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
> <span data-ttu-id="031a7-169">Blazor WebAssembly uygulama bileşeninde, `Microsoft.AspNetCore.Components.Authorization` ad alanını (`@using Microsoft.AspNetCore.Components.Authorization`) ekleyin.</span><span class="sxs-lookup"><span data-stu-id="031a7-169">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Components.Authorization` namespace (`@using Microsoft.AspNetCore.Components.Authorization`).</span></span>

<span data-ttu-id="031a7-170">`user.Identity.IsAuthenticated` `true`, talepler numaralandırılabilir ve değerlendirilen rollerde üyeliğe eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-170">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="031a7-171">*App. Razor* dosyasındaki `AuthorizeRouteView` ve `CascadingAuthenticationState` bileşenlerini kullanarak `Task<AuthenticationState>` geçişli parametreyi ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="031a7-171">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the *App.razor* file:</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization

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

<span data-ttu-id="031a7-172">`Program.Main`seçenekler ve yetkilendirme için hizmetler ekleme:</span><span class="sxs-lookup"><span data-stu-id="031a7-172">Add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

## <a name="authorization"></a><span data-ttu-id="031a7-173">Yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="031a7-173">Authorization</span></span>

<span data-ttu-id="031a7-174">Bir kullanıcının kimliği doğrulandıktan sonra, kullanıcının neler yapabileceğini denetlemek için *Yetkilendirme* kuralları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="031a7-174">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="031a7-175">Erişim, genellikle aşağıdakileri yapıp verilmeksizin verilir veya reddedilir:</span><span class="sxs-lookup"><span data-stu-id="031a7-175">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="031a7-176">Bir kullanıcının kimliği doğrulanır (oturum açıldı).</span><span class="sxs-lookup"><span data-stu-id="031a7-176">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="031a7-177">Bir Kullanıcı bir *roldür*.</span><span class="sxs-lookup"><span data-stu-id="031a7-177">A user is in a *role*.</span></span>
* <span data-ttu-id="031a7-178">Bir kullanıcının *talebi*vardır.</span><span class="sxs-lookup"><span data-stu-id="031a7-178">A user has a *claim*.</span></span>
* <span data-ttu-id="031a7-179">Bir *ilke* karşılandı.</span><span class="sxs-lookup"><span data-stu-id="031a7-179">A *policy* is satisfied.</span></span>

<span data-ttu-id="031a7-180">Bu kavramların her biri, ASP.NET Core MVC veya Razor Pages uygulamasındaki ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="031a7-180">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="031a7-181">ASP.NET Core güvenliği hakkında daha fazla bilgi için, [ASP.NET Core güvenlik ve kimlik](xref:security/index)' in altındaki makalelere bakın.</span><span class="sxs-lookup"><span data-stu-id="031a7-181">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="031a7-182">AuthorizeView bileşeni</span><span class="sxs-lookup"><span data-stu-id="031a7-182">AuthorizeView component</span></span>

<span data-ttu-id="031a7-183">`AuthorizeView` bileşeni, kullanıcının onu görme yetkisine sahip olup olmadığına bağlı olarak Kullanıcı ARABIRIMINI seçmeli olarak görüntüler.</span><span class="sxs-lookup"><span data-stu-id="031a7-183">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="031a7-184">Bu yaklaşım yalnızca Kullanıcı için veri *görüntülemesi* gerektiğinde ve Kullanıcı kimliğini yordamsal mantığda kullanmanıza gerek olmadığında yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="031a7-184">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="031a7-185">Bileşeni, oturum açmış kullanıcıyla ilgili bilgilere erişmek için kullanabileceğiniz `AuthenticationState`türünde bir `context` değişkeni kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="031a7-185">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="031a7-186">Kullanıcının kimliği doğrulanmadıysa, görüntülenmek üzere farklı içerikler de sağlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="031a7-186">You can also supply different content for display if the user isn't authenticated:</span></span>

```razor
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

<span data-ttu-id="031a7-187">`<Authorized>` ve `<NotAuthorized>` etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-187">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="031a7-188">Kullanıcı arabirimi seçeneklerini veya erişimini denetleyen roller veya ilkeler gibi yetkilendirme koşulları, [Yetkilendirme](#authorization) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="031a7-188">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="031a7-189">Yetkilendirme koşulları belirtilmemişse, `AuthorizeView` varsayılan bir ilke kullanır ve şu şekilde davranır:</span><span class="sxs-lookup"><span data-stu-id="031a7-189">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="031a7-190">Kimliği doğrulanmış (oturum açmış) kullanıcılar yetkili olarak.</span><span class="sxs-lookup"><span data-stu-id="031a7-190">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="031a7-191">Kimliği doğrulanmamış (oturumu açılmış) kullanıcılar yetkilendirilmemiş.</span><span class="sxs-lookup"><span data-stu-id="031a7-191">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="031a7-192">Rol tabanlı ve ilke tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="031a7-192">Role-based and policy-based authorization</span></span>

<span data-ttu-id="031a7-193">`AuthorizeView` bileşeni *rol tabanlı* veya *ilke tabanlı* yetkilendirmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="031a7-193">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="031a7-194">Rol tabanlı yetkilendirme için `Roles` parametresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="031a7-194">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="031a7-195">Daha fazla bilgi için bkz. <xref:security/authorization/roles>.</span><span class="sxs-lookup"><span data-stu-id="031a7-195">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="031a7-196">İlke tabanlı yetkilendirme için `Policy` parametresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="031a7-196">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="031a7-197">Talep tabanlı yetkilendirme, ilke tabanlı yetkilendirme için özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="031a7-197">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="031a7-198">Örneğin, kullanıcıların belirli bir talebe sahip olmasını gerektiren bir ilke tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="031a7-198">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="031a7-199">Daha fazla bilgi için bkz. <xref:security/authorization/policies>.</span><span class="sxs-lookup"><span data-stu-id="031a7-199">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="031a7-200">Bu API 'Ler Blazor sunucuda Blazor ya da WebAssembly uygulamalarında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-200">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="031a7-201">Ne `Roles` ne de `Policy` belirtilmemişse, `AuthorizeView` varsayılan ilkeyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="031a7-201">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="031a7-202">Zaman uyumsuz kimlik doğrulaması sırasında görünen içerik</span><span class="sxs-lookup"><span data-stu-id="031a7-202">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="031a7-203">, kimlik doğrulaması durumunun *zaman uyumsuz*olarak belirlenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="031a7-203"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="031a7-204">Bu yaklaşım için birincil senaryo, kimlik doğrulaması için bir dış uç noktaya istek yapan Blazor WebAssembly Apps ' dedir.</span><span class="sxs-lookup"><span data-stu-id="031a7-204">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="031a7-205">Kimlik doğrulaması devam ederken, `AuthorizeView` varsayılan olarak içerik görüntülemez.</span><span class="sxs-lookup"><span data-stu-id="031a7-205">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="031a7-206">Kimlik doğrulama gerçekleştiğinde içeriği göstermek için `<Authorizing>` öğesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="031a7-206">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```razor
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

<span data-ttu-id="031a7-207">Bu yaklaşım normalde Blazor Server uygulamaları için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="031a7-207">This approach isn't normally applicable to Blazor Server apps.</span></span> Blazor<span data-ttu-id="031a7-208"> sunucu uygulamaları, durum oluşturulur almaz kimlik doğrulama durumunu bilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-208"> Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="031a7-209">`Authorizing` içerik Blazor sunucu uygulamasının `AuthorizeView` bileşeninde bulunabilir, ancak içerik hiçbir şekilde gösterilmez.</span><span class="sxs-lookup"><span data-stu-id="031a7-209">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="031a7-210">[Yetkilendir] özniteliği</span><span class="sxs-lookup"><span data-stu-id="031a7-210">[Authorize] attribute</span></span>

<span data-ttu-id="031a7-211">`[Authorize]` özniteliği Razor bileşenlerinde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="031a7-211">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> <span data-ttu-id="031a7-212">Blazor WebAssembly uygulama bileşeninde, bu bölümdeki örneklere `Microsoft.AspNetCore.Authorization` ad alanını (`@using Microsoft.AspNetCore.Authorization`) ekleyin.</span><span class="sxs-lookup"><span data-stu-id="031a7-212">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` namespace (`@using Microsoft.AspNetCore.Authorization`) to the examples in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="031a7-213">Yalnızca Blazor yönlendirici üzerinden ulaşılan `@page` bileşenlerinde `[Authorize]` kullanın.</span><span class="sxs-lookup"><span data-stu-id="031a7-213">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="031a7-214">Yetkilendirme yalnızca, bir sayfada işlenen alt bileşenler için *değil* , yönlendirmenin bir yönü olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-214">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="031a7-215">Bir sayfa içindeki belirli parçaların görüntülenmesini yetkilendirmek için, bunun yerine `AuthorizeView` kullanın.</span><span class="sxs-lookup"><span data-stu-id="031a7-215">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="031a7-216">`[Authorize]` özniteliği rol tabanlı veya ilke tabanlı yetkilendirmeyi de destekler.</span><span class="sxs-lookup"><span data-stu-id="031a7-216">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="031a7-217">Rol tabanlı yetkilendirme için `Roles` parametresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="031a7-217">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="031a7-218">İlke tabanlı yetkilendirme için `Policy` parametresini kullanın:</span><span class="sxs-lookup"><span data-stu-id="031a7-218">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="031a7-219">Ne `Roles` ne de `Policy` belirtilmemişse, `[Authorize]` varsayılan ilkeyi kullanır, bu varsayılan olarak kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="031a7-219">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="031a7-220">Kimliği doğrulanmış (oturum açmış) kullanıcılar yetkili olarak.</span><span class="sxs-lookup"><span data-stu-id="031a7-220">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="031a7-221">Kimliği doğrulanmamış (oturumu açılmış) kullanıcılar yetkilendirilmemiş.</span><span class="sxs-lookup"><span data-stu-id="031a7-221">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="031a7-222">Yönlendirici bileşeniyle yetkisiz içeriği özelleştirme</span><span class="sxs-lookup"><span data-stu-id="031a7-222">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="031a7-223">`AuthorizeRouteView` bileşeniyle birlikte `Router` bileşeni, uygulamanın şu durumlarda özel içerik belirlemesine izin verir:</span><span class="sxs-lookup"><span data-stu-id="031a7-223">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="031a7-224">İçerik bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="031a7-224">Content isn't found.</span></span>
* <span data-ttu-id="031a7-225">Kullanıcı, bileşene uygulanan bir `[Authorize]` koşulunu başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="031a7-225">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="031a7-226">`[Authorize]` özniteliği [`[Authorize]` öznitelik](#authorize-attribute) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="031a7-226">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="031a7-227">Zaman uyumsuz kimlik doğrulama devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="031a7-227">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="031a7-228">Varsayılan Blazor sunucusu proje şablonunda, *app. Razor* dosyası nasıl özel içerik ayarlanacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="031a7-228">In the default Blazor Server project template, the *App.razor* file demonstrates how to set custom content:</span></span>

```razor
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

<span data-ttu-id="031a7-229">`<NotFound>`, `<NotAuthorized>`ve `<Authorizing>` etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-229">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="031a7-230">`<NotAuthorized>` öğesi belirtilmemişse, `AuthorizeRouteView` aşağıdaki geri dönüş iletisini kullanır:</span><span class="sxs-lookup"><span data-stu-id="031a7-230">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="031a7-231">Kimlik doğrulama durumu değişiklikleri hakkında bildirim</span><span class="sxs-lookup"><span data-stu-id="031a7-231">Notification about authentication state changes</span></span>

<span data-ttu-id="031a7-232">Uygulama, temeldeki kimlik doğrulama durumu verilerinin değiştiğini belirlerse (örneğin, Kullanıcı oturumu kapattığından veya başka bir kullanıcı rollerini değiştirse), özel bir `AuthenticationStateProvider` isteğe bağlı olarak `AuthenticationStateProvider` temel sınıfında yöntemi `NotifyAuthenticationStateChanged` çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-232">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a custom `AuthenticationStateProvider` can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="031a7-233">Bu, yeni verileri kullanarak yeniden kimlik doğrulama durumu verilerini (örneğin, `AuthorizeView`) tüketicilere bildirir.</span><span class="sxs-lookup"><span data-stu-id="031a7-233">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="031a7-234">Yordamsal mantık</span><span class="sxs-lookup"><span data-stu-id="031a7-234">Procedural logic</span></span>

<span data-ttu-id="031a7-235">Uygulama, yordamsal mantığın bir parçası olarak yetkilendirme kurallarını denetmek için gerekliyse, kullanıcının <xref:System.Security.Claims.ClaimsPrincipal>almak için `Task<AuthenticationState>` türünde basamaklı bir parametre kullanın.</span><span class="sxs-lookup"><span data-stu-id="031a7-235">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="031a7-236">`Task<AuthenticationState>`, ilkeleri değerlendirmek için `IAuthorizationService`gibi diğer hizmetlerle birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-236">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```razor
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
> <span data-ttu-id="031a7-237">Blazor WebAssembly uygulama bileşeninde, `Microsoft.AspNetCore.Authorization` ve `Microsoft.AspNetCore.Components.Authorization` ad alanlarını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="031a7-237">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="031a7-238">Blazor WebAssembly uygulamalarında yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="031a7-238">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="031a7-239">Blazor WebAssembly uygulamalarında, tüm istemci tarafı kodlar kullanıcılar tarafından değiştirilemediği için yetkilendirme denetimleri atlanabilir.</span><span class="sxs-lookup"><span data-stu-id="031a7-239">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="031a7-240">Aynı, JavaScript SPA çerçeveleri veya herhangi bir işletim sistemi için yerel uygulamalar dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="031a7-240">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="031a7-241">**İstemci tarafı uygulamanız tarafından erişilen tüm API uç noktalarında sunucuda her zaman yetkilendirme denetimleri gerçekleştirin.**</span><span class="sxs-lookup"><span data-stu-id="031a7-241">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="031a7-242">Sorun giderme hataları</span><span class="sxs-lookup"><span data-stu-id="031a7-242">Troubleshoot errors</span></span>

<span data-ttu-id="031a7-243">Yaygın hatalar:</span><span class="sxs-lookup"><span data-stu-id="031a7-243">Common errors:</span></span>

* <span data-ttu-id="031a7-244">**Yetkilendirme, görev\<AuthenticationState > türünde bir geçişli parametre gerektirir. Bunu sağlamak için basamaklı Dingauthenticationstate kullanmayı göz önünde bulundurun.**</span><span class="sxs-lookup"><span data-stu-id="031a7-244">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="031a7-245">**`null` değer `authenticationStateTask` alındı**</span><span class="sxs-lookup"><span data-stu-id="031a7-245">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="031a7-246">Projenin kimlik doğrulaması etkin bir Blazor sunucu şablonu kullanılarak oluşturulmamış olması olasıdır.</span><span class="sxs-lookup"><span data-stu-id="031a7-246">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="031a7-247">`<CascadingAuthenticationState>` UI ağacının bir parçası etrafında sarmalayın, örneğin, *app. Razor* içinde aşağıdaki gibi:</span><span class="sxs-lookup"><span data-stu-id="031a7-247">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in *App.razor* as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="031a7-248">`CascadingAuthenticationState`, `Task<AuthenticationState>` basamaklı parametresini sağlar ve bu, temel alınan `AuthenticationStateProvider` DI hizmetinden alır.</span><span class="sxs-lookup"><span data-stu-id="031a7-248">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="031a7-249">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="031a7-249">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="031a7-250">[Başar Blazor: kimlik doğrulama](https://github.com/AdrienTorris/awesome-blazor#authentication) topluluğu örnek bağlantıları</span><span class="sxs-lookup"><span data-stu-id="031a7-250">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
