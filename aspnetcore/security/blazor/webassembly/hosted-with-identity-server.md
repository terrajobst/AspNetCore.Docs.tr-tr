---
title: Kimlik sunucusuyla bir ASP.NET Core Blazor Weelsembly barındırılan uygulamasını güvenli hale getirme
author: guardrex
description: Bir [IdentityServer](https://identityserver.io/) arka ucu kullanan Visual Studio içinden kimlik doğrulaması ile yeni Blazor barındırılan bir uygulama oluşturmak için
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: a3993bf635e5a7aae408d72796015f2414e13c14
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434479"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a>Kimlik sunucusuyla bir ASP.NET Core Blazor Weelsembly barındırılan uygulamasını güvenli hale getirme

, [Javier Calvarro Nelson](https://github.com/javiercn) ve [Luke Latham](https://github.com/guardrex) 'e göre

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Visual Studio 'da, kullanıcıların ve API çağrılarının kimliğini doğrulamak için [IdentityServer](https://identityserver.io/) kullanan yeni bir Blazor barındırılan uygulama oluşturmak için:

1. Yeni bir **Blazor WebAssembly** uygulaması oluşturmak Için Visual Studio 'yu kullanın. Daha fazla bilgi için bkz. <xref:blazor/get-started>.
1. **Yeni Blazor uygulaması oluştur** Iletişim kutusunda **kimlik doğrulama** bölümünde **Değiştir** ' i seçin.
1. **Her kullanıcı hesabını** ve ardından **Tamam ' ı**seçin.
1. **Gelişmiş** bölümünde **ASP.NET Core barındırılan** onay kutusunu seçin.
1. **Oluştur** düğmesini seçin.

Uygulamayı bir komut kabuğunda oluşturmak için aşağıdaki komutu yürütün:

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

Mevcut değilse bir proje klasörü oluşturan çıkış konumunu belirtmek için, çıkış seçeneğini komuta bir yol (örneğin, `-o BlazorSample`) ile birlikte ekleyin. Klasör adı Ayrıca projenin adının bir parçası haline gelir.

## <a name="server-app-configuration"></a>Sunucu uygulaması yapılandırması

Aşağıdaki bölümlerde, kimlik doğrulama desteği dahil edildiğinde projenin eklemeleri açıklanır.

### <a name="startup-class"></a>Başlangıç sınıfı

`Startup` sınıfı aşağıdaki eklemelere sahiptir:

* `Startup.ConfigureServices` içinde:

  * Varsayılan Kullanıcı arabirimine sahip kimlik:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer 'ın en üstünde bazı varsayılan ASP.NET Core kuralları ayarlayan ek bir <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> yardımcı yöntemi olan IdentityServer:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * Kimliği, IdentityServer tarafından üretilen JWT belirteçlerini doğrulamak üzere uygulamayı yapılandıran ek bir <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> Yardımcısı yöntemiyle kimlik doğrulaması:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* `Startup.Configure` içinde:

  * İstek kimlik bilgilerini doğrulamadan ve Kullanıcı istek bağlamında ayarlamaktan sorumlu kimlik doğrulama ara yazılımı:

    ```csharp
    app.UseAuthentication();
    ```

  * Açık KIMLIK Connect (OıDC) uç noktalarını kullanıma sunan IdentityServer ara yazılımı:

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>Addadpiauthorization

<xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> yardımcı yöntemi, [IdentityServer](https://identityserver.io/) 'ı ASP.NET Core senaryolar için yapılandırır. IdentityServer, uygulama güvenliği sorunlarını işlemeye yönelik güçlü ve genişletilebilir bir çerçevedir. IdentityServer, en yaygın senaryolar için gereksiz karmaşıklık sunar. Sonuç olarak, iyi bir başlangıç noktası düşüntiğimiz bir dizi kural ve yapılandırma seçeneği sağlanır. Kimlik doğrulama gereksinimleriniz değiştikçe, IdentityServer 'ın tam gücü, kimlik doğrulamasını uygulamanın gereksinimlerine uyacak şekilde özelleştirmek için hala kullanılabilir.

### <a name="addidentityserverjwt"></a>Addentityserverjwt

<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> yardımcı yöntemi, varsayılan kimlik doğrulama işleyicisi olarak uygulama için bir ilke düzeni yapılandırır. İlke, kimliğin kimlik URL 'SI alanında `/Identity`herhangi bir alt yolda yönlendirilen tüm istekleri işlemesini sağlamak üzere yapılandırılır. <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> diğer tüm istekleri işler. Ayrıca, bu yöntem:

* Bir `{APPLICATION NAME}API` API kaynağını, IdentityServer ile `{APPLICATION NAME}API`varsayılan kapsamına kaydeder.
* Uygulama için IdentityServer tarafından verilen belirteçleri doğrulamak üzere JWT taşıyıcı belirteç ara yazılımını yapılandırır.

### <a name="weatherforecastcontroller"></a>Dalgalı bir denetleyici

`WeatherForecastController` (*denetleyiciler/dalgalı değer denetleyicisi. cs*) içinde, [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) özniteliği sınıfa uygulanır. Özniteliği, kullanıcıya kaynağa erişim için varsayılan ilkeye göre yetkilendirilmiş olması gerektiğini belirtir. Varsayılan yetkilendirme ilkesi, daha önce bahsedilen ilke şemasına <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> tarafından ayarlanan varsayılan kimlik doğrulama düzenini kullanacak şekilde yapılandırılmıştır. Yardımcı yöntemi, uygulama istekleri için varsayılan işleyici olarak <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> yapılandırır.

### <a name="applicationdbcontext"></a>ApplicationDbContext

`ApplicationDbContext` (*Data/ApplicationDbContext. cs*) öğesinde aynı <xref:Microsoft.EntityFrameworkCore.DbContext> kimlik Içinde, IdentityServer şemasını dahil etmek için <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> genişlettiği özel durum ile birlikte kullanılır. <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>türetilir.

Veritabanı şeması üzerinde tam denetim elde etmek için, kullanılabilir kimlik <xref:Microsoft.EntityFrameworkCore.DbContext> sınıflarından birinden devralma ve bağlamı `OnModelCreating` yönteminde `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` çağırarak kimlik şemasını içerecek şekilde yapılandırma.

### <a name="oidcconfigurationcontroller"></a>Oıdcconfigurationcontroller

`OidcConfigurationController` (*Controllers/Oıdcconfigurationcontroller. cs*), istemci uç noktası OIDC parametrelerine hizmeti sunacak şekilde sağlanır.

### <a name="app-settings-files"></a>Uygulama ayarları dosyaları

Proje kökündeki App settings dosyasında (*appSettings. JSON*), `IdentityServer` bölümünde yapılandırılan istemcilerin listesi açıklanmaktadır. Aşağıdaki örnekte, tek bir istemci vardır. İstemci adı, uygulama adına karşılık gelir ve bir kural tarafından OAuth `ClientId` parametresine eşlenir. Profil, yapılandırılan uygulama türünü gösterir. Profil, sunucu için yapılandırma işlemini basitleştiren kuralları yönlendirmek için dahili olarak kullanılır. <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

Geliştirme ortamı uygulama ayarları dosyasında (*appSettings. Development. JSON*) proje kökünde, `IdentityServer` bölümünde belirteçleri imzalamak için kullanılan anahtar açıklanmaktadır. <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a>İstemci uygulama yapılandırması

### <a name="authentication-package"></a>Kimlik doğrulama paketi

Bireysel kullanıcı hesapları (`Individual`) kullanmak üzere bir uygulama oluşturulduğunda, uygulama otomatik olarak uygulamanın proje dosyasındaki `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketi için bir paket başvurusu alır. Paket, uygulamanın kullanıcıların kimliğini doğrulamasına ve korunan API 'Leri çağırmak için belirteçleri almasına yardımcı olan bir dizi temel sunar.

Bir uygulamaya kimlik doğrulaması ekliyorsanız, paketi uygulamanın proje dosyasına el ile ekleyin:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Önceki paket başvurusunda `{VERSION}`, <xref:blazor/get-started> makalesinde gösterilen `Microsoft.AspNetCore.Blazor.Templates` paketinin sürümü ile değiştirin.

### <a name="api-authorization-support"></a>API yetkilendirme desteği

Kullanıcıları kimlik doğrulama desteği, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` paketi içinde sunulan genişletme yöntemi tarafından hizmet kapsayıcısına takılır. Bu yöntem, uygulamanın var olan yetkilendirme sistemiyle etkileşime geçmesini sağlamak için gereken tüm hizmetleri ayarlar.

```csharp
builder.Services.AddApiAuthorization();
```

Varsayılan olarak, uygulama yapılandırmasını `_configuration/{client-id}`' dan kurala göre yükler. Kural gereği, istemci KIMLIĞI uygulamanın derleme adına ayarlanır. Bu URL, aþýrý yükleme seçeneklerini çağırarak ayrı bir uç noktaya işaret etmek üzere değiştirilebilir.

### <a name="index-page"></a>Dizin sayfası

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a>Uygulama bileşeni

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>RedirectToLogin bileşeni

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>LoginDisplay bileşeni

`LoginDisplay` bileşeni (*Shared/LoginDisplay.* razor) `MainLayout` bileşeninde (*Shared/mainlayout. Razor*) işlenir ve aşağıdaki davranışları yönetir:

* Kimliği doğrulanmış kullanıcılar için:
  * Geçerli Kullanıcı adını görüntüler.
  * ASP.NET Core kimliği içindeki kullanıcı profili sayfasına bir bağlantı sunar.
  * Uygulamanın oturumunu kapatmak için bir düğme sunar.
* Anonim kullanıcılar için:
  * Kayıt için seçeneği sunar.
  * Oturum açma seçeneği sunar.

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a>Kimlik doğrulama bileşeni

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>FetchData bileşeni

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı sunucu projesinden çalıştırın. Visual Studio 'Yu kullanırken **Çözüm Gezgini** ' de sunucu projesini seçin ve araç çubuğundaki **Çalıştır** düğmesini seçin veya uygulamayı **Hata Ayıkla** menüsünden başlatın.

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
