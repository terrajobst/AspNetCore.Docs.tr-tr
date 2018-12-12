---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x Hubs API Kılavuzu - JavaScript istemcisi | Microsoft Docs
author: pfletcher
description: Bu belge, SignalR sürüm 1.1 tarayıcılar ve Windows Store (WinJS) applic gibi JavaScript istemcileri için hub'ları API kullanarak bir giriş sağlar...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 2d50a92cff96be5c5c60105bba6682d38f9666b6
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288098"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a>SignalR 1.x Hubs API Kılavuzu - JavaScript İstemcisi
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Bu belge, SignalR sürüm 1.1 tarayıcıları ve Windows Store (WinJS) uygulamalar gibi JavaScript istemcileri için hub'ları API kullanmaya giriş bilgileri sağlar.
> 
> SignalR hub'ları API, bir sunucuya bağlanan istemcilerin ve istemcilerin sunucuya uzaktan yordam çağrısı (RPC) oluşturmanıza olanak sağlar. Sunucu kodu, istemciler tarafından çağrılabilen yöntemleri tanımlamak ve bir istemcide çalışmasına yöntemler çağırır. İstemci kodu sunucudan çağıran yöntemleri tanımlamak ve sunucu üzerinde çalışan yöntemleri çağırın. SignalR tüm istemci-sunucu tesisat sizin için üstlenir.
> 
> SignalR kalıcı bağlantı adlı bir alt düzey API'si de sunar. SignalR hub'ları ve kalıcı bağlantılar için giriş veya tam bir SignalR uygulamanın nasıl oluşturulacağını gösteren bir öğretici için bkz: [SignalR çalışmaya başlama -](../getting-started/index.md).


## <a name="overview"></a>Genel Bakış

Bu belgede aşağıdaki bölümler yer alır:

- [Oluşturulan proxy ve bunu sizin yerinize yapar](#genproxy)

    - [Oluşturulan proxy kullanma zamanı](#cantusegenproxy)
- [İstemci Kurulumu](#clientsetup)

    - [Dinamik olarak oluşturulan proxy başvuru yapma](#dynamicproxy)
    - [Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur](#manualproxy)
- [Nasıl bir bağlantı kurmak için](#establishconnection)

    - [$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi](#connequivalence)
    - [Zaman uyumsuz yürütme başlangıç yöntemi](#asyncstart)
- [Etki alanları arası bağlantı oluşturma](#crossdomain)
- [Bağlantı yapılandırma](#configureconnection)

    - [Sorgu dizesi parametrelerini belirtme](#querystring)
    - [Taşıma yöntemini belirleme](#transport)
- [Bir Hub sınıfı için bir ara sunucu alma](#getproxy)
- [Sunucu çağıran istemciye yöntemleri tanımlama](#callclient)
- [İstemciden sunucu yöntemleri çağırma](#callserver)
- [Bağlantı ömrü olaylarını işlemek nasıl](#connectionlifetime)
- [Hatalarını işleme](#handleerrors)
- [İstemci tarafı günlük kaydını etkinleştirme](#logging)

Sunucu veya .NET istemcileri program hakkında daha fazla belge için aşağıdaki kaynaklara bakın:

- [SignalR hub API Kılavuzu - sunucu](../guide-to-the-api/hubs-api-guide-server.md)
- [SignalR hub API Kılavuzu - .NET istemcisi](../guide-to-the-api/hubs-api-guide-net-client.md)

API başvuru konularına bağlar API .NET 4.5 sürümü var. .NET 4 kullanıyorsanız, bkz. [API konuları .NET 4 sürümünü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Oluşturulan proxy ve bunu sizin yerinize yapar

Bir SignalR hizmeti ile veya olmadan SignalR sizin için oluşturduğu bir ara sunucu ile iletişim kurmak için JavaScript istemci programlayabilirsiniz. Proxy sizin için ne yaptığını, sunucuya çağrıları, yazma yöntemleri bağlanın ve sunucu üzerinde yöntemleri çağırmak kod dizimini basitleştiren olur.

Sunucu yöntemlerini çağırmak için kod yazdığınızda, oluşturulan proxy, yerel bir işlev yürütülürken ancak gibi görünen sözdizimi kullanmanıza olanak sağlar: yazabileceğiniz `serverMethod(arg1, arg2)` yerine `invoke('serverMethod', arg1, arg2)`. Bir sunucu yöntem adı yanlış ise oluşturulan proxy söz dizimi anında ve anlaşılır bir istemci tarafı hata de sağlar. Ve proxy'ler tanımlayan dosyayı el ile oluşturursanız, ayrıca IntelliSense desteği server yöntemlerini çağıran kod yazmak için alabilirsiniz.

Örneğin, sunucu üzerinde aşağıdaki Hub sınıfına olduğunu varsayalım:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Aşağıdaki kod örnekleri ne JavaScript kodu çağırmak için nasıl göründüğüne Göster `NewContosoChatMessage` sunucu ve çağrılarını alma yöntemini `addContosoChatMessageToPage` sunucudan yöntemi.

**Oluşturulan proxy ile**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Üretilen Ara sunucu olmadan**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Oluşturulan proxy kullanma zamanı

Sunucu çağıran bir istemci yöntemi için birden çok olay işleyicisi kaydetmek istiyorsanız, oluşturulan proxy kullanamazsınız. Aksi takdirde, oluşturulan proxy kullanmayı seçebilirsiniz veya kodlama tercihinize bağlı değil. Bunu kullanmayı seçerseniz, "signalr/hubs" URL'de başvuru yoksa bir `script` istemci kodunun öğesi.

<a id="clientsetup"></a>

## <a name="client-setup"></a>İstemci Kurulumu

JavaScript istemcisi, jQuery ve SignalR çekirdek JavaScript dosyası başvuruları gerektirir. JQuery sürümü 1.6.4 veya 1.7.2'ye, 1.8.2 veya 1.9.1 gibi önemli sonraki sürümler olmalıdır. Oluşturulan proxy kullanmaya karar verirseniz, ayrıca SignalR oluşturulan proxy JavaScript dosyasına bir başvuru gerekir. Aşağıdaki örnek, başvuruları oluşturulan proxy kullanan bir HTML sayfasında nasıl görünebileceğini göstermektedir.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Bu sırada bu başvuruları eklenmelidir: jQuery ilk, son bundan sonra SignalR çekirdek ve SignalR proxy'leri.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Dinamik olarak oluşturulan proxy başvuru yapma

Önceki örnekte oluşturulan SignalR proxy fiziksel bir dosya için dinamik olarak oluşturulan JavaScript kodu başvurudur. SignalR hareket halindeyken proxy için JavaScript kodunu oluşturur ve istemciye yanıt "/ signalr/hubs" URL olarak görev yapar. Sunucuda SignalR bağlantıları için farklı bir temel URL belirttiyseniz, `MapHubs` yöntemi dinamik olarak oluşturulan proxy dosyanın URL'si olduğundan, özel bir URL ile "/ hubs" eklenmiş.

> [!NOTE]
> Windows 8 (Windows Store) JavaScript istemciler için dinamik olarak üretilen bir yerine fiziksel proxy dosyası kullanın. Daha fazla bilgi için [SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulan proxy](#manualproxy) bu konuda.


Bir ASP.NET MVC 4 Razor Görünümü'nde, proxy dosya Başvurusu'ndaki uygulama kökü başvurmak için tilde kullanın:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

MVC 4'te SignalR kullanma hakkında daha fazla bilgi için bkz. [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md).

Bir ASP.NET MVC 3 Razor Görünümü'nde kullanmak `Url.Content` proxy dosya başvuru amacıyla:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

Bir ASP.NET Web formları uygulamasında `ResolveClientUrl` , proxy'leri dosya başvurusu veya (bir tilde işareti ile başlayan) bir uygulama kök göreli bir yol kullanarak ScriptManager aracılığıyla kaydetmek için:

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Genel bir kural olarak, CSS ya da JavaScript dosyaları için kullanan "/ signalr/hubs" URL'yi belirtmek için aynı yöntemi kullanın. Bir tilde kullanmadan bir URL belirtirseniz, bazı senaryolarda, IIS Express kullanarak Visual Studio'da test ancak tam IIS dağıttığınızda bir 404 hatası ile başarısız olur, uygulamanızın düzgün çalışacaktır. Daha fazla bilgi için **kök düzeyinde kaynaklara başvurular çözümleniyor** içinde [ASP.NET Web projeleri için Visual Studio'daki Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx) MSDN sitesinden.

Bir web projesi, Visual Studio 2012'de hata ayıklama modunda çalıştırdığınızda ve Internet Explorer tarayıcı olarak kullanıyorsanız, proxy dosyasında görebilirsiniz **Çözüm Gezgini** altında **betik belgelerini**gösterildiği Aşağıdaki çizim.

![Çözüm Gezgini'nde bir JavaScript oluşturulan proxy dosyası](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Dosyanın içeriğini görmek için çift tıklatın **hubs**. Visual Studio 2012 ve Internet Explorer kullanmıyorsanız veya hata ayıklama modunda değilse, dosyanın içeriğini "/ signalR/hubs" URL'sine göz atarak da edinebilirsiniz. Sitenizi en çalışıyorsa Örneğin, `http://localhost:56699`Git `http://localhost:56699/SignalR/hubs` tarayıcınızda.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur

Dinamik olarak oluşturulan proxy alternatif, proxy kodu fiziksel bir dosya oluşturun ve bu dosyaya başvuru. Denetim önbelleğe alma veya davranışı'nı paketlemek için bunu veya sunucu yöntemlere yapılan çağrılar Kodladığınız IntelliSense almak isteyebilirsiniz.

Proxy dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Yükleme [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet paketi.
2. Bir komut istemi açın ve *Araçları* SignalR.exe dosyasını içeren klasör. Araçlar klasöründe şu konumdadır:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Aşağıdaki komutu girin:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Yolu, *.dll* genellikle *bin* proje klasörünüzde klasör.

    Bu komut, adlı bir dosya oluşturur. *server.js* aynı klasörde *signalr.exe*.
4. PUT *server.js* projenize uygun bir klasörde dosya, uygulamanız için uygun şekilde yeniden adlandırın ve "signalr/hubs" başvurusu yerine bir başvuru ekleyin.

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Nasıl bir bağlantı kurmak için

Bir bağlantı kurabilmesi için önce bir bağlantı nesnesi oluşturun, bir proxy oluşturma ve sunucudan çağrılabilir yöntemleri için olay işleyicilerini kaydedin gerekir. Proxy ve olay işleyicileri ayarlandığında çağırarak bağlantı kurmak `start` yöntemi.

Oluşturulan proxy kullanıyorsanız, oluşturulan proxy kodu bunu sizin yerinize yapar kendi kodunuzda bağlantı nesnesi oluşturmanız gerekmez.

<a id="nogenconnection"></a>

**(Oluşturulan proxy ile) bağlantı kurma**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Bir bağlantı (olmadan oluşturulan proxy)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Varsayılan örnek kodu kullanır "/ signalr" SignalR hizmetinize bağlanmak için URL. Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Normalde, olay işleyicileri çağırmadan önce kaydetmeniz `start` bağlantı kurmak için yöntemi. Bağlantı kurulduktan sonra bazı olay işleyicileri kaydetmek istediğiniz, bunu yapabilirsiniz, ancak en az bir çağırmadan önce olay handler(s) kaydetmelisiniz `start` yöntemi. Bunun bir nedeni olduğundan bir uygulamada birçok Hubs olabilir, ancak tetiklemesini istemezsiniz `OnConnected` yalnızca bunlardan birini kullanmak için kullanacaksanız, her hub'da olay. Bağlantı kurulduğunda, bir istemci yöntemi bir Hub'ın proxy varlığını ne tetiklemek için SignalR söyler olan `OnConnected` olay. Tüm olay işleyicileri çağırmadan önce kaydetmezseniz `start` yöntemi oluşturabileceksiniz Hub ancak Hub'ın üzerinde yöntem çağırmak `OnConnected` yöntemi olmaz çağrılabilir ve sunucudan hiçbir istemci yöntemi çağrılır.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi

Oluşturulan proxy kullandığınızda örneklerden gördüğünüz gibi `$.connection.hub` bağlantı nesneye başvurur. Çağırarak alma aynı nesne budur `$.hubConnection()` zaman kullanmadığınız oluşturulan proxy. Oluşturulan proxy kodu, aşağıdaki deyimi yürüterek bağlantıyı sizin için oluşturur:

![Oluşturulan proxy dosyasında bağlantı oluşturma](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Oluşturulan proxy kullanırken ile her şeyi yapabilirsiniz `$.connection.hub` oluşturulan proxy kullanmadığınızda bir bağlantı nesnesi ile yapabilirsiniz.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Zaman uyumsuz yürütme başlangıç yöntemi

`start` Yöntemi zaman uyumsuz olarak yürütür. Döndürür bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/), yani geri çağırma işlevleri gibi yöntemleri çağırarak ekleyebilirsiniz `pipe`, `done`, ve `fail`. Bağlantı kurulduktan sonra yürütmek istediğiniz kodu varsa, sunucu yönteme bir çağrı gibi kod içindeki geri arama işlevi put veya bir geri çağırma işlevini çağırın. `.done` Bağlantı kurulduktan sonra herhangi kod sonra sahip olduğunuz geri çağırma yöntemi yürütüldüğünde, `OnConnected` sunucuda olay işleyicisi yöntemi tamamlandıktan yürütülüyor.

Olarak kodun sonraki satırında, sonra "Şimdi bağlı" deyimi önceki örnekten koyarsanız `start` yöntem çağrısının (değil, bir `.done` geri çağırma), `console.log` satır çalıştırılacağı bağlantı kurulmadan önce aşağıda gösterildiği gibi Örnek:

![Bağlantı kurulduktan sonra çalışan kod yazmak için yanlış bir yol](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Etki alanları arası bağlantı oluşturma

Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantı `http://contoso.com/signalr`. Varsa sayfasından `http://contoso.com` için bir bağlantı kurar `http://fabrikam.com/signalr`, diğer bir deyişle etki alanları arası bağlantı. Güvenlik nedenleriyle, etki alanları arası bağlantıları varsayılan olarak devre dışıdır. Etki alanları arası bağlantı kurmak için etki alanları arası bağlantıları sunucu üzerinde etkin olduğundan emin olun ve bağlantı nesnesi oluşturduğunuzda, bağlantı URL'si belirtin. SignalR kullanacağı en uygun teknolojiyi etki alanları arası bağlantılar için gibi [JSONP](http://en.wikipedia.org/wiki/JSONP) veya [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

Sunucuda çağırdığınızda, bu seçeneği belirleyerek etki alanları arası bağlantıları etkinleştirme `MapHubs` yöntemi.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

İstemcide (olmadan oluşturulan proxy) bağlantı nesnesi oluşturduğunuzda ya da (ile oluşturulan proxy) başlatma yöntemi çağırmadan önce URL'yi belirtin.

**(İle oluşturulan proxy) etki alanları arası bağlantı belirten istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Etki alanları arası bağlantı (olmadan oluşturulan proxy) belirten bir istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

Kullanırken `$.hubConnection` oluşturucusu yok içerecek şekilde `signalr` URL'sindeki otomatik olarak eklendiğinden (siz belirtmediğiniz sürece `useDefaultUrl` olarak `false`).

Farklı uç noktalar birden fazla bağlantı oluşturabilirsiniz.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - Ayarlamamanız `jQuery.support.cors` kodunuzda true.
> 
>     ![JQuery.support.cors true olarak ayarlamanız gerekmez](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR CORS ve JSONP kullanımı işler. Ayar `jQuery.support.cors` doğru devre dışı bırakır için JSONP tarayıcıyı destekleyen CORS varsaymak SignalR neden olduğundan.
> - Localhost URL bağlanırken sunucunun etki alanları arası bağlantılarda etkinleştirmediyseniz bile uygulama yerel olarak IE 10 ile çalışması için Internet Explorer 10, etki alanları arası bağlantı, göz önünde bulundurun olmaz.
> - Etki alanları arası bağlantıları Internet Explorer 9 ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Etki alanları arası bağlantıları Chrome ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Varsayılan örnek kodu kullanır "/ signalr" SignalR hizmetinize bağlanmak için URL. Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Bağlantı yapılandırma

Bir bağlantı kurmadan önce sorgu dizesi parametreleri belirtin veya taşıma yöntemini belirtin.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Sorgu dizesi parametrelerini belirtme

İstemci bağlandığında sunucuya veri göndermek istiyorsanız, bağlantı nesnesi için sorgu dizesi parametreleri ekleyebilirsiniz. Aşağıdaki örnekler bir sorgu dizesi parametresi istemci kodda ayarlama işlemini göstermektedir.

**(İle oluşturulan proxy) başlatma yöntemi çağrılmadan önce bir sorgu dizesi değerini ayarlayın**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**(Olmadan oluşturulan proxy) başlatma yöntemi çağrılmadan önce bir sorgu dizesi değerini ayarlayın**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

Aşağıdaki örnekte, sunucu kodu bir sorgu dizesi parametresi okunacak gösterilmektedir.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Taşıma yöntemini belirleme

Bağlama işleminin bir parçası, SignalR istemci ile sunucu hem sunucu hem de istemci tarafından desteklenen en iyi aktarım belirlemek için normalde görüşür. Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, taşıma yöntemini çağırdığınızda belirterek bu anlaşma işlemi atlayabilirsiniz `start` yöntemi.

**Belirtir (ile oluşturulan proxy) aktarım yöntemi istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Belirtir (olmadan oluşturulan proxy) aktarım yöntemi istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Alternatif olarak, bunları denemek için SignalR istediğiniz sırayla birden fazla taşıma yöntemleri belirtebilirsiniz:

**İstemci kodu, bir özel taşıma geri dönüş şeması (ile oluşturulan proxy) belirtir**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**İstemci kodu, bir özel taşıma geri dönüş şeması (olmadan oluşturulan proxy) belirtir**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Taşıma yöntemi belirtmek için aşağıdaki değerleri kullanabilirsiniz:

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Aşağıdaki örnekler, hangi aktarım yöntemi bir bağlantı tarafından kullanıldığını nasıl gösterir.

**Görüntüler (ile oluşturulan proxy) bir bağlantı tarafından kullanılan aktarım yöntemi istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Görüntüler (olmadan oluşturulan proxy) bir bağlantı tarafından kullanılan aktarım yöntemi istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Sunucu kodu aktarım yöntemi denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - bağlam özelliği istemci hakkında bilgi almak nasıl](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Aktarım ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarım ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Bir Hub sınıfı için bir ara sunucu alma

Oluşturduğunuz her bir bağlantı nesnesi bir veya daha fazla Hub sınıfları içeren bir SignalR service bağlantısı ile ilgili bilgileri yalıtır. Bir Hub sınıfı ile iletişim kurmak için bir proxy nesnesi (oluşturulan proxy değil kullanıyorsanız), sizin oluşturduğunuz veya sizin için oluşturulduğu kullanın.

İstemcide Ara sunucu adını Hub sınıf adı ortası büyük küçük harfleri bir sürümü var. Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.

**Sunucudaki hub sınıfı**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Hub için oluşturulan istemci proxy'si bir başvuru alma**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Hub sınıfı (olmadan oluşturulan proxy) için istemci proxy oluşturma**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Hub sınıfınıza tasarlamanız, bir `HubName` özniteliği, servis talebi değiştirmeden tam adını kullanın.

**HubName özniteliğine sahip sunucudaki hub sınıfı**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Hub için oluşturulan istemci proxy'si bir başvuru alma**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Hub sınıfı (olmadan oluşturulan proxy) için istemci proxy oluşturma**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Sunucu çağıran istemciye yöntemleri tanımlama

Bir Hub'ından sunucu çağıran bir yöntemi tanımlamak için bir olay işleyicisi Hub proxy için kullanarak ekleme `client` oluşturulan proxy veya arama özelliğini `on` oluşturulan proxy kullanmıyorsanız yöntemi. Parametreleri karmaşık nesneler olabilir.

Çağırmadan önce olay işleyicisi ekleme `start` bağlantı kurmak için yöntemi. (Çağırdıktan sonra olay işleyicileri eklemek istiyorsanız `start` yöntemi,'ündeki nota bakın [bağlantı kurmak nasıl](#establishconnection) daha önce açıklanan belge ve oluşturulan proxy kullanmadan bir yöntemi tanımlamak için sözdizimini kullanın.)

Yöntem adı ile eşleşen büyük küçük harfe duyarlıdır. Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütülür `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, veya `addcontosochatmessagetopage` istemci üzerinde.

**(İle oluşturulan proxy) istemcide yöntemi tanımlayın.**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**İstemci (ile oluşturulan proxy) yöntemi tanımlamak için alternatif bir yolu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**İstemci (oluşturulan proxy olmadan veya başlangıç yöntemi çağrıldıktan sonra eklerken) yöntemi tanımlayın.**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**İstemci yöntemini çağıran sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Aşağıdaki örnekler, karmaşık bir nesne bir yöntem parametresi olarak içerir.

**Karmaşık bir nesnenin (ile oluşturulan proxy) alan istemci yöntemi tanımlayın.**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Karmaşık bir nesnenin (olmadan oluşturulan proxy) alan istemci yöntemi tanımlayın.**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Karmaşık bir nesne tanımlayan bir sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Karmaşık bir nesne kullanarak istemci yöntemini çağıran sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>İstemciden sunucu yöntemleri çağırma

İstemciden gelen sunucu yöntemini çağırmak için kullanın `server` oluşturulan proxy özelliğini veya `invoke` oluşturulan proxy kullanmıyorsanız Hub proxy yöntemi. Dönüş değeri veya parametreleri karmaşık nesneler olabilir.

Yöntem adı ortası büyük sürümünde hub'da geçirin. Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.

Aşağıdaki örnekler, dönüş değeri olmayan bir sunucu yöntemin nasıl çağrılacağını ve bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.

**Sunucu yöntemiyle HubMethodName öznitelik yok**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Karmaşık bir nesne tanımlayan bir sunucu kodu bir parametre geçirildi.**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Çağıran Metoda (ile oluşturulan proxy) istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Çağıran Metoda (olmadan oluşturulan proxy) istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Hub yönteminin ile donatılmış varsa bir `HubMethodName` özniteliği, servis talebi değiştirmeden bu adı kullanın.

**Sunucu yöntemi** HubMethodName özniteliğine sahip

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Çağıran Metoda (ile oluşturulan proxy) istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Çağıran Metoda (olmadan oluşturulan proxy) istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

Önceki örneklerde, dönüş değeri olmayan bir sunucu yöntemin nasıl çağrılacağını gösterir. Aşağıdaki örnekler bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.

**Bir dönüş değerine sahip bir yöntem için sunucu kodu**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**Stok sınıfı için kullanılan** dönüş değeri

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Çağıran Metoda (ile oluşturulan proxy) istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Çağıran Metoda (olmadan oluşturulan proxy) istemci kodu**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Bağlantı ömrü olaylarını işlemek nasıl

SignalR işleyebilirsiniz ömür olayları aşağıdaki bağlantı sağlar:

- `starting`: Herhangi bir veri bağlantısı üzerinden gönderilmeden önce oluşturulur.
- `received`: Herhangi bir veri bağlantısı alındığında oluşturulur. Alınan veriler sağlar.
- `connectionSlow`: İstemci yavaş veya sık bırakma bağlantı tespit ettiğinde oluşturulur.
- `reconnecting`: Temel alınan aktarımda yeniden bağlanmayı başladığında oluşturulur.
- `reconnected`: Temel alınan aktarımda bağlandığınızda oluşturulur.
- `stateChanged`: Bağlantı durumu değiştiğinde oluşturulur. Eski durum ve yeni bir durum (bağlanma, bağlı, yeniden bağlanılıyor veya bağlantısı kesilmiş) sağlar.
- `disconnected`: Bağlantı kesildiğinde oluşturulur.

Örneğin, belirgin gecikmeler neden olabilecek bağlantı sorunları olduğunda uyarı iletileri görüntülemek istiyorsanız, tanıtıcı `connectionSlow` olay.

**(İle oluşturulan proxy) connectionSlow olayını işle**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**(Olmadan oluşturulan proxy) connectionSlow olayını işle**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Daha fazla bilgi için [anlama ve signalr'da bağlantı ömrü olaylarını işleme](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Hatalarını işleme

SignalR JavaScript istemci sağlayan bir `error` olayı için bir işleyici ekleyebilirsiniz. Fail yöntemi, bir sunucu yöntem çağrısından kaynaklanan hatalar için bir işleyici eklemek için de kullanabilirsiniz.

Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, bir hatanın ardından SignalR döndüren özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir. Örneğin, bir çağrı `newContosoChatMessage` başarısız, hata iletisi hata nesnesi içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" güvenlik nedeniyle, ayrıntılı hata iletileri için etkinleştirmek istiyorsanız ancak üretimde istemciler için ayrıntılı hata iletileri önerilmez gönderme sorun giderme amacıyla sunucu üzerinde aşağıdaki kodu kullanın.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

Aşağıdaki örnek, bir hata olayı işleyicisi eklemek gösterilmektedir.

**(İle oluşturulan proxy) bir hata işleyicisi ekleme**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Bir hata işleyicisi (olmadan oluşturulan proxy) ekleme**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Aşağıdaki örnek, bir yöntem çağrısı bir hata nasıl ele alınacağını gösterir.

**Bir yöntem çağırma (ile oluşturulan proxy) bir hata işleme**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Bir yöntem çağırma (olmadan oluşturulan proxy) bir hata işleme**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Bir yöntem çağrısı başarısız olursa, `error` olayı yükseltildiğinde de, bu nedenle kodunuzda `error` yöntemin işleyicisi ve `.fail` yöntemi geri çağırma yürütün.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>İstemci tarafı günlük kaydını etkinleştirme

Bir bağlantı üzerinde istemci tarafı günlüğe kaydetmeyi etkinleştirmek için ayarlanmış `logging` özelliği çağırmadan önce bağlantı nesnesindeki `start` bağlantı kurmak için yöntemi.

**(İle oluşturulan proxy) günlük kaydını etkinleştirme**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**(Olmadan oluşturulan proxy) günlüğe yazmayı etkinleştir**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Günlükleri görmek için tarayıcınızın Geliştirici Araçları'nı açın ve konsol sekmesine gidin. Adım adım yönergeler ve ekran görüntüsü bunun nasıl yapılacağını gösteren gösteren bir öğretici için bkz. [ASP.NET Signalr - günlüğü etkinleştir ile sunucu yayını](index.md).
