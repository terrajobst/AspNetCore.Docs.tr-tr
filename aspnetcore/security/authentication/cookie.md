---
title: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullan
author: rick-anderson
description: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullanarak bir açıklama
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: f55b36cf3fc3b60e9d592348625f58ebaba90da7
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570119"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="3fb6e-103">ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullan</span><span class="sxs-lookup"><span data-stu-id="3fb6e-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="3fb6e-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3fb6e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3fb6e-105">Önceki kimlik doğrulaması konularındaki gördüğünüz gibi [ASP.NET Core kimliği](xref:security/authentication/identity) oluşturma ve oturum açma bilgileri korumak için bir tam, tam özellikli kimlik doğrulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="3fb6e-106">Ancak, tanımlama bilgisi tabanlı kimlik doğrulaması ile zaman zaman kendi özel kimlik doğrulama mantığı kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="3fb6e-107">Tanımlama bilgisi tabanlı kimlik doğrulaması, ASP.NET Core kimliği olmadan tek başına kimlik doğrulama sağlayıcısı olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="3fb6e-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3fb6e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3fb6e-109">Örnek uygulamada tanıtım amacıyla kullanıcı kuramsal Maria Rodriguez, kullanıcının uygulamada oturum kodlanmış hesabıdır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="3fb6e-110">E-posta kullanıcı adını kullanın "maria.rodriguez@contoso.com" ve kullanıcı oturum açmak için herhangi bir parola.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="3fb6e-111">Kullanıcının kimliğinin `AuthenticateUser` yönteminde *Pages/Account/Login.cshtml.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="3fb6e-112">Gerçek hayatta kullanılan örnekte, bir veritabanında kullanıcı kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="3fb6e-113">Geçirme tanımlama bilgisi tabanlı kimlik doğrulamasını ASP.NET Core hakkında bilgi için bkz 1.x sürümünden 2.0 sürümüne, [geçirme kimlik doğrulaması ve kimlik için ASP.NET Core 2.0 konu (tanımlama bilgisi tabanlı kimlik doğrulaması)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="3fb6e-114">ASP.NET Core kimliği kullanmak için bkz: [kimliğe giriş](xref:security/authentication/identity) konu.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="3fb6e-115">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3fb6e-115">Configuration</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3fb6e-116">Uygulama kullanmıyorsa [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), proje dosyası için bir paket başvuruya [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paketi (sürüm 2.1.0 veya Daha sonra).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-116">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="3fb6e-117">İçinde `ConfigureServices` yöntemi ile kimlik doğrulaması ara yazılımı hizmeti oluşturma `AddAuthentication` ve `AddCookie` yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-117">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="3fb6e-118">`AuthenticationScheme` geçirilen `AddAuthentication` uygulama için varsayılan kimlik doğrulama şeması ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-118">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="3fb6e-119">`AuthenticationScheme` tanımlama bilgisi kimlik doğrulamasını birden çok örneği vardır ve istediğiniz durumlarda kullanışlıdır [belirli bir düzeni ile yetkilendirme](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-119">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="3fb6e-120">Ayarı `AuthenticationScheme` için `CookieAuthenticationDefaults.AuthenticationScheme` düzeni için "Tanımlama bilgileri" değerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-120">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="3fb6e-121">Düzeni ayırt eden herhangi bir dize değeri sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-121">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="3fb6e-122">Uygulamanın kimlik doğrulama düzeni, uygulamanın tanımlama bilgisi kimlik doğrulaması düzeninden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-122">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="3fb6e-123">Ne zaman bir tanımlama bilgisi kimlik doğrulama düzeni değildir sağlanan <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, kullandığı [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("tanımlama bilgileri").</span><span class="sxs-lookup"><span data-stu-id="3fb6e-123">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="3fb6e-124">İçinde `Configure` yöntemi, kullanım `UseAuthentication` ayarlar kimlik doğrulaması ara yazılımı çağrılacak yöntem `HttpContext.User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-124">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="3fb6e-125">Çağrı `UseAuthentication` yöntemi çağırmadan önce `UseMvcWithDefaultRoute` veya `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-125">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="3fb6e-126">**AddCookie seçenekleri**</span><span class="sxs-lookup"><span data-stu-id="3fb6e-126">**AddCookie Options**</span></span>

<span data-ttu-id="3fb6e-127">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-127">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="3fb6e-128">Seçenek</span><span class="sxs-lookup"><span data-stu-id="3fb6e-128">Option</span></span> | <span data-ttu-id="3fb6e-129">Açıklama</span><span class="sxs-lookup"><span data-stu-id="3fb6e-129">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="3fb6e-130">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="3fb6e-130">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-131">Bir 302 bulundu (URL yeniden yönlendirme) ile sağlamak için yolunu tarafından tetiklendiğinde `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-131">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="3fb6e-132">Varsayılan değer `/Account/AccessDenied` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-132">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="3fb6e-133">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="3fb6e-133">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-134">İçin kullanılan verici [veren](/dotnet/api/system.security.claims.claim.issuer) tanımlama bilgisi kimlik doğrulama hizmeti tarafından oluşturulan herhangi bir talep özelliği.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-134">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="3fb6e-135">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="3fb6e-135">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-136">Tanımlama bilgisi Burada sunulan etki alanı adı.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-136">The domain name where the cookie is served.</span></span> <span data-ttu-id="3fb6e-137">Varsayılan olarak, isteğin ana bilgisayar adı budur.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-137">By default, this is the host name of the request.</span></span> <span data-ttu-id="3fb6e-138">Tarayıcı yalnızca tanımlama bilgisi istekleri için eşleşen bir konak adı gönderir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-138">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="3fb6e-139">Bu etki alanınızda tanımlama bilgilerini herhangi bir konağa kullanılabilir olmasını isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-139">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="3fb6e-140">Örneğin, tanımlama bilgisinin etki alanı ayarlama `.contoso.com` kullanılabilir hale getirir `contoso.com`, `www.contoso.com`, ve `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-140">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="3fb6e-141">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="3fb6e-141">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-142">Alır veya ayarlar bir tanımlama bilgisinin ömrü.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-142">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="3fb6e-143">Şu anda ops Hayır seçeneği ve ASP.NET Core 2.1 + eski hale gelir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-143">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="3fb6e-144">Kullanım `ExpireTimeSpan` tanımlama bilgisi süre sonunu ayarlama seçeneği.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-144">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="3fb6e-145">Daha fazla bilgi için [CookieAuthenticationOptions.Cookie.Expiration davranışını açıklamak](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-145">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="3fb6e-146">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="3fb6e-146">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-147">Tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını belirten bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-147">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="3fb6e-148">Bu değer ile değiştirmek `false` tanımlama bilgisi erişmek için istemci tarafı betikleri verir ve uygulamanızı tanımlama bilgisi hırsızlığı için uygulamanızı olmalıdır açın bir [siteler arası betik (XSS)](xref:security/cross-site-scripting) güvenlik açığı.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-148">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="3fb6e-149">Varsayılan değer `true` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-149">The default value is `true`.</span></span> |
| [<span data-ttu-id="3fb6e-150">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="3fb6e-150">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-151">Tanımlama bilgisinin adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-151">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="3fb6e-152">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="3fb6e-152">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-153">Aynı ana bilgisayar adını çalışan uygulamaları yalıtmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-153">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="3fb6e-154">Çalışan bir uygulama varsa `/app1` ve bu uygulama için tanımlama bilgileri kısıtlamak istediğiniz ayarlamak `CookiePath` özelliğini `/app1`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-154">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="3fb6e-155">Bunu yaptığınızda, tanımlama bilgisinin yalnızca isteklerinde kullanılabilir `/app1` ve bunun altındaki herhangi bir uygulama.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-155">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="3fb6e-156">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="3fb6e-156">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-157">Tarayıcı yalnızca site aynı isteklerine eklenecek tanımlama bilgisinin izin verip vermeyeceğini belirtir (`SameSiteMode.Strict`) veya Güvenli HTTP yöntemleri ve aynı sitede isteklerini kullanarak siteler arası istekleri (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-157">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="3fb6e-158">Ayarlandığında `SameSiteMode.None`, tanımlama bilgisi üstbilgisi değeri ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-158">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="3fb6e-159">Unutmayın [tanımlama bilgisi ilkesi Ara](#cookie-policy-middleware) sağladığınız değerin üzerine yazılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-159">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="3fb6e-160">OAuth kimlik doğrulamasını desteklemek için varsayılan değer: `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-160">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="3fb6e-161">Daha fazla bilgi için [SameSite tanımlama bilgisi ilkesi nedeniyle bozuk OAuth kimlik doğrulaması](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-161">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="3fb6e-162">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="3fb6e-162">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-163">Oluşturulan tanımlama bilgisinin HTTPS için sınırlı olup olmayacağını belirten bir bayrak (`CookieSecurePolicy.Always`), HTTP veya HTTPS (`CookieSecurePolicy.None`), veya istek olarak aynı protokol (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-163">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="3fb6e-164">Varsayılan değer `CookieSecurePolicy.SameAsRequest` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-164">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="3fb6e-165">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="3fb6e-165">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-166">Kümeleri `DataProtectionProvider` varsayılan oluşturmak için kullanılan `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-166">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="3fb6e-167">Varsa `TicketDataFormat` özelliği ayarlanmış `DataProtectionProvider` seçeneği kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-167">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="3fb6e-168">Sağlanmazsa, uygulamanın varsayılan veri koruma sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-168">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="3fb6e-169">Olaylar</span><span class="sxs-lookup"><span data-stu-id="3fb6e-169">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-170">İşleyici, bazı işleme noktalarda uygulama denetimi sunan sağlayıcıda yöntemler çağırır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-170">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="3fb6e-171">Varsa `Events` varsayılan örnek yöntemler çağrıldığında, hiçbir şey yapmaz olarak sağlanır, değildir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-171">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="3fb6e-172">EventsType</span><span class="sxs-lookup"><span data-stu-id="3fb6e-172">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-173">Alınacak hizmet türü kullanılan `Events` örnek özelliği yerine.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-173">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="3fb6e-174">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="3fb6e-174">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-175">`TimeSpan` , Tanımlama bilgisi içinde depolanan kimlik doğrulaması bileti süresi dolduktan sonra.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-175">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="3fb6e-176">`ExpireTimeSpan` anahtar sona erme süresini oluşturmak için geçerli zaman eklenir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-176">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="3fb6e-177">`ExpiredTimeSpan` Değeri her zaman gider ve sunucu tarafından doğrulanan şifrelenmiş AuthTicket.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-177">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="3fb6e-178">Ayrıca içine gidebilir [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) üstbilgisi, ancak yalnızca `IsPersistent` ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-178">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="3fb6e-179">Ayarlanacak `IsPersistent` için `true`, yapılandırma [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) geçirilen `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-179">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="3fb6e-180">Varsayılan değer olan `ExpireTimeSpan` 14 gündür.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-180">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="3fb6e-181">LoginPath</span><span class="sxs-lookup"><span data-stu-id="3fb6e-181">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-182">Bir 302 bulundu (URL yeniden yönlendirme) ile sağlamak için yolunu tarafından tetiklendiğinde `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-182">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="3fb6e-183">401'i oluşturan geçerli URL eklenir `LoginPath` tarafından adlandırılan bir sorgu dizesi parametresi olarak `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-183">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="3fb6e-184">Bir istek için bir kez `LoginPath` yeni bir oturum kimliği, veren `ReturnUrlParameter` değeri, tarayıcının özgün yetkilendirilmemiş durum koduna neden olan URL'ye yeniden yönlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-184">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="3fb6e-185">Varsayılan değer `/Account/Login` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-185">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="3fb6e-186">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="3fb6e-186">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-187">Varsa `LogoutPath` yol bir isteği yeniden yönlendiren işleyicisine belirtilmemesi değerine göre `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-187">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="3fb6e-188">Varsayılan değer `/Account/Logout` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-188">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="3fb6e-189">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="3fb6e-189">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-190">Bir 302 bulundu (URL yeniden yönlendirme) yanıt işleyicisi tarafından eklenen sorgu dizesi parametresinin adını belirler.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-190">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="3fb6e-191">`ReturnUrlParameter` bir istek ulaştığında kullanılan `LoginPath` veya `LogoutPath` oturum açma veya kapatma eylemi gerçekleştirdikten sonra tarayıcının özgün URL'ye geri dönün.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-191">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="3fb6e-192">Varsayılan değer `ReturnUrl` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-192">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="3fb6e-193">SessionStore</span><span class="sxs-lookup"><span data-stu-id="3fb6e-193">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-194">İsteklerdeki kimliklerin depolanacağı için kullanılan isteğe bağlı bir kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-194">An optional container used to store identity across requests.</span></span> <span data-ttu-id="3fb6e-195">Kullanıldığında istemciye yalnızca bir oturum tanımlayıcısı gönderilir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-195">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="3fb6e-196">`SessionStore` büyük kimliklerle olası sorunları gidermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-196">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="3fb6e-197">ilerlemiş</span><span class="sxs-lookup"><span data-stu-id="3fb6e-197">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-198">Yeni bir tanımlama bilgisi güncelleştirilmiş sona erme süresini ile dinamik olarak verilmesi, belirten bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-198">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="3fb6e-199">Bu işlem, % 50'den burada geçerli tanımlama bilgisi süre sonu dönemi süresi doldu herhangi bir istek üzerinde oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-199">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="3fb6e-200">Yeni sona erme tarihi geçerli tarih olmasını İleri taşınır artı `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-200">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="3fb6e-201">Bir [mutlak tanımlama bilgisi süre sonu zamanı](xref:security/authentication/cookie#absolute-cookie-expiration) kullanarak ayarlayabilirsiniz `AuthenticationProperties` sınıfı çağrılırken `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-201">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="3fb6e-202">Mutlak sona erme süresini, kimlik doğrulama tanımlama bilgisinin geçerli olduğu süre miktarını sınırlayarak uygulamanızın güvenliğini artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-202">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="3fb6e-203">Varsayılan değer `true` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-203">The default value is `true`.</span></span> |
| [<span data-ttu-id="3fb6e-204">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="3fb6e-204">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-205">`TicketDataFormat` Korumak ve kimliği ve tanımlama bilgisi değerinde depolanan diğer özellikleri korumasını kaldırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-205">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="3fb6e-206">Sağlanmazsa, bir `TicketDataFormat` kullanılarak oluşturulan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-206">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="3fb6e-207">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="3fb6e-207">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="3fb6e-208">Bu yöntem seçenekleri geçerli olduğunu denetler.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-208">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="3fb6e-209">Ayarlama `CookieAuthenticationOptions` kimlik doğrulaması için hizmet yapılandırmasının `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-209">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3fb6e-210">ASP.NET Core 1.x kullandığı tanımlama bilgisi [ara yazılım](xref:fundamentals/middleware/index) şifrelenmiş bir tanımlama bilgisi, kullanıcı asıl serileştirir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="3fb6e-211">Sonraki isteklerde authenticateasync tanımlama bilgisi doğrulanır ve asıl yeniden ve atanan `HttpContext.User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="3fb6e-212">Yükleme [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet paketini projenize.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="3fb6e-213">Bu paket tanımlama bilgisi Ara içerir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="3fb6e-214">Kullanım `UseCookieAuthentication` yönteminde `Configure` yönteminde, *Startup.cs* önce dosya `UseMvc` veya `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="3fb6e-215">**CookieAuthenticationOptions seçenekleri**</span><span class="sxs-lookup"><span data-stu-id="3fb6e-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="3fb6e-216">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="3fb6e-217">Seçenek</span><span class="sxs-lookup"><span data-stu-id="3fb6e-217">Option</span></span> | <span data-ttu-id="3fb6e-218">Açıklama</span><span class="sxs-lookup"><span data-stu-id="3fb6e-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="3fb6e-219">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="3fb6e-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-220">Kimlik doğrulama şeması ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-220">Sets the authentication scheme.</span></span> <span data-ttu-id="3fb6e-221">`AuthenticationScheme` kimlik doğrulamasının birden çok örneği vardır ve istediğiniz durumlarda kullanışlıdır [belirli bir düzeni ile yetkilendirme](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="3fb6e-222">Ayarı `AuthenticationScheme` için `CookieAuthenticationDefaults.AuthenticationScheme` düzeni için "Tanımlama bilgileri" değerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="3fb6e-223">Düzeni ayırt eden herhangi bir dize değeri sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="3fb6e-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="3fb6e-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-225">Tanımlama bilgisi kimlik doğrulamasını ve her istekte doğrulamak ve herhangi bir seri hale getirilmiş oluşturulduğu asıl yeniden denemesi gerektiğini belirtmek için bir değer ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="3fb6e-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="3fb6e-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-227">TRUE ise, kimlik doğrulaması ara yazılımı otomatik zorlukları işler.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="3fb6e-228">False, kimlik doğrulaması ara yazılımı yalnızca açıkça belirttiği zaman yanıtlarını değiştirir, `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="3fb6e-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="3fb6e-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-230">İçin kullanılan verici [veren](/dotnet/api/system.security.claims.claim.issuer) tanımlama bilgisi kimlik doğrulaması ara yazılımı tarafından oluşturulan herhangi bir talep özelliği.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="3fb6e-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="3fb6e-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-232">Tanımlama bilgisi Burada sunulan etki alanı adı.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="3fb6e-233">Varsayılan olarak, isteğin ana bilgisayar adı budur.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="3fb6e-234">Tarayıcı tanımlama bilgisi eşleşen bir konak adı için yalnızca işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="3fb6e-235">Bu etki alanınızda tanımlama bilgilerini herhangi bir konağa kullanılabilir olmasını isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="3fb6e-236">Örneğin, tanımlama bilgisinin etki alanı ayarlama `.contoso.com` kullanılabilir hale getirir `contoso.com`, `www.contoso.com`, ve `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="3fb6e-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="3fb6e-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-238">Tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını belirten bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="3fb6e-239">Bu değer ile değiştirmek `false` tanımlama bilgisi erişmek için istemci tarafı betikleri verir ve uygulamanızı tanımlama bilgisi hırsızlığı için uygulamanızı olmalıdır açın bir [siteler arası betik (XSS)](xref:security/cross-site-scripting) güvenlik açığı.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="3fb6e-240">Varsayılan değer `true` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="3fb6e-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="3fb6e-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-242">Aynı ana bilgisayar adını çalışan uygulamaları yalıtmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="3fb6e-243">Çalışan bir uygulama varsa `/app1` ve bu uygulama için tanımlama bilgileri kısıtlamak istediğiniz ayarlamak `CookiePath` özelliğini `/app1`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="3fb6e-244">Bunu yaptığınızda, tanımlama bilgisinin yalnızca isteklerinde kullanılabilir `/app1` ve bunun altındaki herhangi bir uygulama.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="3fb6e-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="3fb6e-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-246">Oluşturulan tanımlama bilgisinin HTTPS için sınırlı olup olmayacağını belirten bir bayrak (`CookieSecurePolicy.Always`), HTTP veya HTTPS (`CookieSecurePolicy.None`), veya istek olarak aynı protokol (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="3fb6e-247">Varsayılan değer `CookieSecurePolicy.SameAsRequest` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="3fb6e-248">Açıklama</span><span class="sxs-lookup"><span data-stu-id="3fb6e-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-249">Uygulama için kullanılabilir hale getirileceğini kimlik doğrulama türü hakkında ek bilgi.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="3fb6e-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="3fb6e-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-251">`TimeSpan` Hangi kimlik doğrulama anahtarının süresi dolduktan sonra.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="3fb6e-252">Anahtar sona erme süresini oluşturmak için geçerli zaman eklenir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="3fb6e-253">Kullanılacak `ExpireTimeSpan`, ayarlamalısınız `IsPersistent` için `true` içinde `AuthenticationProperties` geçirilen `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="3fb6e-254">Varsayılan değer 14 gündür.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="3fb6e-255">ilerlemiş</span><span class="sxs-lookup"><span data-stu-id="3fb6e-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="3fb6e-256">Tanımlama bilgisi süre sonu ne zaman birden fazla yarısını sıfırlar olup olmadığını belirten bir bayrak `ExpireTimeSpan` aralığı sonlanana.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="3fb6e-257">Yeni exipiration saati geçerli tarihi olacak şekilde İleri taşınır artı `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="3fb6e-258">Bir [mutlak tanımlama bilgisi süre sonu zamanı](xref:security/authentication/cookie#absolute-cookie-expiration) kullanarak ayarlayabilirsiniz `AuthenticationProperties` sınıfı çağrılırken `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="3fb6e-259">Mutlak sona erme süresini, kimlik doğrulama tanımlama bilgisinin geçerli olduğu süre miktarını sınırlayarak uygulamanızın güvenliğini artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="3fb6e-260">Varsayılan değer `true` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-260">The default value is `true`.</span></span> |

<span data-ttu-id="3fb6e-261">Ayarlama `CookieAuthenticationOptions` tanımlama bilgisi kimlik doğrulaması ara yazılımı `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a><span data-ttu-id="3fb6e-262">Tanımlama bilgisi ilkesi Ara</span><span class="sxs-lookup"><span data-stu-id="3fb6e-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="3fb6e-263">[Tanımlama bilgisi ilkesi Ara](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) bir uygulamada tanımlama bilgisi ilkesi özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="3fb6e-264">Uygulama işleme ardışık düzenine bir ara yazılım ekleme hassas sırasıdır; yalnızca bundan sonra işlem hattı, kayıtlı bileşenleri de etkiler.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="3fb6e-265">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) tanımlama bilgisi ilkesi ara yazılımı için sağlanan tanımlama bilgilerini eklenmiş veya silinmiş tanımlama bilgisi işleme işleyicileri tanımlama bilgisi işleme ve kanca genel özelliklerini denetlemenize izin.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="3fb6e-266">Özellik</span><span class="sxs-lookup"><span data-stu-id="3fb6e-266">Property</span></span> | <span data-ttu-id="3fb6e-267">Açıklama</span><span class="sxs-lookup"><span data-stu-id="3fb6e-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="3fb6e-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="3fb6e-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="3fb6e-269">Tanımlama bilgilerini HttpOnly mi, bayrak tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını gösteren etkiler.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="3fb6e-270">Varsayılan değer `HttpOnlyPolicy.None` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="3fb6e-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="3fb6e-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="3fb6e-272">Tanımlama bilgisinin site aynı özniteliği (aşağıya bakın) etkiler.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="3fb6e-273">Varsayılan değer `SameSiteMode.Lax` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="3fb6e-274">Bu seçenek için ASP.NET Core 2.0 + kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="3fb6e-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="3fb6e-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="3fb6e-276">Bir tanımlama bilgisi eklenir çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="3fb6e-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="3fb6e-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="3fb6e-278">Bir tanımlama bilgisi silindiğinde çağırılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="3fb6e-279">güvenliğini sağlama</span><span class="sxs-lookup"><span data-stu-id="3fb6e-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="3fb6e-280">Tanımlama bilgilerini güvenli mi etkiler.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="3fb6e-281">Varsayılan değer `CookieSecurePolicy.None` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="3fb6e-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + yalnızca)</span><span class="sxs-lookup"><span data-stu-id="3fb6e-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="3fb6e-283">Varsayılan `MinimumSameSitePolicy` değer `SameSiteMode.Lax` OAuth2 kimlik doğrulaması izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="3fb6e-284">Bir site aynı ilke kesinlikle uygulamak `SameSiteMode.Strict`ayarlayın `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="3fb6e-285">Bu ayar, OAuth2 ve diğer kaynaklar arası kimlik doğrulaması şemalarını keser olsa da, başka türden bir çıkış noktaları arası istek işleme güvenmeyin uygulamalar tanımlama bilgisinin güvenlik düzeyini yükseltir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="3fb6e-286">Tanımlama bilgisi ilkesi Ara ayarı `MinimumSameSitePolicy` , ayarı etkileyebilir `Cookie.SameSite` içinde `CookieAuthenticationOptions` aşağıdaki matris göre ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="3fb6e-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="3fb6e-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="3fb6e-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="3fb6e-288">Cookie.SameSite</span></span> | <span data-ttu-id="3fb6e-289">Sonuç Cookie.SameSite ayarı</span><span class="sxs-lookup"><span data-stu-id="3fb6e-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="3fb6e-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3fb6e-290">SameSiteMode.None</span></span>     | <span data-ttu-id="3fb6e-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3fb6e-291">SameSiteMode.None</span></span><br><span data-ttu-id="3fb6e-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3fb6e-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="3fb6e-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3fb6e-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="3fb6e-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3fb6e-294">SameSiteMode.None</span></span><br><span data-ttu-id="3fb6e-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3fb6e-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="3fb6e-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3fb6e-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="3fb6e-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3fb6e-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="3fb6e-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3fb6e-298">SameSiteMode.None</span></span><br><span data-ttu-id="3fb6e-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3fb6e-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="3fb6e-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3fb6e-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="3fb6e-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3fb6e-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="3fb6e-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3fb6e-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="3fb6e-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3fb6e-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="3fb6e-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3fb6e-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="3fb6e-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="3fb6e-305">SameSiteMode.None</span></span><br><span data-ttu-id="3fb6e-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="3fb6e-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="3fb6e-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3fb6e-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="3fb6e-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3fb6e-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="3fb6e-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3fb6e-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="3fb6e-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="3fb6e-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="3fb6e-311">Bir kimlik doğrulama tanımlama bilgisi oluştur</span><span class="sxs-lookup"><span data-stu-id="3fb6e-311">Create an authentication cookie</span></span>

<span data-ttu-id="3fb6e-312">Kullanıcı bilgileri bulunduran bir tanımlama bilgisi oluşturmak için oluşturmalıdır bir [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="3fb6e-313">Kullanıcı bilgileri serileştirilmiş ve tanımlama bilgisinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-313">The user information is serialized and stored in the cookie.</span></span> 

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3fb6e-314">Oluşturma bir [Claimsıdentity](/dotnet/api/system.security.claims.claimsidentity) gerekli ile [talep](/dotnet/api/system.security.claims.claim)s ve çağrı [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) kullanıcının oturum açmak için:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-314">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3fb6e-315">Çağrı [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) kullanıcının oturum açmak için:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-315">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

<span data-ttu-id="3fb6e-316">`SignInAsync` şifrelenmiş bir tanımlama bilgisi oluşturur ve geçerli yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-316">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="3fb6e-317">Belirtmezseniz bir `AuthenticationScheme`, varsayılan düzenini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-317">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="3fb6e-318">Kullanılan şifreleme ASP.NET Core'nın temel alıyor [veri koruma](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistem.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-318">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="3fb6e-319">Birden çok makine, uygulamalar arasında Yük Dengeleme veya bir web grubu kullanarak uygulama düzenliyoruz. sonra yapmanız gerekenler [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) aynı anahtarı halka ve uygulama tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-319">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="3fb6e-320">Oturumu Kapat</span><span class="sxs-lookup"><span data-stu-id="3fb6e-320">Sign out</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3fb6e-321">Geçerli kullanıcının oturumunu kapatmaz ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="3fb6e-321">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3fb6e-322">Geçerli kullanıcının oturumunu kapatmaz ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="3fb6e-322">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

<span data-ttu-id="3fb6e-323">Kullanmıyorsanız `CookieAuthenticationDefaults.AuthenticationScheme` (veya "Tanımlama bilgileri") kimlik doğrulama sağlayıcısı yapılandırırken kullandığınız şeması (örneğin, "ContosoCookie") düzeni olarak sağlayın.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-323">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="3fb6e-324">Aksi takdirde, varsayılan düzenini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-324">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="3fb6e-325">Arka uç değişikliklerine tepki</span><span class="sxs-lookup"><span data-stu-id="3fb6e-325">React to back-end changes</span></span>

<span data-ttu-id="3fb6e-326">Bir tanımlama bilgisi oluşturulduktan sonra kimlik tek kaynağı haline gelir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-326">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="3fb6e-327">Arka uç sistemlerinizi bir kullanıcıyı devre dışı olsa bile, bu bilgi tanımlama bilgisi kimlik doğrulama sistemi vardır ve kendi tanımlama bilgisinin geçerli olduğu sürece bir kullanıcı olarak oturum açmış kalır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-327">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="3fb6e-328">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) olayı ASP.NET Core 2.x veya [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) yöntemi ASP.NET Core 1.x kesebilir ve tanımlama bilgisi kimlik doğrulaması geçersiz kılmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-328">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="3fb6e-329">Bu yaklaşım, iptal edilen kullanıcıların uygulamaya erişmeden riskini azaltır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-329">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="3fb6e-330">Tanımlama bilgisi doğrulama için bir yaklaşım, kullanıcı veritabanına değiştirildiğinde izleyen üzerinde temel alır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-330">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="3fb6e-331">Kullanıcının tanımlama bilgisi düzenlendiğinden veritabanının değişip değişmediğini, kendi tanımlama bilgisini hala geçerli ise kullanıcının yeniden kimlik doğrulaması gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-331">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="3fb6e-332">Bu senaryo, uygulanan veritabanı uygulamak için `IUserRepository` depolar Bu örnekte, bir `LastChanged` değeri.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-332">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="3fb6e-333">Herhangi bir kullanıcı veritabanında güncelleştirildiğinde `LastChanged` değeri, geçerli saate ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-333">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="3fb6e-334">Veritabanı değişikliklerini temel alan bir tanımlama bilgisi geçersiz kılmak için `LastChanged` değeri, tanımlama bilgisi ile oluşturma bir `LastChanged` geçerli içeren talep `LastChanged` veritabanından değer:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-334">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3fb6e-335">Uygulama için bir geçersiz kılma `ValidatePrincipal` olay, bir yöntemin öğesinden türetilen bir sınıfta imzayla yazma [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="3fb6e-335">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="3fb6e-336">Bir örnek, aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-336">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="3fb6e-337">Tanımlama bilgisi hizmet kaydı sırasında olayları örneğini kaydetme `ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-337">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="3fb6e-338">Kapsamlı hizmeti kaydı için sağlayın, `CustomCookieAuthenticationEvents` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-338">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3fb6e-339">Uygulama için bir geçersiz kılma `ValidateAsync` olay, bir yöntemin imzayla yazma:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-339">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="3fb6e-340">ASP.NET Core kimliği bir parçası olarak bu onay uygulayan kendi [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="3fb6e-340">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="3fb6e-341">Bir örnek, aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-341">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="3fb6e-342">Tanımlama bilgisi kimlik doğrulaması yapılandırması sırasında olay kaydetme `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3fb6e-342">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

<span data-ttu-id="3fb6e-343">Kullanıcının name güncelleştirildiği bir durum düşünün &mdash; karar güvenlik herhangi bir şekilde etkilemez.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-343">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="3fb6e-344">Kullanıcı asıl adı dönüşlü güncelleştirmek istiyorsanız, çağrı `context.ReplacePrincipal` ayarlayıp `context.ShouldRenew` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-344">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="3fb6e-345">Burada açıklanan yaklaşımı, her istek tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-345">The approach described here is triggered on every request.</span></span> <span data-ttu-id="3fb6e-346">Bu uygulama için bir büyük bir performans sorununa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-346">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="3fb6e-347">Kalıcı tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="3fb6e-347">Persistent cookies</span></span>

<span data-ttu-id="3fb6e-348">Tarayıcı oturumlarında kalıcı hale getirmek için tanımlama bilgisi isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-348">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="3fb6e-349">Oturum açma veya benzer bir mekanizma "Beni Hatırla" onay kutusu açık kullanıcı onayı ile yalnızca bu Kalıcılık etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-349">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on login or a similar mechanism.</span></span> 

<span data-ttu-id="3fb6e-350">Aşağıdaki kod parçacığı, bir kimlik ve tarayıcı kapanışlar şekilde kalmıştır ilgili tanımlama bilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-350">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="3fb6e-351">Daha önce yapılandırılmış tüm kayan zaman aşımı ayarları dikkate alınır.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-351">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="3fb6e-352">Tarayıcı kapalıyken tanımlama bilgisi süresi dolarsa, yeniden başlatıldıktan sonra tarayıcı tanımlama bilgisi temizler.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-352">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="3fb6e-353">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) sınıfı bulunduğu `Microsoft.AspNetCore.Authentication` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-353">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="3fb6e-354">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) sınıfı bulunduğu `Microsoft.AspNetCore.Http.Authentication` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-354">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

::: moniker-end

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="3fb6e-355">Mutlak tanımlama bilgisi süre sonu</span><span class="sxs-lookup"><span data-stu-id="3fb6e-355">Absolute cookie expiration</span></span>

<span data-ttu-id="3fb6e-356">İle mutlak sona erme süresini ayarlayabilirsiniz `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-356">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="3fb6e-357">Ayrıca ayarlamanız gerekir `IsPersistent`; Aksi takdirde `ExpiresUtc` göz ardı edilir ve tek oturum tanımlama bilgisi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-357">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="3fb6e-358">Zaman `ExpiresUtc` ayarlanır `SignInAsync`, değerini geçersiz kılar `ExpireTimeSpan` seçeneği `CookieAuthenticationOptions`, ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-358">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="3fb6e-359">Aşağıdaki kod parçacığı, bir kimlik ve 20 dakika sürer ilgili tanımlama bilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-359">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="3fb6e-360">Bu, daha önce yapılandırılmış tüm kayan zaman aşımı ayarları yok sayar.</span><span class="sxs-lookup"><span data-stu-id="3fb6e-360">This ignores any sliding expiration settings previously configured.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3fb6e-361">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3fb6e-361">Additional resources</span></span>

* [<span data-ttu-id="3fb6e-362">Kimlik doğrulama 2.0 değişiklikleri / geçiş Duyurusu</span><span class="sxs-lookup"><span data-stu-id="3fb6e-362">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="3fb6e-363">İlke tabanlı rol denetimleri</span><span class="sxs-lookup"><span data-stu-id="3fb6e-363">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
