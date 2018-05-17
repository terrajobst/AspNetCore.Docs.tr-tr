---
title: ASP.NET ve ASP.NET Core tanımlama bilgilerini uygulamalar arasında paylaşın
author: rick-anderson
description: Kimlik doğrulaması tanımlama bilgileri ASP.NET arasında paylaşmak öğrenin 4.x ve ASP.NET Core uygulamaları.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: 5f77377f168993d48686217adac54a75313766ec
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="3e34e-103">ASP.NET ve ASP.NET Core tanımlama bilgilerini uygulamalar arasında paylaşın</span><span class="sxs-lookup"><span data-stu-id="3e34e-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="3e34e-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3e34e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3e34e-105">Web siteleri, birlikte çalışan bağımsız web uygulamaları genellikle oluşur.</span><span class="sxs-lookup"><span data-stu-id="3e34e-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="3e34e-106">Bir çoklu oturum açma (SSO) deneyimi sağlamak için web uygulamaları bir site içinde kimlik doğrulaması tanımlama bilgileri paylaşması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e34e-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="3e34e-107">Bu senaryoyu desteklemek için Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı veri koruma yığını sağlar.</span><span class="sxs-lookup"><span data-stu-id="3e34e-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="3e34e-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3e34e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3e34e-109">Tanımlama bilgisi, tanımlama bilgisi kimlik doğrulamasını kullanan üç uygulamalar arasında paylaşımı örnek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="3e34e-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="3e34e-110">ASP.NET Core 2.0 Razor sayfalarının uygulama kullanmadan [ASP.NET Core kimliği](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="3e34e-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="3e34e-111">ASP.NET Core kimlikle ASP.NET Core 2.0 MVC uygulama</span><span class="sxs-lookup"><span data-stu-id="3e34e-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="3e34e-112">ASP.NET kimliği ile ASP.NET Framework 4.6.1 MVC uygulama</span><span class="sxs-lookup"><span data-stu-id="3e34e-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="3e34e-113">Örneklerde, izleyin:</span><span class="sxs-lookup"><span data-stu-id="3e34e-113">In the examples that follow:</span></span>

* <span data-ttu-id="3e34e-114">Kimlik doğrulama tanımlama bilgisi adı ortak bir değere ayarlanır `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="3e34e-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="3e34e-115">`AuthenticationType` Ayarlanır `Identity.Application` açıkça ya da varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="3e34e-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="3e34e-116">Ortak bir uygulama adı, veri koruma anahtarları paylaşmak veri koruma sisteminde etkinleştirmek için kullanılır (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="3e34e-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="3e34e-117">`Identity.Application` kimlik doğrulama düzeni olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e34e-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="3e34e-118">Ne olursa olsun düzeni kullanıldığında, tutarlı bir şekilde kullanılmalıdır *içinde ve arasında* paylaşılan tanımlama bilgisi uygulamalar varsayılan düzeni olarak veya açık olarak ayarlayarak.</span><span class="sxs-lookup"><span data-stu-id="3e34e-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="3e34e-119">Şifreleme ve tanımlama bilgileri, uygulamalar arasında tutarlı bir düzen kullanılması gerekir çözme düzeni kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e34e-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="3e34e-120">Ortak bir [veri koruma anahtarı](xref:security/data-protection/implementation/key-management) depolama konumu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e34e-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="3e34e-121">Örnek uygulaması adlı bir klasör kullanan *KeyRing* kökündeki çözümünün veri koruma tuşlarını basılı tutun.</span><span class="sxs-lookup"><span data-stu-id="3e34e-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="3e34e-122">ASP.NET Core uygulamalardaki [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) anahtar depolama konumu ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e34e-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="3e34e-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3e34e-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="3e34e-124">.NET Framework uygulamasında, tanımlama bilgisi kimlik doğrulaması Ara uygulaması kullanan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="3e34e-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="3e34e-125">`DataProtectionProvider` şifreleme ve kimlik doğrulama tanımlama bilgisi yük verilerinin şifresinin çözülmesi için veri koruma hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="3e34e-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="3e34e-126">`DataProtectionProvider` Örneğidir diğer uygulama bölümleri tarafından kullanılan veri koruma sisteminden yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="3e34e-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="3e34e-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo, eylemi\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) kabul eden bir [DirectoryInfo](/dotnet/api/system.io.directoryinfo) veri koruma anahtarı depolama konumunu belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="3e34e-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="3e34e-128">Örnek uygulaması yolunu sağlar *KeyRing* klasörüne `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="3e34e-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="3e34e-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) ortak uygulama adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3e34e-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="3e34e-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) gerektirir [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="3e34e-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="3e34e-131">Bu paket ASP.NET Core 2.0 ve daha sonraki uygulamalar için elde etmek için başvuru [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="3e34e-131">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="3e34e-132">.NET Framework hedeflerken, paket için bir başvuru ekleyin `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="3e34e-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="3e34e-133">Kimlik doğrulaması tanımlama bilgileri ASP.NET Core uygulamaları arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="3e34e-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="3e34e-134">ASP.NET Core kimliği kullanırken:</span><span class="sxs-lookup"><span data-stu-id="3e34e-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3e34e-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3e34e-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="3e34e-136">İçinde `ConfigureServices` yöntemi, kullanım [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) veri koruma hizmeti için tanımlama bilgilerini ayarlamak için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3e34e-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="3e34e-137">Uygulamalar arasında paylaşılan veri koruma anahtarları ve uygulama adı gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e34e-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="3e34e-138">Örnek uygulamalarda `GetKeyRingDirInfo` ortak anahtar depolama konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3e34e-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="3e34e-139">Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için (`SharedCookieApp` örnekteki).</span><span class="sxs-lookup"><span data-stu-id="3e34e-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="3e34e-140">Daha fazla bilgi için bkz: [veri koruması yapılandırma](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="3e34e-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="3e34e-141">Bkz: *CookieAuthWithIdentity.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="3e34e-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3e34e-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3e34e-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="3e34e-143">İçinde `Configure` yöntemi, kullanım [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="3e34e-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="3e34e-144">Veri koruma hizmeti için tanımlama bilgileri.</span><span class="sxs-lookup"><span data-stu-id="3e34e-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="3e34e-145">`AuthenticationScheme` ASP.NET eşleşecek şekilde 4.x.</span><span class="sxs-lookup"><span data-stu-id="3e34e-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="3e34e-146">Tanımlama bilgilerini doğrudan kullanırken:</span><span class="sxs-lookup"><span data-stu-id="3e34e-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3e34e-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3e34e-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="3e34e-148">Uygulamalar arasında paylaşılan veri koruma anahtarları ve uygulama adı gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e34e-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="3e34e-149">Örnek uygulamalarda `GetKeyRingDirInfo` ortak anahtar depolama konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3e34e-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="3e34e-150">Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için (`SharedCookieApp` örnekteki).</span><span class="sxs-lookup"><span data-stu-id="3e34e-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="3e34e-151">Daha fazla bilgi için bkz: [veri koruması yapılandırma](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="3e34e-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="3e34e-152">Bkz: *CookieAuth.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="3e34e-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3e34e-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3e34e-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="3e34e-154">Veri koruma anahtarları REST şifreleme</span><span class="sxs-lookup"><span data-stu-id="3e34e-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="3e34e-155">Üretim dağıtımları için yapılandırma `DataProtectionProvider` DPAPI veya bir X509Certificate REST anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="3e34e-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="3e34e-156">Bkz: [adresindeki anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3e34e-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3e34e-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3e34e-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3e34e-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3e34e-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="3e34e-159">ASP.NET arasında kimlik doğrulaması tanımlama bilgileri paylaşımı 4.x ve ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="3e34e-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="3e34e-160">Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan ASP.NET 4.x uygulamaların ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e34e-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="3e34e-161">Bu, büyük bir sitenin tek tek uygulamalar parça parça site genelinde kesintisiz SSO bir deneyim sağlarken yükseltme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3e34e-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="3e34e-162">Bir uygulama Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullandığında, çağıran `UseCookieAuthentication` projenin *Startup.Auth.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="3e34e-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="3e34e-163">ASP.NET 4.x web uygulama projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e34e-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="3e34e-164">ASP.NET 4.x uygulama .NET Framework 4.5.1 hedeflemesi gerekir ya da daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="3e34e-164">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="3e34e-165">Aksi takdirde, gerekli NuGet paketleri yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="3e34e-165">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="3e34e-166">ASP.NET 4.x uygulamaları ve ASP.NET Core uygulamaları arasında kimlik doğrulaması tanımlama bilgileri paylaşmak için yukarıda belirtildiği gibi ASP.NET Core uygulama yapılandırın, ardından aşağıdaki adımları izleyerek ASP.NET 4.x uygulamaları yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="3e34e-166">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="3e34e-167">Paketi yüklemek [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) her ASP.NET 4.x uygulamada.</span><span class="sxs-lookup"><span data-stu-id="3e34e-167">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="3e34e-168">İçinde *Startup.Auth.cs*, çağrısı bulun `UseCookieAuthentication` ve aşağıdaki gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3e34e-168">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="3e34e-169">ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından kullanılan adla eşleşmesi için tanımlama bilgisi adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3e34e-169">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="3e34e-170">Örneği sağlayan bir `DataProtectionProvider` ortak veri koruma anahtarı depolama konumu başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="3e34e-170">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="3e34e-171">Uygulama adı tanımlama bilgileri, paylaşan tüm uygulamalar tarafından kullanılan ortak uygulama adına ayarlandığından emin olun `SharedCookieApp` örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="3e34e-171">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3e34e-172">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3e34e-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="3e34e-173">Bkz: *CookieAuthWithIdentity.NETFramework* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="3e34e-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="3e34e-174">Bir kullanıcı kimliği oluşturulurken, kimlik doğrulama türü tanımlanan türüyle eşleşmelidir `AuthenticationType` kümesiyle `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="3e34e-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="3e34e-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="3e34e-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3e34e-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3e34e-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="3e34e-177">Ayarlama `CookieManager` birlikte çalışma `ChunkingCookieManager` kümeleme biçimi uyumlu olacak şekilde.</span><span class="sxs-lookup"><span data-stu-id="3e34e-177">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a><span data-ttu-id="3e34e-178">Ortak bir kullanıcı veritabanını kullanın</span><span class="sxs-lookup"><span data-stu-id="3e34e-178">Use a common user database</span></span>

<span data-ttu-id="3e34e-179">Kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret ediyor onaylayın.</span><span class="sxs-lookup"><span data-stu-id="3e34e-179">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="3e34e-180">Aksi takdirde, veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgisi ile eşleşen çalıştığında kimlik sistemi hataları çalışma zamanında üretir.</span><span class="sxs-lookup"><span data-stu-id="3e34e-180">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
