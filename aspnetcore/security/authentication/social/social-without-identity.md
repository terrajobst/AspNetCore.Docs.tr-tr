---
title: Facebook, Google ve ASP.NET Core kimliği olmadan dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Facebook, Google, Twitter, ASP.NET Core kimliği olmadan vb. hesabı kullanıcı kimlik doğrulaması kullanarak bir açıklama.
ms.author: riande
ms.date: 07/04/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: e67da513fef1ce453110c465b08e9c7965e71df5
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67557667"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a><span data-ttu-id="35589-103">ASP.NET Core kimliği olmadan sosyal oturum açma sağlayıcısı kimlik doğrulaması kullan</span><span class="sxs-lookup"><span data-stu-id="35589-103">Use social sign-in provider authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="35589-104"><xref:security/authentication/social/index> OAuth 2.0 ile dış kimlik sağlayıcılarına ait kimlik bilgilerini kullanarak oturum açmasına etkinleştirmeyi açıklar.</span><span class="sxs-lookup"><span data-stu-id="35589-104"><xref:security/authentication/social/index> describes how to enable users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span> <span data-ttu-id="35589-105">Bu konuda açıklanan yaklaşımı, ASP.NET Core kimliği bir kimlik doğrulama sağlayıcısı olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="35589-105">The approach described in that topic includes ASP.NET Core Identity as an authentication provider.</span></span>

<span data-ttu-id="35589-106">Bu örnek, bir dış kimlik doğrulama sağlayıcısının nasıl kullanılacağını gösterir. **olmadan** ASP.NET Core kimliği.</span><span class="sxs-lookup"><span data-stu-id="35589-106">This sample demonstrates how to use an external authentication provider **without** ASP.NET Core Identity.</span></span> <span data-ttu-id="35589-107">Bu, tüm ASP.NET Core kimliği özelliklerini gerektirmez, ancak yine de güvenilir bir dış kimlik doğrulama sağlayıcısı ile tümleştirme gerektiren uygulamalar için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="35589-107">This is useful for apps that don't require all of the features of ASP.NET Core Identity, but still require integration with a trusted external authentication provider.</span></span>

<span data-ttu-id="35589-108">Bu örnekte [Google kimlik doğrulaması](xref:security/authentication/google-logins) kullanıcıların kimlik doğrulaması için.</span><span class="sxs-lookup"><span data-stu-id="35589-108">This sample uses [Google authentication](xref:security/authentication/google-logins) for authenticating users.</span></span> <span data-ttu-id="35589-109">Google'ı kullanarak kimlik doğrulaması birçok Google oturum açma işlemi yönetmenin karmaşıklığını kaydırır.</span><span class="sxs-lookup"><span data-stu-id="35589-109">Using Google authentication shifts many of the complexities of managing the sign-in process to Google.</span></span> <span data-ttu-id="35589-110">Bir farklı Dış kimlik doğrulama sağlayıcısı ile tümleştirmek için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="35589-110">To integrate with a different external authentication provider, see the following topics:</span></span>

* [<span data-ttu-id="35589-111">Facebook kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="35589-111">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="35589-112">Microsoft kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="35589-112">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="35589-113">Twitter kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="35589-113">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="35589-114">Diğer sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="35589-114">Other providers</span></span>](xref:security/authentication/otherlogins)

## <a name="configuration"></a><span data-ttu-id="35589-115">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="35589-115">Configuration</span></span>

<span data-ttu-id="35589-116">İçinde `ConfigureServices` yöntemi, uygulamanın kimlik doğrulama şeması ile yapılandırma `AddAuthentication`, `AddCookie` ve `AddGoogle` yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="35589-116">In the `ConfigureServices` method, configure the app's authentication schemes with the `AddAuthentication`, `AddCookie` and `AddGoogle` methods:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="35589-117">Çağrı [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) uygulamanın ayarlar [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span><span class="sxs-lookup"><span data-stu-id="35589-117">The call to [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sets the app's [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme).</span></span> <span data-ttu-id="35589-118">`DefaultScheme` Aşağıdaki tarafından kullanılan varsayılan düzen olan `HttpContext` kimlik doğrulaması için genişletme yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="35589-118">The `DefaultScheme` is the default scheme used by the following `HttpContext` authentication extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

<span data-ttu-id="35589-119">Uygulama ayarı `DefaultScheme` için [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("tanımlama bilgileri") tanımlama bilgisi bu genişletme yöntemleri için varsayılan düzeni olarak kullanmak üzere uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="35589-119">Setting the app's `DefaultScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") configures the app to use Cookies as the default scheme for these extension methods.</span></span> <span data-ttu-id="35589-120">Uygulama ayarı <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> için [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), Google, çağrılar için varsayılan düzeni olarak kullanmak üzere uygulamayı yapılandırır `ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="35589-120">Setting the app's <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> to [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") configures the app to use Google as the default scheme for calls to `ChallengeAsync`.</span></span> <span data-ttu-id="35589-121">`DefaultChallengeScheme` geçersiz kılmalar `DefaultScheme`.</span><span class="sxs-lookup"><span data-stu-id="35589-121">`DefaultChallengeScheme` overrides `DefaultScheme`.</span></span> <span data-ttu-id="35589-122">Bkz: <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> geçersiz ek özellik `DefaultScheme` ayarlandığında.</span><span class="sxs-lookup"><span data-stu-id="35589-122">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> for additional properties that override `DefaultScheme` when set.</span></span>

<span data-ttu-id="35589-123">İçinde `Configure` yöntemi, çağrı `UseAuthentication` ayarlar kimlik doğrulaması ara yazılımı çağrılacak yöntem `HttpContext.User` özelliği.</span><span class="sxs-lookup"><span data-stu-id="35589-123">In the `Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="35589-124">Çağrı `UseAuthentication` yöntemi çağırmadan önce `UseMvcWithDefaultRoute` veya `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="35589-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="35589-125">Kimlik doğrulama düzenleri ve tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz: <xref:security/authentication/cookie>.</span><span class="sxs-lookup"><span data-stu-id="35589-125">To learn more about authentication schemes and cookie authentication, see <xref:security/authentication/cookie>.</span></span>

## <a name="applying-basic-authorization"></a><span data-ttu-id="35589-126">Temel uygulama yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="35589-126">Applying basic authorization</span></span>

<span data-ttu-id="35589-127">Uygulamanın kimlik doğrulama yapılandırmasına uygulayarak test `AuthorizeAttribute` öznitelik bir denetleyicide, eylemde veya sayfa.</span><span class="sxs-lookup"><span data-stu-id="35589-127">Test the app's authentication configuration by applying the `AuthorizeAttribute` attribute to a controller, action, or page.</span></span> <span data-ttu-id="35589-128">Aşağıdaki kod erişimi sınırlar *gizlilik* doğrulanan kullanıcılara sayfası:</span><span class="sxs-lookup"><span data-stu-id="35589-128">The following code limits access to the *Privacy* page to users that have been authenticated:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a><span data-ttu-id="35589-129">Oturumu kapat</span><span class="sxs-lookup"><span data-stu-id="35589-129">Sign out</span></span>

<span data-ttu-id="35589-130">Geçerli kullanıcının oturumunu kapatmaz ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="35589-130">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0).</span></span> <span data-ttu-id="35589-131">Aşağıdaki kodu ekler bir `Logout` sayfa işleyicisine *dizin* sayfası:</span><span class="sxs-lookup"><span data-stu-id="35589-131">The following code adds a `Logout` page handler to the *Index* page:</span></span>

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

<span data-ttu-id="35589-132">Dikkat çağrısı `SignOutAsync` bir kimlik doğrulama düzeni belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="35589-132">Notice that the call to `SignOutAsync` does not specify an authentication scheme.</span></span> <span data-ttu-id="35589-133">Uygulamanın `DefaultScheme` , `CookieAuthenticationDefaults.AuthenticationScheme` bir sıfırlamaya kullanılır.</span><span class="sxs-lookup"><span data-stu-id="35589-133">The app's `DefaultScheme` of `CookieAuthenticationDefaults.AuthenticationScheme` is used as a fall back.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35589-134">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="35589-134">Additional resources</span></span>

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
