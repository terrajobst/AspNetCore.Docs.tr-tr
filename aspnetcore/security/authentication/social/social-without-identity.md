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

@No__t-0 yönteminde, uygulamanın kimlik doğrulama düzenlerini <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>, <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> ve <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> yöntemleriyle yapılandırın:

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet1)]

@No__t-0 ' a yapılan çağrı, uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme> ' i ayarlar. @No__t-0, aşağıdaki `HttpContext` kimlik doğrulama uzantısı yöntemleri tarafından kullanılan varsayılan şemadır:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Uygulamanın `DefaultScheme` ' [yı ("](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) tanımlama bilgileri"), bu uzantı yöntemlerinin varsayılan şeması olarak tanımlama bilgilerini kullanacak şekilde yapılandırır. Uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> ' yı [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") olarak ayarlamak, uygulamayı Google 'ı `ChallengeAsync` ' ye çağrılar için varsayılan düzen olarak kullanacak şekilde yapılandırır. `DefaultChallengeScheme` geçersiz kılmaları `DefaultScheme`. Ayarlandığında, `DefaultScheme` ' i geçersiz kılan ek özellikler için bkz. <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.

@No__t-0 ' da `UseAuthentication` ve `UseAuthorization` ' yi çağırıp `HttpContext.User` özelliğini ayarlayın ve istekler için yetkilendirme ara yazılımını çalıştırın. @No__t-2 ' i çağırmadan önce `UseAuthentication` ve `UseAuthorization` yöntemlerini çağırın:

[!code-csharp[](social-without-identity/3.0sample/Startup.cs?name=snippet2)]

Kimlik doğrulama şemaları ve tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Yetkilendirmeyi uygula

Bir denetleyiciye, eyleme veya sayfaya `AuthorizeAttribute` özniteliğini uygulayarak uygulamanın kimlik doğrulama yapılandırmasını test edin. Aşağıdaki kod, *Gizlilik* sayfasına erişimi doğrulanan kullanıcılarla sınırlandırır:

[!code-csharp[](social-without-identity/3.0sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Oturumu kapat

Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için [Signoutasync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)çağırın. Aşağıdaki kod *Dizin* sayfasına `Logout` sayfa işleyicisi ekler:

[!code-csharp[](social-without-identity/3.0sample/Pages/Index.cshtml.cs?name=snippet&highlight=14-18)]

@No__t-0 ' a yapılan çağrının bir kimlik doğrulama düzeni belirtmediğine dikkat edin. Uygulamanın `DefaultScheme` ' dan `CookieAuthenticationDefaults.AuthenticationScheme` ' in geri dönüş olarak kullanıldığı.

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

@No__t-0 yönteminde, uygulamanın kimlik doğrulama düzenlerini `AddAuthentication`, `AddCookie` ve `AddGoogle` yöntemleriyle yapılandırın:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

[Addaduthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) çağrısı, uygulamanın [defaultscheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)değerini ayarlar. @No__t-0, aşağıdaki `HttpContext` kimlik doğrulama uzantısı yöntemleri tarafından kullanılan varsayılan şemadır:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Uygulamanın `DefaultScheme` ' [yı ("](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) tanımlama bilgileri"), bu uzantı yöntemlerinin varsayılan şeması olarak tanımlama bilgilerini kullanacak şekilde yapılandırır. Uygulamanın <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> ' yı [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google") olarak ayarlamak, uygulamayı Google 'ı `ChallengeAsync` ' ye çağrılar için varsayılan düzen olarak kullanacak şekilde yapılandırır. `DefaultChallengeScheme` geçersiz kılmaları `DefaultScheme`. Ayarlandığında, `DefaultScheme` ' i geçersiz kılan ek özellikler için bkz. <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>.

@No__t-0 yönteminde, `HttpContext.User` özelliğini ayarlayan kimlik doğrulama ara yazılımını çağırmak için `UseAuthentication` yöntemini çağırın. @No__t-1 veya `UseMvc` ' i çağırmadan önce `UseAuthentication` yöntemini çağırın:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

Kimlik doğrulama şemaları ve tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz. <xref:security/authentication/cookie>.

## <a name="apply-authorization"></a>Yetkilendirmeyi uygula

Bir denetleyiciye, eyleme veya sayfaya `AuthorizeAttribute` özniteliğini uygulayarak uygulamanın kimlik doğrulama yapılandırmasını test edin. Aşağıdaki kod, *Gizlilik* sayfasına erişimi doğrulanan kullanıcılarla sınırlandırır:

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Oturumu kapat

Geçerli kullanıcının oturumunu kapatmak ve tanımlama bilgilerini silmek için [Signoutasync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)çağırın. Aşağıdaki kod *Dizin* sayfasına `Logout` sayfa işleyicisi ekler:

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

@No__t-0 ' a yapılan çağrının bir kimlik doğrulama düzeni belirtmediğine dikkat edin. Uygulamanın `DefaultScheme` ' dan `CookieAuthenticationDefaults.AuthenticationScheme` ' in geri dönüş olarak kullanıldığı.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
