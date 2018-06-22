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
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="53cf9-103">ASP.NET ve ASP.NET Core tanımlama bilgilerini uygulamalar arasında paylaşın</span><span class="sxs-lookup"><span data-stu-id="53cf9-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="53cf9-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="53cf9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="53cf9-105">Web siteleri, birlikte çalışan bağımsız web uygulamaları genellikle oluşur.</span><span class="sxs-lookup"><span data-stu-id="53cf9-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="53cf9-106">Bir çoklu oturum açma (SSO) deneyimi sağlamak için web uygulamaları bir site içinde kimlik doğrulaması tanımlama bilgileri paylaşması gerekir.</span><span class="sxs-lookup"><span data-stu-id="53cf9-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="53cf9-107">Bu senaryoyu desteklemek için Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı veri koruma yığını sağlar.</span><span class="sxs-lookup"><span data-stu-id="53cf9-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="53cf9-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="53cf9-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="53cf9-109">Tanımlama bilgisi, tanımlama bilgisi kimlik doğrulamasını kullanan üç uygulamalar arasında paylaşımı örnek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="53cf9-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="53cf9-110">ASP.NET Core 2.0 Razor sayfalarının uygulama kullanmadan [ASP.NET Core kimliği](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="53cf9-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="53cf9-111">ASP.NET Core kimlikle ASP.NET Core 2.0 MVC uygulama</span><span class="sxs-lookup"><span data-stu-id="53cf9-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="53cf9-112">ASP.NET kimliği ile ASP.NET Framework 4.6.1 MVC uygulama</span><span class="sxs-lookup"><span data-stu-id="53cf9-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="53cf9-113">Örneklerde, izleyin:</span><span class="sxs-lookup"><span data-stu-id="53cf9-113">In the examples that follow:</span></span>

* <span data-ttu-id="53cf9-114">Kimlik doğrulama tanımlama bilgisi adı ortak bir değere ayarlanır `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="53cf9-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="53cf9-115">`AuthenticationType` Ayarlanır `Identity.Application` açıkça ya da varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="53cf9-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="53cf9-116">Ortak bir uygulama adı, veri koruma anahtarları paylaşmak veri koruma sisteminde etkinleştirmek için kullanılır (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="53cf9-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="53cf9-117">`Identity.Application` kimlik doğrulama düzeni olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53cf9-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="53cf9-118">Ne olursa olsun düzeni kullanıldığında, tutarlı bir şekilde kullanılmalıdır *içinde ve arasında* paylaşılan tanımlama bilgisi uygulamalar varsayılan düzeni olarak veya açık olarak ayarlayarak.</span><span class="sxs-lookup"><span data-stu-id="53cf9-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="53cf9-119">Şifreleme ve tanımlama bilgileri, uygulamalar arasında tutarlı bir düzen kullanılması gerekir çözme düzeni kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53cf9-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="53cf9-120">Ortak bir [veri koruma anahtarı](xref:security/data-protection/implementation/key-management) depolama konumu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53cf9-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="53cf9-121">Örnek uygulaması adlı bir klasör kullanan *KeyRing* kökündeki çözümünün veri koruma tuşlarını basılı tutun.</span><span class="sxs-lookup"><span data-stu-id="53cf9-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="53cf9-122">ASP.NET Core uygulamalardaki [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) anahtar depolama konumu ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53cf9-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="53cf9-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="53cf9-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="53cf9-124">.NET Framework uygulamasında, tanımlama bilgisi kimlik doğrulaması Ara uygulaması kullanan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="53cf9-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="53cf9-125">`DataProtectionProvider` şifreleme ve kimlik doğrulama tanımlama bilgisi yük verilerinin şifresinin çözülmesi için veri koruma hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="53cf9-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="53cf9-126">`DataProtectionProvider` Örneğidir diğer uygulama bölümleri tarafından kullanılan veri koruma sisteminden yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="53cf9-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="53cf9-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, eylemi\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) kabul eden bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) veri koruma anahtarı depolama konumunu belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="53cf9-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="53cf9-128">Örnek uygulaması yolunu sağlar *KeyRing* klasörüne `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="53cf9-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="53cf9-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) ortak uygulama adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="53cf9-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="53cf9-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) gerektirir [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="53cf9-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="53cf9-131">Bu paket ASP.NET Core 2.1 ve üzeri uygulamalar için elde etmek için başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="53cf9-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="53cf9-132">.NET Framework hedeflerken, paket için bir başvuru ekleyin `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="53cf9-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="53cf9-133">Kimlik doğrulaması tanımlama bilgileri ASP.NET Core uygulamaları arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="53cf9-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="53cf9-134">ASP.NET Core kimliği kullanırken:</span><span class="sxs-lookup"><span data-stu-id="53cf9-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="53cf9-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="53cf9-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="53cf9-136">İçinde `ConfigureServices` yöntemi, kullanım [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) veri koruma hizmeti için tanımlama bilgilerini ayarlamak için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="53cf9-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="53cf9-137">Uygulamalar arasında paylaşılan veri koruma anahtarları ve uygulama adı gerekir.</span><span class="sxs-lookup"><span data-stu-id="53cf9-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="53cf9-138">Örnek uygulamalarda `GetKeyRingDirInfo` ortak anahtar depolama konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="53cf9-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="53cf9-139">Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için (`SharedCookieApp` örnekteki).</span><span class="sxs-lookup"><span data-stu-id="53cf9-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="53cf9-140">Daha fazla bilgi için bkz: [veri koruması yapılandırma](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="53cf9-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="53cf9-141">Bkz: *CookieAuthWithIdentity.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="53cf9-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="53cf9-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="53cf9-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="53cf9-143">İçinde `Configure` yöntemi, kullanım [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="53cf9-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="53cf9-144">Veri koruma hizmeti için tanımlama bilgileri.</span><span class="sxs-lookup"><span data-stu-id="53cf9-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="53cf9-145">`AuthenticationScheme` ASP.NET eşleşecek şekilde 4.x.</span><span class="sxs-lookup"><span data-stu-id="53cf9-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="53cf9-146">Tanımlama bilgilerini doğrudan kullanırken:</span><span class="sxs-lookup"><span data-stu-id="53cf9-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="53cf9-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="53cf9-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="53cf9-148">Uygulamalar arasında paylaşılan veri koruma anahtarları ve uygulama adı gerekir.</span><span class="sxs-lookup"><span data-stu-id="53cf9-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="53cf9-149">Örnek uygulamalarda `GetKeyRingDirInfo` ortak anahtar depolama konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="53cf9-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="53cf9-150">Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için (`SharedCookieApp` örnekteki).</span><span class="sxs-lookup"><span data-stu-id="53cf9-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="53cf9-151">Daha fazla bilgi için bkz: [veri koruması yapılandırma](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="53cf9-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="53cf9-152">Bkz: *CookieAuth.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="53cf9-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="53cf9-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="53cf9-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="53cf9-154">Veri koruma anahtarları REST şifreleme</span><span class="sxs-lookup"><span data-stu-id="53cf9-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="53cf9-155">Üretim dağıtımları için yapılandırma `DataProtectionProvider` DPAPI veya bir X509Certificate REST anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="53cf9-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="53cf9-156">Bkz: [adresindeki anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="53cf9-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="53cf9-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="53cf9-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="53cf9-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="53cf9-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="53cf9-159">ASP.NET arasında kimlik doğrulaması tanımlama bilgileri paylaşımı 4.x ve ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="53cf9-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="53cf9-160">Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan ASP.NET 4.x uygulamaların ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="53cf9-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="53cf9-161">Bu, büyük bir sitenin tek tek uygulamalar parça parça site genelinde kesintisiz SSO bir deneyim sağlarken yükseltme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="53cf9-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="53cf9-162">Bir uygulama Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullandığında, çağıran `UseCookieAuthentication` projenin *Startup.Auth.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="53cf9-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="53cf9-163">ASP.NET 4.x web uygulama projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="53cf9-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="53cf9-164">Ancak `UseCookieAuthentication` artık kullanılmayan ve ASP.NET Core uygulamaları, çağırmak için desteklenmeyen `UseCookieAuthentication` Katana kullanan ASP.NET 4.x uygulamada tanımlama bilgisi kimlik doğrulaması ara yazılımı geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="53cf9-164">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="53cf9-165">ASP.NET 4.x uygulama .NET Framework 4.5.1 hedeflemesi gerekir ya da daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="53cf9-165">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="53cf9-166">Aksi takdirde, gerekli NuGet paketleri yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="53cf9-166">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="53cf9-167">ASP.NET 4.x uygulama ve ASP.NET Core uygulama arasındaki kimlik doğrulaması tanımlama bilgileri paylaşmak için yukarıda belirtildiği gibi ASP.NET Core uygulama yapılandırın, ardından aşağıdaki adımları izleyerek ASP.NET 4.x uygulamayı yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="53cf9-167">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="53cf9-168">Paketi yüklemek [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) her ASP.NET 4.x uygulamada.</span><span class="sxs-lookup"><span data-stu-id="53cf9-168">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="53cf9-169">İçinde *Startup.Auth.cs*, çağrısı bulun `UseCookieAuthentication` ve aşağıdaki gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="53cf9-169">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="53cf9-170">ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından kullanılan adla eşleşmesi için tanımlama bilgisi adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="53cf9-170">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="53cf9-171">Örneği sağlayan bir `DataProtectionProvider` ortak veri koruma anahtarı depolama konumu başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="53cf9-171">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="53cf9-172">Uygulama adı tanımlama bilgileri, paylaşan tüm uygulamalar tarafından kullanılan ortak uygulama adına ayarlandığından emin olun `SharedCookieApp` örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="53cf9-172">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="53cf9-173">Bkz: *CookieAuthWithIdentity.NETFramework* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="53cf9-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="53cf9-174">Bir kullanıcı kimliği oluşturulurken, kimlik doğrulama türü tanımlanan türüyle eşleşmelidir `AuthenticationType` kümesiyle `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="53cf9-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="53cf9-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="53cf9-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="53cf9-176">Ortak bir kullanıcı veritabanını kullanın</span><span class="sxs-lookup"><span data-stu-id="53cf9-176">Use a common user database</span></span>

<span data-ttu-id="53cf9-177">Kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret ediyor onaylayın.</span><span class="sxs-lookup"><span data-stu-id="53cf9-177">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="53cf9-178">Aksi takdirde, veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgisi ile eşleşen çalıştığında kimlik sistemi hataları çalışma zamanında üretir.</span><span class="sxs-lookup"><span data-stu-id="53cf9-178">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
