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
ms.openlocfilehash: 2c0f5de4ecedb796e85c08fc50d9697947a75a3f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="de58b-103">ASP.NET ve ASP.NET Core tanımlama bilgilerini uygulamalar arasında paylaşın</span><span class="sxs-lookup"><span data-stu-id="de58b-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="de58b-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="de58b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="de58b-105">Web siteleri, birlikte çalışan bağımsız web uygulamaları genellikle oluşur.</span><span class="sxs-lookup"><span data-stu-id="de58b-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="de58b-106">Bir çoklu oturum açma (SSO) deneyimi sağlamak için web uygulamaları bir site içinde kimlik doğrulaması tanımlama bilgileri paylaşması gerekir.</span><span class="sxs-lookup"><span data-stu-id="de58b-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="de58b-107">Bu senaryoyu desteklemek için Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı veri koruma yığını sağlar.</span><span class="sxs-lookup"><span data-stu-id="de58b-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="de58b-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="de58b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="de58b-109">Tanımlama bilgisi, tanımlama bilgisi kimlik doğrulamasını kullanan üç uygulamalar arasında paylaşımı örnek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="de58b-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="de58b-110">ASP.NET Core 2.0 Razor sayfalarının uygulama kullanmadan [ASP.NET Core kimliği](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="de58b-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="de58b-111">ASP.NET Core kimlikle ASP.NET Core 2.0 MVC uygulama</span><span class="sxs-lookup"><span data-stu-id="de58b-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="de58b-112">ASP.NET kimliği ile ASP.NET Framework 4.6.1 MVC uygulama</span><span class="sxs-lookup"><span data-stu-id="de58b-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="de58b-113">Örneklerde, izleyin:</span><span class="sxs-lookup"><span data-stu-id="de58b-113">In the examples that follow:</span></span>

* <span data-ttu-id="de58b-114">Kimlik doğrulama tanımlama bilgisi adı ortak bir değere ayarlanır `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="de58b-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="de58b-115">`AuthenticationType` Ayarlanır `Identity.Application` açıkça ya da varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="de58b-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="de58b-116">Ortak bir uygulama adı, veri koruma anahtarları paylaşmak veri koruma sisteminde etkinleştirmek için kullanılır (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="de58b-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="de58b-117">`Identity.Application` kimlik doğrulama düzeni olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de58b-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="de58b-118">Ne olursa olsun düzeni kullanıldığında, tutarlı bir şekilde kullanılmalıdır *içinde ve arasında* paylaşılan tanımlama bilgisi uygulamalar varsayılan düzeni olarak veya açık olarak ayarlayarak.</span><span class="sxs-lookup"><span data-stu-id="de58b-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="de58b-119">Şifreleme ve tanımlama bilgileri, uygulamalar arasında tutarlı bir düzen kullanılması gerekir çözme düzeni kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de58b-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="de58b-120">Ortak bir [veri koruma anahtarı](xref:security/data-protection/implementation/key-management) depolama konumu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de58b-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="de58b-121">Örnek uygulaması adlı bir klasör kullanan *KeyRing* kökündeki çözümünün veri koruma tuşlarını basılı tutun.</span><span class="sxs-lookup"><span data-stu-id="de58b-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="de58b-122">ASP.NET Core uygulamalardaki [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) anahtar depolama konumu ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de58b-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="de58b-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de58b-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="de58b-124">.NET Framework uygulamasında, tanımlama bilgisi kimlik doğrulaması Ara uygulaması kullanan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="de58b-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="de58b-125">`DataProtectionProvider` şifreleme ve kimlik doğrulama tanımlama bilgisi yük verilerinin şifresinin çözülmesi için veri koruma hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="de58b-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="de58b-126">`DataProtectionProvider` Örneğidir diğer uygulama bölümleri tarafından kullanılan veri koruma sisteminden yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="de58b-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="de58b-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span><span class="sxs-lookup"><span data-stu-id="de58b-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="de58b-128">Örnek uygulaması yolunu sağlar *KeyRing* klasörüne `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="de58b-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="de58b-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span><span class="sxs-lookup"><span data-stu-id="de58b-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="de58b-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) gerektirir [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="de58b-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="de58b-131">Bu paket ASP.NET Core 2.0 ve daha sonraki uygulamalar için elde etmek için başvuru [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="de58b-131">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="de58b-132">.NET Framework hedeflerken, paket için bir başvuru ekleyin `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="de58b-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="de58b-133">Kimlik doğrulaması tanımlama bilgileri ASP.NET Core uygulamaları arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="de58b-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="de58b-134">ASP.NET Core kimliği kullanırken:</span><span class="sxs-lookup"><span data-stu-id="de58b-134">When using ASP.NET Core Identity:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="de58b-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="de58b-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="de58b-136">İçinde `ConfigureServices` yöntemi, kullanım [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) veri koruma hizmeti için tanımlama bilgilerini ayarlamak için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="de58b-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="de58b-137">Uygulamalar arasında paylaşılan veri koruma anahtarları ve uygulama adı gerekir.</span><span class="sxs-lookup"><span data-stu-id="de58b-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="de58b-138">Örnek uygulamalarda `GetKeyRingDirInfo` ortak anahtar depolama konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="de58b-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="de58b-139">Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için (`SharedCookieApp` örnekteki).</span><span class="sxs-lookup"><span data-stu-id="de58b-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="de58b-140">Daha fazla bilgi için bkz: [veri koruması yapılandırma](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="de58b-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="de58b-141">Bkz: *CookieAuthWithIdentity.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="de58b-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="de58b-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="de58b-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="de58b-143">İçinde `Configure` yöntemi, kullanım [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="de58b-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="de58b-144">Veri koruma hizmeti için tanımlama bilgileri.</span><span class="sxs-lookup"><span data-stu-id="de58b-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="de58b-145">`AuthenticationScheme` ASP.NET eşleşecek şekilde 4.x.</span><span class="sxs-lookup"><span data-stu-id="de58b-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

* * *
<span data-ttu-id="de58b-146">Tanımlama bilgilerini doğrudan kullanırken:</span><span class="sxs-lookup"><span data-stu-id="de58b-146">When using cookies directly:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="de58b-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="de58b-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="de58b-148">Uygulamalar arasında paylaşılan veri koruma anahtarları ve uygulama adı gerekir.</span><span class="sxs-lookup"><span data-stu-id="de58b-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="de58b-149">Örnek uygulamalarda `GetKeyRingDirInfo` ortak anahtar depolama konumuna döndürür [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="de58b-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="de58b-150">Kullanım [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) ortak bir paylaşılan uygulama adına yapılandırmak için (`SharedCookieApp` örnekteki).</span><span class="sxs-lookup"><span data-stu-id="de58b-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="de58b-151">Daha fazla bilgi için bkz: [veri koruması yapılandırma](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="de58b-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="de58b-152">Bkz: *CookieAuth.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="de58b-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="de58b-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="de58b-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

* * *
## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="de58b-154">Veri koruma anahtarları REST şifreleme</span><span class="sxs-lookup"><span data-stu-id="de58b-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="de58b-155">Üretim dağıtımları için yapılandırma `DataProtectionProvider` DPAPI veya bir X509Certificate REST anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="de58b-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="de58b-156">Bkz: [adresindeki anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="de58b-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="de58b-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="de58b-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="de58b-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="de58b-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="de58b-159">ASP.NET arasında kimlik doğrulaması tanımlama bilgileri paylaşımı 4.x ve ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="de58b-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="de58b-160">Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan ASP.NET 4.x uygulamaların ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="de58b-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="de58b-161">Bu, büyük bir sitenin tek tek uygulamalar parça parça site genelinde kesintisiz SSO bir deneyim sağlarken yükseltme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="de58b-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="de58b-162">Bir uygulama Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullandığında, çağıran `UseCookieAuthentication` projenin *Startup.Auth.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="de58b-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="de58b-163">ASP.NET 4.x web uygulama projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="de58b-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="de58b-164">ASP.NET 4.x uygulama .NET Framework 4.5.1 hedeflemesi gerekir ya da daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="de58b-164">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="de58b-165">Aksi takdirde, gerekli NuGet paketleri yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="de58b-165">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="de58b-166">ASP.NET 4.x uygulamaları ve ASP.NET Core uygulamaları arasında kimlik doğrulaması tanımlama bilgileri paylaşmak için yukarıda belirtildiği gibi ASP.NET Core uygulama yapılandırın, ardından aşağıdaki adımları izleyerek ASP.NET 4.x uygulamaları yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="de58b-166">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="de58b-167">Paketi yüklemek [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) her ASP.NET 4.x uygulamada.</span><span class="sxs-lookup"><span data-stu-id="de58b-167">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="de58b-168">İçinde *Startup.Auth.cs*, çağrısı bulun `UseCookieAuthentication` ve aşağıdaki gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="de58b-168">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="de58b-169">ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından kullanılan adla eşleşmesi için tanımlama bilgisi adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="de58b-169">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="de58b-170">Örneği sağlayan bir `DataProtectionProvider` ortak veri koruma anahtarı depolama konumu başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="de58b-170">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="de58b-171">Uygulama adı tanımlama bilgileri, paylaşan tüm uygulamalar tarafından kullanılan ortak uygulama adına ayarlandığından emin olun `SharedCookieApp` örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="de58b-171">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="de58b-172">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="de58b-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="de58b-173">Bkz: *CookieAuthWithIdentity.NETFramework* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="de58b-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="de58b-174">Bir kullanıcı kimliği oluşturulurken, kimlik doğrulama türü tanımlanan türüyle eşleşmelidir `AuthenticationType` kümesiyle `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="de58b-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="de58b-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="de58b-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="de58b-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="de58b-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="de58b-177">Ayarlama `CookieManager` birlikte çalışma `ChunkingCookieManager` kümeleme biçimi uyumlu olacak şekilde.</span><span class="sxs-lookup"><span data-stu-id="de58b-177">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

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

* * *
## <a name="use-a-common-user-database"></a><span data-ttu-id="de58b-178">Ortak bir kullanıcı veritabanını kullanın</span><span class="sxs-lookup"><span data-stu-id="de58b-178">Use a common user database</span></span>

<span data-ttu-id="de58b-179">Kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret ediyor onaylayın.</span><span class="sxs-lookup"><span data-stu-id="de58b-179">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="de58b-180">Aksi takdirde, veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgisi ile eşleşen çalıştığında kimlik sistemi hataları çalışma zamanında üretir.</span><span class="sxs-lookup"><span data-stu-id="de58b-180">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
