---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: ASP.NET Web API 2'de kaynaklar arası istekleri etkinleştirme | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API'de çıkış noktaları arası kaynak paylaşımı (CORS) destekleyecek şekilde gösterilmektedir.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 97a0027194b019b09e220493dcb593e682027fe3
ms.sourcegitcommit: d22b3c23c45a076c4f394a70b1c8df2fbcdf656d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55428453"
---
<a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>ASP.NET Web API 2'de kaynaklar arası istekleri etkinleştirme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Tarayıcı güvenliği, bir web sitesinin başka bir etki alanına AJAX istekleri göndermesini engeller. Bu kısıtlama adlı *aynı çıkış noktası İlkesi*ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler. Ancak, bazen, web API'si çağırma diğer sitelere izin vermek isteyebilirsiniz.
>
> [Kaynağın kaynak paylaşımını çapraz](http://www.w3.org/TR/cors/) (CORS) olan gevşek bir aynı çıkış noktası ilkesi izin veren bir W3C standart. CORS kullanarak, bir sunucu açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini. CORS güvenli ve önceki teknikler daha esnek gibi [JSONP](http://en.wikipedia.org/wiki/JSONP). Bu öğreticide, Web API uygulamanıza CORS'yi etkinleştirme işlemi gösterilmektedir.
>
> ## <a name="software-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web API 2.2

## <a name="introduction"></a>Giriş

Bu öğreticide, ASP.NET Web API CORS desteği gösterilmektedir. – "Web API denetleyicisi barındıran, bir çağrılan Web hizmetini" ve "Web hizmetini çağıran diğer adlı WebClient", iki ASP.NET projesi oluşturarak başlayacağız. İki uygulama farklı etki alanlarında barındırıldığından, WebClient bir AJAX isteği WebService çıkış noktaları arası isteğidir.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>"Aynı kaynak" nedir?

Aynı düzeni, konaklar ve bağlantı noktaları varsa iki URL aynı kaynağa sahip. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Bu iki URL aynı kaynağa sahip:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Bu URL'ler önceki değerinden farklı kaynakları iki vardır:

- `http://example.net` -Farklı bir etki alanı
- `http://example.com:9000/foo.html` -Farklı bir bağlantı noktası
- `https://example.com/foo.html` -Farklı düzeni
- `http://www.example.com/foo.html` -Farklı bir alt etki alanı

> [!NOTE]
> Internet Explorer bağlantı noktası kaynakları karşılaştırılırken göz önünde bulundurmaz.

## <a name="create-the-webservice-project"></a>Web hizmeti projesi oluşturma

> [!NOTE]
> Bu bölümde, Web API projeleri oluşturma bildiğiniz varsayılır. Aksi takdirde bkz [ASP.NET Web API'si ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

1. Visual Studio'yu başlatın ve yeni bir **ASP.NET Web uygulaması (.NET Framework)** proje.
2. İçinde **yeni ASP.NET Web uygulaması** iletişim kutusunda **boş** proje şablonu. Altında **klasörleri ekleyin ve çekirdek başvuruları**seçin **Web API** onay kutusu.

   ![Visual Studio'da yeni ASP.NET projesi iletişim kutusu](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. Adlı bir Web API denetleyicisi ekleme `TestController` aşağıdaki kod ile:

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. Uygulamayı yerel olarak çalıştırmak veya Azure'a dağıtın. (Ekran görüntüleri için Bu öğreticide, Azure App Service Web Apps için uygulamayı dağıtır.) Web API'si çalıştığını doğrulamak için gidin `http://hostname/api/test/`burada *hostname* uygulamanın dağıtıldığı etki alanıdır. Yanıt metni görmelisiniz &quot;alın: Sınama iletisi&quot;.

   ![Web tarayıcı gösteren sınama iletisi](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>WebClient projesi oluşturma

1. Hesaplanmış sütun oluşturabiliriz **ASP.NET Web uygulaması (.NET Framework)** seçin ve proje **MVC** proje şablonu. İsteğe bağlı olarak **kimlik doğrulamayı Değiştir** > **kimlik doğrulaması yok**. Bu öğretici için kimlik doğrulaması gerekmez.

   ![Yeni ASP.NET projesi iletişim kutusunda Visual Studio'da MVC şablonu](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. İçinde **Çözüm Gezgini**, dosyayı açma *Views/Home/Index.cshtml*. Bu dosyadaki kodu aşağıdakiyle değiştirin:

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   İçin *serviceUrl* değişkeni, Web hizmeti uygulama URI'sini kullanın.

3. WebClient uygulamayı yerel olarak çalıştırmak veya başka bir Web sitesine yayımlayabilirsiniz.

"Try It" düğmesine tıkladığınızda, listelenen HTTP yöntemini kullanarak Web hizmeti uygulaması için bir AJAX isteği gönderilir açılan kutuda (GET, POST veya PUT). Bu farklı çıkış noktaları arası istekleri incelemenize olanak sağlar. Şu anda düğmesine tıklarsanız bir hata alırsınız. Bu nedenle Web hizmeti uygulama CORS desteklemiyor.

![Tarayıcı 'try' hatası](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Bir aracının HTTP trafiğini izleme hoşlanıyorsanız [Fiddler](https://www.telerik.com/fiddler), tarayıcı GET isteği gönderir ve isteğin başarılı ancak AJAX çağrısı bir hata döndürür görürsünüz. Aynı çıkış noktası İlkesi tarayıcıdan engellemez anlaşılması önemlidir *gönderme* istek. Bunun yerine, uygulama görmesini engeller *yanıt*.

![Fiddler'ı web hata ayıklayıcısı Web istekleri gösteriliyor](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>CORS'yi etkinleştirme

Şimdi github'dan WebService uygulamada CORS'yi etkinleştirme. İlk olarak, CORS NuGet paketini ekleyin. Visual Studio'da gelen **Araçları** menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu yazın:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Bu komut, en son paketini yükler ve çekirdek Web API kitaplıkları dahil olmak üzere tüm bağımlılıkları güncelleştirir. Kullanım `-Version` belirli bir sürümü hedeflemek için bayrak. CORS paketi, Web API 2.0 veya sonraki sürümünü gerektirir.

Dosyayı açmak *uygulama\_Start/WebApiConfig.cs*. Aşağıdaki kodu ekleyin **WebApiConfig.Register** yöntemi:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Ardından, ekleme **[EnableCors]** özniteliğini `TestController` sınıfı:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

İçin *kaynakları* parametresi WebClient uygulamanın dağıtıldığı bir URI kullanın. Bu çıkış noktaları arası istekleri WebClient tüm diğer etki alanları arası istekleri engelleyerek hala çalışırken sağlar. Daha sonra parametrelerini açıklayacağız **[EnableCors]** daha ayrıntılı bir şekilde.

Sonunda eğik çizgi içermez *kaynakları* URL'si.

Güncelleştirilmiş WebService uygulamayı yeniden dağıtın. WebClient güncelleştirmek gerekmez. WebClient AJAX isteği başarılı olmalıdır. GET, PUT ve POST yöntemleri tüm izin verilir.

![Web tarayıcı gösteren başarılı bir sınama iletisi](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>CORS nasıl çalışır?

Bu bölümde, HTTP iletileri düzeyinde bir CORS isteğinde ne açıklanmaktadır. Böylece yapılandırabileceğiniz CORS nasıl çalıştığını anlamak önemlidir **[EnableCors]** doğru öznitelik ve beklediğiniz gibi şeyler çalışmazsa sorun giderme.

CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üst bilgilerini ortaya çıkarır. Bir tarayıcı CORS destekliyorsa, bu üstbilgileri çıkış noktaları arası istekleri için otomatik olarak ayarlar; JavaScript kodunuza özel herhangi bir şey yapmanız gerekmez.

Çıkış noktaları arası isteğinin bir örneği aşağıda verilmiştir. "Kaynak" üst bilgisi istekte site etki alanı sağlar.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Sunucu isteği izin veriyorsa, Access-Control-Allow-Origin üst bilgi ayarlar. Bu üst bilgi değeri ile eşleşen kaynak üst bilgisi ya da joker karakter değeri "\*", yani her türlü kaynağa izin verilir.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Access-Control-Allow-Origin üst bilgi yanıtı içermez AJAX isteği başarısız olur. Özellikle, tarayıcının isteği izin vermiyor. Tarayıcı yanıt, sunucunun başarılı bir yanıt döndürürse bile, istemci uygulama tarafından kullanılabilir yapmaz.

**Denetim öncesi isteği**

Bazı CORS istekleri için tarayıcı kaynak gerçek isteği göndermeden önce bir "Denetim öncesi isteği" adlı ek bir istek gönderir.

Aşağıdaki koşullar doğruysa, tarayıcının denetim öncesi isteği atlayabilirsiniz:

- İstek yöntemini GET, HEAD veya sonrası, olan *ve*
- Uygulama dışındaki içeriği kabul et, Accept-Language, dil, herhangi bir istek üst ayarlamaz Content-Type veya son-Event-ID *ve*
- Content-Type üst bilgisi (varsa ayarlayın) aşağıdakilerden biridir:

    - Application/x-www-form-urlencoded işlemek
    - multipart/form-data
    - metin/düz

Uygulama çağırarak ayarlar üst istek üst bilgileri hakkında kuralın uygulanacağı **setRequestHeader** üzerinde **XMLHttpRequest** nesne. (Bu "Yazar istek üstbilgilerini" CORS belirtimi çağırır.) Kural üstbilgileri uygulanmaz *tarayıcı* kullanıcı aracısı, konak veya Content-Length gibi ayarlayabilirsiniz.

Denetim öncesi isteğinin bir örneği aşağıda verilmiştir:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Uçuş öncesi isteğinin HTTP OPTIONS yöntemini kullanır. Bu, iki özel üst bilgileri içerir:

- Erişim-Control-Request-Method: Fiili istek için kullanılacak HTTP yöntemi.
- Access-Control-Request-Headers: İstek üst bilgilerinin bir listesi, *uygulama* gerçek istek üzerinde ayarlanan. (Yeniden, bu tarayıcı ayarlar üst bilgileri içermez.)

Sunucunun isteği izin varsayılarak bir yanıt örneği, şu şekildedir:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Yanıt, izin verilen yöntemleri listeleyen bir erişim-denetim-Allow-Methods üst bilgisi ve isteğe bağlı olarak izin verilen üstbilgileri listeleyen bir Access-Control-izin ver-Headers üstbilgisi içeriyor. Denetim öncesi isteği başarıyla sonuçlanırsa, tarayıcı daha önce açıklandığı gibi gerçek bir istek gönderir.

Uç nokta denetim öncesi OPTIONS istekleri ile test etmek için yaygın olarak kullanılan araçlar (örneğin, [Fiddler](https://www.telerik.com/fiddler) ve [Postman](https://www.getpostman.com/)) varsayılan olarak gerekli seçenekleri üst bilgileri gönderme. Onaylayın `Access-Control-Request-Method` ve `Access-Control-Request-Headers` üstbilgileri, istekle birlikte gönderilir ve Seçenekler üst bilgileri uygulama IIS üzerinden ulaşın.

IIS almak ve seçeneği isteklerini işlemek bir ASP.NET uygulaması izin verecek şekilde yapılandırmak için aşağıdaki yapılandırma uygulamanın ekleme *web.config* dosyası `<system.webServer><handlers>` bölümü:

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

Kaldırılmasını `OPTIONSVerbHandler` IIS OPTIONS istekleri işleme öğesinden engeller. Adlandırılan `ExtensionlessUrlHandler-Integrated-4.0` OPTIONS istekleri varsayılan modülün kaydını yalnızca GET, HEAD, POST ve hata ayıklama isteği uzantısız URL'lerle izin verdiğinden, uygulamanıza ulaşmasına izin verir.

## <a name="scope-rules-for-enablecors"></a>Kapsam kuralları [EnableCors] için

Uygulamanızdaki her eylem, denetleyici başına veya genel olarak tüm Web APİ'si denetleyicilerinin için CORS etkinleştirebilirsiniz.

**Eylem başına**

Tek bir eylem için CORS etkinleştirmek için ayarlayın **[EnableCors]** özniteliği eylem yöntemi. Aşağıdaki örnek için CORS `GetItem` yalnızca yöntemi.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Denetleyici**

Ayarlarsanız **[EnableCors]** denetleyici sınıfı üzerinde denetleyicinin tüm eylemleri için geçerlidir. Bir eylem için CORS devre dışı bırakmak için ekleme **[DisableCors]** eyleme özniteliği. Aşağıdaki örnek her yöntemi dışında için CORS sağlar `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Genel olarak**

Uygulamanızdaki tüm Web APİ'si denetleyicilerinin CORS'yi etkinleştirmek için geçirmek bir **EnableCorsAttribute** için örnek **EnableCors** yöntemi:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Birden fazla kapsamda öznitelik ayarlanırsa, öncelik sırası şöyledir:

1. Eylem
2. Denetleyici
3. Global

## <a name="set-the-allowed-origins"></a>İzin verilen çıkış noktaları ayarlama

*Kaynakları* parametresinin **[EnableCors]** özniteliği belirtir kaynağa erişmek için hangi çıkış noktaları kullanılabilir. İzin verilen çıkış noktaları, virgülle ayrılmış listesini değerdir.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Joker karakter değeri de kullanabilirsiniz "\*" tüm kaynaklar gelen isteklere izin vermek için.

Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün. Bu, tam anlamıyla herhangi bir Web sitesinde Web API AJAX çağrıları yapabileceğini anlamına gelir.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>İzin verilen HTTP yöntemleri Ayarla

*Yöntemleri* parametresinin **[EnableCors]** özniteliği belirtir kaynağa erişmek için hangi HTTP yöntemlerine izin verilir. Tüm yöntemlere izin için joker karakter değeri kullanın. "\*". Aşağıdaki örnek yalnızca GET ve POST istekleri sağlar.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>İzin verilen istek üstbilgilerini Ayarla

Daha önce nasıl bir denetim öncesi isteği uygulama tarafından ayarlanıp HTTP üst bilgileri listeleyen bir Access-Control-Request-Headers üstbilgisi içeriyor olabilir, bu makalede açıklanan (Malum "yazar, istek üst bilgileri"). *Üstbilgileri* parametresinin **[EnableCors]** özniteliği belirtir hangi yazar istek üst bilgiye izin verilir. Üst bilgileri izin verecek şekilde ayarlanmış *üstbilgileri* için "\*". Beyaz liste belirli üstbilgileri için ayarlanmış *üstbilgileri* izin verilen üstbilgileri virgülle ayrılmış bir listesi için:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Ancak, tarayıcılar nasıl bunlar Access-Control-Request-Headers kümesinde tamamen tutarlı değil. Örneğin, Chrome, şu anda "Başlangıç" içerir. Bile uygulama bunları betikte ayarladığında FireFox "Kabul et" gibi standart başlıklarını içermez.

Ayarlarsanız *üstbilgileri* dışında herhangi bir şey için "\*", "kabul", "content-type" ve "Başlangıç" yanı sıra, desteklemek istediğiniz tüm özel üst en az içermelidir.

## <a name="set-the-allowed-response-headers"></a>İzin verilen yanıt üstbilgilerini Ayarla

Varsayılan olarak, tarayıcı tüm yanıt üstbilgilerini uygulamaya kullanıma sunmuyor. Varsayılan olarak kullanılabilir olan yanıt üstbilgilerini şunlardır:

- Önbellek denetimi
- İçerik dili
- İçerik Türü
- Süre sonu
- Son değiştirilme
- Pragma

CORS spec bu çağrıları [basit yanıt üstbilgilerini](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için ayarlanmış *exposedHeaders* parametresinin **[EnableCors]**.

Aşağıdaki örnekte, denetleyici 's `Get` yöntemi 'X-Custom-Header' adlı bir özel üst bilgi ayarlar. Varsayılan olarak, tarayıcı bu çıkış noktaları arası istek üstbilgisinde açığa çıkarmamak. Üst bilgi kullanılabilir hale getirmek için 'X-Custom-Header' dahil *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>Çıkış noktaları arası istekleri Pass kimlik bilgileri

Özel işleme bir CORS isteğinde kimlik bilgilerini gerektirir. Varsayılan olarak, tarayıcının çıkış noktaları arası istek ile herhangi bir kimlik bilgisi göndermez. Kimlik bilgileri, tanımlama bilgilerinin yanı sıra HTTP kimlik doğrulama düzenleri içerir. İstemci kimlik bilgileriyle bir çıkış noktaları arası istek göndermek için ayarlamalısınız **XMLHttpRequest.withCredentials** true.

Kullanarak **XMLHttpRequest** doğrudan:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

JQuery içinde:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Ayrıca, sunucunun kimlik bilgilerini izin vermeniz gerekir. Web API'de çıkış noktaları arası kimlik bilgileri'izin verecek şekilde ayarlanmış **SupportsCredentials** özelliğini true olarak **[EnableCors]** özniteliği:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Bu özellik true ise, HTTP yanıtı erişim-denetim-Allow-Credentials üst bilgisini içerir. Bu üst bilgi, tarayıcı sunucunun bir çıkış noktaları arası istek için kimlik bilgilerini sağlar söyler.

Tarayıcı bilgilerini gönderir, ancak yanıt, geçerli bir erişim-denetim-Allow-Credentials üst bilgisi içermez, tarayıcı uygulamaya yanıt açığa çıkarmamak ve AJAX isteği başarısız olur.

Ayar hakkında dikkatli olun **SupportsCredentials** başka bir etki alanındaki bir Web sitesi gönderebilir bir oturum açmış kullanıcı kimlik bilgilerini kullanıcının adına, Web API'niz için farkına varmadan kullanıcı anlamına geldiğinden, true. CORS spec Ayrıca bu ayar durumları *kaynakları* için &quot; \* &quot; geçersiz olursa **SupportsCredentials** geçerlidir.

## <a name="custom-cors-policy-providers"></a>CORS ilkesini sağlayıcılar

**[EnableCors]** özniteliğini uygular **ICorsPolicyProvider** arabirimi. Türetilen bir sınıf oluşturarak kendi uygulamanız sağlayabilir **özniteliği** ve uygulayan **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Artık tüm yerleştirme, koyabilirsiniz öznitelik uygulayabilirsiniz **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Örneğin, özel bir CORS ilke sağlayıcısı, ayarlar yapılandırma dosyasından okuyabilir.

Öznitelikleri kullanarak alternatif olarak, kaydedebileceğiniz bir **ICorsPolicyProviderFactory** oluşturan nesne **ICorsPolicyProvider** nesneleri.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Ayarlanacak **ICorsPolicyProviderFactory**, çağrı **SetCorsPolicyProviderFactory** genişletme yöntemi aşağıdaki gibi başlangıçta:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>Tarayıcı desteği

Web API CORS, bir sunucu tarafı teknoloji paketidir. Ayrıca kullanıcının tarayıcı CORS desteği gerekir. Neyse ki, tüm temel tarayıcılarda geçerli sürümleri dahil [desteklemek için CORS](http://caniuse.com/cors).
