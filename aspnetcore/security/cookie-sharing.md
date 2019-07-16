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
# <a name="share-authentication-cookies-among-aspnet-apps"></a><span data-ttu-id="d99a7-103">ASP.NET uygulamaları arasında kimlik doğrulaması tanımlama bilgilerini paylaşma</span><span class="sxs-lookup"><span data-stu-id="d99a7-103">Share authentication cookies among ASP.NET apps</span></span>

<span data-ttu-id="d99a7-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d99a7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d99a7-105">Web siteleri, genellikle birlikte çalışan tek tek web uygulamalarını oluşur.</span><span class="sxs-lookup"><span data-stu-id="d99a7-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="d99a7-106">Bir çoklu oturum açma (SSO) deneyimi sağlamak için bir site içinde web apps kimlik doğrulaması tanımlama bilgileri paylaşmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d99a7-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="d99a7-107">Bu senaryoyu desteklemek için veri koruma yığın Katana tanımlama bilgisi kimlik doğrulamasını ve ASP.NET Core tanımlama bilgisi kimlik doğrulama biletlerini paylaşımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d99a7-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="d99a7-108">Aşağıdaki örneklerde içinde:</span><span class="sxs-lookup"><span data-stu-id="d99a7-108">In the examples that follow:</span></span>

* <span data-ttu-id="d99a7-109">Kimlik doğrulama tanımlama bilgisi adı, ortak bir değere ayarlanmış `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="d99a7-109">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="d99a7-110">`AuthenticationType` Ayarlanır `Identity.Application` açıkça veya varsayılan.</span><span class="sxs-lookup"><span data-stu-id="d99a7-110">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="d99a7-111">Ortak bir uygulama adı, veri koruma anahtarları paylaşmak veri koruma sisteminde etkinleştirmek için kullanılır (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="d99a7-111">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="d99a7-112">`Identity.Application` kimlik doğrulama düzeni kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d99a7-112">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="d99a7-113">Hangi şeması kullanılır, tutarlı bir şekilde kullanılmalıdır *içinde ve arasında* varsayılan düzenini ya da açıkça ayarlayarak paylaşılan tanımlama bilgisi uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="d99a7-113">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="d99a7-114">Düzeni, şifreleme ve tanımlama bilgilerini uygulamalar arasında tutarlı bir düzen kullanılması gerekir şifresini çözme sırasında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d99a7-114">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="d99a7-115">Bir ortak [veri koruma anahtarı](xref:security/data-protection/implementation/key-management) depolama konumu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d99a7-115">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span>
  * <span data-ttu-id="d99a7-116">ASP.NET Core uygulamalarında <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> anahtar depolama konumunu ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d99a7-116">In ASP.NET Core apps, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> is used to set the key storage location.</span></span>
  * <span data-ttu-id="d99a7-117">.NET Framework uygulamalarında tanımlama bilgisi kimlik doğrulaması ara yazılımı uygulaması kullanan <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span><span class="sxs-lookup"><span data-stu-id="d99a7-117">In .NET Framework apps, Cookie Authentication Middleware uses an implementation of <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span></span> <span data-ttu-id="d99a7-118">`DataProtectionProvider` şifreleme ve kimlik doğrulama tanımlama bilgisi yükü verilerinin şifresinin çözülmesi için veri koruma hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="d99a7-118">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="d99a7-119">`DataProtectionProvider` Örneği, uygulamanın diğer parçaları tarafından kullanılan veri koruma sisteminde yalıtılır.</span><span class="sxs-lookup"><span data-stu-id="d99a7-119">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span> <span data-ttu-id="d99a7-120">[DataProtectionProvider.Create (System.IO.DirectoryInfo, eylem\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) kabul eden bir <xref:System.IO.DirectoryInfo> veri koruma anahtar depolama konumunu belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="d99a7-120">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepts a <xref:System.IO.DirectoryInfo> to specify the location for data protection key storage.</span></span>
* <span data-ttu-id="d99a7-121">`DataProtectionProvider` gerektirir [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="d99a7-121">`DataProtectionProvider` requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package:</span></span>
  * <span data-ttu-id="d99a7-122">ASP.NET Core 2.x uygulamaları başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d99a7-122">In ASP.NET Core 2.x apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="d99a7-123">.NET Framework uygulamalarında paket başvurusu ekleme [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="d99a7-123">In .NET Framework apps, add a package reference to [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span></span>
* <span data-ttu-id="d99a7-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Ortak uygulama adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="d99a7-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> sets the common app name.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="d99a7-125">ASP.NET Core uygulamaları arasında kimlik doğrulaması tanımlama bilgilerini paylaşma</span><span class="sxs-lookup"><span data-stu-id="d99a7-125">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="d99a7-126">ASP.NET Core kimliği kullanırken:</span><span class="sxs-lookup"><span data-stu-id="d99a7-126">When using ASP.NET Core Identity:</span></span>

* <span data-ttu-id="d99a7-127">Veri koruma anahtarları ve uygulama adı, uygulamalar arasında paylaşılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d99a7-127">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="d99a7-128">Bir ortak anahtar depolama konumu için sağlanan <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> ilişkin aşağıdaki örneklerde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d99a7-128">A common key storage location is provided to the <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> method in the following examples.</span></span> <span data-ttu-id="d99a7-129">Kullanım <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> ortak bir paylaşılan uygulama adı yapılandırmak için (`SharedCookieApp` ilişkin aşağıdaki örneklerde).</span><span class="sxs-lookup"><span data-stu-id="d99a7-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> to configure a common shared app name (`SharedCookieApp` in the following examples).</span></span> <span data-ttu-id="d99a7-130">Daha fazla bilgi için bkz. <xref:security/data-protection/configuration/overview>.</span><span class="sxs-lookup"><span data-stu-id="d99a7-130">For more information, see <xref:security/data-protection/configuration/overview>.</span></span>
* <span data-ttu-id="d99a7-131">Kullanım <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> tanımlama bilgilerinin veri koruma hizmetini ayarlama için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d99a7-131">Use the <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> extension method to set up the data protection service for cookies.</span></span>
* <span data-ttu-id="d99a7-132">Aşağıdaki örnekte, kimlik doğrulaması türünü ayarlamak `Identity.Application` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="d99a7-132">In the following example, the authentication type is set to `Identity.Application` by default.</span></span>

<span data-ttu-id="d99a7-133">İçinde `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d99a7-133">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

<span data-ttu-id="d99a7-134">ASP.NET Core kimliği olmadan doğrudan tanımlama bilgileri kullanırken, veri koruma ve kimlik doğrulaması yapılandırma `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d99a7-134">When using cookies directly without ASP.NET Core Identity, configure data protection and authentication in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d99a7-135">Aşağıdaki örnekte, kimlik doğrulaması türünü ayarlamak `Identity.Application`:</span><span class="sxs-lookup"><span data-stu-id="d99a7-135">In the following example, the authentication type is set to `Identity.Application`:</span></span>

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

<span data-ttu-id="d99a7-136">Alt etki alanları arasında tanımlama bilgilerini paylaşma uygulamayı barındırırken, ortak bir etki alanında belirtin [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) özelliği.</span><span class="sxs-lookup"><span data-stu-id="d99a7-136">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) property.</span></span> <span data-ttu-id="d99a7-137">Tanımlama bilgileri uygulama arasında paylaşmak için `contoso.com`, gibi `first_subdomain.contoso.com` ve `second_subdomain.contoso.com`, belirtin `Cookie.Domain` olarak `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="d99a7-137">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a><span data-ttu-id="d99a7-138">Veri koruma anahtarları bekleme sırasında şifreleme</span><span class="sxs-lookup"><span data-stu-id="d99a7-138">Encrypt data protection keys at rest</span></span>

<span data-ttu-id="d99a7-139">Üretim dağıtımları için yapılandırma `DataProtectionProvider` DPAPI veya bir X509Certificate bekleyen anahtarlarını şifrelemek için.</span><span class="sxs-lookup"><span data-stu-id="d99a7-139">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="d99a7-140">Daha fazla bilgi için bkz. <xref:security/data-protection/implementation/key-encryption-at-rest>.</span><span class="sxs-lookup"><span data-stu-id="d99a7-140">For more information, see <xref:security/data-protection/implementation/key-encryption-at-rest>.</span></span> <span data-ttu-id="d99a7-141">Aşağıdaki örnekte, bir sertifika parmak izi için sağlanan <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span><span class="sxs-lookup"><span data-stu-id="d99a7-141">In the following example, a certificate thumbprint is provided to <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span></span>

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="d99a7-142">ASP.NET arasında kimlik doğrulaması tanımlama bilgilerini paylaşma 4.x ve ASP.NET Core uygulamaları</span><span class="sxs-lookup"><span data-stu-id="d99a7-142">Share authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="d99a7-143">Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan ASP.NET 4.x uygulamalar, ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı ile uyumlu olan kimlik doğrulama tanımlama bilgisi oluşturmak için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d99a7-143">ASP.NET 4.x apps that use Katana Cookie Authentication Middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core Cookie Authentication Middleware.</span></span> <span data-ttu-id="d99a7-144">Bu, birkaç adımda büyük bir sitenin tek tek uygulamalar site genelinde kesintisiz SSO bir deneyim sağlarken yükseltme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="d99a7-144">This allows upgrading a large site's individual apps in several steps while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="d99a7-145">Bir uygulamayı Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanırken çağırdığı `UseCookieAuthentication` projenin *Startup.Auth.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="d99a7-145">When an app uses Katana Cookie Authentication Middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="d99a7-146">ASP.NET 4.x web uygulaması projeleri, Visual Studio 2013 ile oluşturulan ve daha sonra Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı varsayılan olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="d99a7-146">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana Cookie Authentication Middleware by default.</span></span> <span data-ttu-id="d99a7-147">Ancak `UseCookieAuthentication` artık kullanılmıyor ve ASP.NET Core uygulamaları, arama için desteklenmeyen `UseCookieAuthentication` bir ASP.NET Katana tanımlama bilgisi kimlik doğrulaması ara yazılımı kullanan 4.x uygulamayı geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="d99a7-147">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana Cookie Authentication Middleware is valid.</span></span>

<span data-ttu-id="d99a7-148">ASP.NET 4.x uygulama .NET Framework 4.5.1'i hedefleyen gerekir veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="d99a7-148">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="d99a7-149">Aksi takdirde yüklemek gerekli NuGet paketlerini başarısız.</span><span class="sxs-lookup"><span data-stu-id="d99a7-149">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="d99a7-150">ASP.NET Core uygulaması ile bir ASP.NET 4.x uygulama arasındaki kimlik doğrulaması tanımlama bilgileri paylaşmak için ASP.NET Core uygulaması içinde belirtildiği gibi yapılandırma [ASP.NET Core uygulamaları arasında kimlik doğrulaması tanımlama bilgilerini paylaşma](#share-authentication-cookies-among-aspnet-core-apps) bölümüne ve ardından ASP.NET 4.x uygulamayı olarak yapılandırma izler.</span><span class="sxs-lookup"><span data-stu-id="d99a7-150">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated in the [Share authentication cookies among ASP.NET Core apps](#share-authentication-cookies-among-aspnet-core-apps) section, then configure the ASP.NET 4.x app as follows.</span></span>

<span data-ttu-id="d99a7-151">Uygulama paketleri için en son sürümlerine güncelleştirildiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="d99a7-151">Confirm that the app's packages are updated to the latest releases.</span></span> <span data-ttu-id="d99a7-152">Yükleme [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) her ASP.NET 4.x uygulama paket.</span><span class="sxs-lookup"><span data-stu-id="d99a7-152">Install the [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package into each ASP.NET 4.x app.</span></span>

<span data-ttu-id="d99a7-153">Bulun ve çağrısını değiştirin `UseCookieAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="d99a7-153">Locate and modify the call to `UseCookieAuthentication`:</span></span>

* <span data-ttu-id="d99a7-154">ASP.NET Core tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından kullanılan ad eşleştirilecek tanımlama bilgisi adını değiştirin (`.AspNet.SharedCookie` örnekte).</span><span class="sxs-lookup"><span data-stu-id="d99a7-154">Change the cookie name to match the name used by the ASP.NET Core Cookie Authentication Middleware (`.AspNet.SharedCookie` in the example).</span></span>
* <span data-ttu-id="d99a7-155">Aşağıdaki örnekte, kimlik doğrulaması türünü ayarlamak `Identity.Application`.</span><span class="sxs-lookup"><span data-stu-id="d99a7-155">In the following example, the authentication type is set to `Identity.Application`.</span></span>
* <span data-ttu-id="d99a7-156">Örneği sağlayan bir `DataProtectionProvider` genel veri koruma anahtarı depolama alanı konumuna başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="d99a7-156">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>
* <span data-ttu-id="d99a7-157">Uygulama adı, kimlik doğrulama tanımlama bilgilerini paylaşma tüm uygulamalar tarafından kullanılan ortak uygulama adına ayarlandığından emin olun (`SharedCookieApp` örnekte).</span><span class="sxs-lookup"><span data-stu-id="d99a7-157">Confirm that the app name is set to the common app name used by all apps that share authentication cookies (`SharedCookieApp` in the example).</span></span>

<span data-ttu-id="d99a7-158">Değil ayarlıyorsanız `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` ve `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`ayarlayın <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> benzersiz kullanıcıları ayırt eden bir talep.</span><span class="sxs-lookup"><span data-stu-id="d99a7-158">If not setting `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` and `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, set <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> to a claim that distinguishes unique users.</span></span>

<span data-ttu-id="d99a7-159">*App_Start/Startup.auth.cs*:</span><span class="sxs-lookup"><span data-stu-id="d99a7-159">*App_Start/Startup.Auth.cs*:</span></span>

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

<span data-ttu-id="d99a7-160">Bir kullanıcı kimliği, kimlik doğrulaması türünü oluştururken (`Identity.Application`) tanımlanan türüyle eşleşmelidir `AuthenticationType` kümesi `UseCookieAuthentication` içinde *App_Start/Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="d99a7-160">When generating a user identity, the authentication type (`Identity.Application`) must match the type defined in `AuthenticationType` set with `UseCookieAuthentication` in *App_Start/Startup.Auth.cs*.</span></span>

<span data-ttu-id="d99a7-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="d99a7-161">*Models/IdentityModels.cs*:</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="d99a7-162">Ortak bir kullanıcı veritabanını kullanın</span><span class="sxs-lookup"><span data-stu-id="d99a7-162">Use a common user database</span></span>

<span data-ttu-id="d99a7-163">Aynı kimliğe (kimliğinin aynı sürüm), şema onaylayın kimlik sistemi her uygulama için aynı kullanıcı veritabanına işaret edilen uygulamaları kullandığınızda.</span><span class="sxs-lookup"><span data-stu-id="d99a7-163">When apps use the same Identity schema (same version of Identity), confirm that the Identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="d99a7-164">Aksi takdirde, kendi veritabanındaki bilgileri karşı kimlik doğrulama tanımlama bilgileri eşleşecek şekilde çalıştığında kimlik sistemi hataları zamanında üretir.</span><span class="sxs-lookup"><span data-stu-id="d99a7-164">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

<span data-ttu-id="d99a7-165">Kimlik şema uygulamalar arasında farklı olduğunda, uygulamaları farklı kimlik sürümlerini kullandığından genellikle ortak bir veritabanını en son kimlik sürümüne paylaşımı yeniden eşleme ve diğer uygulamanın kimlik şemaları sütunlar ekleyerek olmadan mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="d99a7-165">When the Identity schema is different among apps, usually because apps are using different Identity versions, sharing a common database based on the latest version of Identity isn't possible without remapping and adding columns in other app's Identity schemas.</span></span> <span data-ttu-id="d99a7-166">Genellikle daha fazla olduğu ortak bir veritabanını uygulamalar tarafından paylaşılabilir böylece en son kimlik sürümünü kullanmak için diğer uygulamaları yükseltmek için etkin.</span><span class="sxs-lookup"><span data-stu-id="d99a7-166">It's often more efficient to upgrade the other apps to use the latest Identity version so that a common database can be shared by the apps.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d99a7-167">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d99a7-167">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
