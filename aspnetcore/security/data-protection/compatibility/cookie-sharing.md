---
title: "Tanımlama bilgilerini uygulamalar arasında paylaşma"
author: rick-anderson
description: "Kimlik doğrulaması tanımlama bilgileri ASP.NET arasında paylaşmak öğrenin 4.x ve ASP.NET Core uygulamaları."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e87caa5ba78c6b4c365facc0dea07d747e7c9589
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="sharing-cookies-among-apps"></a><span data-ttu-id="0b481-103">Tanımlama bilgilerini uygulamalar arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="0b481-103">Sharing cookies among apps</span></span>

<span data-ttu-id="0b481-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0b481-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0b481-105">Web siteleri, birlikte çalışan bağımsız web uygulamaları genellikle oluşur.</span><span class="sxs-lookup"><span data-stu-id="0b481-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="0b481-106">Bir çoklu oturum açma (SSO) deneyimi sağlamak için web uygulamaları bir site içinde kimlik doğrulaması tanımlama bilgileri paylaşması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0b481-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="0b481-107">Bu senaryoyu desteklemek için Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı veri koruma yığını sağlar.</span><span class="sxs-lookup"><span data-stu-id="0b481-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="0b481-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0b481-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0b481-109">Tanımlama bilgisi, tanımlama bilgisi kimlik doğrulamasını kullanan üç uygulamalar arasında paylaşımı örnek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0b481-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="0b481-110">ASP.NET Core 2.0 Razor sayfalarının uygulama kullanmadan [ASP.NET Core kimliği](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="0b481-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="0b481-111">ASP.NET Core kimlikle ASP.NET Core 2.0 MVC uygulama</span><span class="sxs-lookup"><span data-stu-id="0b481-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="0b481-112">ASP.NET kimliği ile ASP.NET Framework 4.6.1 MVC uygulama</span><span class="sxs-lookup"><span data-stu-id="0b481-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="0b481-113">Örneklerde, izleyin:</span><span class="sxs-lookup"><span data-stu-id="0b481-113">In the examples that follow:</span></span>

* <span data-ttu-id="0b481-114">Kimlik doğrulama tanımlama bilgisi adı ortak bir değere ayarlanır `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="0b481-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="0b481-115">`AuthenticationType` Ayarlanır `Identity.Application` açıkça ya da varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="0b481-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="0b481-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) is used as the authentication scheme.</span><span class="sxs-lookup"><span data-stu-id="0b481-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) is used as the authentication scheme.</span></span> <span data-ttu-id="0b481-117">Sabit değeri çözümler `Cookies`.</span><span class="sxs-lookup"><span data-stu-id="0b481-117">The constant resolves to a value of `Cookies`.</span></span>
* <span data-ttu-id="0b481-118">Tanımlama bilgisi kimlik doğrulaması Ara uygulaması kullanan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="0b481-118">The cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="0b481-119">`DataProtectionProvider`şifreleme ve kimlik doğrulama tanımlama bilgisi yük verilerinin şifresinin çözülmesi için veri koruma hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="0b481-119">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="0b481-120">`DataProtectionProvider` Örneğidir diğer uygulama bölümleri tarafından kullanılan veri koruma sisteminden yalıtılmış.</span><span class="sxs-lookup"><span data-stu-id="0b481-120">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="0b481-121">Ortak bir [veri koruma anahtarı](xref:security/data-protection/implementation/key-management) depolama konumu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0b481-121">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="0b481-122">Örnek uygulaması adlı bir klasör kullanan *KeyRing* kökündeki çözümünün veri koruma tuşlarını basılı tutun.</span><span class="sxs-lookup"><span data-stu-id="0b481-122">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
  * <span data-ttu-id="0b481-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) for use with authentication cookies.</span><span class="sxs-lookup"><span data-stu-id="0b481-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) for use with authentication cookies.</span></span> <span data-ttu-id="0b481-124">Örnek uygulaması yolunu sağlar *KeyRing* klasörüne `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="0b481-124">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span>
  * <span data-ttu-id="0b481-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) gerektirir [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="0b481-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="0b481-126">Bu paket ASP.NET Core 2.0 ve daha sonraki uygulamalar için elde etmek için başvuru [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="0b481-126">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="0b481-127">.NET Framework'ü hedefleme, paket için bir başvuru ekleyin `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="0b481-127">When targeting .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="0b481-128">Kimlik doğrulaması tanımlama bilgileri ASP.NET Core uygulamaları arasında paylaşma</span><span class="sxs-lookup"><span data-stu-id="0b481-128">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="0b481-129">ASP.NET Core kimliği kullanırken:</span><span class="sxs-lookup"><span data-stu-id="0b481-129">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0b481-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0b481-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0b481-131">İçinde `ConfigureServices` yöntemi, kullanım [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) veri koruma hizmeti için tanımlama bilgilerini ayarlamak için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0b481-131">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="0b481-132">Bkz: *CookieAuthWithIdentity.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0b481-132">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0b481-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0b481-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0b481-134">İçinde `Configure` yöntemi, kullanım [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) ayarlamak için:</span><span class="sxs-lookup"><span data-stu-id="0b481-134">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="0b481-135">Veri koruma hizmeti için tanımlama bilgileri.</span><span class="sxs-lookup"><span data-stu-id="0b481-135">The data protection service for cookies.</span></span>
* <span data-ttu-id="0b481-136">`AuthenticationScheme` ASP.NET eşleşecek şekilde 4.x.</span><span class="sxs-lookup"><span data-stu-id="0b481-136">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="0b481-137">Tanımlama bilgilerini doğrudan kullanırken:</span><span class="sxs-lookup"><span data-stu-id="0b481-137">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0b481-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0b481-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="0b481-139">Bkz: *CookieAuth.Core* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0b481-139">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0b481-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0b481-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="0b481-141">Veri koruma anahtarları REST şifreleme</span><span class="sxs-lookup"><span data-stu-id="0b481-141">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="0b481-142">Üretim dağıtımları için yapılandırma `DataProtectionProvider` DPAPI veya bir X509Certificate REST anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="0b481-142">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="0b481-143">Bkz: [adresindeki anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0b481-143">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0b481-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0b481-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0b481-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0b481-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="0b481-146">ASP.NET arasında kimlik doğrulaması tanımlama bilgileri paylaşımı 4.x ve ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="0b481-146">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="0b481-147">Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan ASP.NET 4.x uygulamaların ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0b481-147">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="0b481-148">Bu, büyük bir sitenin tek tek uygulamalar parça parça site genelinde kesintisiz SSO bir deneyim sağlarken yükseltme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="0b481-148">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="0b481-149">Bir uygulama Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullandığında, çağıran `UseCookieAuthentication` projenin *Startup.Auth.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="0b481-149">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="0b481-150">ASP.NET 4.x web uygulama projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="0b481-150">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="0b481-151">ASP.NET 4.x uygulama .NET Framework 4.5.1 hedeflemesi gerekir ya da daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="0b481-151">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="0b481-152">Aksi takdirde, gerekli NuGet paketleri yüklenemedi.</span><span class="sxs-lookup"><span data-stu-id="0b481-152">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="0b481-153">ASP.NET 4.x uygulamaları ve ASP.NET Core uygulamaları arasında kimlik doğrulaması tanımlama bilgileri paylaşmak için yukarıda belirtildiği gibi ASP.NET Core uygulama yapılandırın, ardından aşağıdaki adımları izleyerek ASP.NET 4.x uygulamaları yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="0b481-153">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="0b481-154">Paketi yüklemek [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) her ASP.NET 4.x uygulamada.</span><span class="sxs-lookup"><span data-stu-id="0b481-154">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="0b481-155">İçinde *Startup.Auth.cs*, çağrısı bulun `UseCookieAuthentication` ve aşağıdaki gibi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0b481-155">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="0b481-156">ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından kullanılan adla eşleşmesi için tanımlama bilgisi adını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="0b481-156">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="0b481-157">Örneği sağlayan bir `DataProtectionProvider` ortak veri koruma anahtarı depolama konumu başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="0b481-157">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0b481-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0b481-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="0b481-159">Bkz: *CookieAuthWithIdentity.NETFramework* proje [örnek koduna](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0b481-159">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="0b481-160">Bir kullanıcı kimliği oluşturulurken, kimlik doğrulama türü tanımlanan türüyle eşleşmelidir `AuthenticationType` kümesiyle `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="0b481-160">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="0b481-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b481-161">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[Main](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0b481-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0b481-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0b481-163">Ayarlama `CookieManager` birlikte çalışma `ChunkingCookieManager` kümeleme biçimi uyumlu olacak şekilde.</span><span class="sxs-lookup"><span data-stu-id="0b481-163">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="0b481-164">Ortak bir kullanıcı veritabanını kullanın</span><span class="sxs-lookup"><span data-stu-id="0b481-164">Use a common user database</span></span>

<span data-ttu-id="0b481-165">Kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret ediyor onaylayın.</span><span class="sxs-lookup"><span data-stu-id="0b481-165">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="0b481-166">Aksi takdirde, veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgisi ile eşleşen çalıştığında kimlik sistemi hataları çalışma zamanında üretir.</span><span class="sxs-lookup"><span data-stu-id="0b481-166">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
