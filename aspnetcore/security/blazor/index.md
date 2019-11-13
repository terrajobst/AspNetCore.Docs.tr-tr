---
title: ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme
author: guardrex
description: Blazor kimlik doğrulaması ve yetkilendirme senaryoları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: 2ebc4d72191dff33a7fb6170650be67c3836cdaa
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73964001"
---
# <a name="aspnet-core-opno-locblazor-authentication-and-authorization"></a>ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme

[Steve Sanderson](https://github.com/SteveSandersonMS) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

ASP.NET Core, Blazor uygulamalardaki güvenlik yapılandırmasını ve yönetimini destekler.

Güvenlik senaryoları Blazor Server ve WebAssembly Apps Blazor farklıdır. Blazor sunucu uygulamaları sunucuda çalıştığından, yetkilendirme denetimleri şunları tespit edebilir:

* Kullanıcıya sunulan kullanıcı ARABIRIMI seçenekleri (örneğin, bir kullanıcı için hangi menü girişlerinin kullanılabildiği).
* Uygulama ve bileşenlerin bölgeleri için erişim kuralları.

Blazor WebAssembly uygulamaları istemcide çalışır. Yetkilendirme *yalnızca* hangi kullanıcı arabirimi seçeneklerinin gösterileceğini belirlemede kullanılır. İstemci tarafı denetimleri bir kullanıcı tarafından değiştirililerek veya atlandığından, bir Blazor WebAssembly uygulaması yetkilendirme erişim kurallarını zorunlu kılamaz.

## <a name="authentication"></a>Kimlik doğrulaması

Blazor, kullanıcının kimliğini kurmak için mevcut ASP.NET Core kimlik doğrulama mekanizmalarını kullanır. Tam mekanizma Blazor uygulamasının nasıl barındırıldığını, Blazor sunucunun veya Blazor WebAssembly ' ne bağlıdır.

### <a name="opno-locblazor-server-authentication"></a>Blazor sunucusu kimlik doğrulaması

Blazor Server Apps, SignalRkullanılarak oluşturulan gerçek zamanlı bir bağlantı üzerinden çalışır. [SignalRtabanlı uygulamalarda kimlik doğrulaması](xref:signalr/authn-and-authz) , bağlantı oluşturulduğunda işlenir. Kimlik doğrulaması, bir tanımlama bilgisine veya başka bir taşıyıcı belirtecine dayalı olabilir.

Blazor sunucusu proje şablonu, proje oluşturulduğunda kimlik doğrulamasını sizin için ayarlayabilir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kimlik doğrulama mekanizmasına sahip yeni bir Blazor sunucu projesi oluşturmak için <xref:blazor/get-started> makalesindeki Visual Studio kılavuzunu izleyin.

**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda **Blazor sunucusu uygulama** şablonunu seçtikten sonra, **kimlik doğrulaması**altında **Değiştir** ' i seçin.

Diğer ASP.NET Core projelerine yönelik aynı kimlik doğrulama mekanizması kümesini sunmak için bir iletişim kutusu açılır:

* **Kimlik doğrulaması yok**
* Kullanıcı hesaplarının &ndash; **bireysel kullanıcı hesapları** depolanabilir:
  * ASP.NET Core [kimlik](xref:security/authentication/identity) sistemini kullanarak uygulama içinde.
  * [Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* **İş veya okul hesapları**
* **Windows kimlik doğrulaması**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Kimlik doğrulama mekanizması ile yeni bir Blazor sunucu projesi oluşturmak için <xref:blazor/get-started> makalesindeki Visual Studio Code kılavuzunu izleyin:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

İzin verilen kimlik doğrulama değerleri (`{AUTHENTICATION}`) aşağıdaki tabloda gösterilmiştir.

| Kimlik doğrulama mekanizması                                                                 | `{AUTHENTICATION}` değeri |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Kimlik doğrulaması yok                                                                        | `None`                   |
| Ye<br>Uygulamada ASP.NET Core kimlikle depolanan kullanıcılar.                        | `Individual`             |
| Ye<br>[Azure AD B2C](xref:security/authentication/azure-ad-b2c)' de depolanan kullanıcılar. | `IndividualB2C`          |
| İş veya okul hesapları<br>Tek bir kiracı için kuruluş kimlik doğrulaması.            | `SingleOrg`              |
| İş veya okul hesapları<br>Birden çok kiracı için kuruluş kimlik doğrulaması.           | `MultiOrg`               |
| Windows Kimlik Doğrulaması                                                                   | `Windows`                |

Komut, `{APP NAME}` yer tutucusu için belirtilen değere sahip adlı bir klasör oluşturur ve uygulamanın adı olarak klasör adını kullanır. Daha fazla bilgi için .NET Core kılavuzundaki [DotNet New](/dotnet/core/tools/dotnet-new) komutuna bakın.

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

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor WebAssembly kimlik doğrulaması

Blazor WebAssembly uygulamalarında, tüm istemci tarafı kodlar kullanıcılar tarafından değiştirilemediği için kimlik doğrulama denetimleri atlanabilir. Aynı, JavaScript SPA çerçeveleri veya herhangi bir işletim sistemi için yerel uygulamalar dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için de geçerlidir.

Uygulamanın proje dosyasına [Microsoft. AspNetCore. components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) için bir paket başvurusu ekleyin.

Blazor WebAssembly uygulamaları için özel bir `AuthenticationStateProvider` hizmeti uygulaması aşağıdaki bölümlerde ele alınmıştır.

## <a name="authenticationstateprovider-service"></a>AuthenticationStateProvider hizmeti

Blazor Server uygulamaları ASP.NET Core `HttpContext.User`kimlik doğrulama durumu verilerini alan yerleşik bir `AuthenticationStateProvider` hizmeti içerir. Kimlik doğrulama durumu, mevcut ASP.NET Core sunucu tarafı kimlik doğrulama mekanizmalarıyla tümleştirilir.

`AuthenticationStateProvider`, kimlik doğrulama durumunu almak için `AuthorizeView` bileşeni ve `CascadingAuthenticationState` bileşeni tarafından kullanılan temel hizmettir.

Genellikle `AuthenticationStateProvider` ' i doğrudan kullanmayın. Bu makalenin ilerleyen kısımlarında açıklanan [Authorizeview bileşenini](#authorizeview-component) veya [görev<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) yaklaşımlarını kullanın. `AuthenticationStateProvider` doğrudan kullanmanın ana dezavantajı, temeldeki kimlik doğrulama durumu verileri değişirse bileşen tarafından otomatik olarak bildirilmemektedir.

`AuthenticationStateProvider` hizmeti, aşağıdaki örnekte gösterildiği gibi geçerli kullanıcının <xref:System.Security.Claims.ClaimsPrincipal> verilerini sağlayabilir:

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

`user.Identity.IsAuthenticated` `true` ve Kullanıcı bir <xref:System.Security.Claims.ClaimsPrincipal>olduğundan, talepler, değerlendirilen rollerde numaralandırılabilir ve üyelik yapılabilir.

Bağımlılık ekleme (dı) ve hizmetleri hakkında daha fazla bilgi için bkz. <xref:blazor/dependency-injection> ve <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Özel bir AuthenticationStateProvider uygulama

Blazor WebAssembly uygulaması oluşturuyorsanız veya uygulamanızın belirtimi kesinlikle özel bir sağlayıcı gerektiriyorsa, bir sağlayıcı uygulayın ve `GetAuthenticationStateAsync`geçersiz kılın:

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

`CustomAuthStateProvider` hizmeti `Startup.ConfigureServices`kaydedilir:

```csharp
// using Microsoft.AspNetCore.Components.Authorization;
// using BlazorSample.Services;

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

`CustomAuthStateProvider`kullanarak, tüm kullanıcıların Kullanıcı adı `mrfibuli`kimlik doğrulaması yapılır.

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Kimlik doğrulama durumunu basamaklı bir parametre olarak kullanıma sunma

Kullanıcı tarafından tetiklenen bir eylem gerçekleştirirken olduğu gibi, yordamsal mantık için kimlik doğrulama durumu verileri gerekliyse, `Task<AuthenticationState>` türünde basamaklı bir parametre tanımlayarak kimlik doğrulama durumu verilerini alın:

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
> Blazor WebAssembly uygulama bileşeninde, `Microsoft.AspNetCore.Components.Authorization` ad alanını (`@using Microsoft.AspNetCore.Components.Authorization`) ekleyin.

`user.Identity.IsAuthenticated` `true`, talepler numaralandırılabilir ve değerlendirilen rollerde üyeliğe eklenebilir.

`AuthorizeRouteView` ve `CascadingAuthenticationState` bileşenleri kullanarak `Task<AuthenticationState>` basamaklı parametreyi ayarlayın:

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

## <a name="authorization"></a>Yetkilendirme

Bir kullanıcının kimliği doğrulandıktan sonra, kullanıcının neler yapabileceğini denetlemek için *Yetkilendirme* kuralları uygulanır.

Erişim, genellikle aşağıdakileri yapıp verilmeksizin verilir veya reddedilir:

* Bir kullanıcının kimliği doğrulanır (oturum açıldı).
* Bir Kullanıcı bir *roldür*.
* Bir kullanıcının *talebi*vardır.
* Bir *ilke* karşılandı.

Bu kavramların her biri, ASP.NET Core MVC veya Razor Pages uygulamasındaki ile aynıdır. ASP.NET Core güvenliği hakkında daha fazla bilgi için, [ASP.NET Core güvenlik ve kimlik](xref:security/index)' in altındaki makalelere bakın.

## <a name="authorizeview-component"></a>AuthorizeView bileşeni

`AuthorizeView` bileşeni, kullanıcının onu görme yetkisine sahip olup olmadığına bağlı olarak Kullanıcı ARABIRIMINI seçmeli olarak görüntüler. Bu yaklaşım yalnızca Kullanıcı için veri *görüntülemesi* gerektiğinde ve Kullanıcı kimliğini yordamsal mantığda kullanmanıza gerek olmadığında yararlıdır.

Bileşeni, oturum açmış kullanıcıyla ilgili bilgilere erişmek için kullanabileceğiniz `AuthenticationState` türünde `context` değişkeni kullanıma sunar:

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

Kullanıcının kimliği doğrulanmadıysa, görüntülenmek üzere farklı içerikler de sağlayabilirsiniz:

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

`<Authorized>` ve `<NotAuthorized>` etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.

Kullanıcı arabirimi seçeneklerini veya erişimini denetleyen roller veya ilkeler gibi yetkilendirme koşulları, [Yetkilendirme](#authorization) bölümünde ele alınmıştır.

Yetkilendirme koşulları belirtilmemişse, `AuthorizeView` varsayılan bir ilke kullanır ve şu şekilde davranır:

* Kimliği doğrulanmış (oturum açmış) kullanıcılar yetkili olarak.
* Kimliği doğrulanmamış (oturumu açılmış) kullanıcılar yetkilendirilmemiş.

### <a name="role-based-and-policy-based-authorization"></a>Rol tabanlı ve ilke tabanlı yetkilendirme

`AuthorizeView` bileşeni *rol tabanlı* veya *ilke tabanlı* yetkilendirmeyi destekler.

Rol tabanlı yetkilendirme için `Roles` parametresini kullanın:

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Daha fazla bilgi için bkz. <xref:security/authorization/roles>.

İlke tabanlı yetkilendirme için `Policy` parametresini kullanın:

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

Talep tabanlı yetkilendirme, ilke tabanlı yetkilendirme için özel bir durumdur. Örneğin, kullanıcıların belirli bir talebe sahip olmasını gerektiren bir ilke tanımlayabilirsiniz. Daha fazla bilgi için bkz. <xref:security/authorization/policies>.

Bu API 'Ler Blazor sunucuda Blazor ya da WebAssembly uygulamalarında kullanılabilir.

Ne `Roles` ne de `Policy` belirtilmemişse, `AuthorizeView` varsayılan ilkeyi kullanır.

### <a name="content-displayed-during-asynchronous-authentication"></a>Zaman uyumsuz kimlik doğrulaması sırasında görünen içerik

Blazor, kimlik doğrulaması durumunun *zaman uyumsuz*olarak belirlenmesine izin verir. Bu yaklaşım için birincil senaryo, kimlik doğrulaması için bir dış uç noktaya istek yapan Blazor WebAssembly Apps ' dedir.

Kimlik doğrulaması devam ederken, `AuthorizeView` varsayılan olarak içerik görüntülemez. Kimlik doğrulama gerçekleştiğinde içeriği göstermek için `<Authorizing>` öğesini kullanın:

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

Bu yaklaşım normalde Blazor Server uygulamaları için geçerli değildir. Blazor sunucu uygulamaları, durum oluşturulur almaz kimlik doğrulama durumunu bilir. `Authorizing` içerik Blazor sunucu uygulamasının `AuthorizeView` bileşeninde bulunabilir, ancak içerik hiçbir şekilde gösterilmez.

## <a name="authorize-attribute"></a>[Yetkilendir] özniteliği

`[Authorize]` özniteliği Razor bileşenlerinde kullanılabilir:

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> Blazor WebAssembly uygulama bileşeninde, bu bölümdeki örneklere `Microsoft.AspNetCore.Authorization` ad alanını (`@using Microsoft.AspNetCore.Authorization`) ekleyin.

> [!IMPORTANT]
> Yalnızca Blazor yönlendirici üzerinden ulaşılan `@page` bileşenlerinde `[Authorize]` kullanın. Yetkilendirme yalnızca, bir sayfada işlenen alt bileşenler için *değil* , yönlendirmenin bir yönü olarak gerçekleştirilir. Bir sayfa içindeki belirli bölümlerin görüntülenmesini yetkilendirmek için bunun yerine `AuthorizeView` kullanın.

`[Authorize]` özniteliği rol tabanlı veya ilke tabanlı yetkilendirmeyi de destekler. Rol tabanlı yetkilendirme için `Roles` parametresini kullanın:

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

İlke tabanlı yetkilendirme için `Policy` parametresini kullanın:

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Ne `Roles` ne de `Policy` belirtilmemişse, `[Authorize]` varsayılan ilkeyi kullanır, bu varsayılan olarak kabul edilir:

* Kimliği doğrulanmış (oturum açmış) kullanıcılar yetkili olarak.
* Kimliği doğrulanmamış (oturumu açılmış) kullanıcılar yetkilendirilmemiş.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Yönlendirici bileşeniyle yetkisiz içeriği özelleştirme

`AuthorizeRouteView` bileşeniyle birlikte `Router` bileşeni, uygulamanın şu durumlarda özel içerik belirlemesine izin verir:

* İçerik bulunamadı.
* Kullanıcı, bileşene uygulanan bir `[Authorize]` koşulunu başarısız olur. `[Authorize]` özniteliği [[Yetkilendir] öznitelik](#authorize-attribute) bölümünde ele alınmıştır.
* Zaman uyumsuz kimlik doğrulama devam ediyor.

Varsayılan Blazor sunucusu proje şablonunda, *app. Razor* dosyası nasıl özel içerik ayarlanacağını gösterir:

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

`<NotFound>`, `<NotAuthorized>`ve `<Authorizing>` etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.

`<NotAuthorized>` öğesi belirtilmemişse, `AuthorizeRouteView` aşağıdaki geri dönüş iletisini kullanır:

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Kimlik doğrulama durumu değişiklikleri hakkında bildirim

Uygulama, temeldeki kimlik doğrulama durumu verilerinin değiştiğini belirlerse (örneğin, Kullanıcı oturumu kapattığından veya başka bir kullanıcı rollerini değiştirse), özel bir `AuthenticationStateProvider` isteğe bağlı olarak `AuthenticationStateProvider` temel sınıfında `NotifyAuthenticationStateChanged` metodunu çağırabilir. Bu, yeni verileri kullanarak yeniden kimlik doğrulama durumu verilerinin (örneğin, `AuthorizeView`) bir şekilde yeniden kullanılması hakkında bilgilendirir.

## <a name="procedural-logic"></a>Yordamsal mantık

Uygulama, yordamsal mantığın bir parçası olarak yetkilendirme kurallarını denetmek için gerekliyse, kullanıcının <xref:System.Security.Claims.ClaimsPrincipal> ' i elde etmek için `Task<AuthenticationState>` türünde basamaklı bir parametre kullanın. `Task<AuthenticationState>`, ilkeleri değerlendirmek için `IAuthorizationService` gibi diğer hizmetlerle birleştirilebilir.

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
> Blazor WebAssembly uygulama bileşeninde, `Microsoft.AspNetCore.Authorization` ve `Microsoft.AspNetCore.Components.Authorization` ad alanlarını ekleyin:
>
> ```cshtml
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a>Blazor WebAssembly uygulamalarında yetkilendirme

Blazor WebAssembly uygulamalarında, tüm istemci tarafı kodlar kullanıcılar tarafından değiştirilemediği için yetkilendirme denetimleri atlanabilir. Aynı, JavaScript SPA çerçeveleri veya herhangi bir işletim sistemi için yerel uygulamalar dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için de geçerlidir.

**İstemci tarafı uygulamanız tarafından erişilen tüm API uç noktalarında sunucuda her zaman yetkilendirme denetimleri gerçekleştirin.**

## <a name="troubleshoot-errors"></a>Sorun giderme hataları

Yaygın hatalar:

* **Yetkilendirme, görev\<AuthenticationState > türünde bir geçişli parametre gerektirir. Bunu sağlamak için basamaklı Dingauthenticationstate kullanmayı göz önünde bulundurun.**

* **`null` değeri `authenticationStateTask` için alındı**

Projenin kimlik doğrulaması etkin bir Blazor sunucu şablonu kullanılarak oluşturulmamış olması olasıdır. `<CascadingAuthenticationState>` UI ağacının bir parçası etrafında sarmalayın, örneğin, *app. Razor* içinde aşağıdaki gibi:

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

`CascadingAuthenticationState`, `Task<AuthenticationState>` basamaklı parametresini sağlar ve bu, temel alınan `AuthenticationStateProvider` DI hizmetinden alır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
