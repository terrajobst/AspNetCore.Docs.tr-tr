---
title: ASP.NET Core kimliği olmadan Facebook, Google ve dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Facebook, Google, Twitter vb. hesap Kullanıcı kimlik doğrulamasını ASP.NET Core kimliği olmadan kullanma açıklaması.
ms.author: riande
ms.date: 12/10/2019
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: 612964ec9ed4975cdc81780dda3bac6cce96037f
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75359064"
---
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>ASP.NET Core kimlik olmadan sosyal oturum açma sağlayıcısı kimlik doğrulamasını kullanma

::: moniker range=">= aspnetcore-3.0"

<xref:security/authentication/social/index>, kullanıcıların dış kimlik doğrulama sağlayıcılarının kimlik bilgileriyle OAuth 2,0 kullanarak oturum açmasını nasıl sağladığını açıklar. Bu konuda açıklanan yaklaşım kimlik doğrulama sağlayıcısı olarak ASP.NET Core kimliğini içerir.

Bu örnek, ASP.NET Core kimliği **olmadan** bir dış kimlik doğrulama sağlayıcısının nasıl kullanılacağını gösterir. Bu, ASP.NET Core kimliğin tüm özelliklerini gerektirmeyen, ancak yine de güvenilen bir dış kimlik doğrulama sağlayıcısıyla tümleştirme gerektiren uygulamalar için yararlıdır.

Bu örnek, kullanıcıların kimliğini doğrulamak için [Google kimlik doğrulamasını](xref:security/authentication/google-logins) kullanır. Google kimlik doğrulamasını kullanmak, oturum açma işlemini Google 'a yönetmenin karmaşıklıklarından çoğunu kaydırır. Farklı bir dış kimlik doğrulama sağlayıcısıyla tümleştirme için aşağıdaki konulara bakın:

* [Facebook kimlik doğrulaması](xref:security/authentication/facebook-logins)
* [Microsoft kimlik doğrulaması](xref:security/authentication/microsoft-logins)
* [Twitter kimlik doğrulaması](xref:security/authentication/twitter-logins)
* [Diğer sağlayıcılar](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Yapılandırma

`ConfigureServices` yönteminde, uygulamanın kimlik doğrulama düzenlerini <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>ve <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> yöntemleriyle yapılandırın:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> çağrısı, uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme>ayarlar. `DefaultScheme`, aşağıdaki `HttpContext` kimlik doğrulama uzantısı yöntemleri tarafından kullanılan varsayılan şemadır:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Uygulamanın `DefaultScheme`, [ıeauthenticationdefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") olarak ayarlanması, uygulamayı bu uzantı yöntemlerinin varsayılan şeması olarak tanımlama bilgilerini kullanacak şekilde yapılandırır. Uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") olarak ayarlanması, uygulamayı, `ChallengeAsync`çağrıları için varsayılan düzen olarak Google 'ı kullanacak şekilde yapılandırır. `DefaultChallengeScheme` geçersiz kılmaları `DefaultScheme`. Ayarlandığında `DefaultScheme` geçersiz kılan ek özellikler için <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> bakın.

`Startup.Configure`' de, çağırma `UseRouting` ve `UseEndpoints`arasında `UseAuthentication` ve `UseAuthorization` çağırın. Bu, `HttpContext.User` özelliğini ayarlar ve istekler için yetkilendirme ara yazılımını çalıştırır:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

Kimlik doğrulama şemaları hakkında daha fazla bilgi için bkz. [kimlik doğrulama kavramları](xref:security/authentication/index#authentication-concepts). Tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Yetkilendirmeyi uygula

Bir denetleyiciye, eyleme veya sayfaya `AuthorizeAttribute` özniteliğini uygulayarak uygulamanın kimlik doğrulama yapılandırmasını test edin. Aşağıdaki kod, *Gizlilik* sayfasına erişimi doğrulanan kullanıcılarla sınırlandırır:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Oturumu kapat

Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için [Signoutasync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)çağırın. Aşağıdaki kod, *Dizin* sayfasına bir `Logout` sayfa işleyicisi ekler:

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

`SignOutAsync` çağrısının bir kimlik doğrulama düzeni belirtmediğine dikkat edin. Uygulamanın `CookieAuthenticationDefaults.AuthenticationScheme` `DefaultScheme` geri dönüş olarak kullanılır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<xref:security/authentication/social/index>, kullanıcıların dış kimlik doğrulama sağlayıcılarının kimlik bilgileriyle OAuth 2,0 kullanarak oturum açmasını nasıl sağladığını açıklar. Bu konuda açıklanan yaklaşım kimlik doğrulama sağlayıcısı olarak ASP.NET Core kimliğini içerir.

Bu örnek, ASP.NET Core kimliği **olmadan** bir dış kimlik doğrulama sağlayıcısının nasıl kullanılacağını gösterir. Bu, ASP.NET Core kimliğin tüm özelliklerini gerektirmeyen, ancak yine de güvenilen bir dış kimlik doğrulama sağlayıcısıyla tümleştirme gerektiren uygulamalar için yararlıdır.

Bu örnek, kullanıcıların kimliğini doğrulamak için [Google kimlik doğrulamasını](xref:security/authentication/google-logins) kullanır. Google kimlik doğrulamasını kullanmak, oturum açma işlemini Google 'a yönetmenin karmaşıklıklarından çoğunu kaydırır. Farklı bir dış kimlik doğrulama sağlayıcısıyla tümleştirme için aşağıdaki konulara bakın:

* [Facebook kimlik doğrulaması](xref:security/authentication/facebook-logins)
* [Microsoft kimlik doğrulaması](xref:security/authentication/microsoft-logins)
* [Twitter kimlik doğrulaması](xref:security/authentication/twitter-logins)
* [Diğer sağlayıcılar](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Yapılandırma

`ConfigureServices` yönteminde, uygulamanın kimlik doğrulama düzenlerini `AddAuthentication`, `AddCookie`ve `AddGoogle` yöntemleriyle yapılandırın:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

[Addaduthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) çağrısı, uygulamanın [defaultscheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)değerini ayarlar. `DefaultScheme`, aşağıdaki `HttpContext` kimlik doğrulama uzantısı yöntemleri tarafından kullanılan varsayılan şemadır:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Uygulamanın `DefaultScheme`, [ıeauthenticationdefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies") olarak ayarlanması, uygulamayı bu uzantı yöntemlerinin varsayılan şeması olarak tanımlama bilgilerini kullanacak şekilde yapılandırır. Uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") olarak ayarlanması, uygulamayı, `ChallengeAsync`çağrıları için varsayılan düzen olarak Google 'ı kullanacak şekilde yapılandırır. `DefaultChallengeScheme` geçersiz kılmaları `DefaultScheme`. Ayarlandığında `DefaultScheme` geçersiz kılan ek özellikler için <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> bakın.

`Configure` yönteminde, `HttpContext.User` özelliğini ayarlayan kimlik doğrulama ara yazılımını çağırmak için `UseAuthentication` yöntemini çağırın. `UseMvcWithDefaultRoute` veya `UseMvc`çağrılmadan önce `UseAuthentication` yöntemi çağırın:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

Kimlik doğrulama şemaları hakkında daha fazla bilgi için bkz. [kimlik doğrulama kavramları](xref:security/authentication/index#authentication-concepts). Tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Yetkilendirmeyi uygula

Bir denetleyiciye, eyleme veya sayfaya `AuthorizeAttribute` özniteliğini uygulayarak uygulamanın kimlik doğrulama yapılandırmasını test edin. Aşağıdaki kod, *Gizlilik* sayfasına erişimi doğrulanan kullanıcılarla sınırlandırır:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Oturumu kapat

Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için [Signoutasync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)çağırın. Aşağıdaki kod, *Dizin* sayfasına bir `Logout` sayfa işleyicisi ekler:

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

`SignOutAsync` çağrısının bir kimlik doğrulama düzeni belirtmediğine dikkat edin. Uygulamanın `CookieAuthenticationDefaults.AuthenticationScheme` `DefaultScheme` geri dönüş olarak kullanılır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
