---
uid: web-api/overview/security/authentication-filters
title: "ASP.NET Web API 2 kimlik doğrulaması filtrelerini | Microsoft Docs"
author: MikeWasson
description: "Bir kimlik doğrulaması filtresini bir HTTP isteğinin kimliğini doğrulayan bir bileşenidir. Web API 2 ve MVC 5 hem de kimlik doğrulaması filtreleri desteklemez, ancak bunların biraz farklı..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 7c704cc351876b49ec143a49b25cc0ca83876e06
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 kimlik doğrulaması filtreleri
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Bir kimlik doğrulaması filtresini bir HTTP isteğinin kimliğini doğrulayan bir bileşenidir. Web API 2 ve MVC 5 hem de kimlik doğrulaması filtreleri desteklemez, ancak bunların filtre arabirimi için adlandırma kuralları, çoğunlukla biraz farklı. Bu konuda, Web API kimlik doğrulaması filtreleri açıklanmaktadır.


Kimlik doğrulaması filtreleri tek tek denetleyicileri veya eylemler için bir kimlik doğrulama düzeni ayarlamanıza olanak tanır. Böylece, uygulamanız farklı HTTP kaynakları için farklı kimlik doğrulama mekanizmaları destekleyebilir.

Bu makalede, ı koddan göstereceğiz [temel kimlik doğrulaması](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) üzerinde örnek [http://aspnet.codeplex.com](http://aspnet.codeplex.com). Örnek HTTP temel erişim kimlik doğrulama düzenini (RFC 2617) uygulayan bir kimlik doğrulaması filtresini gösterir. Filtre adlı bir sınıfta uygulanır `IdentityBasicAuthenticationAttribute`. I tüm örnek koddan hemen bir kimlik doğrulaması filtresini yazmak nasıl çalışılacağını bölümleri göstermeyecektir.

## <a name="setting-an-authentication-filter"></a>Bir kimlik doğrulaması filtresini ayarlama

Diğer filtreleri gibi kimlik doğrulaması filtreleri uygulanan denetleyicisi başına, eylem başına veya genel olarak tüm Web API denetleyicilerinin olabilir.

Bir denetleyici için bir kimlik doğrulaması filtresini uygulamak için filtre özniteliği denetleyicisi sınıfıyla tasarlamanız. Aşağıdaki kod kümeleri `[IdentityBasicAuthentication]` tüm denetleyicinin eylemleri için temel kimlik doğrulamasına olanak tanıyan bir denetleyici sınıfını filtre.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Bir eylem filtresi uygulamak için eylem filtreyle olarak tasarlamanız. Aşağıdaki kod kümeleri `[IdentityBasicAuthentication]` denetleyicinin filtresini `Post` yöntemi.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Tüm Web API denetleyicilerinin filtre uygulamak için eklemeniz **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Bir Web API kimlik doğrulama filtre uygulama

Web API'de kimlik doğrulama filtreler uygulamasına [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) arabirimi. Bunlar ayrıca devralınmalıdır **System.Attribute'un**öznitelikleri olarak uygulanması için.

**IAuthenticationFilter** arabirimi iki yöntem vardır:

- **AuthenticateAsync** isteğin istek kimlik bilgilerini doğrulayarak varsa kimliğini doğrular.
- **ChallengeAsync** gerekirse HTTP yanıtına bir kimlik doğrulaması sınaması ekler.

Bu yöntemler tanımlanan kimlik doğrulama akışı karşılık [RFC 2612](http://tools.ietf.org/html/rfc2616) ve [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. İstemci kimlik bilgileri yetkilendirme üst bilgi gönderir. Bu, genellikle istemcinin sunucudan 401 (yetkisiz) yanıt aldıktan sonra gerçekleşir. Ancak, bir istemci, yalnızca bir 401 edindikten sonra kimlik bilgileriyle herhangi bir istek gönderebilir.
2. Sunucu kimlik bilgileri kabul etmez, 401 (yetkisiz) bir yanıt döndürür. Yanıt, bir veya daha fazla zorluklar içeren bir Www-Authenticate üstbilgisi içeriyor. Her sınama sunucu tarafından tanınan bir kimlik doğrulama düzeni belirtir.

Sunucu anonim bir istekten 401 geri dönebilirsiniz. Aslında, genellikle kimlik doğrulama işlemini nasıl başlatılan olmasıdır:

1. İstemci bir anonim istek gönderir.
2. Sunucu 401 döndürür.
3. İstemcileri kimlik bilgilerine sahip istek düzenini yeniden gönderir.

Bu akış her ikisini de içeren *kimlik doğrulaması* ve *yetkilendirme* adımları.

- Kimlik doğrulaması istemci kimliğini kanıtlar.
- Yetkilendirme, istemci belirli bir kaynak erişip erişemeyeceğini belirler.

Web API'de kimlik doğrulama, ancak değil yetkilendirme kimlik doğrulaması filtreleri işleyin. Yetkilendirme denetleyici eylemi içinde veya bir yetkilendirme Filtresi tarafından yapılması gerekir.

Web API 2 ardışık düzeninde akışı şöyledir:

1. Bir eylem çağırmadan önce Web API Bu eylem için kimlik doğrulama filtrelerin listesi oluşturur. Bu eylem kapsam, denetleyici kapsam ve genel kapsama sahip filtreler içerir.
2. Web API çağrıları **AuthenticateAsync** listesinde her filtre üzerinde. Her filtre istekte kimlik doğrulayabilir. Herhangi bir filtre başarıyla kimlik bilgilerini doğrular, filtre oluşturur. bir **IPrincipal** ve isteği ekler. Bir filtre da bu noktada bir hata tetikleyebilir. Bu durumda, kalan ardışık düzenini çalışmaz.
3. Bir hata varsayıldığında, istek ardışık düzen rest akar.
4. Son olarak, her kimlik doğrulama filtrenin Web API çağrılarının **ChallengeAsync** yöntemi. Filtreler bu yöntem bir sınama yanıtı, eklemek için gerekirse kullanın. Genellikle (ancak her zaman), durum 401 hatası için yanıt olacaktır.

Aşağıdaki diyagramlarda iki olası durumların gösterir. İlk kimlik doğrulaması filtresini başarıyla isteğin kimliğini doğrular, bir yetkilendirme filtresi isteği yetkilendirir ve denetleyici eylemini 200 (Tamam) döndürür.

![](authentication-filters/_static/image1.png)

İkinci örnekte, kimlik doğrulaması filtresini isteğin kimliğini doğrular, ancak yetkilendirme filtresini 401 (yetkisiz) döndürür. Bu durumda, denetleyici eylem çağrılır. Kimlik doğrulaması filtresini bir Www-Authenticate üstbilgisi yanıta ekler.

![](authentication-filters/_static/image2.png)

Diğer olası birleşimleridir&mdash;denetleyici eylemini anonim isteklere izin veriyorsa, örneğin, bir kimlik doğrulaması filtresini ancak hiçbir yetkilendirme olabilir.

## <a name="implementing-the-authenticateasync-method"></a>AuthenticateAsync yöntemi uygulama

**AuthenticateAsync** yöntemi isteğin kimliğini dener. Yöntem imzası şöyledir:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**AuthenticateAsync** yöntemi aşağıdakilerden birini yapmanız gerekir:

1. Nothing (no-op).
2. Oluşturma bir **IPrincipal** ve isteği ayarlayın.
3. Bir hata sonucu olarak ayarlayın.

Seçeneği (1) anlamına gelir isteği filtrenin anlayan herhangi bir kimlik bilgisi yoktu. Seçeneği (2) anlamına gelir filtre başarıyla isteğin kimliği. Bir hata yanıtı tetikler seçeneği (3) anlamına gelir isteği, geçersiz kimlik bilgileri (yanlış parola gibi), vardı.

Uygulama için genel anahattı işte **AuthenticateAsync**.

1. İstekteki kimlik bilgilerini arayın.
2. Kimlik bilgisi varsa, hiçbir şey yapma ve (no-op) döndürür.
3. Kimlik bilgileri vardır, ancak filtresi kimlik doğrulama şeması tanımıyor, hiçbir şey yapma ve (no-op) döndürür. Başka bir filtre ardışık düzeninde düzenini anlamak.
4. Filtrenin anlayan kimlik bilgileri varsa, bunları kimlik doğrulamayı deneyin.
5. Kimlik bilgileri yanlış olduğunda, 401 ayarlayarak döndürün `context.ErrorResult`.
6. Kimlik bilgilerinin geçerli olduğundan, oluşturma bir **IPrincipal** ve `context.Principal`.

İzleme kodu gösterir **AuthenticateAsync** yönteminden [temel kimlik doğrulaması](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) örnek. Her adım yorumları belirtin. Çeşitli hata kodunu gösterir: hiçbir kimlik bilgileri, hatalı biçimlendirilmiş kimlik bilgilerini ve hatalı kullanıcı adı/parola ile bir Authorization Üstbilgisi.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Bir hata sonucu ayarlama

Kimlik bilgileri geçersiz olduğunda filtresi ayarlamalısınız `context.ErrorResult` için bir **Ihttpactionresult** bir hata yanıtı oluşturur. Hakkında daha fazla bilgi için **Ihttpactionresult**, bkz: [eylem sonuçlarını Web API 2'deki](../getting-started-with-aspnet-web-api/action-results.md).

Temel kimlik doğrulaması örnek içeren bir `AuthenticationFailureResult` bu amaç için uygun sınıfı.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementing ChallengeAsync

Amacı **ChallengeAsync** yöntemi ise kimlik doğrulama sınaması yanıt olarak eklemek için gerekli. Yöntem imzası şöyledir:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Yöntem, her kimlik doğrulaması filtresini istek kanalında çağrılır.

Anlamak önemlidir **ChallengeAsync** çağrılır *önce* oluşturulan ve büyük olasılıkla bile denetleyici eylemi çalıştırılmadan önce HTTP yanıtı. Zaman **ChallengeAsync** olarak adlandırılır, `context.Result` içeren bir **Ihttpactionresult**, daha sonra HTTP yanıtı oluşturmak için kullanılan. Böyle olduğunda **ChallengeAsync** olduğu olarak adlandırılan, HTTP yanıtı hakkında hiçbir şey henüz tanımadığınız. **ChallengeAsync** replace özgün değeri yöntemi `context.Result` yeni bir **Ihttpactionresult**. Bu **Ihttpactionresult** özgün sarmalamanız gerekir `context.Result`.

![](authentication-filters/_static/image3.png)

Özgün çağrı **Ihttpactionresult** *iç sonuç*ve yeni **Ihttpactionresult** *dış sonuç*. Dış sonuç aşağıdakileri yapmanız gerekir:

1. HTTP yanıtı oluşturmak için iç sonucunu çağırır.
2. Yanıt inceleyin.
3. Bir kimlik doğrulaması sınaması gerekirse yanıta ekleyin.

Aşağıdaki örnek, temel kimlik doğrulaması örnekten alınır. Bunu tanımlayan bir **Ihttpactionresult** dış sonucu.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult` Özelliği tutan iç **Ihttpactionresult**. `Challenge` Özelliği bir Www kimlik doğrulama üstbilgisi temsil eder. Dikkat **ExecuteAsync** önce çağırır `InnerResult.ExecuteAsync` HTTP yanıtı oluşturmak için ve ardından gerekirse sınaması ekler.

Yanıt kodu challenge eklemeden önce denetleyin. Birçok kimlik doğrulama şemasını yalnızca bir sınama Ekle yanıt, aşağıda gösterildiği gibi 401 ise. Ancak, bazı kimlik doğrulama şemasını bir sınama başarılı yanıt olarak ekleyin. Örneğin, [anlaş](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Verilen `AddChallengeOnUnauthorizedResult` sınıfı, Gerçek kodda **ChallengeAsync** basittir. Yalnızca sonucu oluşturun ve ona ekleyin `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Not: Temel kimlik doğrulaması örnek bu mantığı biraz, bir uzantı yönteminde koyarak soyutlar.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Ana bilgisayar düzeyinde kimlik doğrulaması ile kimlik doğrulaması filtrelerini birleştirme

"Ana bilgisayar düzeyinde kimlik doğrulaması" (örneğin, IIS), ana bilgisayar tarafından gerçekleştirilen kimlik doğrulaması önce istek ulaştığında Web API çerçevedir.

Genellikle, uygulamanızın geri kalanı için ana bilgisayar düzeyinde kimlik doğrulamasını etkinleştirmek, ancak Web API denetleyicileri için devre dışı bırakmak isteyebilirsiniz. Örneğin, ana bilgisayar düzeyinde form kimlik doğrulamasını etkinleştirmek, ancak belirteç tabanlı kimlik doğrulaması için Web API kullanma için tipik bir senaryo olur.

Web API ardışık düzeni içinde ana bilgisayar düzeyinde kimlik doğrulaması devre dışı bırakmak için çağrı `config.SuppressHostPrincipal()` yapılandırma. Bu Web API'ı kaldırmak neden **IPrincipal** Web API ardışık düzeni girer istek. Etkili bir şekilde bu &quot;kaydını-kimliğini doğrulayan&quot; istek.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web API güvenliğini filtreler](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN dergisi)
