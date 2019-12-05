---
title: ASP.NET Core kimlik doğrulamasına genel bakış
author: mjrousos
description: ASP.NET Core kimlik doğrulaması hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 12/04/2019
uid: security/authentication/index
ms.openlocfilehash: 324b2669d3b69e4757a284e4ae7e1de5f4e87e5a
ms.sourcegitcommit: 05ca05a5c8f6ae556aaad66ad9e4ec1e6b643c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74810244"
---
# <a name="overview-of-aspnet-core-authentication"></a>ASP.NET Core kimlik doğrulamasına genel bakış

, [Mike Rousos](https://github.com/mjrousos) tarafından

Kimlik doğrulaması, bir kullanıcının kimliğini belirleme işlemidir. [Yetkilendirme](xref:security/authorization/introduction) , bir kullanıcının bir kaynağa erişip erişemeyeceğini belirleme işlemidir. ASP.NET Core, kimlik doğrulaması, kimlik doğrulama [ara yazılımı](xref:fundamentals/middleware/index)tarafından kullanılan `IAuthenticationService`tarafından işlenir. Kimlik doğrulama hizmeti, kimlik doğrulama ile ilgili eylemleri tamamlamaya yönelik kayıtlı kimlik doğrulama işleyicilerini kullanır. Kimlik doğrulaması ile ilgili eylemlere örnek olarak şunlar verilebilir:

* Kullanıcı kimlik doğrulaması.
* Kimliği doğrulanmamış bir Kullanıcı kısıtlı bir kaynağa erişmeyi denediğinde yanıt verir.

Kayıtlı kimlik doğrulama işleyicileri ve yapılandırma seçenekleri "şemalar" olarak adlandırılır.

Kimlik doğrulama düzenleri `Startup.ConfigureServices`kimlik doğrulama hizmetleri kaydedilerek belirtilir:

* Bir `services.AddAuthentication` çağrısından sonra düzene özgü bir genişletme yöntemini çağırarak (örneğin `AddJwtBearer` veya `AddCookie`gibi). Bu uzantı yöntemleri, uygun ayarlarla şemaları kaydetmek için [Authenticationbuilder. AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*) kullanır.
* Daha seyrek, [Authenticationbuilder. AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*) doğrudan çağırarak.

Örneğin, aşağıdaki kod, tanımlama bilgisi ve JWT taşıyıcı kimlik doğrulama şemaları için kimlik doğrulama hizmetlerini ve işleyicileri kaydeder:

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(JwtBearerDefaults.AuthenticationScheme, options => Configuration.Bind("JwtSettings", options))
    .AddCookie(CookieAuthenticationDefaults.AuthenticationScheme, options => Configuration.Bind("CookieSettings", options));
```

`AddAuthentication` parametresi `JwtBearerDefaults.AuthenticationScheme`, belirli bir düzen istenmediğinde varsayılan olarak kullanılacak düzenin adıdır.

Birden çok düzen kullanılırsa, yetkilendirme ilkeleri (veya yetkilendirme öznitelikleri) kullanıcının kimliğini doğrulamak için bağımlı oldukları [kimlik doğrulama düzenini (veya düzenleri) belirtebilir](xref:security/authorization/limitingidentitybyscheme) . Yukarıdaki örnekte, tanımlama bilgisi kimlik doğrulama düzeni adı belirtilerek kullanılabilir (varsayılan olarak`CookieAuthenticationDefaults.AuthenticationScheme`, ancak `AddCookie`çağrılırken farklı bir ad sağlandıysa).

Bazı durumlarda `AddAuthentication` çağrısı diğer uzantı yöntemleri tarafından otomatik olarak yapılır. Örneğin, [ASP.NET Core kimliği](xref:security/authentication/identity)kullanılırken,`AddAuthentication` dahili olarak çağrılır.

Kimlik doğrulama ara yazılımı, uygulamanın `IApplicationBuilder`<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> uzantısı yöntemi çağırarak `Startup.Configure` eklenir. `UseAuthentication` çağırmak, daha önce kayıtlı kimlik doğrulama düzenlerini kullanan ara yazılımı kaydeder. Kimliği doğrulanan kullanıcılara bağlı olan tüm ara yazılımlar için `UseAuthentication` çağırın. Endpoint Routing kullanılırken, `UseAuthentication` çağrısı şu şekilde olmalıdır:

* `UseRouting`sonra, kimlik doğrulama kararlarında yol bilgileri kullanılabilir.
* `UseEndpoints`önce, kullanıcıların, uç noktalara erişmeden önce kimlik doğrulaması yapılır.

## <a name="authentication-concepts"></a>Kimlik doğrulama kavramları

### <a name="authentication-scheme"></a>Kimlik doğrulama düzeni

Kimlik doğrulama düzeni, öğesine karşılık gelen bir addır:

* Bir kimlik doğrulama işleyicisi.
* İşleyicinin belirli bir örneğini yapılandırma seçenekleri.

Şemaları, ilişkili işleyicinin kimlik doğrulama, sınama ve fordeklarasyonu davranışlarına başvurma mekanizması olarak faydalıdır. Örneğin, bir yetkilendirme ilkesi, kullanıcının kimliğini doğrulamak için hangi kimlik doğrulama şemasının (veya düzenlerinin) kullanılması gerektiğini belirtmek için şema adlarını kullanabilir. Kimlik doğrulamasını yapılandırırken, varsayılan kimlik doğrulama düzenini belirlemek yaygın bir hale gelir. Varsayılan şema, bir kaynak belirli bir düzeni istemediği takdirde kullanılır. Şunları da mümkündür:

* Kimlik doğrulama, sınama ve fordeklarasyon eylemleri için kullanılacak farklı varsayılan şemaları belirtin.
* Birden çok düzeni [ilke düzenlerini](xref:security/authentication/policyschemes)kullanarak bir tek birleştirme.

### <a name="authentication-handler"></a>Kimlik doğrulama işleyicisi

Kimlik doğrulama işleyicisi:

* , Bir düzenin davranışını uygulayan bir türdür.
* <xref:Microsoft.AspNetCore.Authentication.IAuthenticationHandler> veya <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>türetilir.
* , Kullanıcıların kimliğini doğrulamak için Birincil sorumluluğa sahiptir.

Kimlik doğrulama düzeninin yapılandırmasına ve gelen istek bağlamına göre, kimlik doğrulama işleyicileri:

* Kimlik doğrulaması başarılı olursa kullanıcının kimliğini temsil eden <xref:Microsoft.AspNetCore.Authentication.AuthenticationTicket> nesneleri oluşturun.
* Kimlik doğrulaması başarısız olursa ' No Result ' veya ' Failure ' döndürün.
* Kullanıcıların kaynaklara erişmeyi deneyeceği işlemler ve yasaklamaya yönelik yöntemlere sahiptir:
  * Bunlara erişim yetkisi yoktur (fordeklarasyon).
  * Kimliği doğrulanmamış olduğunda (zorluk).

### <a name="authenticate"></a>Kimlik doğrulama

Kimlik doğrulama düzeninin kimlik doğrulama eylemi, kullanıcının kimliğini istek bağlamına göre oluşturmaktan sorumludur. Kimlik doğrulamanın başarılı olup olmadığını ve bu durumda kullanıcının kimlik doğrulama biletinde kimliğini belirten bir <xref:Microsoft.AspNetCore.Authentication.AuthenticateResult> döndürür. Bkz. HttpContext. doğrubir ekip eşitleme. Kimlik doğrulama örnekleri şunları içerir:

* Tanımlama bilgisi kimlik doğrulama şeması kullanıcının kimlik bilgilerinden kimliğini oluşturan.
* Bir JWT taşıyıcı şeması, kullanıcının kimliğini oluşturmak için bir JWT taşıyıcı belirtecini seri durumdan çıkarma ve doğrulama.

### <a name="challenge"></a>Sınama

Kimliği doğrulanmamış bir kullanıcı kimlik doğrulaması gerektiren bir uç nokta istediğinde yetkilendirme ile kimlik doğrulama sınaması çağrılır. Örneğin, anonim bir Kullanıcı kısıtlı bir kaynak istediğinde veya bir oturum açma bağlantısına tıkladığı zaman bir kimlik doğrulama sınaması verilir. Yetkilendirme, belirtilen kimlik doğrulama düzenlerini kullanarak bir zorluk çağırır ya da hiçbiri belirtilmemişse varsayılan değer. Bkz. HttpContext. bir Geasync. Kimlik doğrulama sınaması örnekleri şunları içerir:

* Bir tanımlama bilgisi kimlik doğrulama şeması kullanıcıyı bir oturum açma sayfasına yönlendiriyor.
* Bir `www-authenticate: bearer` üst bilgisiyle 401 sonucu döndüren JWT taşıyıcı şeması.

Bir sınama eylemi, kullanıcının istenen kaynağa erişmek için hangi kimlik doğrulama mekanizmasını kullanacağınızı bilmesine izin verir.

### <a name="forbid"></a>Yasaklamaz

Kimliği doğrulanmış bir kullanıcı erişimine izin verilmeyen bir kaynağa erişmeyi denediğinde kimlik doğrulama düzeninin fordeklarasyon eylemi yetkilendirme tarafından çağrılır. Bkz. HttpContext. ForbidAsync. Kimlik doğrulama fordeklarasyon örnekleri şunları içerir:
* Bir tanımlama bilgisi kimlik doğrulama şeması, kullanıcıyı erişim yasak bir sayfaya yönlendirdi.
* 403 sonucunu döndüren bir JWT taşıyıcı şeması.
* Özel bir kimlik doğrulama şeması, kullanıcının kaynağa erişim isteyebileceği bir sayfaya yeniden yönlendiriliyor.

Bir fordeklarasyon eylemi, kullanıcının şunları bilmesini sağlayabilir:

* Bunlar doğrulanır.
* İstenen kaynağa erişmelerine izin verilmez.

Challenge ve fordeklarasyon arasındaki farklılıklar için aşağıdaki bağlantılara bakın:

* [İşletimsel kaynak Işleyicisiyle Challenge ve fordeklarasyonu](xref:security/authorization/resourcebased#challenge-and-forbid-with-an-operational-resource-handler).
* [Çekişme ve fordeklarasyon arasındaki farklar](xref:security/authorization/secure-data#challenge).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authentication/policyschemes>
* <xref:security/authorization/secure-data>
