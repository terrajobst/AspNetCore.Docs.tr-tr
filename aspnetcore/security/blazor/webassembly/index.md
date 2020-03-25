---
title: Güvenli ASP.NET Core Blazor WebAssembly
author: guardrex
description: Blazor WebAssemlby uygulamalarını tek sayfalı uygulamalar (maça 'Lar) olarak güvenli hale getirme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/index
ms.openlocfilehash: 652d4c61110f786396d9d5af4f131b817c40e333
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219252"
---
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a>Güvenli ASP.NET Core Blazor WebAssembly

Sağlayan [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Blazor WebAssembly uygulamalarının tek sayfalı uygulamalarla (maça) aynı şekilde güvenliği sağlanır. Kullanıcıların maça üzerinde kimlik doğrulaması için birkaç yaklaşım vardır, ancak en yaygın ve kapsamlı yaklaşım, [Open ID Connect (OıDC)](https://openid.net/connect/)gibi [OAuth 2,0 protokolüne](https://oauth.net/)dayalı bir uygulama kullanmaktır.

## <a name="authentication-library"></a>Kimlik doğrulama kitaplığı

Blazor WebAssembly, `Microsoft.AspNetCore.Components.WebAssembly.Authentication` kitaplığı aracılığıyla OıDC kullanarak uygulamaları kimlik doğrulama ve yetkilendirme işlemini destekler. Kitaplığı ASP.NET Core arka uçlara karşı sorunsuz kimlik doğrulama için bir dizi temel sağlar. Kitaplık, [kimlik sunucusu](https://identityserver.io/)ÜZERINDE oluşturulan API yetkilendirme desteğiyle ASP.NET Core kimliğini tümleştirir. Kitaplık, OpenID sağlayıcıları (OP) olarak adlandırılan OıDC 'yi destekleyen herhangi bir üçüncü taraf kimlik sağlayıcısında (IP) kimlik doğrulaması yapabilir.

Blazor WebAssembly ' deki kimlik doğrulama desteği, temeldeki kimlik doğrulama protokolü ayrıntılarını işlemek için kullanılan *oidc-Client. js* kitaplığının üzerine kurulmuştur.

Benzer site tanımlama bilgilerinin kullanımı gibi, maça 'Ları doğrulamak için diğer seçenekler. Ancak, Blazor WebAssembly 'nin mühendislik tasarımı, Blazor WebAssembly uygulamalarında kimlik doğrulaması için en iyi seçenek olarak oAuth ve OıDC üzerinde kapatılmıştır. JSON Web belirteçlerini temel alan [belirteç tabanlı kimlik doğrulaması](xref:security/anti-request-forgery#token-based-authentication) [(jwts)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) , işlevsel ve güvenlik nedenleriyle [tanımlama bilgisi tabanlı kimlik doğrulaması](xref:security/anti-request-forgery#cookie-based-authentication) üzerinden seçilmiştir:

* Belirteç tabanlı bir protokol kullanmak, belirteçler tüm isteklerde gönderilmediğinden daha küçük bir saldırı yüzeyi alanı sunar.
* Belirteçleri açıkça gönderildiğinden, sunucu uç noktaları [siteler arası Istek sahteciliği (CSRF)](xref:security/anti-request-forgery) için koruma gerektirmez. Bu, MVC veya Razor sayfaları uygulamalarıyla birlikte Blazor Weelsembly uygulamalarını barındırmanıza olanak tanır.
* Belirteçlerin tanımlama bilgilerinden daha dar izinleri vardır. Örneğin, belirteçler Kullanıcı hesabını yönetmek ya da bu işlevsellik açık bir şekilde uygulanmadığı takdirde kullanıcının parolasını değiştirmek için kullanılamaz.
* Belirteçler, varsayılan olarak bir saat, saldırı penceresini sınırlayan kısa bir yaşam süresine sahiptir. Belirteçler de herhangi bir zamanda iptal edilebilir.
* İstemci ve sunucu için kimlik doğrulama işlemiyle ilgili olarak kendi içinde bulunan JWTs teklifi garantisi. Örneğin, bir istemci, aldığı belirteçlerin meşru olduğunu ve belirli bir kimlik doğrulama işleminin bir parçası olarak yayıldığını tespit etmek ve doğrulamak anlamına gelir. Üçüncü bir taraf, kimlik doğrulama işleminin ortasında bir belirteci geçirmeyi denerse, istemci anahtarlamalı belirteci algılayabilir ve kullanmaktan kaçınabilir.
* OAuth ve OıDC içeren belirteçler, uygulamanın güvenli olduğundan emin olmak için doğru şekilde davranan kullanıcı aracısına güvenmeyin.
* OAuth ve OıDC gibi belirteç tabanlı protokoller aynı güvenlik özellikleriyle barındırılan ve tek başına uygulamaların kimliğini doğrulamaya ve yetkilendirmeye olanak tanır.

## <a name="authentication-process-with-oidc"></a>OıDC ile kimlik doğrulama işlemi

`Microsoft.AspNetCore.Components.WebAssembly.Authentication` kitaplığı, OıDC kullanarak kimlik doğrulaması ve yetkilendirme uygulamak için çeşitli temel olanaklar sunar. Büyük şartlar altında, kimlik doğrulaması aşağıdaki gibi çalışmaktadır:

* Anonim Kullanıcı oturum açma düğmesini seçtiğinde veya [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) özniteliği uygulanmış bir sayfa istediğinde, kullanıcı uygulamanın oturum açma sayfasına (`/authentication/login`) yönlendirilir.
* Oturum açma sayfasında, kimlik doğrulama kitaplığı yetkilendirme uç noktasına yeniden yönlendirme hazırlar. Yetkilendirme uç noktası, Blazor WebAssembly uygulamasının dışındadır ve ayrı bir kaynak üzerinde barındırılabilir. Uç nokta, kullanıcının kimliğinin yapılıp yapılmayacağını ve yanıt olarak bir veya daha fazla belirteç vermeyi belirlemekten sorumludur. Kimlik doğrulama kitaplığı, kimlik doğrulama yanıtını almak için bir oturum açma geri araması sağlar.
  * Kullanıcının kimliği doğrulanmadıysa, Kullanıcı, genellikle ASP.NET Core kimlik olan temel alınan kimlik doğrulama sistemine yönlendirilir.
  * Kullanıcının kimliği zaten doğrulandıktan sonra, yetkilendirme uç noktası uygun belirteçleri oluşturur ve tarayıcıyı oturum açma geri çağırma uç noktasına (`/authentication/login-callback`) yeniden yönlendirir.
* Blazor WebAssembly uygulaması, oturum açma geri çağırma uç noktasını (`/authentication/login-callback`) yüklediğinde, kimlik doğrulama yanıtı işlenir.
  * Kimlik doğrulama işlemi başarıyla tamamlanırsa, kullanıcının kimliği doğrulanır ve isteğe bağlı olarak kullanıcının istediği özgün URL 'ye geri gönderilir.
  * Kimlik doğrulama işlemi herhangi bir nedenle başarısız olursa, Kullanıcı oturum açma başarısız sayfasına gönderilir (`/authentication/login-failed`) ve bir hata görüntülenir.
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a>Barındırılan uygulamalar ve üçüncü taraf oturum açma sağlayıcıları için seçenekler

Bir üçüncü taraf sağlayıcı ile barındırılan bir Blazor WebAssembly uygulaması kimlik doğrulaması ve yetkilendirirken, kullanıcının kimliğini doğrulamak için kullanabileceğiniz çeşitli seçenekler vardır. Seçtiğiniz bir senaryo, senaryonuza bağlıdır.

Daha fazla bilgi için bkz. <xref:security/authentication/social/additional-claims>.

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a>Yalnızca korumalı üçüncü taraf API 'Leri çağırmak için kullanıcıların kimliğini doğrulama

Üçüncü taraf API sağlayıcısına karşı, istemci tarafı oAuth akışı ile kullanıcının kimliğini doğrulayın:

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 Bu senaryoda:

* Uygulamayı barındıran sunucu bir rol oynamıyor.
* Sunucudaki API 'Ler korunamaz.
* Uygulama yalnızca korunan üçüncü taraf API 'Leri çağırabilir.

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a>Bir üçüncü taraf sağlayıcı ile kullanıcıların kimliğini doğrulama ve konak sunucusunda ve üçüncü taraftan korunan API 'Leri çağırma

Kimliği bir üçüncü taraf oturum açma sağlayıcısıyla yapılandırın. Üçüncü taraf API erişimi için gereken belirteçleri edinin ve bunları depolayın.

Bir Kullanıcı oturum açtığında kimlik doğrulama işleminin bir parçası olarak, kimlik, erişimi toplar ve belirteçleri yenileyebilir. Bu noktada, üçüncü taraf API 'lere yönelik API çağrıları yapmak için kullanabileceğiniz birkaç yaklaşım vardır.

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a>Üçüncü taraf erişim belirtecini almak için bir sunucu erişim belirteci kullanın

Sunucu API uç noktasından üçüncü taraf erişim belirtecini almak için sunucuda oluşturulan erişim belirtecini kullanın. Buradan, üçüncü taraf API kaynaklarını doğrudan istemcideki kimlikle çağırmak için üçüncü taraf erişim belirtecini kullanın.

Bu yaklaşımı önermiyoruz. Bu yaklaşım, üçüncü taraf erişim belirtecinin ortak bir istemci için oluşturulmuş gibi davranılması gerektirir. OAuth koşullarında, genel uygulamanın gizli dizileri güvenli bir şekilde depolamak için güvenli hale gelmediği için bir istemci parolası yoktur ve erişim belirteci gizli bir istemci için oluşturulur. Gizli bir istemci, bir istemci gizli anahtarı olan bir istemcsahiptir ve gizli dizileri güvenli bir şekilde depolayabilecek varsayılır.

* Üçüncü taraf erişim belirtecine, üçüncü tarafın daha güvenilir bir istemcinin belirtecini kendine yayıldığından emin olmak için, hassas işlemleri gerçekleştirmek üzere ek kapsamlar verilebilir.
* Benzer şekilde, yenileme belirteçleri güvenilir olmayan bir istemciye verilmemelidir, aksi takdirde başka kısıtlamalar yerleştirilmediği sürece istemciye sınırsız erişim sağlar.

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a>Üçüncü taraf API 'Leri çağırmak için istemciden sunucu API 'sine API çağrıları yapın

İstemciden sunucu API 'sine bir API çağrısı yapın. Sunucudan, üçüncü taraf API kaynağı için erişim belirtecini alın ve hangi çağrının gerekli olduğunu sorun.

Bu yaklaşım, bir üçüncü taraf API çağrısı yapmak için sunucu aracılığıyla fazladan bir ağ atlaması gerektirdiğinden, sonunda daha güvenli bir deneyim oluşur:

* Sunucu yenileme belirteçlerini saklayabilir ve uygulamanın üçüncü taraf kaynaklarına erişimi kaybetmemesini sağlayabilir.
* Uygulama, daha hassas izinler içerebilen sunucudan erişim belirteçlerini sızıntısına neden olabilir.
