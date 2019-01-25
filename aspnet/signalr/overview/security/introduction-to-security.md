---
uid: signalr/overview/security/introduction-to-security
title: SignalR güvenliğine giriş | Microsoft Docs
author: bradygaster
description: Bir SignalR Uygulama geliştirirken dikkate almanız gereken güvenlik konularını açıklar.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ed562717-8591-4936-8e10-c7e63dcb570a
msc.legacyurl: /signalr/overview/security/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 311bfb4279b1c919fddcef2c0aed657083f9c34f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837331"
---
<a name="introduction-to-signalr-security"></a>SignalR Güvenliğine Giriş
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Bu makalede bir SignalR Uygulama geliştirirken dikkate almanız gereken güvenlik konularını açıklar.
>
> ## <a name="software-versions-used-in-this-topic"></a>Bu konu başlığında kullanılan yazılım sürümleri
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR sürüm 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Bu konunun önceki sürümleri
>
> SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
>
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Genel Bakış

Bu belgede aşağıdaki bölümler yer alır:

- [SignalR güvenlik kavramları](#concepts)

    - [Kimlik Doğrulaması ve Yetkilendirme](#authentication)
    - [Bağlantı simgesi](#connectiontoken)
    - [Bağlanırken gruplarına yeniden katılma](#rejoingroup)
- [SignalR siteler arası istek sahteciliğini nasıl önler?](#csrf)
- [SignalR güvenlik önerileri](#recommendations)

    - [Güvenli Yuva Katmanı (SSL) Protokolü](#ssl)
    - [Grupları bir güvenlik mekanizması olarak kullanmayın](#groupsecurity)
    - [Güvenli bir şekilde istemcilerden giriş işleme](#input)
    - [Kullanıcı durumu etkin bir bağlantı ile bir değişiklik mutabık kılma](#reconcile)
    - [Otomatik olarak oluşturulan JavaScript proxy'si dosyaları](#autogen)
    - [Özel Durumlar](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>SignalR güvenlik kavramları

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme

SignalR, kullanıcıların kimliğini doğrulamak için tüm özellikleri sağlamaz. Bunun yerine, bir uygulama için var olan kimlik doğrulama yapısında SignalR özellikleri tümleştirin. Uygulama ve iş sonuçlarını kimlik doğrulaması, SignalR ile normalde kod gibi kullanıcı kimlik doğrulaması. Örneğin, ASP.NET formları kimlik doğrulaması ile kullanıcılarınızın kimliğini ve ardından hub'ınızda, hangi kullanıcıların zorunlu veya bir yöntemi çağırmak için roller yetkilidir. Hub'ınızda, kullanıcı adı veya bir kullanıcı istemciye bir role ait olup gibi kimlik doğrulama bilgilerini de geçirebilirsiniz.

SignalR sağlar [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) hangi kullanıcıların erişimi bir hub veya yöntemi belirtmek için özniteliği. Bir hub'ı veya belirli bir hub yöntemleri için Authorize özniteliğini uygulayın. Authorize özniteliği hub'da tüm genel yöntemleri hub'ına bağlı bir istemci kullanılabilir. Hub'ları hakkında daha fazla bilgi için bkz. [kimlik doğrulama ve yetkilendirme SignalR hub'ları için](hub-authorization.md).

Uyguladığınız `Authorize` hub'ları, ancak değil kalıcı bağlantılar için özniteliği. Kullanırken yetkilendirme kurallarını zorunlu tutmak için bir `PersistentConnection` geçersiz kılmanız gerekir `AuthorizeRequest` yöntemi. Kalıcı bağlantılar hakkında daha fazla bilgi için bkz. [kimlik doğrulama ve yetkilendirme kalıcı SignalR bağlantıları için](persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Bağlantı simgesi

SignalR gönderenin kimliğini doğrulayarak kötü amaçlı komutlar yürütülürken riskini azaltır. Her istek için sunucu ve istemci kimliği doğrulanmış kullanıcılar için kullanıcı adı ve bağlantı kimliğini içeren bir bağlantı belirtecini geçirin. Bağlantı kimliği bağlanan her istemci benzersiz olarak tanımlar. Yeni bir bağlantı oluşturulur ve bu kimliğe bağlantı süresi boyunca devam ederse, sunucuyu rastgele bağlantı kimliği oluşturur. Kullanıcı adını web uygulaması için kimlik doğrulama mekanizması sağlar. SignalR, bağlantı belirteci korumak için şifreleme ve dijital imzayı kullanır.

![](introduction-to-security/_static/image2.png)

Her istek için sunucu içeriği isteğinin belirtilen kullanıcıdan geldiğini emin olmak için belirteci doğrular. Kullanıcı adı, bağlantı kimliğine karşılık gelmelidir. Bağlantı kimliği hem kullanıcı adı doğrulayarak SignalR kötü niyetli bir kullanıcı başka bir kullanıcının kimliğine bürünmek kolayca gelen engeller. Sunucu bağlantı belirteci doğrulanamıyor. istek başarısız olur.

![](introduction-to-security/_static/image4.png)

Bağlantı kimliğini doğrulama işleminin bir parçası olduğundan, bir kullanıcının bağlantı kimliği diğer kullanıcılara göstermek veya gerekir değeri istemcide bir tanımlama bilgisinde depolayıp gibi.

#### <a name="connection-tokens-vs-other-token-types"></a>Bağlantı belirteçleri diğer belirteç türleri karşılaştırması

Oturum belirteçleri veya kullanıma sunulan, bir risk doğurur kimlik doğrulama belirteçlerini filtreleyebileceğiniz bağlantı belirteçleri bazen güvenlik araçları tarafından işaretlenir.

SignalR bağlantı belirteci kimlik doğrulaması belirteci değil. Bu isteği yapan kullanıcı bağlantıyı oluşturan örneğiyle aynı olduğundan emin olmak için kullanılır. ASP.NET SignalR bağlantıları sunucular arasında taşımak izin verdiği için bağlantı belirteci gereklidir. Belirteci bağlantı belirli bir kullanıcıyla ilişkilendirir, ancak istekte bulunan kullanıcının kimliğini assert değil. Bir SignalR isteğini düzgün bir şekilde kimlik doğrulaması başka bir tanımlama bilgisi gibi kullanıcı kimliğini onaylar belirteci veya taşıyıcı belirteç olmalıdır. Ancak, bağlantı belirteci kendisi yapar, isteğin yalnızca o bağlantı kimliği belirteçteki bu kullanıcı tarafından yapılan hiçbir talep Bu kullanıcıyla ilişkili olur.

Hiçbir kimlik doğrulaması talep kendi bağlantı belirteci sağlar olduğundan, "oturumu" veya "kimlik doğrulaması" kabul değil belirteci. İstek kullanıcı kimliğini ve belirteçte depolanan kimlik eşleşmeyecektir çünkü belirli bir kullanıcının bağlantı belirteci alma ve farklı bir kullanıcı olarak kimliği doğrulanmış bir istek (veya kimliği doğrulanmamış bir isteği) yeniden yürüterek başarısız olur.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Bağlanırken gruplarına yeniden katılma

Varsayılan olarak, SignalR uygulama otomatik olarak bir kullanıcı uygun gruplara ne zaman bir bağlantı bırakılan ve bağlantı zaman aşımına uğramadan önce yeniden oluşturulan gibi geçici bir kesintisinden bağlanırken yeniden atar. Bağlanırken, istemcinin bağlantı kimliği ve atanan grupları içeren bir grup belirteç geçirir. Grup belirteç dijital olarak imzalanmış ve şifrelenmiş. İstemci aynı bağlantı kimliği sonra bir yeniden korur; Bu nedenle, yeniden bağlanan istemciden geçirilen bağlantı kimliği istemci tarafından kullanılan önceki bağlantı kimliği ile eşleşmelidir. Bu doğrulama, kötü niyetli bir kullanıcı bağlanırken yetkisiz gruplara katılma isteklerini geçmesini engeller.

Ancak, unutmayın Grup belirteci dolmaz önemlidir. Bir kullanıcı, geçmişte bir gruba ait, ancak o gruptan yasaklanmış, kullanıcı yasaklanmış grubu içeren bir grup belirteç taklit etmek mümkün olabilir. Hangi kullanıcıların hangi gruba ait güvenli bir şekilde yönetmeniz gerekiyorsa, bir veritabanı gibi sunucu üzerinde bu verileri depolamak gerekir. Ardından, mantıksal uygulamanızı sunucu üzerinde bir kullanıcı grubuna ait olup olmadığını doğrular ekleyin. Grup üyeliği doğrulanırken bir örnek için bkz: [gruplarıyla çalışma](../guide-to-the-api/working-with-groups.md).

Geçici bir kesinti sonra bağlantı yeniden bağlandığında otomatik olarak grupları aşamalarını yalnızca geçerlidir. Bir kullanıcı tarafından kesilirse giderek uygulama veya uygulama yeniden başlatılmadan, uygulamanızın uzağa doğru gruplara kullanıcı ekleme işlemesi gerekir. Daha fazla bilgi için [gruplarıyla çalışma](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>SignalR siteler arası istek sahteciliğini nasıl önler?

Siteler arası istek sahteciliği (CSRF), burada bir kötü niyetli site burada kullanıcı şu anda oturum savunmasız sitesine bir istek gönderir bir saldırıdır. SignalR, SignalR uygulamanız için geçerli bir isteği oluşturmak kötü amaçlı bir site için son derece düşüktür yaparak CSRF engeller.

### <a name="description-of-csrf-attack"></a>CSRF saldırı açıklaması

CSRF saldırılarının bir örnek aşağıda verilmiştir:

1. Bir kullanıcının oturum açtığı www.example.com, kullanarak kimlik doğrulaması oluşturur.
2. Sunucusu kullanıcının kimliğini doğrular. Sunucu yanıtı bir kimlik doğrulama tanımlama bilgisi içerir.
3. Kullanıcı oturumu kapatmak olmadan kötü amaçlı web sitesini ziyaret eder. Bu kötü niyetli site aşağıdaki HTML formu içerir:

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Form eylemi kötü niyetli site için güvenlik açığı siteye gönderir dikkat edin. CSRF "siteler arası" parçasıdır.
4. Kullanıcının gönder düğmesine tıklar. Tarayıcı, kimlik doğrulama tanımlama bilgisi ile istek içerir.
5. İstek, kullanıcının kimlik doğrulaması bağlamı example.com sunucunuz üzerinde çalışan ve kimliği doğrulanmış bir kullanıcıyı yapmak için izin verilen herhangi bir şey yapabilirsiniz.

Bu örnekte form düğmesini tıklatarak kullanıcı gerektirse de, kötü amaçlı sayfası kolayca SignalR uygulamanız için bir AJAX isteği gönderen betik çalıştırma gibi olabilir. Ayrıca, SSL kullanarak, bunlar "https://" istek kötü niyetli site gönderebileceğinden bile, CSRF saldırısını engellemez.

Genellikle, tarayıcılar ilgili tüm tanımlama bilgilerini hedef web sitesine göndermek için CSRF saldırılarına karşı kimlik doğrulaması için tanımlama bilgileri kullanan web siteleri mümkün olabilir. Ancak, CSRF saldırıları tanımlama bilgilerinin kötüye için sınırlı değildir. Örneğin, temel ve Özet kimlik doğrulaması ayrıca savunmasız. Bir kullanıcının temel veya Özet kimlik doğrulaması ile oturum açtığı sonra oturum sonlandırılana kadar tarayıcının otomatik olarak kimlik bilgilerini gönderir.

### <a name="csrf-mitigations-taken-by-signalr"></a>SignalR tarafından alınan CSRF risk azaltma işlemleri

SignalR kötü niyetli site uygulamanıza geçerli istekleri oluşturmasını önlemek için aşağıdaki adımları alır. Varsayılan olarak bu adımları SignalR alır, kodunuzdaki herhangi bir eylemde bulunmanız gerekmez.

- **Etki alanları arası istekleri devre dışı bırakmak** SignalR, SignalR uç nokta dış etki alanından çağırmasını kullanıcıları engellemek için etki alanları arası isteklere devre dışı bırakır. SignalR, geçersiz bir dış etki alanından herhangi isteği değerlendirir ve istek engeller. Bu varsayılan davranışı tutmanızı öneririz; Aksi takdirde, kötü niyetli site sitenize komut gönderme uygulamasına kullanıcılar ikna edebilir. Etki alanları arası istek kullanmanız gerekiyorsa, bkz. [etki alanları arası bağlantı kurmak nasıl](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Tanımlama bilgisi değil, sorgu dizesi bağlantı belirtecin geçip** SignalR bağlantı belirteci bir sorgu dizesi değeri olarak bir tanımlama bilgisi yerine geçer. Kötü amaçlı kod karşılaşıldığında tarayıcı bağlantı belirteci yanlışlıkla iletebilir için bağlantı belirtecini bir tanımlama bilgisinde depolama güvenli değildir. Ayrıca, sorgu dizesinde bağlantı belirtece geçirerek bağlantı belirteci geçerli bağlantı kalıcı önler. Bu nedenle, kötü niyetli bir kullanıcı bir istek başka bir kullanıcının kimlik doğrulama kimlik bilgileri altında yapamaz.
- **Bağlantı belirteci doğrulamak** açıklandığı [bağlantı belirteci](#connectiontoken) bölümünde, sunucu bilir hangi bağlantı kimliği doğrulanmış her kullanıcıyla ilişkilendirilir. Sunucu, herhangi bir kullanıcı adı eşleşmiyor bir bağlantı kimliği istekten işlemez. Kötü niyetli bir kullanıcının kullanıcı adı ve geçerli işareti, rasgele üretilen bağlantı kimliği biliyor olması gerekir çünkü kötü niyetli bir kullanıcının geçerli bir isteği tahmin edilemedi düşüktür. Bağlantı sona erer hemen sonra bu bağlantı kimliği geçersiz hale gelir. Anonim kullanıcılar, herhangi bir önemli bilgi erişimi olmamalıdır.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>SignalR güvenlik önerileri

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Güvenli Yuva Katmanı (SSL) Protokolü

SSL protokolü, istemci ve sunucu arasında veri taşıma güvenli için şifrelemeyi kullanır. SignalR uygulamanız istemci ve sunucu arasındaki hassas bilgileri iletir, SSL taşıma için kullanın. SSL ayarlama hakkında daha fazla bilgi için bkz. [IIS 7 üzerinde SSL kurma](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Grupları bir güvenlik mekanizması olarak kullanmayın

Grubu toplama ilgili kullanıcılar için kullanışlı bir yol, ancak bunlar hassas bilgilere erişimi sınırlayan güvenli bir mekanizma değildir. Bu, kullanıcıların bir yeniden bağlanma sırasında otomatik olarak grupları katılabilir, özellikle doğrudur. Bunun yerine, ayrıcalıklı kullanıcıların rol ekleme ve yalnızca o rolünün üyeleri bir hub yöntemine erişimi sınırlandırma göz önünde bulundurun. Bir rol tabanlı erişimi kısıtlama örnek için bkz [kimlik doğrulama ve yetkilendirme SignalR hub'ları için](hub-authorization.md). Bağlanırken gruplara kullanıcı erişimi denetimini bir örnek için bkz: [gruplarıyla çalışma](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Güvenli bir şekilde istemcilerden giriş işleme

Kötü niyetli bir kullanıcı diğer kullanıcılara betiği göndermediğinden emin olmak için diğer istemcilere yayın yönelik tüm girişler istemcilerden kodlamanız gerekir. SignalR uygulamanızın farklı türlerde istemciler olabilir çünkü sunucu yerine alan istemciler iletileri kodlayın. Bu nedenle, HTML kodlaması için bir web istemcisi, ancak diğer istemci türlerini çalışır. Örneğin, Sohbet iletisi görüntülemek için bir web istemci yöntemi güvenli bir şekilde kullanıcı adını ve iletisini çağırarak işlemek `html()` işlevi.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Kullanıcı durumu etkin bir bağlantı ile bir değişiklik mutabık kılma

Etkin bir bağlantı bulunduğu sürece bir kullanıcının kimlik doğrulama durumu değişirse, kullanıcı belirten bir hata alır, "kullanıcı kimliği etkin bir SignalR bağlantısı sırasında değiştirilemez." Bu durumda, uygulamanızın kullanıcı adı ve bağlantı kimliği Eşgüdümlü olduğundan emin olmak için sunucuya yeniden bağlanmanız gerekir. Örneğin, uygulamanızın etkin bağlantınız varken oturumunuzu kullanıcıya izin veriyorsa, bağlantı için kullanıcı adı artık bir sonraki istek için geçirilen adla eşleşir. Kullanıcının oturumunu kapatır önce bağlantı durdurmak istiyor ve yeniden başlatın.

Ancak, çoğu uygulama bağlantıyı el ile durdurup gerekmez, dikkat edin önemlidir. Uygulamanızın kullanıcıları, varsayılan davranışı bir Web Forms uygulaması veya MVC uygulaması gibi açtıktan sonra ayrı bir sayfasına yönlendirir. ya da oturum kapatıldıktan sonra geçerli sayfa yenilendikten etkin bağlantı otomatik olarak kesilir ve yok başka bir işlem gerektirir.

Aşağıdaki örnek, durdurmak ve kullanıcı durumu değiştirdiği zaman bir bağlantı başlatmak gösterilmektedir.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Veya, kullanıcının kimlik doğrulama durumu, sitenize form kimlik doğrulaması ile olmaadığını kullanır ve kimlik doğrulama tanımlama geçerliliğini sürdürmek için hiçbir etkinliği değişebilir. Bu durumda, kullanıcı kapatılacak ve kullanıcı adı artık bağlantı belirteci kullanıcı adıyla eşleşir. Düzenli aralıklarla kimlik doğrulama tanımlama geçerliliğini sürdürmek için web sunucusunda bir kaynak isteklerini bazı kod ekleyerek bu sorunu düzeltebilirsiniz. Aşağıdaki örnek, her 30 dakikada bir kaynak isteği gösterilmektedir.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Otomatik olarak oluşturulan JavaScript proxy'si dosyaları

Tüm hub'ları ve yöntemlerinin her kullanıcı için JavaScript proxy'si dosyasını dahil etmek istemiyorsanız, dosyayı otomatik olarak oluşturulmasını devre dışı bırakabilirsiniz. Birden çok hub ve yöntem varsa, ancak her kullanıcının tüm yöntemlerin bilincinde olmasını istemiyorsanız bu seçeneği belirleyebilirsiniz. Ayarlayarak otomatik oluşturma devre dışı **EnableJavaScriptProxies** için **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

JavaScript proxy'si dosyaları hakkında daha fazla bilgi için bkz. [oluşturulan proxy ve sizin için yaptığı](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Özel Durumlar

Nesneleri istemcilere hassas bilgileri kullanıma sunabileceğinden istemcilere özel durum nesneleri geçirme kaçınmanız gerekir. Bunun yerine, ilgili hata iletisini gösteren istemcide bir yöntemini çağırın.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
