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
# <a name="use-social-sign-in-provider-authentication-without-aspnet-core-identity"></a>ASP.NET Core kimliği olmadan sosyal oturum açma sağlayıcısı kimlik doğrulaması kullan

<xref:security/authentication/social/index> OAuth 2.0 ile dış kimlik sağlayıcılarına ait kimlik bilgilerini kullanarak oturum açmasına etkinleştirmeyi açıklar. Bu konuda açıklanan yaklaşımı, ASP.NET Core kimliği bir kimlik doğrulama sağlayıcısı olarak içerir.

Bu örnek, bir dış kimlik doğrulama sağlayıcısının nasıl kullanılacağını gösterir. **olmadan** ASP.NET Core kimliği. Bu, tüm ASP.NET Core kimliği özelliklerini gerektirmez, ancak yine de güvenilir bir dış kimlik doğrulama sağlayıcısı ile tümleştirme gerektiren uygulamalar için kullanışlıdır.

Bu örnekte [Google kimlik doğrulaması](xref:security/authentication/google-logins) kullanıcıların kimlik doğrulaması için. Google'ı kullanarak kimlik doğrulaması birçok Google oturum açma işlemi yönetmenin karmaşıklığını kaydırır. Bir farklı Dış kimlik doğrulama sağlayıcısı ile tümleştirmek için aşağıdaki konulara bakın:

* [Facebook kimlik doğrulaması](xref:security/authentication/facebook-logins)
* [Microsoft kimlik doğrulaması](xref:security/authentication/microsoft-logins)
* [Twitter kimlik doğrulaması](xref:security/authentication/twitter-logins)
* [Diğer sağlayıcılar](xref:security/authentication/otherlogins)

## <a name="configuration"></a>Yapılandırma

İçinde `ConfigureServices` yöntemi, uygulamanın kimlik doğrulama şeması ile yapılandırma `AddAuthentication`, `AddCookie` ve `AddGoogle` yöntemleri:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet1)]

Çağrı [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) uygulamanın ayarlar [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme). `DefaultScheme` Aşağıdaki tarafından kullanılan varsayılan düzen olan `HttpContext` kimlik doğrulaması için genişletme yöntemleri:

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

Uygulama ayarı `DefaultScheme` için [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("tanımlama bilgileri") tanımlama bilgisi bu genişletme yöntemleri için varsayılan düzeni olarak kullanmak üzere uygulamayı yapılandırır. Uygulama ayarı <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> için [GoogleDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ("Google"), Google, çağrılar için varsayılan düzeni olarak kullanmak üzere uygulamayı yapılandırır `ChallengeAsync`. `DefaultChallengeScheme` geçersiz kılmalar `DefaultScheme`. Bkz: <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions> geçersiz ek özellik `DefaultScheme` ayarlandığında.

İçinde `Configure` yöntemi, çağrı `UseAuthentication` ayarlar kimlik doğrulaması ara yazılımı çağrılacak yöntem `HttpContext.User` özelliği. Çağrı `UseAuthentication` yöntemi çağırmadan önce `UseMvcWithDefaultRoute` veya `UseMvc`:

[!code-csharp[](social-without-identity/sample/Startup.cs?name=snippet2)]

Kimlik doğrulama düzenleri ve tanımlama bilgisi kimlik doğrulaması hakkında daha fazla bilgi için bkz: <xref:security/authentication/cookie>.

## <a name="applying-basic-authorization"></a>Temel uygulama yetkilendirme

Uygulamanın kimlik doğrulama yapılandırmasına uygulayarak test `AuthorizeAttribute` öznitelik bir denetleyicide, eylemde veya sayfa. Aşağıdaki kod erişimi sınırlar *gizlilik* doğrulanan kullanıcılara sayfası:

[!code-csharp[](social-without-identity/sample/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>Oturumu kapat

Geçerli kullanıcının oturumunu kapatmaz ve kendi tanımlama bilgisini silmek için çağrı [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0). Aşağıdaki kodu ekler bir `Logout` sayfa işleyicisine *dizin* sayfası:

[!code-csharp[](social-without-identity/sample/Pages/Index.cshtml.cs?name=snippet&highlight=7-11)]

Dikkat çağrısı `SignOutAsync` bir kimlik doğrulama düzeni belirtmiyor. Uygulamanın `DefaultScheme` , `CookieAuthenticationDefaults.AuthenticationScheme` bir sıfırlamaya kullanılır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>
