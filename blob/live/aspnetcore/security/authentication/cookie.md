---
title: "ASP.NET Core kimlik olmadan tanımlama bilgisi kimlik doğrulamasını kullanma"
author: rick-anderson
description: "ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullanarak bir açıklama"
keywords: "ASP.NET Core, tanımlama bilgileri"
ms.author: riande
manager: wpickett
ms.date: 10/11/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: 6279d3b4ac3be102449089dc66eeeb0495cfc4c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="91cca-104">ASP.NET Core kimlik olmadan tanımlama bilgisi kimlik doğrulamasını kullanma</span><span class="sxs-lookup"><span data-stu-id="91cca-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="91cca-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="91cca-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="91cca-106">Önceki kimlik doğrulaması konularında gördüğünüz gibi [ASP.NET Core kimliği](xref:security/authentication/identity) oluşturmak ve oturum açma bilgileri korumak için bir tam, tam özellikli kimlik doğrulaması sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="91cca-106">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="91cca-107">Ancak, tanımlama bilgisi tabanlı kimlik doğrulaması ile zaman zaman kendi özel kimlik doğrulama mantığı kullanmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91cca-107">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="91cca-108">Tanımlama bilgisi tabanlı kimlik doğrulaması, ASP.NET Core kimliği olmadan tek başına kimlik doğrulama sağlayıcısı olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91cca-108">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="91cca-109">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="91cca-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="91cca-110">ASP.NET Core geçirme tanımlama bilgisi tabanlı kimlik hakkında bilgi için 1.x 2.0 için bkz: [geçirme kimlik doğrulama ve ASP.NET Core 2.0 konuya (tanımlama bilgisi tabanlı kimlik doğrulaması) kimlik](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="91cca-110">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrating Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="91cca-111">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91cca-111">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="91cca-112">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="91cca-113">Kullanmıyorsanız [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), 2.0 + sürümü yüklemek [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="91cca-113">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="91cca-114">İçinde `ConfigureServices` yöntemi ile kimlik doğrulaması ara yazılımı hizmet oluşturma `AddAuthentication` ve `AddCookie` yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="91cca-114">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="91cca-115">`AuthenticationScheme`geçirilen `AddAuthentication` uygulama için varsayılan kimlik doğrulama şeması ayarlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-115">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="91cca-116">`AuthenticationScheme`tanımlama bilgisi kimlik doğrulamasını birden çok örneği vardır ve istediğiniz kullanışlıdır [belirli düzeniyle yetkilendirmek](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="91cca-116">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="91cca-117">Ayarı `AuthenticationScheme` için `CookieAuthenticationDefaults.AuthenticationScheme` düzeni için "Tanımlama bilgileri" değerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-117">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="91cca-118">Düzen ayırt herhangi bir dize değeri sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="91cca-118">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="91cca-119">İçinde `Configure` yöntemi, kullanım `UseAuthentication` ayarlar kimlik doğrulaması ara yazılımı çağrılacak yöntem `HttpContext.User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="91cca-119">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="91cca-120">Çağrı `UseAuthentication` yöntemi çağırmadan önce `AddMvcWithDefaultRoute` bir MVC uygulamasında veya `AddMvc` bir Razor sayfalarının uygulamasında:</span><span class="sxs-lookup"><span data-stu-id="91cca-120">Call the `UseAuthentication` method before calling `AddMvcWithDefaultRoute` in an MVC app or `AddMvc` in a Razor Pages app:</span></span>

[!code-csharp[Main](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="91cca-121">**AddCookie seçenekleri**</span><span class="sxs-lookup"><span data-stu-id="91cca-121">**AddCookie Options**</span></span>

<span data-ttu-id="91cca-122">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-122">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="91cca-123">Seçenek</span><span class="sxs-lookup"><span data-stu-id="91cca-123">Option</span></span> | <span data-ttu-id="91cca-124">Açıklama</span><span class="sxs-lookup"><span data-stu-id="91cca-124">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="91cca-125">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="91cca-125">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="91cca-126">302 bulundu (URL yeniden yönlendirme) ile sağlamak için yol sağlayan tarafından tetiklendiğinde `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="91cca-126">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="91cca-127">Varsayılan değer `/Account/AccessDenied` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-127">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="91cca-128">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="91cca-128">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="91cca-129">İçin kullanılan verici [veren](/dotnet/api/system.security.claims.claim.issuer) tanımlama bilgisi kimlik doğrulama hizmeti tarafından oluşturulan herhangi bir talep özelliği.</span><span class="sxs-lookup"><span data-stu-id="91cca-129">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="91cca-130">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="91cca-130">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="91cca-131">Tanımlama bilgisinin Burada sunulan etki alanı adı.</span><span class="sxs-lookup"><span data-stu-id="91cca-131">The domain name where the cookie is served.</span></span> <span data-ttu-id="91cca-132">Varsayılan olarak, istek ana bilgisayar adını budur.</span><span class="sxs-lookup"><span data-stu-id="91cca-132">By default, this is the host name of the request.</span></span> <span data-ttu-id="91cca-133">Tarayıcı eşleşen bir ana bilgisayar adı tanımlama bilgisinin yalnızca istekleri gönderir.</span><span class="sxs-lookup"><span data-stu-id="91cca-133">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="91cca-134">Etki alanınızda tanımlama bilgilerini herhangi bir ana bilgisayara kullanılabilir olması için bunu ayarlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91cca-134">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="91cca-135">Örneğin, tanımlama bilgisi etki alanını ayarlamak `.contoso.com` için kullanılabilir hale getirir `contoso.com`, `www.contoso.com`, ve `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="91cca-135">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="91cca-136">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="91cca-136">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="91cca-137">Alır veya bir tanımlama bilgisi Sysprep'in ayarlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-137">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="91cca-138">Şu anda Hayır ops seçeneği ve ASP.NET çekirdek 2.1 + geçersiz hale gelir.</span><span class="sxs-lookup"><span data-stu-id="91cca-138">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="91cca-139">Kullanım `ExpireTimeSpan` tanımlama bilgisi bitiş tarihini ayarlamak için seçeneği.</span><span class="sxs-lookup"><span data-stu-id="91cca-139">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="91cca-140">Daha fazla bilgi için bkz: [CookieAuthenticationOptions.Cookie.Expiration davranışını açıklamak](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="91cca-140">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="91cca-141">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="91cca-141">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="91cca-142">Tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını belirten bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="91cca-142">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="91cca-143">Bu değer ile değiştirmek `false` tanımlama bilgisinin erişmek için istemci tarafı betikler seçmenize izin verir ve tanımlama bilgisi hırsızlığı uygulamanıza olmalıdır, uygulamanızın açılabilir bir [siteler arası komut dosyası (XSS)](xref:security/cross-site-scripting) güvenlik açığı.</span><span class="sxs-lookup"><span data-stu-id="91cca-143">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="91cca-144">Varsayılan değer `true` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-144">The default value is `true`.</span></span> |
| [<span data-ttu-id="91cca-145">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="91cca-145">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="91cca-146">Tanımlama bilgisinin adını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-146">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="91cca-147">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="91cca-147">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="91cca-148">Aynı ana bilgisayar adını çalışan uygulamalar yalıtmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-148">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="91cca-149">Konumunda çalışan bir uygulamanız varsa `/app1` ve bu uygulama için tanımlama bilgilerini kısıtlamak istediğiniz ayarlamak `CookiePath` özelliğine `/app1`.</span><span class="sxs-lookup"><span data-stu-id="91cca-149">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="91cca-150">Bunu yaparak, tanımlama bilgisinin yalnızca isteklerinde kullanılabilir `/app1` ve bunun altındaki herhangi bir uygulama.</span><span class="sxs-lookup"><span data-stu-id="91cca-150">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="91cca-151">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="91cca-151">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="91cca-152">Tarayıcı yalnızca aynı sitede isteklerine eklenecek tanımlama bilgisinin izin verip vermeyeceğini belirtir (`SameSiteMode.Strict`) veya Güvenli HTTP yöntemleri ve aynı sitede isteklerini kullanarak siteler arası istek (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="91cca-152">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="91cca-153">Ayarlandığında `SameSiteMode.None`, tanımlama bilgisi üstbilgisi değeri ayarlanmamış.</span><span class="sxs-lookup"><span data-stu-id="91cca-153">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="91cca-154">Unutmayın [tanımlama bilgisi ilke Ara](#cookie-policy-middleware) sağladığınız değerin üzerine yazılmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="91cca-154">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="91cca-155">OAuth kimlik doğrulamasını desteklemek için varsayılan değer: `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="91cca-155">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="91cca-156">Daha fazla bilgi için bkz: [SameSite tanımlama bilgisi ilkesi nedeniyle bozuk OAuth kimlik doğrulaması](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="91cca-156">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="91cca-157">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="91cca-157">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="91cca-158">Oluşturulan tanımlama bilgisinin HTTPS için sınırlı olup olmayacağını belirten bir bayrak (`CookieSecurePolicy.Always`), HTTP veya HTTPS (`CookieSecurePolicy.None`), ya da istek olarak aynı protokol (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="91cca-158">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="91cca-159">Varsayılan değer `CookieSecurePolicy.SameAsRequest` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-159">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="91cca-160">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="91cca-160">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="91cca-161">Ayarlar `DataProtectionProvider` varsayılan oluşturmak için kullanılan `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="91cca-161">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="91cca-162">Varsa `TicketDataFormat` özelliği ayarlanmış `DataProtectionProvider` değil seçenek kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-162">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="91cca-163">Sağlanmazsa, uygulamanın varsayılan veri koruma sağlayıcısı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-163">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="91cca-164">Olayları</span><span class="sxs-lookup"><span data-stu-id="91cca-164">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="91cca-165">İşleyicinin belirli işleme noktalarda uygulama denetime sağlayıcısı yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="91cca-165">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="91cca-166">Varsa `Events` varsayılan örnek yöntem çağrıldığında, hiçbir şey yapmaz sağlanır değil.</span><span class="sxs-lookup"><span data-stu-id="91cca-166">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="91cca-167">EventsType</span><span class="sxs-lookup"><span data-stu-id="91cca-167">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="91cca-168">Hizmet türü olarak almak için kullanılan `Events` özelliği yerine örneği.</span><span class="sxs-lookup"><span data-stu-id="91cca-168">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="91cca-169">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="91cca-169">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="91cca-170">`TimeSpan` Hangi tanımlama bilgisi içinde depolanan kimlik doğrulaması biletinin süresinin sonra.</span><span class="sxs-lookup"><span data-stu-id="91cca-170">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="91cca-171">`ExpireTimeSpan`anahtar için kullanım süresi sonu oluşturmak için geçerli süre eklenir.</span><span class="sxs-lookup"><span data-stu-id="91cca-171">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="91cca-172">`ExpiredTimeSpan` Değeri her zaman gider sunucu tarafından doğrulanan şifrelenmiş AuthTicket içine.</span><span class="sxs-lookup"><span data-stu-id="91cca-172">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="91cca-173">Ayrıca içine gidebilir [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) üstbilgisi, ancak yalnızca `IsPersistent` ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="91cca-173">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="91cca-174">Ayarlamak için `IsPersistent` için `true`, yapılandırma [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) geçirilen `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="91cca-174">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="91cca-175">Varsayılan değer olan `ExpireTimeSpan` 14 gündür.</span><span class="sxs-lookup"><span data-stu-id="91cca-175">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="91cca-176">LoginPath</span><span class="sxs-lookup"><span data-stu-id="91cca-176">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="91cca-177">302 bulundu (URL yeniden yönlendirme) ile sağlamak için yol sağlayan tarafından tetiklendiğinde `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="91cca-177">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="91cca-178">401'i oluşturan geçerli URL eklenen `LoginPath` tarafından adlı bir sorgu dizesi parametresi olarak `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="91cca-178">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="91cca-179">Bir istek için bir kez `LoginPath` yeni bir oturum açma kimliği, veren `ReturnUrlParameter` değeri, tarayıcının özgün yetkilendirilmemiş durum koduna neden olan URL'ye yeniden yönlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-179">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="91cca-180">Varsayılan değer `/Account/Login` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-180">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="91cca-181">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="91cca-181">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="91cca-182">Varsa `LogoutPath` bu yol için bir istek yönlendiren sonra işleyicisine sağlanan değerine göre `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="91cca-182">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="91cca-183">Varsayılan değer `/Account/Logout` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-183">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="91cca-184">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="91cca-184">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="91cca-185">302 bulundu (URL yeniden yönlendirme) yanıt işleyici tarafından eklenen sorgu dizesi parametresinin adını belirler.</span><span class="sxs-lookup"><span data-stu-id="91cca-185">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="91cca-186">`ReturnUrlParameter`bir istek ulaştığında kullanılan `LoginPath` veya `LogoutPath` tarayıcı oturum açma veya oturum kapatma eylem gerçekleştirildikten sonra özgün URL'ye geri dönmek için.</span><span class="sxs-lookup"><span data-stu-id="91cca-186">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="91cca-187">Varsayılan değer `ReturnUrl` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-187">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="91cca-188">SessionStore</span><span class="sxs-lookup"><span data-stu-id="91cca-188">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="91cca-189">İsteklerinde kimlik depolamak için kullanılan isteğe bağlı bir kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="91cca-189">An optional container used to store identity across requests.</span></span> <span data-ttu-id="91cca-190">Kullanıldığında istemciye yalnızca bir oturum tanımlayıcısı gönderilir.</span><span class="sxs-lookup"><span data-stu-id="91cca-190">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="91cca-191">`SessionStore`büyük kimlikleri ile ilgili olası sorunları hafifletmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="91cca-191">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="91cca-192">SlidingExpiration değeri</span><span class="sxs-lookup"><span data-stu-id="91cca-192">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="91cca-193">Yeni bir tanımlama bilgisi güncelleştirilmiş süre sonu zamanı ile dinamik olarak verilmesi olmadığını belirten bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="91cca-193">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="91cca-194">Bu, geçerli tanımlama bilgisi sona erme süresi % 50'den fazla burada sona herhangi bir istek üzerinde meydana gelebilir.</span><span class="sxs-lookup"><span data-stu-id="91cca-194">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="91cca-195">Yeni sona erme tarihi geçerli tarih olarak İleri taşınır artı `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="91cca-195">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="91cca-196">Bir [mutlak tanımlama bilgisinin süre sonu zamanı](xref:security/authentication/cookie#absolute-cookie-expiration) kullanarak ayarlayabilirsiniz `AuthenticationProperties` sınıf çağrılırken `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="91cca-196">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="91cca-197">Mutlak zaman aşımı süresi, kimlik doğrulama tanımlama bilgisinin geçerli olduğu süre miktarını sınırlayarak, uygulamanızın güvenliğini artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91cca-197">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="91cca-198">Varsayılan değer `true` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-198">The default value is `true`.</span></span> |
| [<span data-ttu-id="91cca-199">Ticketdataformat öğesi</span><span class="sxs-lookup"><span data-stu-id="91cca-199">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="91cca-200">`TicketDataFormat` Korumak ve kimliği ve tanımlama bilgisi değerinde depolanan diğer özellikleri korumasını kaldırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-200">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="91cca-201">Sağlanmazsa, bir `TicketDataFormat` kullanılarak oluşturulan [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="91cca-201">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="91cca-202">Doğrula</span><span class="sxs-lookup"><span data-stu-id="91cca-202">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="91cca-203">Seçenekleri geçerli olduğunu denetler yöntemi.</span><span class="sxs-lookup"><span data-stu-id="91cca-203">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="91cca-204">Ayarlama `CookieAuthenticationOptions` kimlik doğrulaması için hizmet yapılandırmasının `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="91cca-204">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="91cca-205">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="91cca-206">ASP.NET Core 1.x kullanır tanımlama bilgisi [ara yazılım](xref:fundamentals/middleware) şifrelenmiş bir tanımlama bilgisi, kullanıcı asıl serileştirir.</span><span class="sxs-lookup"><span data-stu-id="91cca-206">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="91cca-207">Sonraki isteklerde tanımlama bilgisinin doğrulanır ve asıl yeniden ve atanan `HttpContext.User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="91cca-207">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="91cca-208">Yükleme [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet paketini projenize.</span><span class="sxs-lookup"><span data-stu-id="91cca-208">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="91cca-209">Bu paket tanımlama bilgisi Ara içerir.</span><span class="sxs-lookup"><span data-stu-id="91cca-209">This package contains the cookie middleware.</span></span>

<span data-ttu-id="91cca-210">Kullanım `UseCookieAuthentication` yönteminde `Configure` yönteminde, *haline* önce dosya `UseMvc` veya `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="91cca-210">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="91cca-211">**CookieAuthenticationOptions seçenekleri**</span><span class="sxs-lookup"><span data-stu-id="91cca-211">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="91cca-212">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-212">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="91cca-213">Seçenek</span><span class="sxs-lookup"><span data-stu-id="91cca-213">Option</span></span> | <span data-ttu-id="91cca-214">Açıklama</span><span class="sxs-lookup"><span data-stu-id="91cca-214">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="91cca-215">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="91cca-215">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="91cca-216">Kimlik doğrulama şeması ayarlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-216">Sets the authentication scheme.</span></span> <span data-ttu-id="91cca-217">`AuthenticationScheme`kimlik doğrulamasının birden çok örneği vardır ve istediğiniz kullanışlıdır [belirli düzeniyle yetkilendirmek](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="91cca-217">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="91cca-218">Ayarı `AuthenticationScheme` için `CookieAuthenticationDefaults.AuthenticationScheme` düzeni için "Tanımlama bilgileri" değerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-218">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="91cca-219">Düzen ayırt herhangi bir dize değeri sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="91cca-219">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="91cca-220">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="91cca-220">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="91cca-221">Tanımlama bilgisi kimlik doğrulamasını ve her istekte doğrulamak ve herhangi bir seri hale getirilmiş oluşturulduğu asıl yeniden denemesi gerektiğini belirtmek için bir değer ayarlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-221">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="91cca-222">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="91cca-222">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="91cca-223">TRUE ise, kimlik doğrulaması ara yazılımı otomatik zorluklar işler.</span><span class="sxs-lookup"><span data-stu-id="91cca-223">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="91cca-224">Yanlış kimlik doğrulaması ara yazılımı yalnızca açıkça belirtildiği zaman yanıtları değiştirir, `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="91cca-224">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="91cca-225">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="91cca-225">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="91cca-226">İçin kullanılan verici [veren](/dotnet/api/system.security.claims.claim.issuer) tanımlama bilgisi kimlik doğrulaması ara yazılım tarafından oluşturulan herhangi bir talep özelliği.</span><span class="sxs-lookup"><span data-stu-id="91cca-226">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="91cca-227">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="91cca-227">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="91cca-228">Tanımlama bilgisinin Burada sunulan etki alanı adı.</span><span class="sxs-lookup"><span data-stu-id="91cca-228">The domain name where the cookie is served.</span></span> <span data-ttu-id="91cca-229">Varsayılan olarak, istek ana bilgisayar adını budur.</span><span class="sxs-lookup"><span data-stu-id="91cca-229">By default, this is the host name of the request.</span></span> <span data-ttu-id="91cca-230">Tarayıcı tanımlama bilgisi eşleşen bir ana bilgisayar adı yalnızca işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="91cca-230">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="91cca-231">Etki alanınızda tanımlama bilgilerini herhangi bir ana bilgisayara kullanılabilir olması için bunu ayarlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91cca-231">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="91cca-232">Örneğin, tanımlama bilgisi etki alanını ayarlamak `.contoso.com` için kullanılabilir hale getirir `contoso.com`, `www.contoso.com`, ve `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="91cca-232">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="91cca-233">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="91cca-233">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="91cca-234">Tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını belirten bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="91cca-234">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="91cca-235">Bu değer ile değiştirmek `false` tanımlama bilgisinin erişmek için istemci tarafı betikler seçmenize izin verir ve tanımlama bilgisi hırsızlığı uygulamanıza olmalıdır, uygulamanızın açılabilir bir [siteler arası komut dosyası (XSS)](xref:security/cross-site-scripting) güvenlik açığı.</span><span class="sxs-lookup"><span data-stu-id="91cca-235">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="91cca-236">Varsayılan değer `true` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-236">The default value is `true`.</span></span> |
| [<span data-ttu-id="91cca-237">CookiePath</span><span class="sxs-lookup"><span data-stu-id="91cca-237">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="91cca-238">Aynı ana bilgisayar adını çalışan uygulamalar yalıtmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-238">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="91cca-239">Konumunda çalışan bir uygulamanız varsa `/app1` ve bu uygulama için tanımlama bilgilerini kısıtlamak istediğiniz ayarlamak `CookiePath` özelliğine `/app1`.</span><span class="sxs-lookup"><span data-stu-id="91cca-239">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="91cca-240">Bunu yaparak, tanımlama bilgisinin yalnızca isteklerinde kullanılabilir `/app1` ve bunun altındaki herhangi bir uygulama.</span><span class="sxs-lookup"><span data-stu-id="91cca-240">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="91cca-241">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="91cca-241">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="91cca-242">Oluşturulan tanımlama bilgisinin HTTPS için sınırlı olup olmayacağını belirten bir bayrak (`CookieSecurePolicy.Always`), HTTP veya HTTPS (`CookieSecurePolicy.None`), ya da istek olarak aynı protokol (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="91cca-242">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="91cca-243">Varsayılan değer `CookieSecurePolicy.SameAsRequest` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-243">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="91cca-244">Açıklama</span><span class="sxs-lookup"><span data-stu-id="91cca-244">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="91cca-245">Uygulama için kullanılabilir hale getirileceğini kimlik doğrulama türü hakkında ek bilgi.</span><span class="sxs-lookup"><span data-stu-id="91cca-245">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="91cca-246">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="91cca-246">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="91cca-247">`TimeSpan` Hangi kimlik doğrulama anahtarının süresi dolduktan sonra.</span><span class="sxs-lookup"><span data-stu-id="91cca-247">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="91cca-248">Anahtar için kullanım süresi sonu oluşturmak için geçerli süre eklenir.</span><span class="sxs-lookup"><span data-stu-id="91cca-248">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="91cca-249">Kullanılacak `ExpireTimeSpan`, ayarlamalısınız `IsPersistent` için `true` içinde `AuthenticationProperties` geçirilen `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="91cca-249">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="91cca-250">Varsayılan değer 14 gündür.</span><span class="sxs-lookup"><span data-stu-id="91cca-250">The default value is 14 days.</span></span> |
| [<span data-ttu-id="91cca-251">SlidingExpiration değeri</span><span class="sxs-lookup"><span data-stu-id="91cca-251">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="91cca-252">Tanımlama bilgisi sona erme tarihi ne zaman birden fazla yarısı sıfırlar olup olmadığını belirten bir bayrak `ExpireTimeSpan` aralığı geçtikten.</span><span class="sxs-lookup"><span data-stu-id="91cca-252">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="91cca-253">Yeni exipiration saati geçerli tarih olması İleri taşınır artı `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="91cca-253">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="91cca-254">Bir [mutlak tanımlama bilgisinin süre sonu zamanı](xref:security/authentication/cookie#absolute-cookie-expiration) kullanarak ayarlayabilirsiniz `AuthenticationProperties` sınıf çağrılırken `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="91cca-254">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="91cca-255">Mutlak zaman aşımı süresi, kimlik doğrulama tanımlama bilgisinin geçerli olduğu süre miktarını sınırlayarak, uygulamanızın güvenliğini artırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91cca-255">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="91cca-256">Varsayılan değer `true` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-256">The default value is `true`.</span></span> |

<span data-ttu-id="91cca-257">Ayarlama `CookieAuthenticationOptions` tanımlama bilgisi kimlik doğrulaması ara için `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="91cca-257">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="91cca-258">Tanımlama bilgisi ilke Ara</span><span class="sxs-lookup"><span data-stu-id="91cca-258">Cookie Policy Middleware</span></span>

<span data-ttu-id="91cca-259">[Tanımlama bilgisi ilke Ara](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) bir uygulamada tanımlama bilgisi ilkesi özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-259">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="91cca-260">Ara yazılım uygulama işleme ardışık düzenine ekleme hassas sıradır; yalnızca ardışık düzeninde sonra kayıtlı bileşenleri de etkiler.</span><span class="sxs-lookup"><span data-stu-id="91cca-260">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="91cca-261">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) tanımlama bilgisi ilke Ara sağlanan, tanımlama bilgilerini eklenmiş veya silinmiş tanımlama bilgisi işleme ve kanca genel özelliklerini tanımlama bilgisi işleme işleyicileri denetlemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-261">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="91cca-262">Özellik</span><span class="sxs-lookup"><span data-stu-id="91cca-262">Property</span></span> | <span data-ttu-id="91cca-263">Açıklama</span><span class="sxs-lookup"><span data-stu-id="91cca-263">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="91cca-264">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="91cca-264">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="91cca-265">Tanımlama bilgilerini HttpOnly olmalıdır olup, bir bayrak tanımlama bilgisinin yalnızca sunucular için erişilebilir olup olmayacağını gösteren etkiler.</span><span class="sxs-lookup"><span data-stu-id="91cca-265">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="91cca-266">Varsayılan değer `HttpOnlyPolicy.None` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-266">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="91cca-267">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="91cca-267">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="91cca-268">Tanımlama bilgisinin site aynı öznitelik (aşağıya bakın) etkiler.</span><span class="sxs-lookup"><span data-stu-id="91cca-268">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="91cca-269">Varsayılan değer `SameSiteMode.Lax` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-269">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="91cca-270">Bu seçenek için ASP.NET Core 2.0 + kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="91cca-270">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="91cca-271">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="91cca-271">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="91cca-272">Bir tanımlama bilgisi eklenir çağrılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-272">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="91cca-273">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="91cca-273">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="91cca-274">Bir tanımlama bilgisi silindiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-274">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="91cca-275">Güvenli</span><span class="sxs-lookup"><span data-stu-id="91cca-275">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="91cca-276">Tanımlama bilgilerini güvenli mi etkiler.</span><span class="sxs-lookup"><span data-stu-id="91cca-276">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="91cca-277">Varsayılan değer `CookieSecurePolicy.None` şeklindedir.</span><span class="sxs-lookup"><span data-stu-id="91cca-277">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="91cca-278">**MinimumSameSitePolicy** (ASP.NET çekirdek 2.0 + yalnızca)</span><span class="sxs-lookup"><span data-stu-id="91cca-278">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="91cca-279">Varsayılan `MinimumSameSitePolicy` değer `SameSiteMode.Lax` OAuth2 kimlik doğrulama izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="91cca-279">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="91cca-280">Bir site aynı ilke kesinlikle zorlamak için `SameSiteMode.Strict`ayarlayın `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="91cca-280">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="91cca-281">Bu ayar OAuth2 ve diğer çıkış noktaları arası kimlik doğrulama şemasını keser rağmen çıkış noktaları arası istek işleme güvenmeyin uygulamalarının diğer türleri tanımlama bilgisinin güvenlik düzeyini yükseltir.</span><span class="sxs-lookup"><span data-stu-id="91cca-281">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="91cca-282">Tanımlama bilgisi ilke Ara ayarını `MinimumSameSitePolicy` , ayarı etkileyebilir `Cookie.SameSite` içinde `CookieAuthenticationOptions` matris göre ayarlar.</span><span class="sxs-lookup"><span data-stu-id="91cca-282">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="91cca-283">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="91cca-283">MinimumSameSitePolicy</span></span> | <span data-ttu-id="91cca-284">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="91cca-284">Cookie.SameSite</span></span> | <span data-ttu-id="91cca-285">Sonuç Cookie.SameSite ayarı</span><span class="sxs-lookup"><span data-stu-id="91cca-285">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="91cca-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="91cca-286">SameSiteMode.None</span></span>     | <span data-ttu-id="91cca-287">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="91cca-287">SameSiteMode.None</span></span><br><span data-ttu-id="91cca-288">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="91cca-288">SameSiteMode.Lax</span></span><br><span data-ttu-id="91cca-289">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="91cca-289">SameSiteMode.Strict</span></span> | <span data-ttu-id="91cca-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="91cca-290">SameSiteMode.None</span></span><br><span data-ttu-id="91cca-291">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="91cca-291">SameSiteMode.Lax</span></span><br><span data-ttu-id="91cca-292">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="91cca-292">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="91cca-293">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="91cca-293">SameSiteMode.Lax</span></span>      | <span data-ttu-id="91cca-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="91cca-294">SameSiteMode.None</span></span><br><span data-ttu-id="91cca-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="91cca-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="91cca-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="91cca-296">SameSiteMode.Strict</span></span> | <span data-ttu-id="91cca-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="91cca-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="91cca-298">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="91cca-298">SameSiteMode.Lax</span></span><br><span data-ttu-id="91cca-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="91cca-299">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="91cca-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="91cca-300">SameSiteMode.Strict</span></span>   | <span data-ttu-id="91cca-301">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="91cca-301">SameSiteMode.None</span></span><br><span data-ttu-id="91cca-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="91cca-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="91cca-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="91cca-303">SameSiteMode.Strict</span></span> | <span data-ttu-id="91cca-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="91cca-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="91cca-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="91cca-305">SameSiteMode.Strict</span></span><br><span data-ttu-id="91cca-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="91cca-306">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="91cca-307">Bir kimlik doğrulama tanımlama bilgisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="91cca-307">Creating an authentication cookie</span></span>

<span data-ttu-id="91cca-308">Kullanıcı bilgileri bulunduran bir tanımlama bilgisi oluşturmak için oluşturmalıdır bir [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="91cca-308">To create a cookie holding user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="91cca-309">Kullanıcı bilgilerini serileştirilmiş ve tanımlama bilgisinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="91cca-309">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="91cca-310">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="91cca-311">Çağrı [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) kullanıcıyla oturum açmak için:</span><span class="sxs-lookup"><span data-stu-id="91cca-311">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="91cca-312">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-312">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="91cca-313">Çağrı [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) kullanıcıyla oturum açmak için:</span><span class="sxs-lookup"><span data-stu-id="91cca-313">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="91cca-314">`SignInAsync`şifrelenmiş bir tanımlama bilgisi oluşturur ve geçerli yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="91cca-314">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="91cca-315">Belirtmediyseniz bir `AuthenticationScheme`, varsayılan düzeni kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-315">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="91cca-316">Perde arkasında kullanılan ASP.NET Core'nın şifrelemesidir [veri koruması](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistem.</span><span class="sxs-lookup"><span data-stu-id="91cca-316">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="91cca-317">Birden fazla makine, uygulamalar arasında Yük Dengeleme veya bir web çiftliği kullanarak uygulama barındırma sonra yapmanız gerekenler [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) aynı anahtar halkası ve uygulama tanımlayıcısı kullanmak üzere.</span><span class="sxs-lookup"><span data-stu-id="91cca-317">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="91cca-318">Oturumu kapatma</span><span class="sxs-lookup"><span data-stu-id="91cca-318">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="91cca-319">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-319">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="91cca-320">Out geçerli kullanıcı oturum açabilir ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="91cca-320">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

```csharp
await HttpContext.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="91cca-321">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-321">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="91cca-322">Out geçerli kullanıcı oturum açabilir ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="91cca-322">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="91cca-323">Kullanmıyorsanız `CookieAuthenticationDefaults.AuthenticationScheme` (veya "Tanımlama bilgileri") kimlik doğrulama sağlayıcısı yapılandırırken kullanılan şema şema (örneğin, "ContosoCookie") olarak sağlayın.</span><span class="sxs-lookup"><span data-stu-id="91cca-323">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="91cca-324">Aksi takdirde, varsayılan düzeni kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91cca-324">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="91cca-325">Arka uç değişiklikler tepki</span><span class="sxs-lookup"><span data-stu-id="91cca-325">Reacting to back-end changes</span></span>

<span data-ttu-id="91cca-326">Bir tanımlama bilgisi oluşturulduktan sonra tek kaynak kimliğinin haline gelir.</span><span class="sxs-lookup"><span data-stu-id="91cca-326">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="91cca-327">Arka uç sistemlerinizde bir kullanıcıyı devre dışı olsa bile bu olanağıyla tanımlama bilgisi kimlik doğrulaması sistemde var ve bunların tanımlama bilgisinin geçerli olduğu sürece bir kullanıcı olarak oturum açmış kalır.</span><span class="sxs-lookup"><span data-stu-id="91cca-327">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="91cca-328">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) ASP.NET Core olayda 2.x veya [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) yönteminde ASP.NET Core 1.x kesecek ve tanımlama bilgisi kimlik doğrulaması geçersiz kılmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="91cca-328">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="91cca-329">Bu yaklaşım, uygulama erişimini iptal edilen kullanıcıların riskini azaltır.</span><span class="sxs-lookup"><span data-stu-id="91cca-329">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="91cca-330">Tanımlama bilgisi doğrulama için bir yaklaşım, kullanıcı veritabanı değiştiğinde izleyen üzerinde temel alır.</span><span class="sxs-lookup"><span data-stu-id="91cca-330">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="91cca-331">Kullanıcının tanımlama bilgisi verilmiş olduğundan veritabanı değişip değişmediğini, kendi tanımlama bilgisi hala geçerli ise, kullanıcının yeniden kimlik doğrulaması için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="91cca-331">If the database hasn't been changed since the user's cookie was issued, there is no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="91cca-332">Bu senaryo, uygulanan veritabanı uygulamak için `IUserRepository` Bu örnekte, depolayan bir `LastChanged` değeri.</span><span class="sxs-lookup"><span data-stu-id="91cca-332">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="91cca-333">Herhangi bir kullanıcı veritabanında güncelleştirildiğinde `LastChanged` değeri, geçerli saate ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="91cca-333">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="91cca-334">Veritabanı değişikliklerini temel alan bir tanımlama bilgisi geçersiz kılmak için `LastChanged` değeri, tanımlama bilgisi ile oluşturma bir `LastChanged` geçerli içeren talep `LastChanged` veritabanından değeri:</span><span class="sxs-lookup"><span data-stu-id="91cca-334">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="91cca-335">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-335">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="91cca-336">İçin bir geçersiz kılma uygulamak için `ValidatePrincipal` olay, aşağıdaki imzası öğesinden türetilen bir sınıfta yöntemiyle bir yazma [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="91cca-336">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="91cca-337">Bir örnek aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="91cca-337">An example looks like the following:</span></span>

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

<span data-ttu-id="91cca-338">Tanımlama bilgisi hizmet kaydı sırasında olayları örneği kaydedin `ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="91cca-338">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="91cca-339">Kapsamlı hizmeti kaydı için sağlayın, `CustomCookieAuthenticationEvents` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="91cca-339">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="91cca-340">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-340">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="91cca-341">İçin bir geçersiz kılma uygulamak için `ValidateAsync` olay, aşağıdaki imzası yöntemiyle bir yazma:</span><span class="sxs-lookup"><span data-stu-id="91cca-341">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="91cca-342">ASP.NET Core kimliği parçası olarak bu onay uygulayan kendi [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="91cca-342">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="91cca-343">Bir örnek aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="91cca-343">An example looks like the following:</span></span>

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

<span data-ttu-id="91cca-344">Tanımlama bilgisi kimlik doğrulama yapılandırması sırasında olay kaydetmek `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="91cca-344">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="91cca-345">Kullanıcının adını güncelleştirilir bir durum düşünün &mdash; güvenlik herhangi bir şekilde etkilemez bir karardır.</span><span class="sxs-lookup"><span data-stu-id="91cca-345">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="91cca-346">Kullanıcı asıl adı dönüşlü güncelleştirmek istiyorsanız, çağrı `context.ReplacePrincipal` ve `context.ShouldRenew` özelliğine `true`.</span><span class="sxs-lookup"><span data-stu-id="91cca-346">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="91cca-347">Burada açıklanan yaklaşımı, her istek tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="91cca-347">The approach described here is triggered on every request.</span></span> <span data-ttu-id="91cca-348">Bu uygulama için büyük performans cezası neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="91cca-348">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="91cca-349">Kalıcı tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="91cca-349">Persistent cookies</span></span>

<span data-ttu-id="91cca-350">Tarayıcı oturumlarında kalıcı hale getirmek için tanımlama bilgisi isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="91cca-350">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="91cca-351">Bu Kalıcılık açık kullanıcı izni olan bir "Beni anımsa" oturum açma veya benzer bir mekanizma ile yalnızca etkinleştirilmiş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="91cca-351">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="91cca-352">Aşağıdaki kod parçacığını bir kimlik ve tarayıcı kapanışlar şekilde kalmıştır karşılık gelen tanımlama bilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="91cca-352">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="91cca-353">Önceden yapılandırılmış kayan sona erme ayarları dikkate alınır.</span><span class="sxs-lookup"><span data-stu-id="91cca-353">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="91cca-354">Tarayıcı kapalıyken tanımlama bilgisinin süresi dolarsa, hizmet yeniden başlatıldıktan sonra tarayıcı tanımlama bilgisi temizler.</span><span class="sxs-lookup"><span data-stu-id="91cca-354">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="91cca-355">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-355">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="91cca-356">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) sınıfı bulunur `Microsoft.AspNetCore.Authentication` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="91cca-356">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="91cca-357">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="91cca-358">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) sınıfı bulunur `Microsoft.AspNetCore.Http.Authentication` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="91cca-358">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="91cca-359">Mutlak tanımlama bilgisinin süre sonu</span><span class="sxs-lookup"><span data-stu-id="91cca-359">Absolute cookie expiration</span></span>

<span data-ttu-id="91cca-360">İle bir mutlak zaman aşımı süresini ayarlayabilirsiniz `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="91cca-360">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="91cca-361">De ayarlamalısınız `IsPersistent`; Aksi takdirde `ExpiresUtc` göz ardı edilir ve tek oturum tanımlama bilgisi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="91cca-361">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="91cca-362">Zaman `ExpiresUtc` ayarlanan `SignInAsync`, değerini geçersiz kılar `ExpireTimeSpan` seçeneği `CookieAuthenticationOptions`, ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="91cca-362">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="91cca-363">Aşağıdaki kod parçacığını bir kimlik ve 20 dakika sürer karşılık gelen tanımlama bilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="91cca-363">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="91cca-364">Bu, önceden yapılandırılmış kayan sona erme ayarları yok sayar.</span><span class="sxs-lookup"><span data-stu-id="91cca-364">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="91cca-365">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-365">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="91cca-366">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="91cca-366">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

---

## <a name="see-also"></a><span data-ttu-id="91cca-367">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="91cca-367">See also</span></span>

* [<span data-ttu-id="91cca-368">Auth 2.0 değişiklikleri / geçiş Duyurusu</span><span class="sxs-lookup"><span data-stu-id="91cca-368">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="91cca-369">Kimlik düzeni tarafından sınırlama</span><span class="sxs-lookup"><span data-stu-id="91cca-369">Limiting identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
