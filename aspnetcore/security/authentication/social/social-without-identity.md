---
title: ASP.NET Core kimliği olmadan Facebook, Google ve dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Facebook, Google, Twitter vb. hesap Kullanıcı kimlik doğrulamasını ASP.NET Core kimliği olmadan kullanma açıklaması.
ms.author: riande
ms.date: 11/19/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 680ea091dcc5ed7f94879b5d277e8be7e5abeb7b
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289111"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="ca313-103">ASP.NET Core kimlik olmadan sosyal oturum açma sağlayıcısı kimlik doğrulamasını kullanma</span><span class="sxs-lookup"><span data-stu-id="ca313-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ca313-104"><xref:security/authentication/social/index>, kullanıcıların dış kimlik doğrulama sağlayıcılarının kimlik bilgileriyle OAuth 2,0 kullanarak oturum açmasını nasıl sağladığını açıklar.</span><span class="sxs-lookup"><span data-stu-id="ca313-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="ca313-105">Bu konuda açıklanan yaklaşım kimlik doğrulama sağlayıcısı olarak ASP.NET Core kimliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="ca313-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="ca313-106">Bu örnek, ASP.NET Core kimliği **olmadan** bir dış kimlik doğrulama sağlayıcısının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="ca313-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="ca313-107">Bu, ASP.NET Core kimliğin tüm özelliklerini gerektirmeyen, ancak yine de güvenilen bir dış kimlik doğrulama sağlayıcısıyla tümleştirme gerektiren uygulamalar için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="ca313-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="ca313-108">Bu örnek, kullanıcıların kimliğini doğrulamak için [Google kimlik doğrulamasını](xref:security/authentication/google-logins) kullanır.</span><span class="sxs-lookup"><span data-stu-id="ca313-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="ca313-109">Google kimlik doğrulamasını kullanmak, oturum açma işlemini Google 'a yönetmenin karmaşıklıklarından çoğunu kaydırır.</span><span class="sxs-lookup"><span data-stu-id="ca313-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="ca313-110">Farklı bir dış kimlik doğrulama sağlayıcısıyla tümleştirme için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="ca313-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="ca313-111">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ca313-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="ca313-112">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ca313-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="ca313-113">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ca313-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="ca313-114">Diğer sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="ca313-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="ca313-115">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ca313-115">Configuration</span></span>

<span data-ttu-id="ca313-116">`ConfigureServices` yönteminde, uygulamanın kimlik doğrulama düzenlerini <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>ve <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> yöntemleriyle yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="ca313-116">In the `ConfigureServices` method, configure the app's authentication schemes with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, and <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

<span data-ttu-id="ca313-117"><xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağrısı, uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ca313-117">The call to <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> sets the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>.</span></span> <span data-ttu-id="ca313-118">`DefaultScheme`, aşağıdaki `HttpContext` kimlik doğrulama uzantısı yöntemleri tarafından kullanılan varsayılan şemadır:</span><span class="sxs-lookup"><span data-stu-id="ca313-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="ca313-119">Uygulamanın `DefaultScheme`, [ıeauthenticationdefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") olarak ayarlanması, uygulamayı bu uzantı yöntemlerinin varsayılan şeması olarak tanımlama bilgilerini kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ca313-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="ca313-120">Uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") olarak ayarlanması, uygulamayı, `ChallengeAsync`çağrıları için varsayılan düzen olarak Google 'ı kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ca313-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="ca313-121">`DefaultChallengeScheme` geçersiz kılmaları `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="ca313-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="ca313-122">Ayarlandığında `DefaultScheme` geçersiz kılan ek özellikler için <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> bakın.</span><span class="sxs-lookup"><span data-stu-id="ca313-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="ca313-123">`Startup.Configure`' de, çağırma `UseRouting` ve `UseEndpoints`arasında `UseAuthentication` ve `UseAuthorization` çağırın.</span><span class="sxs-lookup"><span data-stu-id="ca313-123">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` between calling `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="ca313-124">Bu, `HttpContext.User` özelliğini ayarlar ve istekler için yetkilendirme ara yazılımını çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="ca313-124">This sets the `HttpContext.User` property and runs the Authorization Middleware for requests:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

<span data-ttu-id="ca313-125">Kimlik doğrulama şemaları ve tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="ca313-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="ca313-126">Yetkilendirmeyi uygula</span><span class="sxs-lookup"><span data-stu-id="ca313-126">Apply authorization</span></span>

<span data-ttu-id="ca313-127">Bir denetleyiciye, eyleme veya sayfaya `AuthorizeAttribute` özniteliğini uygulayarak uygulamanın kimlik doğrulama yapılandırmasını test edin.</span><span class="sxs-lookup"><span data-stu-id="ca313-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="ca313-128">Aşağıdaki kod, *Gizlilik* sayfasına erişimi doğrulanan kullanıcılarla sınırlandırır:</span><span class="sxs-lookup"><span data-stu-id="ca313-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="ca313-129">Oturumu kapat</span><span class="sxs-lookup"><span data-stu-id="ca313-129">Sign out</span></span>

<span data-ttu-id="ca313-130">Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için [Signoutasync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)çağırın.</span><span class="sxs-lookup"><span data-stu-id="ca313-130">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="ca313-131">Aşağıdaki kod, *Dizin* sayfasına bir `Logout` sayfa işleyicisi ekler:</span><span class="sxs-lookup"><span data-stu-id="ca313-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="ca313-132">`SignOutAsync` çağrısının bir kimlik doğrulama düzeni belirtmediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ca313-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="ca313-133">Uygulamanın `CookieAuthenticationDefaults.AuthenticationScheme` `DefaultScheme` geri dönüş olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ca313-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ca313-134">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ca313-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ca313-135"><xref:security/authentication/social/index>, kullanıcıların dış kimlik doğrulama sağlayıcılarının kimlik bilgileriyle OAuth 2,0 kullanarak oturum açmasını nasıl sağladığını açıklar.</span><span class="sxs-lookup"><span data-stu-id="ca313-135"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="ca313-136">Bu konuda açıklanan yaklaşım kimlik doğrulama sağlayıcısı olarak ASP.NET Core kimliğini içerir.</span><span class="sxs-lookup"><span data-stu-id="ca313-136">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="ca313-137">Bu örnek, ASP.NET Core kimliği **olmadan** bir dış kimlik doğrulama sağlayıcısının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="ca313-137">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="ca313-138">Bu, ASP.NET Core kimliğin tüm özelliklerini gerektirmeyen, ancak yine de güvenilen bir dış kimlik doğrulama sağlayıcısıyla tümleştirme gerektiren uygulamalar için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="ca313-138">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="ca313-139">Bu örnek, kullanıcıların kimliğini doğrulamak için [Google kimlik doğrulamasını](xref:security/authentication/google-logins) kullanır.</span><span class="sxs-lookup"><span data-stu-id="ca313-139">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="ca313-140">Google kimlik doğrulamasını kullanmak, oturum açma işlemini Google 'a yönetmenin karmaşıklıklarından çoğunu kaydırır.</span><span class="sxs-lookup"><span data-stu-id="ca313-140">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="ca313-141">Farklı bir dış kimlik doğrulama sağlayıcısıyla tümleştirme için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="ca313-141">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="ca313-142">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ca313-142">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="ca313-143">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ca313-143">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="ca313-144">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="ca313-144">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="ca313-145">Diğer sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="ca313-145">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="ca313-146">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ca313-146">Configuration</span></span>

<span data-ttu-id="ca313-147">`ConfigureServices` yönteminde, uygulamanın kimlik doğrulama düzenlerini `AddAuthentication`, `AddCookie`ve `AddGoogle` yöntemleriyle yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="ca313-147">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie`, and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

<span data-ttu-id="ca313-148">[Addaduthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) çağrısı, uygulamanın [defaultscheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)değerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ca313-148">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="ca313-149">`DefaultScheme`, aşağıdaki `HttpContext` kimlik doğrulama uzantısı yöntemleri tarafından kullanılan varsayılan şemadır:</span><span class="sxs-lookup"><span data-stu-id="ca313-149">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="ca313-150">Uygulamanın `DefaultScheme`, [ıeauthenticationdefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") olarak ayarlanması, uygulamayı bu uzantı yöntemlerinin varsayılan şeması olarak tanımlama bilgilerini kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ca313-150">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="ca313-151">Uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") olarak ayarlanması, uygulamayı, `ChallengeAsync`çağrıları için varsayılan düzen olarak Google 'ı kullanacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ca313-151">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="ca313-152">`DefaultChallengeScheme` geçersiz kılmaları `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="ca313-152">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="ca313-153">Ayarlandığında `DefaultScheme` geçersiz kılan ek özellikler için <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> bakın.</span><span class="sxs-lookup"><span data-stu-id="ca313-153">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="ca313-154">`Configure` yönteminde, `HttpContext.User` özelliğini ayarlayan kimlik doğrulama ara yazılımını çağırmak için `UseAuthentication` yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="ca313-154">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="ca313-155">`UseMvcWithDefaultRoute` veya `UseMvc`çağrılmadan önce `UseAuthentication` yöntemi çağırın:</span><span class="sxs-lookup"><span data-stu-id="ca313-155">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

<span data-ttu-id="ca313-156">Kimlik doğrulama şemaları ve tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="ca313-156">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="apply-authorization"></a><span data-ttu-id="ca313-157">Yetkilendirmeyi uygula</span><span class="sxs-lookup"><span data-stu-id="ca313-157">Apply authorization</span></span>

<span data-ttu-id="ca313-158">Bir denetleyiciye, eyleme veya sayfaya `AuthorizeAttribute` özniteliğini uygulayarak uygulamanın kimlik doğrulama yapılandırmasını test edin.</span><span class="sxs-lookup"><span data-stu-id="ca313-158">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="ca313-159">Aşağıdaki kod, *Gizlilik* sayfasına erişimi doğrulanan kullanıcılarla sınırlandırır:</span><span class="sxs-lookup"><span data-stu-id="ca313-159">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="ca313-160">Oturumu kapat</span><span class="sxs-lookup"><span data-stu-id="ca313-160">Sign out</span></span>

<span data-ttu-id="ca313-161">Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için [Signoutasync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)çağırın.</span><span class="sxs-lookup"><span data-stu-id="ca313-161">To sign out the current user and delete their cookie, call [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*).</span></span> <span data-ttu-id="ca313-162">Aşağıdaki kod, *Dizin* sayfasına bir `Logout` sayfa işleyicisi ekler:</span><span class="sxs-lookup"><span data-stu-id="ca313-162">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

<span data-ttu-id="ca313-163">`SignOutAsync` çağrısının bir kimlik doğrulama düzeni belirtmediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ca313-163">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="ca313-164">Uygulamanın `CookieAuthenticationDefaults.AuthenticationScheme` `DefaultScheme` geri dönüş olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ca313-164">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ca313-165">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ca313-165">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
