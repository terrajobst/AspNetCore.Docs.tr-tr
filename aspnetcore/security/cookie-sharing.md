---
title: ASP.NET uygulamaları arasında kimlik doğrulaması tanımlama bilgilerini paylaşma
author: rick-anderson
description: ASP.NET arasında kimlik doğrulaması tanımlama bilgilerini paylaşma hakkında bilgi edinin 4.x ve ASP.NET Core uygulamaları.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2019
uid: security/cookie-sharing
ms.openlocfilehash: b2f906ac97fe79b2a66a5ab709bcbcb03ab8cc39
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223916"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>ASP.NET uygulamaları arasında kimlik doğrulaması tanımlama bilgilerini paylaşma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)

Web siteleri, genellikle birlikte çalışan tek tek web uygulamalarını oluşur. Bir çoklu oturum açma (SSO) deneyimi sağlamak için bir site içinde web apps kimlik doğrulaması tanımlama bilgileri paylaşmanız gerekir. Bu senaryoyu desteklemek için veri koruma yığın Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı sağlar.

Aşağıdaki örneklerde içinde:

* Kimlik doğrulama tanımlama bilgisi adı, ortak bir değere ayarlanmış `.AspNet.SharedCookie`.
* `AuthenticationType` Ayarlanır `Identity.Application` açıkça veya varsayılan.
* Ortak bir uygulama adı, veri koruma anahtarları paylaşmak veri koruma sisteminde etkinleştirmek için kullanılır (`SharedCookieApp`).
* `Identity.Application` kimlik doğrulama düzeni kullanılır. Hangi şeması kullanılır, tutarlı bir şekilde kullanılmalıdır *içinde ve arasında* varsayılan düzenini ya da açıkça ayarlayarak paylaşılan tanımlama bilgisi uygulamalar. Düzeni, şifreleme ve tanımlama bilgilerini uygulamalar arasında tutarlı bir düzen kullanılması gerekir şifresini çözme sırasında kullanılır.
* Bir ortak [veri koruma anahtarı](xref:security/data-protection/implementation/key-management) depolama konumu kullanılır.
  * ASP.NET Core uygulamalarında <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> anahtar depolama konumunu ayarlamak için kullanılır.
  * .NET Framework uygulamalarında tanımlama bilgisi kimlik doğrulaması ara yazılımı uygulaması kullanan <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>. `DataProtectionProvider` şifreleme ve kimlik doğrulama tanımlama bilgisi yükü verilerinin şifresinin çözülmesi için veri koruma hizmetleri sağlar. `DataProtectionProvider` Örneği, uygulamanın diğer parçaları tarafından kullanılan veri koruma sisteminde yalıtılır. [DataProtectionProvider.Create (System.IO.DirectoryInfo, eylem\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) kabul eden bir <xref:System.IO.DirectoryInfo> veri koruma anahtar depolama konumunu belirtmek için.
* `DataProtectionProvider` gerektirir [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet paketi:
  * ASP.NET Core 2.x uygulamaları başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).
  * .NET Framework uygulamalarında paket başvurusu ekleme [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Ortak uygulama adını ayarlar.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>ASP.NET Core uygulamaları arasında kimlik doğrulaması tanımlama bilgilerini paylaşma

ASP.NET Core kimliği kullanırken:

* Veri koruma anahtarları ve uygulama adı, uygulamalar arasında paylaşılması gerekir. Bir ortak anahtar depolama konumu için sağlanan <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> ilişkin aşağıdaki örneklerde yöntemi. Kullanım <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> ortak bir paylaşılan uygulama adı yapılandırmak için (`SharedCookieApp` ilişkin aşağıdaki örneklerde). Daha fazla bilgi için bkz. <xref:security/data-protection/configuration/overview>.
* Kullanım <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> tanımlama bilgilerinin veri koruma hizmetini ayarlama için genişletme yöntemi.
* Aşağıdaki örnekte, kimlik doğrulaması türünü ayarlamak `Identity.Application` varsayılan olarak.

İçinde `Startup.ConfigureServices`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

ASP.NET Core kimliği olmadan doğrudan tanımlama bilgileri kullanırken, veri koruma ve kimlik doğrulaması yapılandırma `Startup.ConfigureServices`. Aşağıdaki örnekte, kimlik doğrulaması türünü ayarlamak `Identity.Application`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

Alt etki alanları arasında tanımlama bilgilerini paylaşma uygulamayı barındırırken, ortak bir etki alanında belirtin [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) özelliği. Tanımlama bilgileri uygulama arasında paylaşmak için `contoso.com`, gibi `first_subdomain.contoso.com` ve `second_subdomain.contoso.com`, belirtin `Cookie.Domain` olarak `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Veri koruma anahtarları bekleme sırasında şifreleme

Üretim dağıtımları için yapılandırma `DataProtectionProvider` DPAPI veya bir X509Certificate bekleyen anahtarlarını şifrelemek için. Daha fazla bilgi için bkz. <xref:security/data-protection/implementation/key-encryption-at-rest>. Aşağıdaki örnekte, bir sertifika parmak izi için sağlanan <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>ASP.NET arasında kimlik doğrulaması tanımlama bilgilerini paylaşma 4.x ve ASP.NET Core uygulamaları

Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan ASP.NET 4.x uygulamalar, ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir. Bu, birkaç adımda büyük bir sitenin tek tek uygulamalar site genelinde kesintisiz SSO bir deneyim sağlarken yükseltme olanağı sağlar.

Bir uygulamayı Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanırken çağırdığı `UseCookieAuthentication` projenin *Startup.Auth.cs* dosya. ASP.NET 4.x web uygulaması projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın. Ancak `UseCookieAuthentication` artık kullanılmıyor ve ASP.NET Core uygulamaları, arama için desteklenmeyen `UseCookieAuthentication` bir ASP.NET Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan 4.x uygulamayı geçerlidir.

ASP.NET 4.x uygulama .NET Framework 4.5.1'i hedefleyen gerekir veya üzeri. Aksi takdirde yüklemek gerekli NuGet paketlerini başarısız.

ASP.NET Core uygulaması ile bir ASP.NET 4.x uygulama arasındaki kimlik doğrulaması tanımlama bilgileri paylaşmak için ASP.NET Core uygulaması içinde belirtildiği gibi yapılandırma [ASP.NET Core uygulamaları arasında kimlik doğrulaması tanımlama bilgilerini paylaşma](#share-authentication-cookies-among-aspnet-core-apps) bölümüne ve ardından ASP.NET 4.x uygulamayı olarak yapılandırma izler.

Uygulama paketleri için en son sürümlerine güncelleştirildiğinden emin olun. Yükleme [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) her ASP.NET 4.x uygulama paket.

Bulun ve çağrısını değiştirin `UseCookieAuthentication`:

* ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından kullanılan ad eşleştirilecek tanımlama bilgisi adını değiştirin (`.AspNet.SharedCookie` örnekte).
* Aşağıdaki örnekte, kimlik doğrulaması türünü ayarlamak `Identity.Application`.
* Örneği sağlayan bir `DataProtectionProvider` genel veri koruma anahtarı depolama alanı konumuna başlatıldı.
* Uygulama adı, kimlik doğrulama tanımlama bilgilerini paylaşma tüm uygulamalar tarafından kullanılan ortak uygulama adına ayarlandığından emin olun (`SharedCookieApp` örnekte).

Değil ayarlıyorsanız `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` ve `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`ayarlayın <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> benzersiz kullanıcıları ayırt eden bir talep.

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
            DataProtectionProvider.Create({PATH TO COMMON KEY RING FOLDER},
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

Bir kullanıcı kimliği, kimlik doğrulaması türünü oluştururken (`Identity.Application`) tanımlanan türüyle eşleşmelidir `AuthenticationType` kümesi `UseCookieAuthentication` içinde *App_Start/Startup.Auth.cs*.

*Models/IdentityModels.cs*:

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

## <a name="use-a-common-user-database"></a>Ortak bir kullanıcı veritabanını kullanın

Aynı kimliğe (kimliğinin aynı sürüm), şema onaylayın kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret edilen uygulamaları kullandığınızda. Aksi takdirde, kendi veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgileri eşleşecek şekilde çalıştığında kimlik sistemi hataları zamanında üretir.

Kimlik şema uygulamalar arasında farklı olduğunda, uygulamaları farklı kimlik sürümlerini kullandığından genellikle ortak bir veritabanını en son kimlik sürümüne paylaşımı yeniden eşleme ve diğer uygulamanın kimlik şemaları sütunlar ekleyerek olmadan mümkün değildir. Genellikle daha fazla olduğu ortak bir veritabanını uygulamalar tarafından paylaşılabilir böylece en son kimlik sürümünü kullanmak için diğer uygulamaları yükseltmek için etkin.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/web-farm>
