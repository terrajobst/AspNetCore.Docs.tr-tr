---
title: ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme
author: guardrex
description: Blazor kimlik doğrulaması ve yetkilendirme senaryoları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/blazor/index
ms.openlocfilehash: c9b57e2a987ae4a49f0965386ad080c98803d8b0
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080644"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme

[Steve Sanderson](https://github.com/SteveSandersonMS) tarafından

ASP.NET Core, Blazor uygulamalarında güvenliğin yapılandırmasını ve yönetimini destekler.

Güvenlik senaryoları Blazor Server ve Blazor WebAssembly Apps arasında farklılık gösterir. Blazor sunucu uygulamaları sunucuda çalıştığı için, yetkilendirme denetimleri şunları tespit edebilir:

* Kullanıcıya sunulan kullanıcı ARABIRIMI seçenekleri (örneğin, bir kullanıcı için hangi menü girişlerinin kullanılabildiği).
* Uygulama ve bileşenlerin bölgeleri için erişim kuralları.

Blazor WebAssembly Apps, istemcide çalışır. Yetkilendirme *yalnızca* hangi kullanıcı arabirimi seçeneklerinin gösterileceğini belirlemede kullanılır. İstemci tarafı denetimleri bir kullanıcı tarafından değiştirililerek veya atlandığından, bir Blazor WebAssembly uygulaması yetkilendirme erişim kurallarını zorunlu kılamaz.

## <a name="authentication"></a>Kimlik doğrulaması

Blazor, kullanıcının kimliğini kurmak için mevcut ASP.NET Core kimlik doğrulama mekanizmalarını kullanır. Tam mekanizma Blazor uygulamasının nasıl barındırıldığını, Blazor Server veya Blazor WebAssembly öğesine bağlıdır.

### <a name="blazor-server-authentication"></a>Blazor sunucusu kimlik doğrulaması

Blazor Server uygulamaları, SignalR kullanılarak oluşturulan gerçek zamanlı bir bağlantı üzerinden çalışır. [SignalR tabanlı uygulamalarda kimlik doğrulaması](xref:signalr/authn-and-authz) , bağlantı kurulduunda işlenir. Kimlik doğrulaması, bir tanımlama bilgisine veya başka bir taşıyıcı belirtecine dayalı olabilir.

Blazor sunucusu proje şablonu, proje oluşturulduğunda kimlik doğrulamasını sizin için ayarlayabilir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Kimlik doğrulama mekanizması ile yeni bir Blazor <xref:blazor/get-started> Server projesi oluşturmak için makalesindeki Visual Studio kılavuzunu izleyin.

**Yeni ASP.NET Core Web uygulaması oluştur** Iletişim kutusunda **Blazor Server uygulama** şablonunu seçtikten sonra, **kimlik doğrulaması**altında **Değiştir** ' i seçin.

Diğer ASP.NET Core projelerine yönelik aynı kimlik doğrulama mekanizması kümesini sunmak için bir iletişim kutusu açılır:

* **Kimlik doğrulaması yok**
* **Bireysel kullanıcı hesapları** &ndash; Kullanıcı hesapları depolanabilir:
  * ASP.NET Core [kimlik](xref:security/authentication/identity) sistemini kullanarak uygulama içinde.
  * [Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* **İş veya okul hesapları**
* **Windows kimlik doğrulaması**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Kimlik doğrulama mekanizması ile yeni bir <xref:blazor/get-started> Blazor Server projesi oluşturmak için makalesindeki Visual Studio Code kılavuzunu izleyin:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

İzin verilen kimlik doğrulama`{AUTHENTICATION}`değerleri () aşağıdaki tabloda gösterilmiştir.

| Kimlik doğrulama mekanizması                                                                 | `{AUTHENTICATION}`deeri |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Kimlik doğrulaması yok                                                                        | `None`                   |
| Ye<br>Uygulamada ASP.NET Core kimlikle depolanan kullanıcılar.                        | `Individual`             |
| Ye<br>[Azure AD B2C](xref:security/authentication/azure-ad-b2c)' de depolanan kullanıcılar. | `IndividualB2C`          |
| İş veya okul hesapları<br>Tek bir kiracı için kuruluş kimlik doğrulaması.            | `SingleOrg`              |
| İş veya okul hesapları<br>Birden çok kiracı için kuruluş kimlik doğrulaması.           | `MultiOrg`               |
| Windows Kimlik Doğrulaması                                                                   | `Windows`                |

Komutu, `{APP NAME}` yer tutucu için belirtilen değere sahip adlı bir klasör oluşturur ve klasör adını uygulamanın adı olarak kullanır. Daha fazla bilgi için .NET Core kılavuzundaki [DotNet New](/dotnet/core/tools/dotnet-new) komutuna bakın.

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

### <a name="blazor-webassembly-authentication"></a>Blazor WebAssembly kimlik doğrulaması

Blazor WebAssembly uygulamalarında, tüm istemci tarafı kodlar kullanıcılar tarafından değiştirilemediği için kimlik doğrulama denetimleri atlanabilir. Aynı, JavaScript SPA çerçeveleri veya herhangi bir işletim sistemi için yerel uygulamalar dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için de geçerlidir.

Blazor webassembly `AuthenticationStateProvider` uygulamaları için özel bir hizmetin uygulanması aşağıdaki bölümlerde ele alınmıştır.

## <a name="authenticationstateprovider-service"></a>AuthenticationStateProvider hizmeti

Blazor Server uygulamaları `AuthenticationStateProvider` `HttpContext.User`ASP.NET Core ' den kimlik doğrulama durumu verilerini alan yerleşik bir hizmet içerir. Kimlik doğrulama durumu, mevcut ASP.NET Core sunucu tarafı kimlik doğrulama mekanizmalarıyla tümleştirilir.

`AuthenticationStateProvider`, `AuthorizeView` bileşen ve `CascadingAuthenticationState` bileşen tarafından kimlik doğrulama durumunu almak için kullanılan temel hizmettir.

Genellikle doğrudan kullanmazsınız `AuthenticationStateProvider` . Bu makalenin ilerleyen kısımlarında açıklanan [authorizeview bileşenini](#authorizeview-component) veya [görev<AuthenticationState> ](#expose-the-authentication-state-as-a-cascading-parameter) yaklaşımlarını kullanın. Doğrudan kullanmanın `AuthenticationStateProvider` ana dezavantajı, temeldeki kimlik doğrulama durumu verileri değişirse bileşen tarafından otomatik olarak bildirilmemektedir.

Hizmet, aşağıdaki örnekte gösterildiği gibi geçerli <xref:System.Security.Claims.ClaimsPrincipal> kullanıcının verilerini sağlayabilir: `AuthenticationStateProvider`

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

İse ve Kullanıcı bir<xref:System.Security.Claims.ClaimsPrincipal>ise, talepler numaralandırılabilir ve değerlendirilen rollerde üyelik yapılabilir. `true` `user.Identity.IsAuthenticated`

Bağımlılık ekleme (dı) ve hizmetleri hakkında daha fazla bilgi için bkz <xref:blazor/dependency-injection> . <xref:fundamentals/dependency-injection>ve.

## <a name="implement-a-custom-authenticationstateprovider"></a>Özel bir AuthenticationStateProvider uygulama

Blazor WebAssembly uygulaması oluşturuyorsanız veya uygulamanızın belirtimi kesinlikle özel bir sağlayıcı gerektiriyorsa, sağlayıcı uygulayın ve geçersiz kılın `GetAuthenticationStateAsync`:

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

Hizmet şu şekilde `Startup.ConfigureServices`kaydedilir: `CustomAuthStateProvider`

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

Kullanarak, tüm kullanıcıların Kullanıcı adı `mrfibuli`ile kimliği doğrulanır. `CustomAuthStateProvider`

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Kimlik doğrulama durumunu basamaklı bir parametre olarak kullanıma sunma

Kullanıcı tarafından tetiklenen bir eylem gerçekleştirilirken gibi yordamsal mantık için kimlik doğrulama durumu verileri gerekliyse, türü `Task<AuthenticationState>`geçişli bir parametre tanımlayarak kimlik doğrulama durumu verilerini alın:

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

`user.Identity.IsAuthenticated` İse`true`, talepler, değerlendirilen roller içinde numaralandırılabilir ve üyelenebilir.

Ve bileşenlerini kullanarak geçişli parametreyi ayarlayın: `Task<AuthenticationState>` `AuthorizeRouteView` `CascadingAuthenticationState`

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

`AuthorizeView` Bileşen Kullanıcı tarafından görme yetkisine sahip olup olmadığına bağlı olarak Kullanıcı arabirimini seçmeli olarak görüntüler. Bu yaklaşım yalnızca Kullanıcı için veri *görüntülemesi* gerektiğinde ve Kullanıcı kimliğini yordamsal mantığda kullanmanıza gerek olmadığında yararlıdır.

Bileşeni, oturum açmış `context` kullanıcıyla ilgili bilgilere `AuthenticationState`erişmek için kullanabileceğiniz türünde bir değişken kullanıma sunar:

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

`<Authorized>` Ve`<NotAuthorized>` etiketlerinin içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.

Kullanıcı arabirimi seçeneklerini veya erişimini denetleyen roller veya ilkeler gibi yetkilendirme koşulları, [Yetkilendirme](#authorization) bölümünde ele alınmıştır.

Yetkilendirme koşulları belirtilmemişse, `AuthorizeView` varsayılan bir ilke kullanır ve şu şekilde davranır:

* Kimliği doğrulanmış (oturum açmış) kullanıcılar yetkili olarak.
* Kimliği doğrulanmamış (oturumu açılmış) kullanıcılar yetkilendirilmemiş.

### <a name="role-based-and-policy-based-authorization"></a>Rol tabanlı ve ilke tabanlı yetkilendirme

Bileşen rol tabanlı veya *ilke tabanlı* yetkilendirmeyi destekler. `AuthorizeView`

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

Bu API 'Ler, Blazor Server ya da Blazor WebAssembly uygulamalarında kullanılabilir.

`Roles` Ne de `Policy` belirtilmemişse ,`AuthorizeView` varsayılan ilkeyi kullanır.

### <a name="content-displayed-during-asynchronous-authentication"></a>Zaman uyumsuz kimlik doğrulaması sırasında görünen içerik

Blazor, kimlik doğrulaması durumunun *zaman uyumsuz*olarak belirlenmesine izin verir. Bu yaklaşım için birincil senaryo, kimlik doğrulaması için bir dış uç noktaya istek yapan Blazor WebAssembly uygulamalarında bulunur.

Kimlik doğrulama devam ederken, `AuthorizeView` varsayılan olarak içerik görüntülemez. Kimlik doğrulaması sırasında içeriği göstermek için `<Authorizing>` öğesini kullanın:

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

Bu yaklaşım normalde Blazor Server uygulamaları için geçerli değildir. Blazor sunucu uygulamaları, durum belirlenir oluşturmaz kimlik doğrulama durumunu bilir. `Authorizing`içerik, Blazor sunucu uygulamasının `AuthorizeView` bileşeninde bulunabilir, ancak içerik hiçbir şekilde gösterilmez.

## <a name="authorize-attribute"></a>[Yetkilendir] özniteliği

Bir uygulamanın MVC denetleyicisi veya Razor `[Authorize]` sayfasıyla birlikte kullanılması gibi, `[Authorize]` Razor bileşenleriyle de kullanılabilir:

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> Yalnızca Blazor `[Authorize]` yönlendiricisi `@page` aracılığıyla ulaşılan bileşenlerde kullanın. Yetkilendirme yalnızca, bir sayfada işlenen alt bileşenler için *değil* , yönlendirmenin bir yönü olarak gerçekleştirilir. Bir sayfa içindeki belirli parçaların görüntülenmesini yetkilendirmek için, bunun yerine kullanın `AuthorizeView` .

Bileşenin derlenmesi için bileşene ya `@using Microsoft.AspNetCore.Authorization` da *_ımports. Razor* dosyasına eklemeniz gerekebilir.

`[Authorize]` Özniteliği rol tabanlı veya ilke tabanlı yetkilendirmeyi de destekler. Rol tabanlı yetkilendirme için `Roles` parametresini kullanın:

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

Ya da `Policy` belirtilmemişse`[Authorize]` varsayılan ilkeyi kullanır, varsayılan olarak şu şekilde değerlendirilir: `Roles`

* Kimliği doğrulanmış (oturum açmış) kullanıcılar yetkili olarak.
* Kimliği doğrulanmamış (oturumu açılmış) kullanıcılar yetkilendirilmemiş.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Yönlendirici bileşeniyle yetkisiz içeriği özelleştirme

Bileşen `Router` ilebirliktebileşeni,uygulamanınşudurumlarda`AuthorizeRouteView` özel içerik belirtmesini sağlar:

* İçerik bulunamadı.
* Kullanıcı, bileşene uygulanan `[Authorize]` bir koşulla başarısız olur. Özniteliği [ [Yetkilendir] öznitelik](#authorize-attribute) bölümünde ele alınmıştır. `[Authorize]`
* Zaman uyumsuz kimlik doğrulama devam ediyor.

Varsayılan Blazor Server proje şablonunda, *app. Razor* dosyası nasıl özel içerik ayarlanacağını gösterir:

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

`<NotFound>` ,`<NotAuthorized>`Ve etiketlerinin`<Authorizing>` içeriği, diğer etkileşimli bileşenler gibi rastgele öğeler içerebilir.

Öğesi belirtilmemişse, aşağıdaki geri dönüş iletisini kullanır:`AuthorizeRouteView` `<NotAuthorized>`

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Kimlik doğrulama durumu değişiklikleri hakkında bildirim

Uygulama, temeldeki kimlik doğrulama durumu verilerinin değiştiğini belirlerse (örneğin, Kullanıcı oturumu kapattığından veya başka bir kullanıcı rollerini değiştirse), özel `AuthenticationStateProvider` olarak isteğe bağlı olarak yöntemi `NotifyAuthenticationStateChanged` `AuthenticationStateProvider` temel üzerinde çağırabilir sınıfı. Bu, yeni verileri kullanarak yeniden kimlik doğrulama durumu verilerini (örneğin `AuthorizeView`,) tüketicilere bildirir.

## <a name="procedural-logic"></a>Yordamsal mantık

Uygulama, yordamsal mantığın bir parçası olarak yetkilendirme kurallarını denetmek için gerekliyse, Kullanıcı tarafından `Task<AuthenticationState>` <xref:System.Security.Claims.ClaimsPrincipal>elde etmek için türünde basamaklı bir parametre kullanın. `Task<AuthenticationState>``IAuthorizationService`, ilkeleri değerlendirmek için gibi diğer hizmetlerle birleştirilebilir.

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

## <a name="authorization-in-blazor-webassembly-apps"></a>Blazor WebAssembly uygulamalarında yetkilendirme

Blazor WebAssembly uygulamalarında, tüm istemci tarafı kodlar kullanıcılar tarafından değiştirilemediği için yetkilendirme denetimleri atlanabilir. Aynı, JavaScript SPA çerçeveleri veya herhangi bir işletim sistemi için yerel uygulamalar dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için de geçerlidir.

**İstemci tarafı uygulamanız tarafından erişilen tüm API uç noktalarında sunucuda her zaman yetkilendirme denetimleri gerçekleştirin.**

## <a name="troubleshoot-errors"></a>Sorun giderme hataları

Yaygın hatalar:

* **Yetkilendirme, görev\<authenticationstate > türünde bir geçişli parametre gerektirir. Bunu sağlamak için basamaklı Dingauthenticationstate kullanmayı göz önünde bulundurun.**

* **`null`için değer alındı`authenticationStateTask`**

Projenin kimlik doğrulaması etkin bir Blazor sunucu şablonu kullanılarak oluşturulmamış olması olasıdır. UI ağacının `<CascadingAuthenticationState>` bir bölümü etrafında sarmalayın, örneğin, *app. Razor* içinde aşağıdaki gibi:

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

, `CascadingAuthenticationState` Temel alınan `Task<AuthenticationState>` dıhizmetindenaldığıgeçişliparametreyisağlar.`AuthenticationStateProvider`

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
