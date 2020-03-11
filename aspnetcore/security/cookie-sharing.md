---
title: ASP.NET uygulamaları arasında kimlik doğrulama tanımlama bilgilerini paylaşma
author: rick-anderson
description: Kimlik doğrulama tanımlama bilgilerini ASP.NET 4. x ve ASP.NET Core uygulamaları arasında paylaşmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: security/cookie-sharing
ms.openlocfilehash: 7e29be22717f0b97fc115ac036cc54e333bed4e2
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658174"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>ASP.NET uygulamaları arasında kimlik doğrulama tanımlama bilgilerini paylaşma

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

Web siteleri genellikle birlikte çalışan ayrı Web uygulamalarından oluşur. Çoklu oturum açma (SSO) deneyimi sağlamak için, bir site içindeki Web uygulamaları kimlik doğrulama tanımlama bilgilerini paylaşmalıdır. Bu senaryoyu desteklemek için, veri koruma yığını, Katana tanımlama bilgisi kimlik doğrulaması ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerinin paylaşılmasını sağlar.

Aşağıdaki örneklerde:

* Kimlik doğrulama tanımlama bilgisi adı, `.AspNet.SharedCookie`ortak bir değere ayarlanır.
* `AuthenticationType`, açıkça ya da varsayılan olarak `Identity.Application` olarak ayarlanır.
* Veri koruma sisteminin veri koruma anahtarlarını (`SharedCookieApp`) paylaşmasını sağlamak için ortak bir uygulama adı kullanılır.
* `Identity.Application`, kimlik doğrulama düzeni olarak kullanılır. Hangi düzenin kullanıldığı, her ne kadar paylaşılan tanımlama bilgisi *uygulamalarında, varsayılan* düzen olarak, ya da açıkça ayarlanarak kullanılması gerekir. Şema, tanımlama bilgilerini şifrelerken ve şifresini çözerken kullanılır, bu nedenle uygulamalar arasında tutarlı bir düzenin kullanılması gerekir.
* Ortak bir [veri koruma anahtarı](xref:security/data-protection/implementation/key-management) depolama konumu kullanılır.
  * ASP.NET Core uygulamalarda, anahtar depolama konumunu ayarlamak için <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> kullanılır.
  * .NET Framework uygulamalarda, tanımlama bilgisi kimlik doğrulama ara yazılımı <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>uygulamasını kullanır. `DataProtectionProvider`, kimlik doğrulama tanımlama bilgileri yük verilerinin şifrelenmesi ve şifresinin çözülmesi için veri koruma hizmetleri sağlar. `DataProtectionProvider` örneği, uygulamanın diğer bölümleri tarafından kullanılan veri koruma sisteminden yalıtılmıştır. [Dataprotectionprovider. Create (System. IO. DirectoryInfo, Action\<ıdataprotectionbuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) veri koruma anahtarı depolamanın konumunu belirtmek için bir <xref:System.IO.DirectoryInfo> kabul eder.
* `DataProtectionProvider`, [Microsoft. AspNetCore. DataProtection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet paketini gerektirir:
  * ASP.NET Core 2. x uygulamalarında [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)öğesine başvurun.
  * .NET Framework uygulamalar ' da [Microsoft. AspNetCore. DataProtection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)'a bir paket başvurusu ekleyin.
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> ortak uygulama adını ayarlar.

## <a name="share-authentication-cookies-with-aspnet-core-identity"></a>Kimlik doğrulama tanımlama bilgilerini ASP.NET Core kimlikle paylaşma

ASP.NET Core kimliği kullanılırken:

* Veri koruma anahtarlarının ve uygulama adının uygulamalar arasında paylaşılması gerekir. Aşağıdaki örneklerde <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> yöntemine ortak bir anahtar depolama konumu sağlanır. Ortak bir paylaşılan uygulama adı (aşağıdaki örneklerde`SharedCookieApp`) yapılandırmak için <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> kullanın. Daha fazla bilgi için bkz. <xref:security/data-protection/configuration/overview>.
* Tanımlama bilgileri için veri koruma hizmetini ayarlamak için <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> uzantısı yöntemini kullanın.
* Varsayılan kimlik doğrulama türü `Identity.Application`.

`Startup.ConfigureServices` içinde:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

## <a name="share-authentication-cookies-without-aspnet-core-identity"></a>Kimlik doğrulama tanımlama bilgilerini ASP.NET Core kimlik olmadan paylaşma

Tanımlama bilgilerini ASP.NET Core kimlik olmadan doğrudan kullanırken, `Startup.ConfigureServices`veri korumayı ve kimlik doğrulamasını yapılandırın. Aşağıdaki örnekte, kimlik doğrulama türü `Identity.Application`olarak ayarlanır:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

## <a name="share-cookies-across-different-base-paths"></a>Farklı temel yollarda tanımlama bilgilerini paylaşma

Bir kimlik doğrulama tanımlama bilgisi, [HttpRequest. PathBase öğesini](xref:Microsoft.AspNetCore.Http.HttpRequest.PathBase) varsayılan [Cookie. Path](xref:Microsoft.AspNetCore.Http.CookieBuilder.Path)olarak kullanır. Uygulamanın tanımlama bilgisinin farklı temel yollarda paylaşılması gerekiyorsa `Path` geçersiz kılınmalıdır:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem("{PATH TO COMMON KEY RING FOLDER}")
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
    options.Cookie.Path = "/";
});
```

## <a name="share-cookies-across-subdomains"></a>Alt etki alanları genelinde tanımlama bilgilerini paylaşma

Etki alanları arasında tanımlama bilgilerini paylaşan uygulamalar barındırırken, [Cookie. Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) özelliğinde ortak bir etki alanı belirtin. `contoso.com`, `first_subdomain.contoso.com` ve `second_subdomain.contoso.com`gibi uygulamalar arasında tanımlama bilgilerini paylaşmak için, `Cookie.Domain` `.contoso.com`olarak belirtin:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Bekleyen veri koruma anahtarlarını şifreleyin

Üretim dağıtımları için `DataProtectionProvider`, bekleyen anahtarları DPAPI veya X509Certificate ile şifrelemek üzere yapılandırın. Daha fazla bilgi için bkz. <xref:security/data-protection/implementation/key-encryption-at-rest>. Aşağıdaki örnekte <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>için bir sertifika parmak izi verilmiştir:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>ASP.NET 4. x ve ASP.NET Core uygulamaları arasında kimlik doğrulama tanımlama bilgilerini paylaşma

Katana tanımlama bilgisi kimlik doğrulama ara yazılımı kullanan ASP.NET 4. x uygulamaları ASP.NET Core tanımlama bilgisi kimlik doğrulama ara yazılımı ile uyumlu kimlik doğrulama tanımlama bilgileri oluşturacak şekilde yapılandırılabilir. Bu, site genelinde sorunsuz bir SSO deneyimi sağlarken, büyük bir sitenin ayrı uygulamalarının çeşitli adımlarda yükseltilmesini sağlar.

Bir uygulama, Katana tanımlama bilgisi kimlik doğrulama ara yazılımı kullandığında, projenin *Startup.auth.cs* dosyasında `UseCookieAuthentication` çağırır. Visual Studio 2013 ve sonraki sürümlerde oluşturulan ASP.NET 4. x Web uygulaması projeleri varsayılan olarak Katana tanımlama bilgisi kimlik doğrulama ara yazılımını kullanır. `UseCookieAuthentication` artık kullanılmıyor ve ASP.NET Core uygulamalar için desteklenmese de, Katana tanımlama bilgisi kimlik doğrulama ara yazılımı kullanan bir ASP.NET 4. x uygulamasında `UseCookieAuthentication` çağırma geçerlidir.

ASP.NET 4. x uygulaması .NET Framework 4.5.1 veya üstünü hedeflemelidir. Aksi takdirde, gerekli NuGet paketleri yüklenemeyebilir.

Bir ASP.NET 4. x uygulaması ve bir ASP.NET Core uygulaması arasında kimlik doğrulama tanımlama bilgilerini paylaşmak için, ASP.NET Core uygulamayı [ASP.NET Core uygulamalar arasında kimlik doğrulama tanımlama bilgilerini paylaşma](#share-authentication-cookies-with-aspnet-core-identity) bölümünde belirtilen şekilde yapılandırın ve ardından ASP.NET 4. x uygulamasını aşağıdaki şekilde yapılandırın.

Uygulamanın paketlerinin en son sürümlere güncelleştirildiğinden emin olun. [Microsoft. Owin. Security. Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) paketini her bir ASP.NET 4. x uygulamasına yükler.

`UseCookieAuthentication`çağrısını bulun ve değiştirin:

* Tanımlama bilgisi adını ASP.NET Core tanımlama bilgisi kimlik doğrulama ara yazılımı (örnekteki`.AspNet.SharedCookie`) tarafından kullanılan adla eşleşecek şekilde değiştirin.
* Aşağıdaki örnekte, kimlik doğrulama türü `Identity.Application`olarak ayarlanır.
* Ortak veri koruma anahtarı depolama konumuna başlatılan bir `DataProtectionProvider` örneğini sağlayın.
* Uygulama adının, kimlik doğrulama tanımlama bilgilerini paylaşan tüm uygulamalar tarafından kullanılan ortak uygulama adına ayarlandığını onaylayın (örnekteki`SharedCookieApp`).

`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` ve `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`ayarı yoksa, <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> benzersiz kullanıcıları ayıran talebe ayarlayın.

*App_Start/Startup.auth.cs*:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = "Identity.Application",
    CookieName = ".AspNet.SharedCookie",
    LoginPath = new PathString("/Account/Login"),
    Provider = new CookieAuthenticationProvider
    {
        OnValidateIdentity =
            SecurityStampValidator
                .OnValidateIdentity<ApplicationUserManager, ApplicationUser>(
                    validateInterval: TimeSpan.FromMinutes(30),
                    regenerateIdentity: (manager, user) =>
                        user.GenerateUserIdentityAsync(manager))
    },
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create("{PATH TO COMMON KEY RING FOLDER}",
                (builder) => { builder.SetApplicationName("SharedCookieApp"); })
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies." +
                    "CookieAuthenticationMiddleware",
                "Identity.Application",
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});

System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier =
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name";
```

Bir kullanıcı kimliği oluştururken, kimlik doğrulama türü (`Identity.Application`), *App_Start/Startup.auth.cs*içinde `UseCookieAuthentication` ile `AuthenticationType` kümesi içinde tanımlanan türle eşleşmelidir.

*Modeller/ıdentitymodeller. cs*:

```csharp
public class ApplicationUser : IdentityUser
{
    public async Task<ClaimsIdentity> GenerateUserIdentityAsync(
        UserManager<ApplicationUser> manager)
    {
        // The authenticationType must match the one defined in 
        // CookieAuthenticationOptions.AuthenticationType
        var userIdentity = 
            await manager.CreateIdentityAsync(this, "Identity.Application");

        // Add custom user claims here

        return userIdentity;
    }
}
```

## <a name="use-a-common-user-database"></a>Ortak bir kullanıcı veritabanı kullan

Uygulamalar aynı kimlik şemasını (kimlik sürümü) kullandıklarında, her bir uygulama için kimlik sisteminin aynı kullanıcı veritabanına işaret olduğunu doğrulayın. Aksi halde kimlik sistemi, kimlik doğrulama tanımlama bilgisindeki bilgileri veritabanındaki bilgilere göre eşleştirmeye çalıştığında çalışma zamanında hatalara neden olur.

Kimlik şeması, uygulamalar arasında farklı olduğunda, genellikle uygulamalar farklı kimlik sürümleri kullandığından, en son kimlik sürümüne göre ortak bir veritabanının paylaşılması, yeniden eşleştirmeden ve diğer uygulamanın kimlik şemalarına sütun eklemeden mümkün değildir. Yaygın bir veritabanının uygulamalar tarafından paylaşılabilmesi için diğer uygulamaları en son kimlik sürümünü kullanacak şekilde yükseltmek genellikle daha etkilidir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/web-farm>
