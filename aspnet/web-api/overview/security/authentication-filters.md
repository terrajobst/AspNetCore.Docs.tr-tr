---
uid: web-api/overview/security/authentication-filters
title: ASP.NET Web API 2'de kimlik doğrulama filtreleri | Microsoft Docs
author: MikeWasson
description: Bir kimlik doğrulaması filtresini, HTTP isteği yapan bir bileşenidir. Web API 2 ve MVC 5 kimlik doğrulama filtreleri her ikisini de destekler, ancak bunlar biraz farklı...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: be2dcb246597f90ed7f00b2cf647b92e44aa254c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385683"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>ASP.NET Web API 2'de kimlik doğrulama filtreleri
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Bir kimlik doğrulaması filtresini, HTTP isteği yapan bir bileşenidir. Web API 2 ve MVC 5 kimlik doğrulama filtreleri her ikisini de destekler, ancak bunlar çoğunlukla filtre arabirimi için adlandırma kuralları olarak biraz farklılık. Bu konuda, Web API'si kimlik doğrulama filtreleri açıklanmaktadır.


Kimlik doğrulaması filtreleri bireysel denetleyicileri veya eylemler için bir kimlik doğrulama düzeni ayarlamanıza olanak tanır. Böylece, uygulamanızı farklı HTTP kaynaklar için farklı kimlik doğrulama mekanizmaları destekler.

Bu makalede, ı koddan göstereceğiz [temel kimlik doğrulaması](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) örneğine [ http://aspnet.codeplex.com ](http://aspnet.codeplex.com). Örnek HTTP temel erişimi kimlik doğrulaması şeması (RFC 2617) uygulayan bir kimlik doğrulaması filtresini gösterir. Filtre adlı bir sınıfta uygulandığını `IdentityBasicAuthenticationAttribute`. Ben tüm örnek kodu bir kimlik doğrulaması filtresini yazmak nasıl çalışılacağını bölümleri gösterilmez.

## <a name="setting-an-authentication-filter"></a>Bir kimlik doğrulaması filtresini ayarlama

Diğer filtreleri gibi kimlik doğrulama filtreleri denetleyici başına uygulanan, eylem başına veya genel olarak tüm Web APİ'si denetleyicilerinin olabilir.

Bir denetleyici için bir kimlik doğrulaması filtresini uygulamak için filtre özniteliği ile denetleyici sınıfı süslemek. Aşağıdaki kod kümeleri `[IdentityBasicAuthentication]` tüm denetleyicinin eylemler için temel kimlik doğrulaması sağlayan bir denetleyici sınıfı filtre.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Bir eylem için filtre uygulamak için eylem filtreyle süslemek. Aşağıdaki kod kümeleri `[IdentityBasicAuthentication]` denetleyicinin filtre `Post` yöntemi.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Tüm Web APİ'si denetleyicilerinin için filtre uygulamak için eklemeniz **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Bir Web API'si kimlik doğrulama filtre uygulama

Web API'de kimlik doğrulaması filtreleri uygulamak [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) arabirimi. Ayrıca devralındığı **System.Attribute**öznitelikler uygulanması için.

**IAuthenticationFilter** arabirimi iki yöntem vardır:

- **AuthenticateAsync** isteğin istek kimlik bilgilerini doğrulayarak varsa kimliğini doğrular.
- **ChallengeAsync** gerekirse HTTP yanıt, bir kimlik doğrulaması sınaması ekler.

Bu yöntemler tanımlanan kimlik doğrulama akışı karşılık [RFC 2612](http://tools.ietf.org/html/rfc2616) ve [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. İstemci kimlik bilgileri yetkilendirme üst bilgisinde gönderir. Bu durum, genellikle istemcinin sunucudan bir 401 (yetkisiz) yanıt aldıktan sonra gerçekleşir. Ancak, bir istemci, yalnızca bir 401 aldıktan sonra herhangi bir isteği ile kimlik bilgileri gönderebilirsiniz.
2. Sunucu kimlik bilgileri kabul etmez 401 (yetkisiz) bir yanıt döndürür. Yanıt, bir veya daha fazla sorunlarını içeren bir Www-Authenticate üstbilgisi içeriyor. Her sınama sunucu tarafından tanınan bir kimlik doğrulama düzeni belirtir.

Sunucu bir anonim isteğinden 401 geri dönebilirsiniz. Aslında, genellikle kimlik doğrulama işlemi nasıl başlatılır olmasıdır:

1. İstemci, anonim bir istek gönderir.
2. Sunucu 401 döndürür.
3. İstemciler, istekle birlikte kimlik bilgileri yeniden gönderir.

Bu akış her ikisini de içeren *kimlik doğrulaması* ve *yetkilendirme* adımları.

- Kimlik doğrulaması, istemci kimliğini kanıtlar.
- Yetkilendirme, bir istemci belirli bir kaynağa erişip erişemeyeceğini belirler.

Web API'de kimlik doğrulaması, ancak değil yetkilendirme kimlik doğrulaması filtreleri işleyin. Yetkilendirme denetleyici eylemi içinde veya bir yetkilendirme Filtresi tarafından yapılması gerekir.

Web API 2 kanaldaki akışı şöyledir:

1. Web API'si, bir eylem çağırmadan önce bu eylemi için kimlik doğrulama filtrelerinin listesi oluşturur. Bu eylem kapsamını, denetleyicisi kapsamı ve genel kapsam filtrelerle içerir.
2. Web API çağrıları **AuthenticateAsync** her filtre listesinde üzerinde. Her filtre istekteki kimlik doğrulayabilir. Herhangi bir filtre başarıyla kimlik bilgilerini doğrular, filtre oluşturur. bir **IPrincipal** ve isteği ekler. Bir filtre da bu noktada hata tetikleyebilirsiniz. Bu durumda, kalan ardışık düzenini çalıştırmaz.
3. Hiçbir hata olmadığını varsayarsak, istek ardışık düzenin rest üzerinden akar.
4. Son olarak, her kimlik doğrulama filtrenin Web API'si çağıran **ChallengeAsync** yöntemi. Filtreler bu yöntem bir challenge, yanıt eklemek için gerekirse kullanın. Genellikle (ama her zaman kullanılmaz), sorun 401 hatası için yanıt olacaktır.

Aşağıdaki diyagramlarda, iki olası durumların gösterir. İlk kimlik doğrulaması filtresini istek kimliğini başarıyla doğrulayan ve bir yetkilendirme filtresi isteği yetkilendirir denetleyici eylemini 200 (Tamam) döndürür.

![](authentication-filters/_static/image1.png)

İkinci örnekte, kimlik doğrulaması filtresini isteğin kimliğini doğrular, ancak yetkilendirme filtresini 401 (yetkisiz) döndürür. Bu durumda, denetleyici eylem çağrılmaz. Kimlik doğrulaması filtresini bir Www-Authenticate üstbilgisi yanıta ekler.

![](authentication-filters/_static/image2.png)

Diğer olası birleşimleridir&mdash;denetleyici eylemini anonim isteklere izin veriyorsa, örneğin, bir kimlik doğrulaması filtresini ancak hiçbir kimlik olabilir.

## <a name="implementing-the-authenticateasync-method"></a>AuthenticateAsync yöntemi uygulama

**AuthenticateAsync** yöntemi isteğin kimliğini dener. Aşağıda, yöntem imzası verilmiştir:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**AuthenticateAsync** yöntemi aşağıdakilerden birini yapması gerekir:

1. Nothing (İşlemsiz).
2. Oluşturma bir **IPrincipal** ve istek üzerinde ayarlanan.
3. Bir hata sonucu ayarlayın.

(1) seçeneği anlamına gelir istek filtresi anlayan herhangi bir kimlik bilgisi yok. Seçeneği (2) yol filtresi, isteği başarıyla kimlik doğrulaması. Bir hata yanıtı tetikler (3) seçeneği anlamına gelir isteği, geçersiz kimlik bilgileri (yanlış parola gibi), vardı.

Uygulama için bir genel anahat işte **AuthenticateAsync**.

1. İstekteki kimlik bilgilerini arayın.
2. Kimlik bilgisi varsa, hiçbir şey yapma ve (İşlemsiz) döndürür.
3. Kimlik bilgileri vardır, ancak filtre kimlik doğrulama şeması tanımıyor, hiçbir şey yapma ve (İşlemsiz) döndürür. Başka bir filtre ardışık düzen anlamak.
4. Filtre anlayan kimlik bilgileri varsa, bunları kimliğini doğrulamak deneyin.
5. Kimlik bilgileri hatalı ise 401 ayarlayarak döndürün `context.ErrorResult`.
6. Kimlik bilgilerinin geçerli olduğundan, oluşturun bir **IPrincipal** ayarlayıp `context.Principal`.

Aşağıdaki kodun gösterdiği **AuthenticateAsync** yönteminden [temel kimlik doğrulaması](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) örnek. Her adım yorumları belirtin. Birden fazla hata kodları göstermektedir: bir yetkilendirme üst bilgisi olmayan kimlik bilgilerini, hatalı biçimlendirilmiş kimlik bilgilerini ve hatalı kullanıcı adı/parola.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Bir hata sonucu ayarlama

Kimlik bilgileri geçersiz olduğunda filtre ayarlamalısınız `context.ErrorResult` için bir **Ihttpactionresult** bir hata yanıtı oluşturur. Hakkında daha fazla bilgi için **Ihttpactionresult**, bkz: [Web API 2'de eylem sonuçları](../getting-started-with-aspnet-web-api/action-results.md).

Temel kimlik doğrulaması örneği içeren bir `AuthenticationFailureResult` bu amaç için uygun olan sınıfı.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>ChallengeAsync uygulama

Amacı **ChallengeAsync** yöntemdir yanıt, kimlik doğrulama sınaması eklemeniz gerekirse. Aşağıda, yöntem imzası verilmiştir:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Her istek ardışık düzeninde kimlik doğrulaması filtresini yöntemi çağrılır.

Kavramak önemlidir **ChallengeAsync** çağrılır *önce* oluşturulan ve denetleyici eylemi çalıştırılmadan önce büyük olasılıkla bile HTTP yanıtı. Zaman **ChallengeAsync** çağrıldığında `context.Result` içeren bir **Ihttpactionresult**, daha sonra HTTP yanıtı oluşturmak için kullanılır. Bu nedenle **ChallengeAsync** olan çağrılır, HTTP yanıtı ilgili hiçbir şeyi henüz bilmiyorsanız önemli değildir. **ChallengeAsync** yöntemi orijinal değerini değiştirin `context.Result` yeni bir **Ihttpactionresult**. Bu **Ihttpactionresult** özgün sarmalamanız gerekir `context.Result`.

![](authentication-filters/_static/image3.png)

Özgün adını veriyorum **Ihttpactionresult** *iç sonucu*ve yeni **Ihttpactionresult** *dış sonucu*. Dış sonucu aşağıdakileri yapmalısınız:

1. HTTP yanıtı oluşturmak için iç sonucu çağırın.
2. Yanıt inceleyin.
3. Bir kimlik doğrulaması sınaması için yanıt, gerekirse ekleyin.

Aşağıdaki örnek, temel kimlik doğrulaması örnekten alınır. Tanımladığı bir **Ihttpactionresult** dış sonuç.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult` Özelliği tutan iç **Ihttpactionresult**. `Challenge` Özelliğini bir Www doğrulama üstbilgisi temsil eder. Dikkat **ExecuteAsync** önce çağırır `InnerResult.ExecuteAsync` HTTP yanıt oluşturan ve sonra gerekirse sınaması ekler.

Sınama eklemeden önce yanıt kodunu denetleyin. Birçok kimlik doğrulama düzeni yalnızca bir sınama ekleme yanıt, burada gösterildiği gibi 401 ise. Ancak, bazı kimlik doğrulama şeması bir sınama başarılı yanıt ekleyin. Örneğin, [anlaş](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Verilen `AddChallengeOnUnauthorizedResult` sınıfı, gerçek kod **ChallengeAsync** basittir. Yalnızca sonuç oluşturun ve buna eklemek `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Not: Temel kimlik doğrulaması örnek bu mantığı biraz, genişletme yönteminin yerleştirerek soyutlar.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Ana bilgisayar düzeyinde kimlik doğrulaması ile kimlik doğrulaması filtreleri birleştirme

"Ana bilgisayar düzeyinde kimlik doğrulaması" (IIS) gibi ana bilgisayar tarafından gerçekleştirilen kimlik doğrulaması önce istek ulaştığında Web API'si çerçevedir.

Genellikle, uygulamanızın rest ana bilgisayar düzeyinde kimlik doğrulamasını etkinleştirmek, ancak, Web APİ'si denetleyicilerinin için devre dışı bırakmak isteyebilirsiniz. Örneğin, ana bilgisayar düzeyinde form kimlik doğrulamasını etkinleştirmek, ancak Web API'si için belirteç tabanlı kimlik doğrulaması kullanmak için tipik bir senaryo olur.

Web API ardışık düzeni içinde ana bilgisayar düzeyinde kimlik doğrulaması devre dışı bırakmak için çağrı `config.SuppressHostPrincipal()` yapılandırmanızda. Web API'ı kaldırmak bu neden **IPrincipal** Web API ardışık düzenine girdikten herhangi bir istek. Etkili bir şekilde, bu &quot;Çalıştır-kimlik doğrulaması&quot; istek.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web API güvenliğini filtreler](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN dergisi)
