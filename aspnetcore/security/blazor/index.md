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
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>ASP.NET Core Blazor kimlik doğrulaması ve yetkilendirme

Tarafından [Steve Sanderson](https://github.com/SteveSandersonMS)

ASP.NET Core güvenlik yönetimi ve yapılandırmayı Blazor uygulamaları destekler.

Güvenlik senaryoları Blazor sunucu tarafı ve istemci tarafı uygulamalar arasında farklılık gösterir. Blazor sunucu tarafı uygulamalar, sunucu üzerinde çalıştığından, yetkilendirme denetimleri belirlemek kullanabilirsiniz:

* Bir kullanıcı (örneğin, bir kullanıcı tarafından hangi menü girişlerini kullanılabilir) için sunulan kullanıcı Arabirimi seçenekleri.
* Erişim kuralları uygulama ve bileşenlerinin alanları.

İstemcide Blazor istemci tarafı uygulamalar çalıştırın. Yetkilendirmesi *yalnızca* hangi göstermek için kullanıcı Arabirimi seçenekleri belirlemek için kullanılır. İstemci tarafı denetimleri değişiklik veya kullanıcı tarafından atlanır Blazor istemci-tarafı uygulaması yetkilendirme erişim kuralları zorunlu kılamaz.

## <a name="authentication"></a>Kimlik doğrulaması

Blazor, kullanıcının kimliğini oluşturmak için mevcut bir ASP.NET Core kimlik doğrulaması mekanizmalarını kullanır. Tam mekanizması Blazor uygulamayı nasıl barındırılıyorsa, sunucu tarafı veya istemci tarafı üzerinde bağlıdır.

### <a name="blazor-server-side-authentication"></a>Blazor sunucu tarafı kimlik doğrulaması

Sunucu tarafı uygulamalar Blazor oluşturulan SignalR kullanarak gerçek zamanlı bir bağlantı üzerinden çalışır. [SignalR tabanlı uygulamalarda kimlik doğrulaması](xref:signalr/authn-and-authz) bağlantı kurulduğunda ele alınır. Kimlik doğrulama tanımlama bilgisi veya diğer bir taşıyıcı belirteç temel alabilir.

Proje oluşturulduğunda kimlik doğrulaması Blazor sunucu tarafı proje şablonu ayarlayabilirsiniz.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio yönergeleri <xref:blazor/get-started> makalenin bir kimlik doğrulama mekanizması ile yeni bir Blazor sunucu tarafı projesi oluşturun.

Seçtikten sonra **Blazor (sunucu tarafı)** şablonunda **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda **değişiklik** altında **kimlik doğrulaması** .

Diğer ASP.NET Core kimlik doğrulaması mekanizmalarını aynı dizi projeleri sunmak için bir iletişim kutusu açılır:

* **Kimlik doğrulama yok**
* **Bireysel kullanıcı hesapları** &ndash; kullanıcı hesaplarını depolanabilir:
  * ASP.NET Core'nın içinde uygulamanın kullanarak [kimlik](xref:security/authentication/identity) sistem.
  * İle [Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* **İş veya Okul hesapları**
* **Windows kimlik doğrulaması**

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Visual Studio Code yönergeleri <xref:blazor/get-started> makale ile kimlik doğrulama mekanizması yeni Blazor sunucu tarafı projesi oluşturmak için:

```console
dotnet new blazorserverside -o {APP NAME} -au {AUTHENTICATION}
```

İzin verilen kimlik doğrulama değerlerini (`{AUTHENTICATION}`) aşağıdaki tabloda gösterilmiştir.

| Kimlik doğrulama mekanizması                                                                 | `{AUTHENTICATION}` Değer |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Kimlik doğrulama yok                                                                        | `None`                   |
| Kişi<br>ASP.NET Core kimliği uygulamayla saklanan kullanıcı.                        | `Individual`             |
| Kişi<br>Depolanan kullanıcılar [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| İş veya Okul hesapları<br>Tek bir kiracı için kuruluş kimlik doğrulaması.            | `SingleOrg`              |
| İş veya Okul hesapları<br>Birden fazla Kiracı için kuruluş kimlik doğrulaması.           | `MultiOrg`               |
| Windows Kimlik Doğrulaması                                                                   | `Windows`                |

Komut için sağlanan değer ile adlı bir klasör oluşturur `{APP NAME}` yer tutucu ve klasör adı uygulamanın adı kullanır. Daha fazla bilgi için [yeni dotnet](/dotnet/core/tools/dotnet-new) .NET Core Kılavuzu'nda komutu.

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

### <a name="blazor-client-side-authentication"></a>Blazor istemci-tarafı kimlik doğrulaması

Tüm istemci tarafı kod, kullanıcılar tarafından değiştirilebilir çünkü Blazor istemci tarafı uygulamalar, kimlik doğrulama denetimleri atlanabilir. Aynı, JavaScript SPA'ya çerçeveleri ya da tüm işletim sistemlerine yönelik yerel uygulamaları dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için geçerlidir.

Özel uygulanışı `AuthenticationStateProvider` hizmet Blazor istemci tarafı uygulamalar için aşağıdaki bölümlerde ele alınmaktadır.

## <a name="authenticationstateprovider-service"></a>AuthenticationStateProvider hizmeti

Blazor sunucu tarafı uygulamalar dahil yerleşik `AuthenticationStateProvider` ASP.NET çekirdek ait kimlik doğrulama durumu verilerini alan hizmet `HttpContext.User`. Kimlik doğrulama durumu mevcut ASP.NET Core sunucu tarafı kimlik doğrulama mekanizmaları ile nasıl tümleştirildiğini budur.

`AuthenticationStateProvider` tarafından kullanılan temel alınan hizmet `AuthorizeView` bileşeni ve `CascadingAuthenticationState` kimlik doğrulama durumu almak için bileşen.

Genellikle kullanmayın `AuthenticationStateProvider` doğrudan. Kullanım [AuthorizeView bileşen](#authorizeview-component) veya [görev<AuthenticationState> ](#expose-the-authentication-state-as-a-cascading-parameter) bu makalenin sonraki bölümlerinde açıklanan yaklaşımlardan. Kullanmanın asıl sakıncası `AuthenticationStateProvider` doğrudan veri değişikliklerini temel kimlik doğrulama durumunu, bileşen otomatik olarak bildirilmez olduğu.

`AuthenticationStateProvider` Hizmet, geçerli kullanıcının sağlayabilir <xref:System.Security.Claims.ClaimsPrincipal> aşağıdaki örnekte gösterildiği gibi veri:

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

Varsa `user.Identity.IsAuthenticated` olduğu `true` ve kullanıcı bir <xref:System.Security.Claims.ClaimsPrincipal>, talep numaralandırılmış ve rol üyelikleri değerlendirilir.

Bağımlılık ekleme (dı) ve Hizmetleri hakkında daha fazla bilgi için bkz. <xref:blazor/dependency-injection> ve <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Özel AuthenticationStateProvider uygulayın

Bir Blazor istemci-tarafı uygulaması oluştururken veya uygulamanızın belirtimi kesinlikle özel bir sağlayıcı gerektiriyorsa, sağlayıcıyı uygulama ve geçersiz kılma `GetAuthenticationStateAsync`:

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

`CustomAuthStateProvider` Hizmet kayıtlı `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
}
```

Kullanarak `CustomAuthStateProvider`, tüm kullanıcıların kullanıcı adıyla kimlik doğrulamalarının `mrfibuli`.

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Kimlik doğrulama durumu basamaklı bir parametre olarak kullanıma sunma

Kimlik doğrulama durumu verilerinin ne zaman kullanıcı tarafından tetiklenen eylemi gerçekleştirme gibi yordam mantığı, gerekiyorsa kimlik doğrulama durumu verileri türü basamaklı bir parametre tanımlayarak almak `Task<AuthenticationState>`:

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

Varsa `user.Identity.IsAuthenticated` olduğu `true`, talep numaralandırılmış ve rol üyelikleri değerlendirilir.

Ayarlanan `Task<AuthenticationState>` basamaklı parametresini kullanarak `CascadingAuthenticationState` bileşeni:

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

## <a name="authorization"></a>Yetkilendirme

Bir kullanıcının kimliği doğrulandıktan sonra *yetkilendirme* kullanıcı neler yapabileceğinizi denetlemek için kurallar uygulanır.

Erişim genellikle verilen veya reddedilen bağlı:

* Bir kullanıcı (açık) doğrulanır.
* Bir kullanıcının bulunduğu bir *rol*.
* Bir kullanıcının bir *talep*.
* A *ilke* karşılanmadı.

Bu kavramlar her bir ASP.NET Core MVC veya Razor sayfaları uygulama olduğu gibi aynı şeydir. Makalelerin altında ASP.NET Core güvenlik hakkında daha fazla bilgi için bkz. [ASP.NET Core güvenlik ve kimlik](xref:security/index).

## <a name="authorizeview-component"></a>AuthorizeView bileşeni

`AuthorizeView` Bileşen seçmeli olarak olup kullanıcı görmek için yetkili bağlı olarak kullanıcı Arabirimi görüntüler. Bu yaklaşım için yalnızca ihtiyacınız olduğunda yararlıdır *görüntüleme* kullanıcı verileri ve kullanıcının kimliğini yordam mantığı kullanmanız gerekmez.

Bileşen sunan bir `context` türündeki değişken `AuthenticationState`, oturum açmış kullanıcı hakkında bilgilere erişmek için kullanabileceğiniz:

```cshtml
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

Kullanıcı kimliği değil, ayrıca görüntülemek için farklı içerik belirtebilirsiniz:

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

İçeriği `<Authorized>` ve `<NotAuthorized>` etkileşimli diğer bileşenler gibi rastgele öğeler içerebilir.

Roller veya kullanıcı Arabirimi seçenekleri veya erişim denetimi ilkeleri gibi yetkilendirme koşulları içinde ele alınmıştır [yetkilendirme](#authorization) bölümü.

Belirtilen yetkilendirme koşulları olmayan, `AuthorizeView` varsayılan ilkeyi kullanır ve algılar:

* Yetkili olarak kimlik doğrulaması (oturum açma) kullanıcılar.
* Kimliği doğrulanmamış (imzalı genişletme) kullanıcıların yetkisiz olarak.

### <a name="role-based-and-policy-based-authorization"></a>Rol tabanlı ve ilke tabanlı yetkilendirme

`AuthorizeView` Bileşeni destekler *rol tabanlı* veya *ilke tabanlı* yetkilendirme.

Rol tabanlı yetkilendirme için kullanan `Roles` parametresi:

```cshtml
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Daha fazla bilgi için bkz. <xref:security/authorization/roles>.

İlke tabanlı yetkilendirme için kullanan `Policy` parametresi:

```cshtml
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

Beyana dayalı yetkilendirme, ilke tabanlı yetkilendirme özel bir durumdur. Örneğin, kullanıcıların belirli bir talep sahip olmasını gerektiren bir ilke tanımlayabilirsiniz. Daha fazla bilgi için bkz. <xref:security/authorization/policies>.

Bu API'ler, Blazor sunucu tarafı ya da Blazor istemci-tarafı uygulamaları içinde kullanılabilir.

Kullanılmazsa `Roles` ya da `Policy` belirtilen `AuthorizeView` varsayılan ilkesi kullanır.

### <a name="content-displayed-during-asynchronous-authentication"></a>Zaman uyumsuz kimlik doğrulaması sırasında görüntülenen içerik

Blazor izin verir, kimlik doğrulama durumu belirlenecek *zaman uyumsuz olarak*. Birincil bu yaklaşım için kimlik doğrulaması için dış uç noktası için bir istekte bulunmak Blazor istemci-tarafı uygulamalarında senaryodur.

Kimlik doğrulaması, devam ederken `AuthorizeView` içerik varsayılan olarak görüntüler. Kimlik doğrulaması gerçekleşirken içeriği görüntülemek için kullanın `<Authorizing>` öğesi:

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

Bu yaklaşım, normalde Blazor sunucu tarafı uygulamalar için geçerli değildir. Durum oluşturulduktan hemen sonra Blazor sunucu tarafı uygulamalar, kimlik doğrulama durumu bildirin. `Authorizing` içeriği bir Blazor sunucu-tarafı uygulaması'nın sağlanabilir `AuthorizeView` bileşeni, ancak içerik hiçbir zaman görüntülenir.

## <a name="authorize-attribute"></a>[Özniteliği yetkilendirmek]

Yalnızca bir uygulama kullanabilirsiniz gibi `[Authorize]` bir MVC denetleyicisi ya da bir Razor sayfası `[Authorize]` Razor bileşenleri ile de kullanılabilir:

```cshtml
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> Yalnızca `[Authorize]` üzerinde `@page` bileşenleri Blazor yönlendirici erişildi. Yetkilendirme yalnızca Yönlendirme bir özelliği gerçekleştirilir ve *değil* içinde bir sayfa alt bileşenler için. Bir sayfa içinde belirli bölümlerinin görüntülenmesini yetkilendirmek için `AuthorizeView` yerine.

Eklemeniz gerekebilir `@using Microsoft.AspNetCore.Authorization` bileşeni için veya çok *_Imports.razor* derlemek Bileşen dosyası.

`[Authorize]` Özniteliği, rol tabanlı veya ilke tabanlı yetkilendirme da destekler. Rol tabanlı yetkilendirme için kullanan `Roles` parametresi:

```cshtml
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

İlke tabanlı yetkilendirme için kullanan `Policy` parametresi:

```cshtml
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Kullanılmazsa `Roles` ya da `Policy` belirtilen `[Authorize]` varsayılan olarak değerlendirilecek olan varsayılan ilkeyi kullanır:

* Yetkili olarak kimlik doğrulaması (oturum açma) kullanıcılar.
* Kimliği doğrulanmamış (imzalı genişletme) kullanıcıların yetkisiz olarak.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Yetkisiz içerik yönlendirici bileşeni ile özelleştirme

`Router` Bileşeni, özel içerik belirtmek için uygulamayı sağlar:

* İçerik bulunamadığında.
* Kullanıcı bir `[Authorize]` bileşenine uygulanmış koşul. `[Authorize]` Özniteliği alınmıştır [[Authorize] özniteliği](#authorize-attribute) bölümü.
* Zaman uyumsuz bir kimlik doğrulama işlemi devam ediyor.

Varsayılan Blazor sunucu tarafı proje şablonunda *App.razor* dosyası özel içerik ayarlama işlemini gösterir:

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

İçeriği `<NotFoundContent>`, `<NotAuthorizedContent>`, ve `<AuthorizingContent>` etkileşimli diğer bileşenler gibi rastgele öğeler içerebilir.

Varsa `<NotAuthorizedContent>` belirtilmediyse, yönlendirici, aşağıdaki geri dönüş iletisi kullanır:

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Kimlik doğrulaması durum değişiklikleri hakkında bildirim

Uygulama (örneğin, oturumunuz kullanıcı veya başka bir kullanıcı rollerinin değiştiğinden) temel kimlik doğrulama durumu verilerini değiştiğini, belirlerse özel `AuthenticationStateProvider` isteğe bağlı olarak yöntem çağırabilirsiniz `NotifyAuthenticationStateChanged` üzerinde `AuthenticationStateProvider` temel sınıf. Bu kimlik doğrulama durumu veri tüketicilerinin bildirir (örneğin, `AuthorizeView`) yeni verileri kullanarak rerender için.

## <a name="procedural-logic"></a>Yordam mantığı

Yetkilendirme kuralları yordam mantıksal bir parçası olarak denetlemek için uygulamanın gerekiyorsa art arda parametre türü kullanmak `Task<AuthenticationState>` kullanıcının edinme <xref:System.Security.Claims.ClaimsPrincipal>. `Task<AuthenticationState>` diğer hizmetlerle gibi birleştirilebilir `IAuthorizationService`ilkelerini değerlendirme için.

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

## <a name="authorization-in-blazor-client-side-apps"></a>Yetkilendirme Blazor istemci tarafı uygulamalar

Tüm istemci tarafı kod, kullanıcılar tarafından değiştirilebilir çünkü Blazor istemci tarafı uygulamalar, yetkilendirme denetimleri atlanabilir. Aynı, JavaScript SPA'ya çerçeveleri ya da tüm işletim sistemlerine yönelik yerel uygulamaları dahil olmak üzere tüm istemci tarafı uygulama teknolojileri için geçerlidir.

**Her zaman sunucu, istemci tarafı uygulamanız tarafından erişilen herhangi bir API uç noktalarını içinde yetkilendirme denetimleri gerçekleştirin.**

## <a name="troubleshoot-errors"></a>Hatalarını giderme

Sık karşılaşılan hatalar:

* **Yetkilendirme gerektirir basamaklı bir parametre türü görev\<AuthenticationState >. Bunu sağlamak için CascadingAuthenticationState kullanmayı düşünün.**

* **`null` için değeri alındı `authenticationStateTask`**

Kimlik doğrulaması etkin Blazor sunucu tarafı şablon kullanarak proje oluşturulmadıysa olasıdır. Kaydırma bir `<CascadingAuthenticationState>` bazı kısımları örneğin UI ağacı etrafında *App.razor* gibi:

```cshtml
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

`CascadingAuthenticationState` Sağlayan `Task<AuthenticationState>` sırayla temel aldığı basamaklı parametresi `AuthenticationStateProvider` DI hizmeti.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/index>
* <xref:security/authentication/windowsauth>
