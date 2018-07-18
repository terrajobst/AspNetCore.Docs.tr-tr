---
title: ASP.NET ve ASP.NET Core ile uygulamalar arasında tanımlama bilgilerini paylaşma
author: rick-anderson
description: ASP.NET arasında kimlik doğrulaması tanımlama bilgilerini paylaşma hakkında bilgi edinin 4.x ve ASP.NET Core uygulamaları.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: f8347b52f68165cdbe4ab77a76664e4767bc4cdf
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095483"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>ASP.NET ve ASP.NET Core ile uygulamalar arasında tanımlama bilgilerini paylaşma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)

Web siteleri, genellikle birlikte çalışan tek tek web uygulamalarını oluşur. Bir çoklu oturum açma (SSO) deneyimi sağlamak için bir site içinde web apps kimlik doğrulaması tanımlama bilgileri paylaşmanız gerekir. Bu senaryoyu desteklemek için veri koruma yığın Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı sağlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek paylaşımı tanımlama bilgisi kimlik doğrulamasını kullanan üç uygulamalar arasında tanımlama bilgisi gösterilmektedir:

* ASP.NET Core 2.0 Razor sayfaları uygulama kullanmadan [ASP.NET Core kimliği](xref:security/authentication/identity)
* ASP.NET Core kimliği ile ASP.NET Core 2.0 MVC uygulaması
* ASP.NET Identity ile ASP.NET Framework 4.6.1 MVC uygulaması

Aşağıdaki örneklerde içinde:

* Kimlik doğrulama tanımlama bilgisi adı, ortak bir değere ayarlanmış `.AspNet.SharedCookie`.
* `AuthenticationType` Ayarlanır `Identity.Application` açıkça veya varsayılan.
* Ortak bir uygulama adı, veri koruma anahtarları paylaşmak veri koruma sisteminde etkinleştirmek için kullanılır (`SharedCookieApp`).
* `Identity.Application` kimlik doğrulama düzeni kullanılır. Hangi şeması kullanılır, tutarlı bir şekilde kullanılmalıdır *içinde ve arasında* varsayılan düzenini ya da açıkça ayarlayarak paylaşılan tanımlama bilgisi uygulamalar. Düzeni, şifreleme ve tanımlama bilgilerini uygulamalar arasında tutarlı bir düzen kullanılması gerekir şifresini çözme sırasında kullanılır.
* Bir ortak [veri koruma anahtarı](xref:security/data-protection/implementation/key-management) depolama konumu kullanılır. Örnek uygulamayı adlı bir klasör kullanan *kimlik Anahtarlığı* veri koruma anahtarları tutmak için çözümün kökü.
* ASP.NET Core uygulamalarında [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) anahtar depolama konumunu ayarlamak için kullanılır. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adı yapılandırmak için kullanılır.
* .NET Framework uygulamasında tanımlama bilgisi kimlik doğrulaması ara yazılımı uygulaması kullanan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` şifreleme ve kimlik doğrulama tanımlama bilgisi yükü verilerinin şifresinin çözülmesi için veri koruma hizmetleri sağlar. `DataProtectionProvider` Örneği, uygulamanın diğer parçaları tarafından kullanılan veri koruma sisteminde yalıtılır.
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo, eylem\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) kabul eden bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) veri koruma anahtar depolama konumunu belirtmek için. Örnek uygulama yolunu sağlar. *kimlik Anahtarlığı* klasöre `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) ortak uygulama adını ayarlar.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) gerektirir [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet paketi. ASP.NET Core 2.1 ve üzeri uygulamalar için bu paketi elde etmek için başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). .NET Framework hedeflenirken için bir paket başvurusu ekleme `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>ASP.NET Core uygulamaları arasında kimlik doğrulaması tanımlama bilgilerini paylaşma

ASP.NET Core kimliği kullanırken:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

İçinde `ConfigureServices` yöntemi, kullanım [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) tanımlama bilgilerinin veri koruma hizmetini ayarlama için genişletme yöntemi.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Veri koruma anahtarları ve uygulama adı, uygulamalar arasında paylaşılması gerekir. Örnek uygulamalarda `GetKeyRingDirInfo` yaygın anahtarı depolama alanı konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi. Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adı yapılandırmak için (`SharedCookieApp` örnekteki). Daha fazla bilgi için [Data Protection'ı yapılandırma](xref:security/data-protection/configuration/overview).

Bkz: *CookieAuthWithIdentity.Core* projesi [örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

İçinde `Configure` yöntemi, kullanım [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) ayarlamak için:

* Veri koruma hizmeti için tanımlama bilgileri.
* `AuthenticationScheme` ASP.NET eşleştirilecek 4.x.

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

Tanımlama bilgilerini doğrudan kullanırken:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Veri koruma anahtarları ve uygulama adı, uygulamalar arasında paylaşılması gerekir. Örnek uygulamalarda `GetKeyRingDirInfo` yaygın anahtarı depolama alanı konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi. Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adı yapılandırmak için (`SharedCookieApp` örnekteki). Daha fazla bilgi için [Data Protection'ı yapılandırma](xref:security/data-protection/configuration/overview). 

Bkz: *CookieAuth.Core* projesi [örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a>Veri koruma anahtarları bekleme sırasında şifreleme

Üretim dağıtımları için yapılandırma `DataProtectionProvider` DPAPI veya bir X509Certificate bekleyen anahtarlarını şifrelemek için. Bkz: [, anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) daha fazla bilgi için.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>ASP.NET kimlik doğrulaması tanımlama bilgilerini paylaşma 4.x ve ASP.NET Core uygulamaları

ASP.NET 4.x Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan uygulamalar, ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir. Bu, site genelindeki bir kesintisiz SSO deneyimi sunarken büyük bir sitenin tek tek uygulamalar parça parça yükseltme olanağı sağlar.

Bir uygulamayı Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanırken çağırdığı `UseCookieAuthentication` projenin *Startup.Auth.cs* dosya. ASP.NET 4.x web uygulaması projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın. Ancak `UseCookieAuthentication` artık kullanılmıyor ve ASP.NET Core uygulamaları, arama için desteklenmeyen `UseCookieAuthentication` Katana kullanan ASP.NET 4.x uygulaması tanımlama bilgisi kimlik doğrulaması ara yazılımı geçerlidir.

ASP.NET 4.x uygulama .NET Framework 4.5.1'i hedefleyen gerekir ya da daha yüksek. Aksi takdirde yüklemek gerekli NuGet paketlerini başarısız.

ASP.NET Core uygulaması ile bir ASP.NET 4.x uygulama arasındaki kimlik doğrulaması tanımlama bilgileri paylaşmak için yukarıda belirtildiği gibi ASP.NET Core uygulaması yapılandırın ve aşağıdaki adımları izleyerek ASP.NET 4.x uygulamayı yapılandırır:

1. Paketi yüklemek [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) her ASP.NET 4.x uygulamasına.

2. İçinde *Startup.Auth.cs*, çağrı bulun `UseCookieAuthentication` ve şu şekilde değiştirin. ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından kullanılan ad eşleştirilecek tanımlama bilgisi adını değiştirin. Örneği sağlayan bir `DataProtectionProvider` genel veri koruma anahtarı depolama alanı konumuna başlatıldı. Uygulama adı, tanımlama bilgileri, paylaştığınız tüm uygulamalar tarafından kullanılan ortak uygulama adına ayarlandığından emin olun `SharedCookieApp` örnek uygulamada.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Bkz: *CookieAuthWithIdentity.NETFramework* projesi [örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).

Bir kullanıcı kimliği oluştururken, kimlik doğrulama türü içinde tanımlanan tür eşleşmelidir `AuthenticationType` kümesi `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a>Ortak bir kullanıcı veritabanını kullanın

Kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret edilen onaylayın. Aksi takdirde, kendi veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgileri eşleşecek şekilde çalıştığında kimlik sistemi hataları zamanında üretir.

## <a name="additional-resources"></a>Ek kaynaklar

<xref:host-and-deploy/web-farm>
