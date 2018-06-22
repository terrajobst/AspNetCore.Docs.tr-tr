---
title: ASP.NET ve ASP.NET Core tanımlama bilgilerini uygulamalar arasında paylaşın
author: rick-anderson
description: Kimlik doğrulaması tanımlama bilgileri ASP.NET arasında paylaşmak öğrenin 4.x ve ASP.NET Core uygulamaları.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: 2636688fa50fb36a8cbd07549e8038474ffa30ca
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278472"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>ASP.NET ve ASP.NET Core tanımlama bilgilerini uygulamalar arasında paylaşın

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)

Web siteleri, birlikte çalışan bağımsız web uygulamaları genellikle oluşur. Bir çoklu oturum açma (SSO) deneyimi sağlamak için web uygulamaları bir site içinde kimlik doğrulaması tanımlama bilgileri paylaşması gerekir. Bu senaryoyu desteklemek için Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı veri koruma yığını sağlar.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Tanımlama bilgisi, tanımlama bilgisi kimlik doğrulamasını kullanan üç uygulamalar arasında paylaşımı örnek gösterilmektedir:

* ASP.NET Core 2.0 Razor sayfalarının uygulama kullanmadan [ASP.NET Core kimliği](xref:security/authentication/identity)
* ASP.NET Core kimlikle ASP.NET Core 2.0 MVC uygulama
* ASP.NET kimliği ile ASP.NET Framework 4.6.1 MVC uygulama

Örneklerde, izleyin:

* Kimlik doğrulama tanımlama bilgisi adı ortak bir değere ayarlanır `.AspNet.SharedCookie`.
* `AuthenticationType` Ayarlanır `Identity.Application` açıkça ya da varsayılan olarak.
* Ortak bir uygulama adı, veri koruma anahtarları paylaşmak veri koruma sisteminde etkinleştirmek için kullanılır (`SharedCookieApp`).
* `Identity.Application` kimlik doğrulama düzeni olarak kullanılır. Ne olursa olsun düzeni kullanıldığında, tutarlı bir şekilde kullanılmalıdır *içinde ve arasında* paylaşılan tanımlama bilgisi uygulamalar varsayılan düzeni olarak veya açık olarak ayarlayarak. Şifreleme ve tanımlama bilgileri, uygulamalar arasında tutarlı bir düzen kullanılması gerekir çözme düzeni kullanılır.
* Ortak bir [veri koruma anahtarı](xref:security/data-protection/implementation/key-management) depolama konumu kullanılır. Örnek uygulaması adlı bir klasör kullanan *KeyRing* kökündeki çözümünün veri koruma tuşlarını basılı tutun.
* ASP.NET Core uygulamalardaki [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) anahtar depolama konumu ayarlamak için kullanılır. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için kullanılır.
* .NET Framework uygulamasında, tanımlama bilgisi kimlik doğrulaması Ara uygulaması kullanan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` şifreleme ve kimlik doğrulama tanımlama bilgisi yük verilerinin şifresinin çözülmesi için veri koruma hizmetleri sağlar. `DataProtectionProvider` Örneğidir diğer uygulama bölümleri tarafından kullanılan veri koruma sisteminden yalıtılmış.
  * [DataProtectionProvider.Create (System.IO.DirectoryInfo, eylemi\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) kabul eden bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) veri koruma anahtarı depolama konumunu belirtmek için. Örnek uygulaması yolunu sağlar *KeyRing* klasörüne `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) ortak uygulama adını ayarlar.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) gerektirir [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet paketi. Bu paket ASP.NET Core 2.1 ve üzeri uygulamalar için elde etmek için başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). .NET Framework hedeflerken, paket için bir başvuru ekleyin `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Kimlik doğrulaması tanımlama bilgileri ASP.NET Core uygulamaları arasında paylaşma

ASP.NET Core kimliği kullanırken:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

İçinde `ConfigureServices` yöntemi, kullanım [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) veri koruma hizmeti için tanımlama bilgilerini ayarlamak için genişletme yöntemi.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Uygulamalar arasında paylaşılan veri koruma anahtarları ve uygulama adı gerekir. Örnek uygulamalarda `GetKeyRingDirInfo` ortak anahtar depolama konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi. Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için (`SharedCookieApp` örnekteki). Daha fazla bilgi için bkz: [veri koruması yapılandırma](xref:security/data-protection/configuration/overview).

Bkz: *CookieAuthWithIdentity.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

İçinde `Configure` yöntemi, kullanım [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) ayarlamak için:

* Veri koruma hizmeti için tanımlama bilgileri.
* `AuthenticationScheme` ASP.NET eşleşecek şekilde 4.x.

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

Uygulamalar arasında paylaşılan veri koruma anahtarları ve uygulama adı gerekir. Örnek uygulamalarda `GetKeyRingDirInfo` ortak anahtar depolama konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi. Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için (`SharedCookieApp` örnekteki). Daha fazla bilgi için bkz: [veri koruması yapılandırma](xref:security/data-protection/configuration/overview). 

Bkz: *CookieAuth.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).

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

## <a name="encrypting-data-protection-keys-at-rest"></a>Veri koruma anahtarları REST şifreleme

Üretim dağıtımları için yapılandırma `DataProtectionProvider` DPAPI veya bir X509Certificate REST anahtarlarını şifrelemek için. Bkz: [adresindeki anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) daha fazla bilgi için.

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>ASP.NET arasında kimlik doğrulaması tanımlama bilgileri paylaşımı 4.x ve ASP.NET Core uygulamaları

Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan ASP.NET 4.x uygulamaların ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir. Bu, büyük bir sitenin tek tek uygulamalar parça parça site genelinde kesintisiz SSO bir deneyim sağlarken yükseltme olanağı sağlar.

Bir uygulama Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullandığında, çağıran `UseCookieAuthentication` projenin *Startup.Auth.cs* dosya. ASP.NET 4.x web uygulama projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın. Ancak `UseCookieAuthentication` artık kullanılmayan ve ASP.NET Core uygulamaları, çağırmak için desteklenmeyen `UseCookieAuthentication` Katana kullanan ASP.NET 4.x uygulamada tanımlama bilgisi kimlik doğrulaması ara yazılımı geçerlidir.

ASP.NET 4.x uygulama .NET Framework 4.5.1 hedeflemesi gerekir ya da daha yüksek. Aksi takdirde, gerekli NuGet paketleri yüklenemedi.

ASP.NET 4.x uygulama ve ASP.NET Core uygulama arasındaki kimlik doğrulaması tanımlama bilgileri paylaşmak için yukarıda belirtildiği gibi ASP.NET Core uygulama yapılandırın, ardından aşağıdaki adımları izleyerek ASP.NET 4.x uygulamayı yapılandırabilirsiniz:

1. Paketi yüklemek [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) her ASP.NET 4.x uygulamada.

2. İçinde *Startup.Auth.cs*, çağrısı bulun `UseCookieAuthentication` ve aşağıdaki gibi değiştirin. ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından kullanılan adla eşleşmesi için tanımlama bilgisi adını değiştirin. Örneği sağlayan bir `DataProtectionProvider` ortak veri koruma anahtarı depolama konumu başlatıldı. Uygulama adı tanımlama bilgileri, paylaşan tüm uygulamalar tarafından kullanılan ortak uygulama adına ayarlandığından emin olun `SharedCookieApp` örnek uygulama.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Bkz: *CookieAuthWithIdentity.NETFramework* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).

Bir kullanıcı kimliği oluşturulurken, kimlik doğrulama türü tanımlanan türüyle eşleşmelidir `AuthenticationType` kümesiyle `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a>Ortak bir kullanıcı veritabanını kullanın

Kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret ediyor onaylayın. Aksi takdirde, veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgisi ile eşleşen çalıştığında kimlik sistemi hataları çalışma zamanında üretir.
