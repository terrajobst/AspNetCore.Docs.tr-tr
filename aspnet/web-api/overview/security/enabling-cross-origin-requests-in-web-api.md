---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: ASP.NET Web API 2 çıkış noktaları arası istekleri etkinleştirme | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API'de çıkış noktaları arası kaynak paylaşımı (CORS) desteklemek nasıl gösterir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566820"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>ASP.NET Web API 2 çıkış noktaları arası istekleri etkinleştirme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Tarayıcı güvenlik bir web sayfası AJAX istekleri başka bir etki alanına yapmasını engeller. Bu kısıtlama adlı *kaynak aynı ilke*ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler. Ancak, bazen web API çağrısı diğer sitelere izin vermek isteyebilirsiniz.
> 
> [Kaynak kaynak paylaşma arası](http://www.w3.org/TR/cors/) (CORS) olan W3C standart bir kaynak aynı ilke hafifletin sunucusunun sağlar. CORS kullanarak, bir sunucu açıkça bazı cross-origin istekleri başkalarının reddetme çalışırken izin verebilirsiniz. CORS daha güvenli ve önceki teknikleri daha esnek gibi [JSONP](http://en.wikipedia.org/wiki/JSONP). Bu öğretici, Web API uygulamanızda CORS'yi etkinleştirmeniz gösterilmektedir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013 Güncelleştirme 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2.2


<a id="intro"></a>
## <a name="introduction"></a>Giriş

Bu öğretici, ASP.NET Web API CORS desteği gösterir. İki ASP.NET projeleri – "bir Web API denetleyicisi barındırır, bir çağrılan WebService" ve "WebService çağıran diğer çağrılan WebClient", oluşturarak başlayacağız. Farklı etki alanlarında iki uygulama barındırıldığından, WebClient bir AJAX isteği WebService çıkış noktaları arası bir istektir.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>"Aynı kaynak" nedir?

Aynı düzenleri, konakları ve bağlantı noktaları varsa iki URL'leri aynı kaynağa sahip. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Bu iki URL'leri aynı kaynağa sahip:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Bu URL'leri iki farklı çıkış önceki daha vardır:

- `http://example.net` -Farklı bir etki alanı
- `http://example.com:9000/foo.html` -Farklı bir bağlantı noktası
- `https://example.com/foo.html` -Farklı düzeni
- `http://www.example.com/foo.html` -Farklı bir alt etki alanı

> [!NOTE]
> Internet Explorer bağlantı noktası kaynakları karşılaştırılırken dikkate almaz.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Web hizmeti projesi oluşturma

> [!NOTE]
> Bu bölümde, Web API projeleri oluşturmak nasıl zaten biliyor varsayar. Aksi takdirde bkz [ASP.NET Web API ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Visual Studio'yu açın ve yeni bir **ASP.NET Web uygulaması** projesi. Seçin **boş** proje şablonu. "Klasörleri ekleyin ve çekirdek için başvurular" altında seçin **Web API** onay kutusu. İsteğe bağlı olarak, uygulama Mircosoft Azure'a dağıtmak için "Bulutta Barındır" seçeneğini belirleyin. Microsoft, 10 Web siteleri için ücretsiz bir web barındırma sunar bir [ücretsiz Azure deneme hesabı](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Adlı bir Web API denetleyicisi ekleme `TestController` aşağıdaki kod ile:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Uygulamayı yerel olarak çalıştırın veya Azure'a dağıtın. (Bu öğreticide ekran ı Azure App Service Web Apps için dağıtılır.) Web API çalıştığını doğrulamak için gidin `http://hostname/api/test/`, burada *ana bilgisayar adı* uygulamanın dağıtıldığı etki alanıdır. Yanıt metni görmelisiniz &quot;alın: sınama iletisi&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>WebClient projesi oluşturma

Başka bir ASP.NET Web uygulaması projesi oluşturmak ve **MVC** proje şablonu. İsteğe bağlı olarak, seçin **kimlik doğrulamayı Değiştir** > **doğrulaması yok**. Bu öğretici için kimlik doğrulama gerekmez.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

Çözüm Gezgini'nde Views/Home/Index.cshtml dosyasını açın. Bu dosyadaki kod aşağıdakiyle değiştirin:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

İçin *serviceUrl* değişkeni, Web hizmeti uygulama URI'sini kullanın. Şimdi WebClient uygulamayı yerel olarak çalıştırma veya başka bir Web sitesine yayımlayın.

Listelenen HTTP yöntemini kullanarak Web hizmeti uygulama AJAX isteği gönderen "deneyin" düğmesini tıklatarak açılan kutusundan (GET, POST veya PUT). Bu, bize farklı cross-origin istekleri inceleyin sağlar. Şu anda, Web hizmeti uygulama CORS, desteklemiyor nedenle düğmesine tıklarsanız, hatayla karşılaşırsınız.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Bir aracının HTTP trafiğini izleme hoşlanıyorsanız [Fiddler](http://www.telerik.com/fiddler), tarayıcıyı GET isteğini göndermek ve istek başarılı ancak AJAX çağrısı bir hata döndürüyor görürsünüz. Kaynak aynı ilke tarayıcıdan engellemez anlamak önemlidir *gönderme* istek. Bunun yerine, uygulama görmesini engeller *yanıt*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>CORS etkinleştir

Şimdi şimdi CORS WebService uygulamada etkinleştirin. İlk olarak, CORS NuGet paketini ekleyin. Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Bu komut, en son paketini yükler ve çekirdek Web API kitaplıkları da dahil olmak üzere tüm bağımlılıkları güncelleştirir. Kullanıcı belirli bir sürümünü hedefleyecek şekilde sürüm bayrağı. CORS paketi Web API 2.0 veya sonraki sürümünü gerektirir.

Uygulama dosyasını açın\_Start/WebApiConfig.cs. Aşağıdaki kodu ekleyin **WebApiConfig.Register** yöntemi.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Ardından, eklemek **[EnableCors]** özniteliğini `TestController` sınıfı:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

İçin *çıkış* parametresi, WebClient uygulamanın dağıtıldığı bir URI kullanın. Bu cross-origin istekleri WebClient tüm diğer etki alanları arası istekleri hala vermemek sırasında sağlar. Daha sonra parametrelerini açýklayacaðýz **[EnableCors]** daha ayrıntılı.

Eğik sonunda içermez *çıkış* URL.

Güncelleştirilmiş WebService uygulamayı yeniden dağıtın. WebClient güncelleştirme gerek yoktur. WebClient AJAX isteği başarılı olmalıdır. GET, PUT ve POST yöntemleri tüm izin verilir.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>CORS nasıl çalışır?

Bu bölümde, HTTP iletileri düzeyinde bir CORS istek olanlar açıklanmaktadır. CORS nasıl çalışır, böylece yapılandırabileceğiniz anlamak önemlidir **[EnableCors]** doğru özniteliği ve beklediğiniz gibi şeyler çalışmazsa sorun giderme.

CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üstbilgileri tanıtır. Bir tarayıcı CORS destekliyorsa, bu üstbilgileri cross-origin istekleri için otomatik olarak ayarlar; JavaScript kodunuzda özel bir şey yapmanız gerekmez.

Çıkış noktaları arası istek bir örneği burada verilmiştir. "Kaynak" başlığı istekte sitenin etki alanını sağlar.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Sunucu isteği izin veriyorsa, Access-Control-Allow-Origin üstbilgisini ayarlar. Bu üstbilgi değerini kaynak üst bilgisini eşleşen veya joker karakter değeri "\*", yani her türlü kaynağa izin verilir.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Yanıt Access-Control-Allow-Origin üst bilgi içermeyen AJAX isteği başarısız olur. Özellikle, tarayıcı istek izin vermez. Tarayıcı yanıt, sunucunun başarılı bir yanıt döndürürse bile, istemci uygulamasının kullanımına yapmaz.

**Denetim öncesi isteği**

Bazı CORS istekler için tarayıcı kaynak gerçek isteği göndermeden önce bir "Denetim öncesi isteği" adlı bir ek istek gönderir.

Aşağıdaki koşullar doğruysa, tarayıcı denetim öncesi isteği atlayabilirsiniz:

- İstek yöntemini GET, HEAD veya POST, olan *ve*
- Uygulama kabul etme, Accept-Language, Content-Language, dışındaki tüm istek üstbilgileri ayarlı değil Content-Type veya son-olay-ID *ve*
- Content-Type üstbilgisi (varsa ayarlayın) aşağıdakilerden biri: 

    - Uygulama/x-www-form-urlencoded
    - multipart/form-data
    - Metin/düz

Uygulama çağırarak ayarlar üstbilgileri istek üstbilgileri hakkında kuralın uygulanacağı **setRequestHeader** üzerinde **XMLHttpRequest** nesnesi. (CORS belirtimi bu "Yazar istek üstbilgileri" çağırır.) Kural üstbilgileri uygulanmaz *tarayıcı* User-Agent, ana bilgisayar veya Content-Length gibi ayarlayabilirsiniz.

Bir denetim öncesi isteği bir örneği burada verilmiştir:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Ön uçuş isteği HTTP OPTIONS yöntemini kullanır. İki özel üstbilgi içerir:

- Access-Control-Request-Method: fiili istek için kullanılacak HTTP yöntemi.
- Access-Control-Request-Headers: İstek üstbilgilerini listesi, *uygulama* gerçek isteği ayarlayın. (Yeniden, bu tarayıcı ayarlar üstbilgileri dahil değildir.)

Sunucu isteği izin verdiğini varsayarak bir örnek yanıt şöyledir:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Yanıt, izin verilen yöntemleri listeleyen bir erişim-denetim-Allow-Methods üstbilgisi ve isteğe bağlı olarak izin verilen üstbilgileri listeler bir Access-Control-izin ver-Headers üstbilgisi içeriyor. Denetim öncesi isteği başarılı olursa, tarayıcı daha önce açıklandığı gibi gerçek isteğini gönderir.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>[EnableCors] için kapsam kuralları

Uygulamanızda eylem, denetleyici başına veya genel olarak tüm Web API denetleyicilerinin başına CORS etkinleştirebilirsiniz.

**Her eylem**

Tek bir eylem için CORS etkinleştirmek için ayarlanmış **[EnableCors]** özniteliği eylem yöntemi. Aşağıdaki örnek için CORS etkinleştirir `GetItem` yalnızca yöntemi.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Denetleyici**

Ayarlarsanız **[EnableCors]** denetleyici sınıfını denetleyicisinde tüm eylemler için geçerli olur. Bir eylem için CORS devre dışı bırakmak için ekleme **[DisableCors]** özniteliği eylem. Aşağıdaki örnek dışında her yöntemi için CORS etkinleştirir `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Genel**

Geçişi, uygulamanızın Web API denetleyicilerinin tümü için CORS etkinleştirmek için bir **EnableCorsAttribute** için örnek **EnableCors** yöntemi:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Özniteliği birden fazla kapsamda olarak ayarlanmış, öncelik sırası şöyledir:

1. Eylem
2. Denetleyici
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>İzin verilen çıkış noktası ayarlama

*Çıkış* parametresinin **[EnableCors]** özniteliği belirtir kaynağa erişmek için hangi çıkış noktaları kullanılabilir. Değer izin verilen çıkış noktası, virgülle ayrılmış bir listesidir.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Joker karakter değer de kullanabilirsiniz "\*" tüm kaynakları gelen isteklere izin vermek için.

Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün. Bu, gerçek anlamda bir Web sitesi web API AJAX çağrıları yapabilirsiniz anlamına gelir.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>İzin verilen HTTP yöntemleri Ayarla

*Yöntemleri* parametresinin **[EnableCors]** özniteliği, kaynağa erişmek için hangi HTTP yöntemlerine izin belirtir. Tüm yöntemlere izin vermek için joker karakter değeri kullanmak "\*". Aşağıdaki örnekte, yalnızca GET ve POST isteklere izin verir.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>İzin verilen istek üstbilgilerini Ayarla

Daha önce t bir denetim öncesi isteği bir Access-Control-Request-Headers üstbilgisi nasıl içerebilir uygulama tarafından ayarlanıp HTTP üstbilgileri listeleme açıklanan (sözde "yazar, istek üstbilgileri"). *Üstbilgileri* parametresinin **[EnableCors]** özniteliği belirtir hangi yazar İstek üstbilgilerinin izin verilir. Tüm üstbilgileri izin verecek şekilde ayarlamanız *üstbilgileri* için "\*". Beyaz liste belirli üst bilgileri için Ayarla *üstbilgileri* izin verilen üstbilgileri virgülle ayrılmış bir listesi için:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Ancak, tarayıcılar nasıl bunlar Access-Control-Request-Headers kümesinde tamamen tutarlı değil. Örneğin, Chrome "kaynak"; şu anda içerir bile uygulama bunları betikte ayarladığında FireFox "Kabul et" gibi standart başlıklarını içermez yapılırken.

Ayarlarsanız *üstbilgileri* dışında herhangi bir şeye "\*", "kabul", "content-type" ve "kaynak" artı desteklemek istediğiniz tüm özel üstbilgileri en az içermelidir.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>İzin verilen yanıt üstbilgilerini Ayarla

Varsayılan olarak, tarayıcı tüm uygulama ve yanıt üst bilgilerini kullanıma sunmuyor. Varsayılan olarak kullanılabilir yanıt üstbilgilerini şunlardır:

- Cache-Control
- İçerik dili
- İçerik Türü
- Süre sonu
- Son değiştiren
- Pragma

CORS spec bunlar çağırır [basit yanıt üstbilgilerini](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Diğer üstbilgileri uygulama kullanılabilir hale getirmek için ayarlanmış *exposedHeaders* parametresinin **[EnableCors]**.

Aşağıdaki örnekte, denetleyici 's `Get` yöntemi 'X-özel-üstbilgi' adlı özel bir üstbilgi ayarlar. Varsayılan olarak, tarayıcı bu çıkış noktaları arası istek üstbilgisinde kullanıma değil. Üstbilgi kullanılabilir hale getirmek 'X-özel-üstbilgi' dahil *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Cross-Origin istekleri geçirme kimlik bilgileri

Kimlik bilgileri bir CORS istek özel işleme gerektirir. Varsayılan olarak, tarayıcı çıkış noktaları arası istek ile herhangi bir kimlik bilgisi göndermez. Kimlik bilgileri, tanımlama bilgilerinin yanı sıra HTTP kimlik doğrulama şemasını içerir. Kimlik bilgileri ile bir çıkış noktaları arası istek göndermek için istemci ayarlamalısınız **XMLHttpRequest.withCredentials** true.

Kullanarak **XMLHttpRequest** doğrudan:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

JQuery içinde:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Ayrıca, sunucu kimlik bilgilerine izin vermeniz gerekir. Çıkış noktaları arası kimlik bilgileri Web API'si izin verecek şekilde ayarlamanız **SupportsCredentials** özelliğini true olarak **[EnableCors]** özniteliği:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Bu özellik true ise, HTTP yanıtı bir erişim-denetim-Allow-Credentials üstbilgisi içerir. Bu üst sunucu çıkış noktaları arası istek için kimlik bilgilerini sağlayan tarayıcı söyler.

Kimlik bilgilerini tarayıcı gönderir, ancak yanıt geçerli erişim-denetim-Allow-Credentials üst bilgi içermeyen, tarayıcı uygulaması yanıtı kullanıma değil ve AJAX isteği başarısız olur.

Ayar hakkında çok dikkatli olun **SupportsCredentials** başka bir etki alanındaki bir Web sitesi gönderebilir oturum açmış kullanıcının kimlik bilgileri Web apı'nize kullanıcı adınıza farkına kullanıcı gösterdiğinden, true olarak. CORS spec de bu ayar durumları *çıkış* için &quot; \* &quot; geçersiz durumunda **SupportsCredentials** doğrudur.

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Özel CORS İlkesi sağlayıcıları

**[EnableCors]** özniteliği uygulayan **ICorsPolicyProvider** arabirimi. Türeyen bir sınıf oluşturarak, kendi uygulama sağlayabilir **özniteliği** ve uygulayan **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Herhangi bir yerleştirme, koyabilirsiniz özniteliği uygulayabilirsiniz artık **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Örneğin, bir özel CORS ilke sağlayıcısı ayarları yapılandırma dosyasından okunur.

Öznitelikleri kullanarak alternatif olarak, kaydettiğiniz bir **ICorsPolicyProviderFactory** oluşturur nesne **ICorsPolicyProvider** nesneleri.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Ayarlamak için **ICorsPolicyProviderFactory**, çağrı **SetCorsPolicyProviderFactory** şekilde başlangıçta genişletme yöntemi:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Tarayıcı desteği

Web API CORS, bir sunucu tarafı teknoloji paketidir. Ayrıca kullanıcının tarayıcısına CORS desteklemesi gerekir. Neyse ki, önde gelen tüm tarayıcılar geçerli sürümleri dahildir [desteklemek için CORS](http://caniuse.com/cors).

Internet Explorer 8 ve Internet Explorer 9 XMLHttpRequest yerine eski XDomainRequest nesnesini kullanarak CORS, kısmi desteği vardır. Daha fazla bilgi için bkz: [XDomainRequest - kısıtlamaları, sınırlamalar ve geçici çözümler](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
