---
title: ASP.NET Core kimliği olmadan Facebook, Google ve dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Facebook, Google, Twitter vb. hesap Kullanıcı kimlik doğrulamasını ASP.NET Core kimliği olmadan kullanma açıklaması.
ms.author: riande
ms.date: 09/25/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 54dd93a13b2f7ed09c2c305f529d5f4610567184
ms.sourcegitcommit: 6d26ab647ede4f8e57465e29b03be5cb130fc872
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "71999891"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="e5b98-103">ASP.NET Core kimlik olmadan sosyal oturum açma sağlayıcısı kimlik doğrulamasını kullanma</span><span class="sxs-lookup"><span data-stu-id="e5b98-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e5b98-104"><xref:security/authentication/social/index>, kullanıcıların dış kimlik doğrulama sağlayıcılarının kimlik bilgileriyle OAuth 2,0 kullanarak oturum açmasını nasıl sağladığını açıklar.</span><span class="sxs-lookup"><span data-stu-id="e5b98-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="e5b98-105">Bu konuda açıklanan yaklaşım kimlik doğrulama sağlayıcısı olarak ASP.NET Core kimliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="e5b98-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="e5b98-106">Bu örnek, ASP.NET Core kimliği **olmadan** bir dış kimlik doğrulama sağlayıcısının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e5b98-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="e5b98-107">Bu, ASP.NET Core kimliğin tüm özelliklerini gerektirmeyen, ancak yine de güvenilen bir dış kimlik doğrulama sağlayıcısıyla tümleştirme gerektiren uygulamalar için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="e5b98-108">Bu örnek, kullanıcıların kimliğini doğrulamak için [Google kimlik doğrulamasını](xref:security/authentication/google-logins) kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="e5b98-109">Google kimlik doğrulamasını kullanmak, oturum açma işlemini Google 'a yönetmenin karmaşıklıklarından çoğunu kaydırır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="e5b98-110">Farklı bir dış kimlik doğrulama sağlayıcısıyla tümleştirme için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="e5b98-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="e5b98-111">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e5b98-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="e5b98-112">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e5b98-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="e5b98-113">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e5b98-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="e5b98-114">Diğer sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="e5b98-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="e5b98-115">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e5b98-115">Configuration</span></span>

<span data-ttu-id="e5b98-116">@No__t-0 yönteminde, uygulamanın kimlik doğrulama düzenlerini <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> ve <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> yöntemleriyle yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="e5b98-116">In the `ConfigureServices` method, configure the app's authentication schemes with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, and <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> methods:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet1)]

<span data-ttu-id="e5b98-117">@No__t-0 ' a yapılan çağrı, uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme> ' i ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e5b98-117">The call to <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> sets the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>.</span></span> <span data-ttu-id="e5b98-118">@No__t-0, aşağıdaki `HttpContext` kimlik doğrulama uzantısı yöntemleri tarafından kullanılan varsayılan şemadır:</span><span class="sxs-lookup"><span data-stu-id="e5b98-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="e5b98-119">Uygulamanın `DefaultScheme` ' [yı ("](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) tanımlama bilgileri"), bu uzantı yöntemlerinin varsayılan şeması olarak tanımlama bilgilerini kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="e5b98-120">Uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> ' yı [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") olarak ayarlamak, uygulamayı Google 'ı `ChallengeAsync` ' ye çağrılar için varsayılan düzen olarak kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="e5b98-121">`DefaultChallengeScheme` geçersiz kılmaları `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="e5b98-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="e5b98-122">Ayarlandığında, `DefaultScheme` ' i geçersiz kılan ek özellikler için bkz. <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.</span><span class="sxs-lookup"><span data-stu-id="e5b98-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="e5b98-123">@No__t-0 ' da `UseAuthentication` ve `UseAuthorization` ' yi çağırıp `HttpContext.User` özelliğini ayarlayın ve istekler için yetkilendirme ara yazılımını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e5b98-123">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="e5b98-124">@No__t-2 ' i çağırmadan önce `UseAuthentication` ve `UseAuthorization` yöntemlerini çağırın:</span><span class="sxs-lookup"><span data-stu-id="e5b98-124">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet2)]

<span data-ttu-id="e5b98-125">Kimlik doğrulama şemaları ve tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="e5b98-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="e5b98-126">Yetkilendirmeyi uygula</span><span class="sxs-lookup"><span data-stu-id="e5b98-126">Apply authorization</span></span>

<span data-ttu-id="e5b98-127">Bir denetleyiciye, eyleme veya sayfaya `AuthorizeAttribute` özniteliğini uygulayarak uygulamanın kimlik doğrulama yapılandırmasını test edin.</span><span class="sxs-lookup"><span data-stu-id="e5b98-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="e5b98-128">Aşağıdaki kod, *Gizlilik* sayfasına erişimi doğrulanan kullanıcılarla sınırlandırır:</span><span class="sxs-lookup"><span data-stu-id="e5b98-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="e5b98-129">Oturumu kapat</span><span class="sxs-lookup"><span data-stu-id="e5b98-129">Sign out</span></span>

<span data-ttu-id="e5b98-130">Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için [Signoutasync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)çağırın.</span><span class="sxs-lookup"><span data-stu-id="e5b98-130">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="e5b98-131">Aşağıdaki kod *Dizin* sayfasına `Logout` sayfa işleyicisi ekler:</span><span class="sxs-lookup"><span data-stu-id="e5b98-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/3.0sample/Pages/Index.cshtml.cs?name=snippet&highlight=14-18)]

<span data-ttu-id="e5b98-132">@No__t-0 ' a yapılan çağrının bir kimlik doğrulama düzeni belirtmediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e5b98-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="e5b98-133">Uygulamanın `DefaultScheme` ' dan `CookieAuthenticationDefaults.AuthenticationScheme` ' in geri dönüş olarak kullanıldığı.</span><span class="sxs-lookup"><span data-stu-id="e5b98-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5b98-134">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e5b98-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e5b98-135"><xref:security/authentication/social/index>, kullanıcıların dış kimlik doğrulama sağlayıcılarının kimlik bilgileriyle OAuth 2,0 kullanarak oturum açmasını nasıl sağladığını açıklar.</span><span class="sxs-lookup"><span data-stu-id="e5b98-135"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="e5b98-136">Bu konuda açıklanan yaklaşım kimlik doğrulama sağlayıcısı olarak ASP.NET Core kimliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="e5b98-136">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="e5b98-137">Bu örnek, ASP.NET Core kimliği **olmadan** bir dış kimlik doğrulama sağlayıcısının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e5b98-137">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="e5b98-138">Bu, ASP.NET Core kimliğin tüm özelliklerini gerektirmeyen, ancak yine de güvenilen bir dış kimlik doğrulama sağlayıcısıyla tümleştirme gerektiren uygulamalar için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-138">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="e5b98-139">Bu örnek, kullanıcıların kimliğini doğrulamak için [Google kimlik doğrulamasını](xref:security/authentication/google-logins) kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-139">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="e5b98-140">Google kimlik doğrulamasını kullanmak, oturum açma işlemini Google 'a yönetmenin karmaşıklıklarından çoğunu kaydırır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-140">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="e5b98-141">Farklı bir dış kimlik doğrulama sağlayıcısıyla tümleştirme için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="e5b98-141">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="e5b98-142">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e5b98-142">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="e5b98-143">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e5b98-143">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="e5b98-144">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e5b98-144">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="e5b98-145">Diğer sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="e5b98-145">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="e5b98-146">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e5b98-146">Configuration</span></span>

<span data-ttu-id="e5b98-147">@No__t-0 yönteminde, uygulamanın kimlik doğrulama düzenlerini `AddAuthentication`, `AddCookie` ve `AddGoogle` yöntemleriyle yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="e5b98-147">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie`, and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="e5b98-148">[Addaduthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) çağrısı, uygulamanın [defaultscheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e5b98-148">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="e5b98-149">@No__t-0, aşağıdaki `HttpContext` kimlik doğrulama uzantısı yöntemleri tarafından kullanılan varsayılan şemadır:</span><span class="sxs-lookup"><span data-stu-id="e5b98-149">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="e5b98-150">Uygulamanın `DefaultScheme` ' [yı ("](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) tanımlama bilgileri"), bu uzantı yöntemlerinin varsayılan şeması olarak tanımlama bilgilerini kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-150">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="e5b98-151">Uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> ' yı [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") olarak ayarlamak, uygulamayı Google 'ı `ChallengeAsync` ' ye çağrılar için varsayılan düzen olarak kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e5b98-151">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="e5b98-152">`DefaultChallengeScheme` geçersiz kılmaları `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="e5b98-152">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="e5b98-153">Ayarlandığında, `DefaultScheme` ' i geçersiz kılan ek özellikler için bkz. <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.</span><span class="sxs-lookup"><span data-stu-id="e5b98-153">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="e5b98-154">@No__t-0 yönteminde, `HttpContext.User` özelliğini ayarlayan kimlik doğrulama ara yazılımını çağırmak için `UseAuthentication` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="e5b98-154">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="e5b98-155">@No__t-1 veya `UseMvc` ' i çağırmadan önce `UseAuthentication` yöntemini çağırın:</span><span class="sxs-lookup"><span data-stu-id="e5b98-155">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="e5b98-156">Kimlik doğrulama şemaları ve tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="e5b98-156">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="e5b98-157">Yetkilendirmeyi uygula</span><span class="sxs-lookup"><span data-stu-id="e5b98-157">Apply authorization</span></span>

<span data-ttu-id="e5b98-158">Bir denetleyiciye, eyleme veya sayfaya `AuthorizeAttribute` özniteliğini uygulayarak uygulamanın kimlik doğrulama yapılandırmasını test edin.</span><span class="sxs-lookup"><span data-stu-id="e5b98-158">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="e5b98-159">Aşağıdaki kod, *Gizlilik* sayfasına erişimi doğrulanan kullanıcılarla sınırlandırır:</span><span class="sxs-lookup"><span data-stu-id="e5b98-159">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="e5b98-160">Oturumu kapat</span><span class="sxs-lookup"><span data-stu-id="e5b98-160">Sign out</span></span>

<span data-ttu-id="e5b98-161">Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için [Signoutasync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)çağırın.</span><span class="sxs-lookup"><span data-stu-id="e5b98-161">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="e5b98-162">Aşağıdaki kod *Dizin* sayfasına `Logout` sayfa işleyicisi ekler:</span><span class="sxs-lookup"><span data-stu-id="e5b98-162">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="e5b98-163">@No__t-0 ' a yapılan çağrının bir kimlik doğrulama düzeni belirtmediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e5b98-163">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="e5b98-164">Uygulamanın `DefaultScheme` ' dan `CookieAuthenticationDefaults.AuthenticationScheme` ' in geri dönüş olarak kullanıldığı.</span><span class="sxs-lookup"><span data-stu-id="e5b98-164">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5b98-165">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e5b98-165">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
