---
uid: signalr/overview/security/introduction-to-security
title: "SignalR güvenlik giriş | Microsoft Docs"
author: pfletcher
description: "Bir SignalR Uygulama geliştirirken dikkate almanız gereken güvenlik sorunları açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: ffe71f8ea7105db4d5a0c156e2b4e76d6e40761d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-signalr-security"></a>SignalR güvenlik giriş
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede bir SignalR Uygulama geliştirirken dikkate almanız gereken güvenlik konuları açıklanmaktadır. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Bu konuda kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Bu konunun önceki sürümleri
> 
> SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Genel Bakış

Bu belgede aşağıdaki bölümler yer alır:

- [SignalR güvenlik kavramları](#concepts)

    - [Kimlik doğrulama ve yetkilendirme](#authentication)
    - [Bağlantı belirteci](#connectiontoken)
    - [Yeniden bağlanma sırasında gruplarına yeniden katılma](#rejoingroup)
- [SignalR siteler arası istek sahteciliğini nasıl engeller](#csrf)
- [SignalR güvenlik önerileri](#recommendations)

    - [Güvenli Yuva Katmanı (SSL) Protokolü](#ssl)
    - [Grup bir güvenlik mekanizması olarak kullanmayın](#groupsecurity)
    - [Güvenli bir şekilde istemcilerden girişini işleme](#input)
    - [Kullanıcı etkin bir bağlantı durumundaki bir değişiklik karşılaştırma](#reconcile)
    - [Otomatik olarak oluşturulan JavaScript proxy dosyaları](#autogen)
    - [Özel durumlar](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>SignalR güvenlik kavramları

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme

SignalR, kullanıcıların kimliğini doğrulamak için herhangi bir özellik sağlamaz. Bunun yerine, bir uygulama için var olan kimlik doğrulama yapıda SignalR özellikleri tümleştirin. Uygulama ve iş sonuçları, SignalR kimlik doğrulaması ile normalde kod gibi kullanıcı kimlik doğrulaması. Örneğin, ASP.NET forms kimlik doğrulaması, kullanıcıların kimliğini doğrulamak ve ardından hub'ınıza, hangi kullanıcıların zorla veya rolleri bir yöntemi çağırmak için yetki. Hub'ınıza, kullanıcı adı veya kullanıcı istemciye bir role ait olup gibi kimlik doğrulama bilgilerini geçirebilirsiniz.

SignalR sağlar [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) özniteliği hangi kullanıcıların bir hub veya yöntemi erişimine sahip olacağını belirtin. Bir hub veya belirli bir hub yöntemlerinde Authorize özniteliğini uygulayın. Authorize özniteliği olmadan hub'ındaki tüm genel yöntemler hub'ına bağlı bir istemci için kullanılabilir. Hub'ları hakkında daha fazla bilgi için bkz: [kimlik doğrulama ve yetkilendirme SignalR hub'ları için](hub-authorization.md).

Uyguladığınız `Authorize` özniteliği hub ancak kalıcı değil bağlantıları. Kullanırken yetkilendirme kurallarını zorunlu tutmak için bir `PersistentConnection` geçersiz kılmanız gerekir `AuthorizeRequest` yöntemi. Kalıcı bağlantılar hakkında daha fazla bilgi için bkz: [kimlik doğrulama ve yetkilendirme için SignalR kalıcı bağlantılar](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Bağlantı belirteci

SignalR gönderenin kimliğini doğrulayarak kötü amaçlı komutları yürütülürken riskini azaltır. Her istek için sunucu ve istemci kimliği doğrulanmış kullanıcılar için kullanıcı adı ve bağlantı kimliğini içeren bir bağlantı belirteci geçirin. Bağlantı kimliği her bağlı istemci benzersiz olarak tanımlar. Yeni bir bağlantı oluşturulur ve bu kimlik bağlantısı süresince devam ederse, sunucunun rastgele bağlantı kimliği oluşturur. Kullanıcı adı web uygulaması için kimlik doğrulama mekanizması sağlar. SignalR, bağlantı belirteci korumak için şifreleme ve dijital imza kullanır.

![](introduction-to-security/_static/image2.png)

Her istek için sunucu isteği belirtilen kullanıcıdan geldiğinden emin olmak için belirtecin içeriği doğrular. Kullanıcı adı için bağlantı kimliği karşılık gelmelidir. Bağlantı kimliği ve kullanıcı adı doğrulayarak SignalR kötü niyetli bir kullanıcı başka bir kullanıcı kolayca taklit engeller. Sunucu bağlantı belirteci doğrulanamıyor isteği başarısız olur.

![](introduction-to-security/_static/image4.png)

Bağlantı kimliğini doğrulama işleminin bir parçası olduğundan, bir kullanıcının bağlantı kimliği diğer kullanıcılara ortaya veya gerekir istemcide değeri gibi bir tanımlama bilgisinde depolayıp.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Yeniden bağlanma sırasında gruplarına yeniden katılma

Varsayılan olarak, SignalR uygulama otomatik olarak bir kullanıcı uygun gruplara durumundan zaman bağlantı bırakılan ve bağlantı zaman aşımına uğramadan önce yeniden kurdu gibi geçici, yeniden bağlanmayı yeniden atayacaktır. Yeniden bağlanma, istemci bağlantı kimliği ve atanan grupları içeren bir grup belirteci geçirir. Grup belirteci dijital olarak imzalanır ve şifrelenir. İstemci aynı bağlantı kimliği sonra yeniden bağlanmayı korur; Bu nedenle, yeniden bağlanan istemciden geçirilen bağlantı kimliği istemci tarafından kullanılan önceki bağlantı kimliği eşleşmesi gerekir. Bu doğrulama kötü niyetli bir kullanıcının yeniden bağlanma, yetkisiz gruplara katılmak için istek geçirme engeller.

Bununla birlikte, Not Grup belirteci dolmayan önemlidir. Bir kullanıcı, geçmişte bir gruba ait ancak o gruptan yasaklanan, o kullanıcı yasaklanmış grup içeren bir grup belirteci taklit etmek mümkün olabilir. Hangi kullanıcıların hangi gruplara ait güvenli bir şekilde yönetmeniz gerekiyorsa, sunucuda, bu verileri gibi bir veritabanında depolamak gerekir. Ardından, sunucu üzerinde bir kullanıcı bir gruba ait olup olmadığını doğrular ve uygulamanıza mantığı ekleyin. Grup üyeliği doğrulanıyor ilişkin bir örnek için bkz: [gruplarıyla çalışma](../guide-to-the-api/working-with-groups.md).

Bir bağlantı geçici bozulma sonra yeniden bağlandığında otomatik olarak gruplarına yeniden katılma yalnızca geçerlidir. Bir kullanıcı tarafından kesilirse gezinme uygulamayı veya uygulama yeniden başlatılmadan, uygulamanızın çıktığınızda doğru gruba kullanıcı eklemek nasıl işlemesi gerekir. Daha fazla bilgi için bkz: [gruplarıyla çalışma](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>SignalR siteler arası istek sahteciliğini nasıl engeller

Siteler arası istek sahteciliği (CSRF), burada kötü amaçlı bir site olduğu kullanıcı şu anda oturum savunmasız bir siteye bir istek gönderir bir saldırı aracıdır. SignalR, SignalR uygulamanız için geçerli bir isteği oluşturmak kötü amaçlı bir site için son derece düşüktür yaparak CSRF engeller.

### <a name="description-of-csrf-attack"></a>CSRF saldırı açıklaması

CSRF saldırı bir örneği burada verilmiştir:

1. Bir kullanıcının oturum açtığı www.example.com, kullanarak kimlik doğrulaması oluşturur.
2. Sunucu kullanıcının kimliğini doğrular. Sunucudan gelen yanıtı bir kimlik doğrulama tanımlama bilgisi içerir.
3. Günlük olmadan, kullanıcının kötü amaçlı web sitesini ziyaret eder. Bu kötü amaçlı site aşağıdaki HTML formu içerir: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 Form eylemi kötü amaçlı siteye değil savunmasız siteye yazılarını dikkat edin. CSRF "siteler arası" parçasıdır.
4. Kullanıcı gönder düğmesine tıklar. Tarayıcı kimlik doğrulama tanımlama bilgisi istekle içerir.
5. İstek, kullanıcının kimlik doğrulaması bağlamı ile example.com sunucuda çalışır ve kimliği doğrulanmış bir kullanıcı yapmak için izin verilen herhangi bir şey yapabilirsiniz.

Bu örnekte form düğmesini kullanıcıya gerektirse de, kötü amaçlı sayfasını kolayca SignalR uygulamanıza bir AJAX isteği gönderir bir komut dosyasını çalıştırmak gibi verebilir. Ayrıca, SSL kullanarak, bunlar kötü amaçlı site "https://" istek gönderdikleri bile, CSRF saldırı engellemez.

Genellikle, tarayıcılar tüm ilgili tanımlama bilgilerini hedef web sitesine göndermek için CSRF saldırılarına karşı kimlik doğrulaması için tanımlama bilgileri kullanan web siteleri mümkün olabilir. Ancak, CSRF saldırıları tanımlama bilgisinden faydalanmakta için sınırlı değildir. Örneğin, temel ve Özet kimlik doğrulaması da savunmasız. Bir kullanıcının temel veya Özet kimlik doğrulaması ile oturum açtığı sonra oturumu sona kadar tarayıcı otomatik olarak kimlik bilgilerini gönderir.

### <a name="csrf-mitigations-taken-by-signalr"></a>SignalR tarafından alınan CSRF Azaltıcı Etkenler

SignalR kötü amaçlı bir site, uygulama için geçerli istekleri oluşturmasını önlemek için aşağıdaki adımları gerçekleştirir. Varsayılan olarak bu adımları SignalR alır, kodunuzda herhangi bir eylemde bulunmanız gerekmez.

- **Etki alanları arası istek devre dışı bırak**  
 SignalR etki alanları arası istek kullanıcıların dış etki alanından bir SignalR uç noktası çağırma önlemek için devre dışı bırakır. SignalR geçersiz olduğu için dış etki alanından herhangi isteği değerlendirir ve istek engeller. Bu varsayılan davranışı tutmanızı öneririz; Aksi takdirde, kötü amaçlı bir site sitenize komutları göndermeyi kullanıcılar kandırarak. Etki alanları arası istek kullanmanız gerekiyorsa, bkz: [etki alanları arası bağlantı kurmak nasıl](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Bağlantı belirteci değil tanımlama bilgisi, sorgu dizesi geçirin**  
 SignalR bağlantı belirteci bir sorgu dizesi değeri olarak bir tanımlama bilgisi olarak yerine geçer. Kötü amaçlı kod karşılaşıldığında tarayıcı bağlantı belirteci yanlışlıkla iletebilir için bağlantı belirtecini bir tanımlama bilgisinde depolama güvenli değildir. Ayrıca, sorgu dizesinde bağlantı belirteci geçirme bağlantı belirteci geçerli bağlantıyı sürdürmek engeller. Bu nedenle, kötü niyetli bir kullanıcı başka bir kullanıcının kimlik doğrulama kimlik bilgileri altında bir istek yapamazsınız.
- **Bağlantı belirteci doğrulayın**  
 Bölümünde açıklandığı gibi [bağlantı belirteci](#connectiontoken) bölümünde, sunucu bilir hangi bağlantı kimliği her kimliği doğrulanmış bir kullanıcı ile ilişkilidir. Sunucu, kullanıcı adı eşleşmiyor bir bağlantı kimliği herhangi bir istekten işlemez. Kötü amaçlı kullanıcı kullanıcı adı ve geçerli işareti, rasgele üretilen bağlantı kimliği bilmesi gerekir çünkü kötü niyetli bir kullanıcının geçerli bir istek tahmin edebilir düşüktür. Bağlantı sona hemen sonra o bağlantı kimliği geçersiz hale gelir. Anonim kullanıcılar tüm hassas bilgilere erişimi yok.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>SignalR güvenlik önerileri

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Güvenli Yuva Katmanı (SSL) Protokolü

SSL protokolü şifreleme, istemci ve sunucu arasında veri aktarımı güvenliğini sağlamak için kullanır. SignalR uygulamanız istemci ve sunucu arasında hassas bilgileri iletir, SSL taşıma için kullanın. SSL ayarlama hakkında daha fazla bilgi için bkz: [IIS 7 üzerinde SSL ayarlamak nasıl](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Grup bir güvenlik mekanizması olarak kullanmayın

Grubu ilgili kullanıcıları toplamanın kolay bir yol, ancak bunlar hassas bilgilere erişimi sınırlamak için güvenli bir mekanizma değildir. Kullanıcıların otomatik olarak yeniden bağlanma sırasında grupları katılabilir durumlarda özellikle geçerlidir. Bunun yerine, ayrıcalıklı kullanıcı role ekleme ve yalnızca o rolünün üyeleri bir hub yöntemine erişimi sınırlayan göz önünde bulundurun. Bir rol tabanlı erişim kısıtlama ilişkin bir örnek için bkz: [kimlik doğrulama ve yetkilendirme SignalR hub'ları için](hub-authorization.md). Yeniden bağlanma gruplara kullanıcı erişimini denetleme ilişkin bir örnek için bkz: [gruplarıyla çalışma](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Güvenli bir şekilde istemcilerden girişini işleme

Kötü niyetli bir kullanıcının diğer kullanıcılara betiği göndermez emin olmak için diğer istemcilere yayın için hedeflenen tüm giriş istemcilerden kodlayın gerekir. SignalR uygulamanız farklı türlerde istemcileri olabileceğinden sunucu yerine alan istemciler iletileri kodlayın. Bu nedenle, HTML kodlaması web istemcisi için ancak diğer istemci türlerini için çalışır. Örneğin, Sohbet iletisi görüntülenecek web istemcisi yöntemi güvenli bir şekilde kullanıcı adı ve ileti çağırarak işlemek `html()` işlevi.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Kullanıcı etkin bir bağlantı durumundaki bir değişiklik karşılaştırma

Etkin bir bağlantı bulunmakla birlikte bir kullanıcının kimlik doğrulama durumu değişirse, kullanıcı belirten bir hata alır, "kullanıcı kimliği etkin bir SignalR bağlantısı sırasında değiştiremezsiniz." Bu durumda, uygulamanızın kullanıcı adı ve bağlantı kimliği uyumlu olduğundan emin olmak için sunucuya yeniden bağlanmanız gerekir. Örneğin, uygulamanızın etkin bir bağlantı bulunmakla birlikte oturumunuzu kullanıcıya izin veriyorsa, bağlantı için kullanıcı adı artık bir sonraki istek için geçirilen adı eşleşir. Kullanıcının oturumunu kapatır önce bağlantı durdurmak istiyor ve yeniden başlatın.

Bununla birlikte, çoğu uygulamayı el ile durdurun ve bağlantıyı başlatın gerekmez dikkate almak önemlidir. Uygulamanız kullanıcıların oturum çıkışı, Web Forms uygulaması veya MVC uygulaması'de varsayılan davranışı gibi sonra ayrı bir sayfaya yönlendirir ya da oturum kapatıldıktan sonra geçerli sayfa yenilendikten, etkin bir bağlantı otomatik olarak kapatılacağı ve desteklemez başka bir işlem gerektirir.

Aşağıdaki örnek, durdurmak ve kullanıcı durumu değiştiğinde bir bağlantı başlatmak gösterilmiştir.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Veya, kullanıcının kimlik doğrulama durumu sitenizi kayan zaman aşımı ile form kimlik doğrulaması kullanır ve kimlik doğrulama tanımlama bilgisinin geçerli tutmak için hiçbir etkinlik ise geçebilir. Bu durumda, kullanıcı kaydedilir ve kullanıcı adı artık bağlantı belirteci kullanıcı adı ile eşleşir. Düzenli aralıklarla kimlik doğrulama tanımlama bilgisinin geçerli tutmak için web sunucusunda bir kaynak istekleri bazı komut dosyası ekleyerek bu sorunu düzeltebilirsiniz. Aşağıdaki örnekte, her 30 dakikada bir kaynak isteği gösterilmektedir.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Otomatik olarak oluşturulan JavaScript proxy dosyaları

Tüm hub'ları ve yöntemlerinin her kullanıcı için JavaScript proxy'si dosyası dahil etmek istemiyorsanız, dosyayı otomatik olarak oluşturulmasını devre dışı bırakabilirsiniz. Birden çok hub ve yöntem varsa, ancak her kullanıcı tüm yöntemlerin unutmayın istemiyorsanız bu seçeneği belirleyebilirsiniz. Ayarlayarak otomatik oluşturma devre dışı **EnableJavaScriptProxies** için **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

JavaScript proxy dosyaları hakkında daha fazla bilgi için bkz: [oluşturulan proxy ve onu sizin için ne yaptığını](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Özel Durumlar

Nesneleri istemcilere hassas bilgileri açığa çünkü istemcilere özel durum nesneleri geçirme kaçınmalısınız. Bunun yerine, ilgili hata iletisi görüntüler istemcide bir yöntemini çağırın.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
