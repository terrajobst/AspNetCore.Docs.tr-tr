---
title: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullan
author: rick-anderson
description: ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622738"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="4ed2e-103">ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullan</span><span class="sxs-lookup"><span data-stu-id="4ed2e-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="4ed2e-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4ed2e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4ed2e-105">ASP.NET Core kimliği bir tam, tam özellikli bir kimlik doğrulama sağlayıcısı oluşturma ve oturum açma bilgileri korumak içindir.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="4ed2e-106">Ancak, bir ASP.NET Core kimliği olmadan tanımlama bilgisi tabanlı kimlik doğrulaması kimlik doğrulama sağlayıcısı kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="4ed2e-107">Daha fazla bilgi için bkz. <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="4ed2e-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4ed2e-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4ed2e-109">Örnek uygulamada tanıtım amacıyla kullanıcı kuramsal Maria Rodriguez, kullanıcının uygulamada oturum kodlanmış hesabıdır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="4ed2e-110">Kullanım **e-posta** kullanıcı adı `maria.rodriguez@contoso.com` ve kullanıcı oturum açmak için herhangi bir parola.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-110">Use the **Email** user name `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="4ed2e-111">Kullanıcının kimliğinin `AuthenticateUser` yönteminde *Pages/Account/Login.cshtml.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="4ed2e-112">Gerçek hayatta kullanılan örnekte, bir veritabanında kullanıcı kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="4ed2e-113">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4ed2e-113">Configuration</span></span>

<span data-ttu-id="4ed2e-114">Uygulama kullanmıyorsa [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), proje dosyası için bir paket başvuruya [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) paket.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="4ed2e-115">İçinde `Startup.ConfigureServices` yöntemi ile kimlik doğrulaması ara yazılımı hizmeti oluşturma <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> ve <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="4ed2e-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> geçirilen `AddAuthentication` uygulama için varsayılan kimlik doğrulama şeması ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="4ed2e-117">`AuthenticationScheme` tanımlama bilgisi kimlik doğrulamasını birden çok örneği vardır ve istediğiniz durumlarda kullanışlıdır [belirli bir düzeni ile yetkilendirme](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="4ed2e-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="4ed2e-118">Ayarı `AuthenticationScheme` için [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) düzeni için "Tanımlama bilgileri" değerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="4ed2e-119">Düzeni ayırt eden herhangi bir dize değeri sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="4ed2e-120">Uygulamanın kimlik doğrulama düzeni, uygulamanın tanımlama bilgisi kimlik doğrulaması düzeninden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="4ed2e-121">Ne zaman bir tanımlama bilgisi kimlik doğrulama düzeni değildir sağlanan <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, kullandığı `CookieAuthenticationDefaults.AuthenticationScheme` ("tanımlama bilgileri").</span><span class="sxs-lookup"><span data-stu-id="4ed2e-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="4ed2e-122">Kimlik doğrulama tanımlama bilgisinin <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> özelliği `true` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="4ed2e-123">Kimlik doğrulaması tanımlama bilgileri, bir ziyaretçi için veri toplama tarafından onaylanan taşınmadığından olduğunda izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="4ed2e-124">Daha fazla bilgi için bkz. <xref:security/gdpr#essential-cookies>.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="4ed2e-125">İçinde `Startup.Configure` yöntemi, çağrı `UseAuthentication` ayarlar kimlik doğrulaması ara yazılımı çağrılacak yöntem `HttpContext.User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-125">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="4ed2e-126">Çağrı `UseAuthentication` yöntemi çağırmadan önce `UseMvcWithDefaultRoute` veya `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="4ed2e-127"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> Sınıfı, kimlik doğrulama sağlayıcısı seçeneklerini yapılandırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="4ed2e-128">Ayarlama `CookieAuthenticationOptions` kimlik doğrulaması için hizmet yapılandırmasının `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="4ed2e-129">Tanımlama bilgisi ilkesi Ara</span><span class="sxs-lookup"><span data-stu-id="4ed2e-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="4ed2e-130">[Tanımlama bilgisi ilkesi Ara](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) tanımlama bilgisi ilkesi özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="4ed2e-131">Uygulama işleme ardışık düzenine bir ara yazılım ekleme sırası gizli olduğu&mdash;yalnızca bir işlem hattı, kayıtlı bir aşağı akış bileşenleri etkiler.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="4ed2e-132">Kullanım <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> tanımlama bilgilerini eklenmiş veya silinmiş tanımlama bilgisi işleme işleyicileri tanımlama bilgisi işleme ve kanca genel özelliklerini denetlemek için tanımlama bilgisi ilkesi Ara sağlanan.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="4ed2e-133">Varsayılan <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> değer `SameSiteMode.Lax` OAuth2 kimlik doğrulaması izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="4ed2e-134">Bir site aynı ilke kesinlikle uygulamak `SameSiteMode.Strict`ayarlayın `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="4ed2e-135">Bu ayar, OAuth2 ve diğer kaynaklar arası kimlik doğrulaması şemalarını keser olsa da, başka türden bir çıkış noktaları arası istek işleme güvenmeyin uygulamalar tanımlama bilgisinin güvenlik düzeyini yükseltir.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="4ed2e-136">Tanımlama bilgisi ilkesi Ara ayarı `MinimumSameSitePolicy` ayarını etkileyebilir `Cookie.SameSite` içinde `CookieAuthenticationOptions` aşağıdaki matris göre ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="4ed2e-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="4ed2e-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="4ed2e-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="4ed2e-138">Cookie.SameSite</span></span> | <span data-ttu-id="4ed2e-139">Sonuç Cookie.SameSite ayarı</span><span class="sxs-lookup"><span data-stu-id="4ed2e-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="4ed2e-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4ed2e-140">SameSiteMode.None</span></span>     | <span data-ttu-id="4ed2e-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4ed2e-141">SameSiteMode.None</span></span><br><span data-ttu-id="4ed2e-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4ed2e-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="4ed2e-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4ed2e-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="4ed2e-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4ed2e-144">SameSiteMode.None</span></span><br><span data-ttu-id="4ed2e-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4ed2e-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="4ed2e-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4ed2e-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="4ed2e-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4ed2e-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="4ed2e-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4ed2e-148">SameSiteMode.None</span></span><br><span data-ttu-id="4ed2e-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4ed2e-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="4ed2e-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4ed2e-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="4ed2e-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4ed2e-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="4ed2e-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4ed2e-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="4ed2e-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4ed2e-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="4ed2e-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4ed2e-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="4ed2e-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4ed2e-155">SameSiteMode.None</span></span><br><span data-ttu-id="4ed2e-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4ed2e-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="4ed2e-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4ed2e-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="4ed2e-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4ed2e-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="4ed2e-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4ed2e-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="4ed2e-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4ed2e-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="4ed2e-161">Bir kimlik doğrulama tanımlama bilgisi oluştur</span><span class="sxs-lookup"><span data-stu-id="4ed2e-161">Create an authentication cookie</span></span>

<span data-ttu-id="4ed2e-162">Kullanıcı bilgileri bulunduran bir tanımlama bilgisi oluşturmak için oluşturun bir <xref:System.Security.Claims.ClaimsPrincipal>.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="4ed2e-163">Kullanıcı bilgileri serileştirilmiş ve tanımlama bilgisinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="4ed2e-164">Oluşturma bir <xref:System.Security.Claims.ClaimsIdentity> gerekli ile <xref:System.Security.Claims.Claim>s ve çağrı <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> kullanıcının oturum açmak için:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="4ed2e-165">`SignInAsync` şifrelenmiş bir tanımlama bilgisi oluşturur ve geçerli yanıta ekler.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="4ed2e-166">Varsa `AuthenticationScheme` belirtilmediyse, varsayılan düzenini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="4ed2e-167">ASP.NET Core'nın [veri koruma](xref:security/data-protection/using-data-protection) sistem şifreleme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="4ed2e-168">Birden fazla makinede barındırılan bir uygulama için uygulama arasında dengeleme ya da bir web grubu yük [veri korumasını yapılandırma](xref:security/data-protection/configuration/overview) aynı anahtarı halka ve uygulama tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="4ed2e-169">Oturumu kapat</span><span class="sxs-lookup"><span data-stu-id="4ed2e-169">Sign out</span></span>

<span data-ttu-id="4ed2e-170">Geçerli kullanıcının oturumunu kapatmaz ve kendi tanımlama bilgisini silmek için çağrı <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="4ed2e-171">Varsa `CookieAuthenticationDefaults.AuthenticationScheme` (veya "Tanımlama bilgileri") sağlayın (örneğin, "ContosoCookie"), düzeni gibi kimlik doğrulama sağlayıcısı yapılandırılırken kullanılan şema kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="4ed2e-172">Aksi takdirde, varsayılan düzenini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="4ed2e-173">Arka uç değişikliklerine tepki</span><span class="sxs-lookup"><span data-stu-id="4ed2e-173">React to back-end changes</span></span>

<span data-ttu-id="4ed2e-174">Bir tanımlama bilgisi oluşturulduktan sonra tanımlama bilgisi kimliği tek kaynaktır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="4ed2e-175">Arka uç sistemleri bir kullanıcı hesabı devre dışı bırakılırsa:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="4ed2e-176">Uygulamanın tanımlama bilgisi kimlik doğrulama sistemi, kimlik doğrulama tanımlama bilgisinde istekleri işlemeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="4ed2e-177">Kimlik doğrulama tanımlama bilgisinin geçerli olduğu sürece, kullanıcı uygulamada oturum imzalı kalır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="4ed2e-178"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> Olay kesebilir ve tanımlama bilgisi kimlik doğrulaması geçersiz kılmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="4ed2e-179">Her istekte tanımlama bilgisi doğrulama iptal edilen kullanıcıların uygulamaya erişmeden riskini azaltır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="4ed2e-180">Tanımlama bilgisi doğrulama için bir yaklaşım, kullanıcı veritabanına değiştiğinde izleyen üzerinde temel alır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="4ed2e-181">Kullanıcının tanımlama bilgisi düzenlendiğinden veritabanının değişip değişmediğini, kendi tanımlama bilgisini hala geçerli ise kullanıcının yeniden kimlik doğrulaması gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="4ed2e-182">Örnek uygulamada, veritabanı uygulanan `IUserRepository` ve depolayan bir `LastChanged` değeri.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="4ed2e-183">Bir kullanıcı veritabanında güncelleştirildiğinde `LastChanged` değeri, geçerli saate ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="4ed2e-184">Veritabanı değişikliklerini temel alan bir tanımlama bilgisi geçersiz kılmak için `LastChanged` değeri, tanımlama bilgisi ile oluşturma bir `LastChanged` geçerli içeren talep `LastChanged` veritabanından değer:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="4ed2e-185">Uygulama için bir geçersiz kılma `ValidatePrincipal` olay, bir yöntemin, türetilen bir sınıfta imzayla yazma <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="4ed2e-186">Aşağıdaki örnek uygulamasıdır `CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="4ed2e-187">Tanımlama bilgisi hizmet kaydı sırasında olayları örneğini kaydetme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="4ed2e-188">Sağlayan bir [hizmet kayıt kapsamı](xref:fundamentals/dependency-injection#service-lifetimes) için `CustomCookieAuthenticationEvents` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="4ed2e-189">Kullanıcının name güncelleştirildiği bir durum düşünün&mdash;karar güvenlik herhangi bir şekilde etkilemez.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="4ed2e-190">Kullanıcı asıl adı dönüşlü güncelleştirmek istiyorsanız, çağrı `context.ReplacePrincipal` ayarlayıp `context.ShouldRenew` özelliğini `true`.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="4ed2e-191">Burada açıklanan yaklaşımı, her istek tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="4ed2e-192">Her istekte tüm kullanıcılar için kimlik doğrulaması tanımlama bilgileri doğrulanıyor uygulama için bir büyük bir performans sorununa neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="4ed2e-193">Kalıcı tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="4ed2e-193">Persistent cookies</span></span>

<span data-ttu-id="4ed2e-194">Tarayıcı oturumlarında kalıcı hale getirmek için tanımlama bilgisi isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="4ed2e-195">Oturum açma "Beni Hatırla" onay kutusu veya benzer bir mekanizma ile açık kullanıcı onayı ile yalnızca bu Kalıcılık etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="4ed2e-196">Aşağıdaki kod parçacığı, bir kimlik ve tarayıcı kapanışlar şekilde kalmıştır ilgili tanımlama bilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="4ed2e-197">Daha önce yapılandırılmış tüm kayan zaman aşımı ayarları dikkate alınır.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="4ed2e-198">Tarayıcı kapalıyken tanımlama bilgisi süresi dolarsa, yeniden başlatıldıktan sonra tarayıcı tanımlama bilgisi temizler.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="4ed2e-199">Ayarlama <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> için `true` içinde <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="4ed2e-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="4ed2e-200">Mutlak tanımlama bilgisi süre sonu</span><span class="sxs-lookup"><span data-stu-id="4ed2e-200">Absolute cookie expiration</span></span>

<span data-ttu-id="4ed2e-201">Mutlak sona erme süresini ayarlanabilir <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="4ed2e-202">Kalıcı bir tanımlama bilgisi oluşturmak için `IsPersistent` de ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="4ed2e-203">Aksi takdirde tanımlama bilgisi ile oturum tabanlı bir yaşam süresi oluşturulur ve önce veya sonra tuttuğu kimlik doğrulama anahtarının süresi.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="4ed2e-204">Zaman `ExpiresUtc` , değerini geçersiz kılar, ayarlanmış <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> seçeneği <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="4ed2e-205">Aşağıdaki kod parçacığı, bir kimlik ve 20 dakika sürer ilgili tanımlama bilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="4ed2e-206">Bu, daha önce yapılandırılmış tüm kayan zaman aşımı ayarları yok sayar.</span><span class="sxs-lookup"><span data-stu-id="4ed2e-206">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

## <a name="additional-resources"></a><span data-ttu-id="4ed2e-207">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4ed2e-207">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="4ed2e-208">İlke tabanlı rol denetimleri</span><span class="sxs-lookup"><span data-stu-id="4ed2e-208">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
