---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: "ASP.NET SignalR hub'ları API Kılavuzu - JavaScript istemci | Microsoft Docs"
author: pfletcher
description: "Bu belge için SignalR sürüm 2'in JavaScript istemciler, tarayıcıları ve Windows Mağazası (WinJS) applicat gibi hub API'sini kullanmaya tanıtılmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 65e369a393a8c5d2d1bba11b5c71347df8f9c69d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>ASP.NET SignalR hub'ları API Kılavuzu - JavaScript istemci
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)

> Bu belge SignalR sürüm 2'in JavaScript istemciler, tarayıcıları ve Windows Mağazası (WinJS) uygulamaları gibi için hub API kullanarak giriş bilgileri sağlar.
> 
> SignalR hub'ları API bir sunucuya bağlanan istemciler ve sunucu istemcilerine uzaktan yordam çağrılarını (RPC) yapmanızı sağlar. Sunucu kodu, istemciler tarafından çağrılabilir yöntemlerini tanımlama ve istemci üzerinde çalışan yöntemlerini çağırın. İstemci kodu sunucudan çağrılabilir yöntemlerini tanımlama ve sunucu üzerinde çalışan yöntemlerini çağırın. SignalR, istemci-sunucu tesisat tümünün ilgilenir.
> 
> SignalR kalıcı bağlantılar olarak adlandırılan bir alt düzey API de sunar. SignalR, hub'lara ve kalıcı bağlantıların giriş için bkz: [SignalR giriş](../getting-started/introduction-to-signalr.md).
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

- [Oluşturulan proxy ve onu sizin için ne yaptığını](#genproxy)

    - [Oluşturulan proxy kullanma zamanı](#cantusegenproxy)
- [İstemci Kurulumu](#clientsetup)

    - [Dinamik olarak üretilen proxy başvuru yapma](#dynamicproxy)
    - [Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur](#manualproxy)
- [Bir bağlantı kurmak nasıl](#establishconnection)

    - [$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi](#connequivalence)
    - [Zaman uyumsuz yürütme başlangıç yöntemi](#asyncstart)
- [Etki alanları arası bağlantı kurmak nasıl](#crossdomain)
- [Bağlantı yapılandırma](#configureconnection)

    - [Sorgu dizesi parametreleri belirtme](#querystring)
    - [Taşıma yöntemini belirleme](#transport)
- [Bir Hub sınıf için bir proxy alma](#getproxy)
- [Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama](#callclient)
- [İstemciden sunucu yöntemleri çağırmak nasıl](#callserver)
- [Bağlantı ömür olayları işleme](#connectionlifetime)
- [Hataların nasıl işleneceğini](#handleerrors)
- [İstemci-tarafı günlük kaydını etkinleştirme](#logging)

Sunucu veya .NET istemcileri program konusunda daha fazla belgeler için aşağıdaki kaynaklara bakın:

- [SignalR hub'ları API Kılavuzu - sunucu](hubs-api-guide-server.md)
- [SignalR hub'ları API Kılavuzu - .NET istemcisi](hubs-api-guide-net-client.md)

SignalR 2 sunucu bileşeni (rağmen bir .NET istemci SignalR 2, .NET 4.0 için) yalnızca .NET 4.5 üzerinde kullanılabilir.

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>Oluşturulan proxy ve onu sizin için ne yaptığını

SignalR hizmet ile veya SignalR sizin için oluşturduğu ara sunucu olmadan ile iletişim kurmak için JavaScript istemci programlama yapabilirsiniz. Proxy sizin için ne yaptığını, sunucuya çağrıları, yazma yöntemleri bağlanmanız ve sunucuda yöntemlerini çağıran kodu söz dizimini basitleştirmek olur.

Sunucu yöntemleri çağırmak için kodu yazarken oluşturulan proxy, yerel bir işlev yürütülmekte ancak gibi görünen sözdizimi kullanmanıza olanak tanır: yazabileceğiniz `serverMethod(arg1, arg2)` yerine `invoke('serverMethod', arg1, arg2)`. Bir sunucu yöntem adı yanlış yazdınız oluşturulan proxy sözdizimi ayrıca istemci tarafı hata hemen ve intelligible sağlar. Ve proxy'leri tanımlayan dosyasını elle oluşturursanız, ayrıca IntelliSense desteği server yöntemlerini çağıran kodu yazmak için alabilirsiniz.

Örneğin, sunucu üzerinde aşağıdaki Hub sınıf olduğunu varsayalım:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Aşağıdaki kod ne JavaScript kodu çağırmaktan gibi görünüyor örnekler `NewContosoChatMessage` sunucu ve çağırmaları alma yöntemini `addContosoChatMessageToPage` sunucudan yöntemi.

**Oluşturulan proxy ile**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Oluşturulan proxy**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Oluşturulan proxy kullanma zamanı

Sunucu çağıran bir istemci yöntemi için birden çok olay işleyicilerini kaydetmek istiyorsanız, oluşturulan proxy kullanamazsınız. Aksi takdirde, oluşturulan proxy kullanmayı seçebilirsiniz veya kodlama tercihinize bağlı değil. Bunu kullanmayı seçerseniz, "signalr/hub" URL'de başvuru yoksa bir `script` istemci kodunuzda öğesi.

<a id="clientsetup"></a>

## <a name="client-setup"></a>İstemci Kurulumu

JavaScript istemci jQuery ve SignalR core JavaScript dosyası başvuruları gerektirir. JQuery sürümü 1.6.4 veya 1.7.2, 1.8.2 veya 1.9.1 gibi önemli sonraki sürümleri olmalıdır. Oluşturulan proxy kullanmaya karar verirseniz ayrıca SignalR oluşturulan proxy JavaScript dosyası başvuru gerekir. Aşağıdaki örnek, başvuru oluşturulan proxy kullanan bir HTML sayfasında nasıl görünebileceğini gösterir.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Bu başvuruları bu sırada yer almalıdır: jQuery ilk, son SignalR core bundan sonra ve SignalR proxy'leri.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Dinamik olarak üretilen proxy başvuru yapma

Önceki örnekte oluşturulan SignalR proxy fiziksel bir dosya için dinamik olarak üretilen JavaScript kodu başvurudur. SignalR anında proxy'si için JavaScript kodunu oluşturur ve istemci yanıt "/ signalr/hub" URL olarak görev yapar. Sunucuda SignalR bağlantıları için farklı bir temel URL belirttiyseniz, `MapSignalR` dinamik olarak üretilen proxy dosyanın URL'sini yöntemidir, özel bir URL ile "/ hub" eklenmiş.

> [!NOTE]
> Windows 8 (Windows mağazası) JavaScript istemciler için dinamik olarak üretilen bir yerine fiziksel proxy dosyasını kullanın. Daha fazla bilgi için bkz: [SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulan proxy](#manualproxy) bu konuda daha sonra.


Bir ASP.NET MVC 4 veya 5 Razor görünümünde tilde proxy dosya Başvurusu'ndaki uygulama kökü başvurmak için kullanın:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

MVC 5'te SignalR kullanma hakkında daha fazla bilgi için bkz: [SignalR ve MVC 5 ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Bir ASP.NET MVC 3 Razor görünümünde kullanın `Url.Content` proxy dosya başvurusu için:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

Bir ASP.NET Web Forms uygulamada kullanan `ResolveClientUrl` , proxy'leri dosya başvuru veya (bir tilde başlayarak) bir uygulama kök göreli bir yol kullanarak ScriptManager aracılığıyla kaydetmek için:

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Genel kural olarak, aynı yöntemi CSS veya JavaScript dosyaları için kullandığınız "/ signalr/hub" URL'yi belirtmek için kullanın. Bir tilde kullanmadan bir URL belirtin, IIS Express kullanarak Visual Studio'da test ancak tam IIS dağıttığınızda 404 hatası ile başarısız olur bazı senaryolarda, uygulamanızın doğru şekilde çalışmayacak. Daha fazla bilgi için bkz: **kök düzeyinde kaynaklara başvurular çözme** içinde [ASP.NET Web projeleri için Visual Studio'da Web sunucuları](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) MSDN sitesinden.

Bir web projesi, Visual Studio 2013'te hata ayıklama modunda çalıştırdığınızda ve tarayıcı olarak Internet Explorer kullanıyorsanız proxy dosyasında görebilirsiniz **Çözüm Gezgini** altında **betik belgelerini**gösterildiği gibi Aşağıdaki çizimde.

![Çözüm Gezgini'nde JavaScript oluşturulan proxy dosyası](hubs-api-guide-javascript-client/_static/image1.png)

Dosya içeriğini görüntülemek için çift **hub**. "/ SignalR/hub" URL'sine göz atarak, Visual Studio 2012 veya 2013 ve Internet Explorer kullanmıyorsanız ya da hata ayıklama modunda değilse, dosyanın içeriğini alabilirsiniz. Örneğin, siteniz en çalışıyorsa, `http://localhost:56699`gidin `http://localhost:56699/SignalR/hubs` tarayıcınızda.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur

Dinamik olarak üretilen proxy alternatif olarak, proxy koduna sahip fiziksel bir dosya oluşturun ve bu dosyayı ilişkilendirin. Önbelleğe alma veya davranışı paketleme üzerinde denetim için bunu veya sunucu yöntem çağrıları kodlama IntelliSense edilirken isteyebilirsiniz.

Bir proxy dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Yükleme [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet paketi.
2. Bir komut istemi açın ve *Araçları* SignalR.exe dosyasını içeren klasör. Araçlar klasörünü şu konumdadır:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Aşağıdaki komutu girin:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    Yolu, *.dll* genellikle *bin* proje klasörünüzdeki klasörüne.

    Bu komut adlı bir dosya oluşturur *server.js* aynı klasörde *signalr.exe*.
4. PUT *server.js* dosya projenizdeki uygun bir klasör içinde uygulamanız için uygun şekilde yeniden adlandırın ve "signalr/hub" başvuru yerine, bir başvuru ekleyin.

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Bir bağlantı kurmak nasıl

Bir bağlantı oluşturabilir önce bir bağlantı nesnesi oluşturmak, bir proxy oluşturmak ve sunucudan çağrılabilir yöntemleri için olay işleyicileri kaydetmeniz gerekir. Proxy ve olay işleyicileri ayarlandığında çağırarak bağlantı `start` yöntemi.

Oluşturulan proxy kullanıyorsanız, oluşturulan proxy kod bunu sizin için yapar kendi kodunuzu bağlantı nesnesi oluşturmak yok.

<a id="nogenconnection"></a>

**(Oluşturulan proxy ile) bağlantı kurun**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Bir bağlantı (olmadan oluşturulan proxy)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

Varsayılan örnek kod kullanır "/ signalr" SignalR hizmete bağlanmak için URL. Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).

Varsayılan olarak, geçerli sunucu hub konumdur; farklı bir sunucuya bağlanıyorsanız çağırmadan önce URL'yi belirtin `start` yöntemi, aşağıdaki örnekte gösterildiği gibi:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalde, olay işleyicileri çağırmadan önce kaydetmeniz `start` bağlantı kurmak için yöntem. Bağlantı kurduktan sonra bazı olay işleyicilerini kaydetmek istiyorsanız, bunu yapabilirsiniz ancak, olay handler(s) çağırmadan önce en az biri kaydetmeniz gerekir `start` yöntemi. Bunun bir nedeni olan bir uygulamada birçok hub olabilir, ancak tetiklemek istemezsiniz `OnConnected` yalnızca bunlardan birini kullanmak için kullanacaksanız her Hub olayı. Bağlantı kurulduğunda istemci yöntemi bir Hub'ın proxy varlığını ne tetiklemek için SignalR söyler olduğu `OnConnected` olay. Tüm olay işleyicileri çağırmadan önce kaydetmezseniz `start` yöntemi, görebilirsiniz Hub ancak Hub'ın yöntemleri çağırmak `OnConnected` yöntemi olmaz çağrılabilir ve sunucudan hiçbir istemci yöntemi çağrılır.


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi

Oluşturulan proxy kullandığınızda örneklerinden gördüğünüz `$.connection.hub` bağlantı nesnesine başvuruyor. Bu çağırarak aldığınız, aynı nesnesidir `$.hubConnection()` zaman kullanmadığınız oluşturulan proxy. Oluşturulan proxy kod şu deyimi yürüterek bağlantı sizin için oluşturur:

![Oluşturulan proxy dosyasında bir bağlantı oluşturma](hubs-api-guide-javascript-client/_static/image3.png)

Oluşturulan proxy kullanırken ile herhangi bir şey yapabilirsiniz `$.connection.hub` oluşturulan proxy kullanmadığınızda bir bağlantı nesnesi ile yapabilirsiniz.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Zaman uyumsuz yürütme başlangıç yöntemi

`start` Yöntemini zaman uyumsuz olarak yürütür. Döndürdüğü bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/), yani geri arama işlevleri yöntemleri gibi çağırarak ekleyebilirsiniz `pipe`, `done`, ve `fail`. Bağlantı kurulduktan sonra yürütmek istediğiniz kodu varsa, sunucu yöntemi çağrısı gibi bu kodu bir geri çağırma işlevini put veya bir geri çağırma işlevini çağırın. `.done` Bağlantı kurulduktan sonra herhangi bir kod sonra elinizde geri çağırma yöntemi yürütüldüğünde, `OnConnected` sunucuda olay işleyicisi yöntemi tamamlandıktan yürütülüyor.

Kodun sonraki satırında, sonra olarak "Şimdi bağlı" deyimi önceki örnekten yerleştirirseniz `start` yöntem çağrısı (değil, bir `.done` geri çağırma), `console.log` satır yürütülecek bağlantı kurulmadan önce aşağıda gösterildiği gibi Örnek:

![Bağlantı kurulduktan sonra çalışan bir kod yazmak için yanlış biçimde](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Etki alanları arası bağlantı kurmak nasıl

Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantısı `http://contoso.com/signalr`. Varsa sayfasından `http://contoso.com` bir bağlantı kurar `http://fabrikam.com/signalr`, yani etki alanları arası bağlantı. Güvenlik nedenleriyle, etki alanları arası bağlantılar, varsayılan olarak devre dışıdır.

Etki alanı istekleri arası 1.x tek EnableCrossDomain tarafından kontrol SignalR bayrak. Bu bayrak JSONP ve CORS isteklerini denetlenir. Daha fazla esneklik için tüm CORS desteği SignalR sunucu bileşeninden kaldırıldı (JavaScript istemcilerinin kullanmaya devam CORS normalde tarayıcı onu destekleyen algılanırsa), ve yeni OWIN ara yazılımı sağlandıktan bu senaryoları desteklemek kullanılabilir.

(Etki alanları arası istekleri eski tarayıcılarda desteklemek için) istemcide JSONP gerekirse, açıkça ayarlayarak etkinleştirilmesi gerekir `EnableJSONP` üzerinde `HubConfiguration` nesnesine `true`, aşağıda gösterildiği gibi. CORS az güvenlidir JSONP varsayılan olarak devre dışıdır.

**Microsoft.Owin.Cors projenize ekleme:** bu Kitaplığı'nı yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:

`Install-Package Microsoft.Owin.Cors`

Bu komut 2.1.0 ekleyeceksiniz projenize paketin sürümü.

### <a name="calling-usecors"></a>UseCors çağırma

 Aşağıdaki kod parçacığını SignalR 2'de etki alanları arası bağlantılarını uygulamadan gösterilmiştir. 

**SignalR 2'deki uygulama etki alanları arası istekleri**

Aşağıdaki kod, CORS veya JSONP SignalR 2 projede etkinleştirme gösterilmiştir. Bu kod örneği kullanan `Map` ve `RunSignalR` yerine `MapSignalR`, CORS desteği gerektiren SignalR istekleri için CORS Ara çalışır (yerine belirtilen yoldaki tüm trafik için `MapSignalR`.) Harita, belirli bir URL öneki için yerine tüm uygulama için çalıştırmak için gereken diğer tüm ara yazılımı için de kullanılabilir.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - Ayarlamazsanız `jQuery.support.cors` kodunuzda true.
> 
>     ![JQuery.support.cors true olarak ayarlamanız gerekmez](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR CORS kullanımını işler. Ayarı `jQuery.support.cors` true devre dışı bırakır için JSONP tarayıcının destekleyip CORS varsaymak SignalR neden olduğu.
> - Localhost URL'sini bağlandığınız, sunucuda etki alanları arası bağlantılar etkinleştirmediniz olsa bile uygulama yerel olarak IE 10 ile çalışması için Internet Explorer 10, etki alanları arası bağlantı göz önünde olmaz.
> - Etki alanları arası bağlantılar Internet Explorer 9 ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Etki alanları arası bağlantılar Chrome ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - Varsayılan örnek kod kullanır "/ signalr" SignalR hizmete bağlanmak için URL. Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Bağlantı yapılandırma

Bir bağlantısı kurmadan önce sorgu dizesi parametreleri belirtin veya taşıma yöntemini belirtin.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Sorgu dizesi parametreleri belirtme

İstemci bağlandığında sunucuya veri göndermek istiyorsanız, sorgu dizesi parametreleri bağlantı nesnesine ekleyebilirsiniz. Aşağıdaki örnekler bir sorgu dizesi parametresi istemci kodu ayarlamak nasıl gösterir.

**Bir sorgu dizesi değeri (ile oluşturulan proxy) başlatma yöntemi çağırmadan önce ayarlayın**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Bir sorgu dizesi değeri (olmadan oluşturulan proxy) başlatma yöntemi çağırmadan önce ayarlayın**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

Aşağıdaki örnek, bir sorgu dizesi parametresi sunucu kodu okuma gösterilmektedir.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Taşıma yöntemini belirleme

Bağlama işleminin bir parçası olarak, bir SignalR istemci sunucuyla desteklenen en iyi aktarım belirlemek için sunucu ve istemci tarafından normalde görüşür. Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, taşıma yöntemini çağırdığınızda belirterek bu anlaşma işlemi atlayabilirsiniz `start` yöntemi.

**Belirtir (ile oluşturulan proxy) aktarım yöntemi istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Belirtir (olmadan oluşturulan proxy) aktarım yöntemi istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Alternatif olarak, bunları denemek için SignalR istediğiniz sırayla birden çok taşıma yöntemleri belirtebilirsiniz:

**(İle oluşturulan proxy) bir özel taşıma geri dönüş düzeni belirtir istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**(Olmadan oluşturulan proxy) bir özel taşıma geri dönüş düzeni belirtir istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Taşıma yöntemini belirtmek için aşağıdaki değerleri kullanabilirsiniz:

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Aşağıdaki örnekler, hangi taşıma yöntemi bir bağlantı tarafından kullanılan çıkış bulmak nasıl gösterir.

**Bir bağlantıyla (oluşturulan proxy) tarafından kullanılan taşıma yöntemini görüntüler istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Bir bağlantı (olmadan oluşturulan proxy) tarafından kullanılan taşıma yöntemini görüntüler istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Sunucu kodu aktarım yönteminde denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - bağlam özelliğinden istemcisi hakkında bilgi almak nasıl](hubs-api-guide-server.md#contextproperty). Taşımalar ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarımları ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Bir Hub sınıf için bir proxy alma

Oluşturduğunuz her bağlantı nesnesi bir veya daha fazla Hub sınıfları içeren bir SignalR hizmet bağlantısıyla ilgili bilgileri yalıtır. Bir Hub sınıf ile iletişim kurmak için bir proxy nesnesi (oluşturulan proxy değil kullanıyorsanız), sizin oluşturduğunuz veya sizin yerinize oluşturulduğu kullanın.

İstemcide Hub sınıf adı başlamalıdır sürümü proxy adıdır. SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.

**Sunucudaki hub sınıfı**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Hub için oluşturulan istemci proxy başvuru al**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**İstemci proxy (olmadan oluşturulan proxy) Hub sınıfı için oluşturma**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Hub sınıfıyla tasarlamanız varsa bir `HubName` özniteliği, durum değiştirmeden tam ad kullanın.

**HubName özniteliğiyle sunucudaki hub sınıfı**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Hub için oluşturulan istemci proxy başvuru al**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**İstemci proxy (olmadan oluşturulan proxy) Hub sınıfı için oluşturma**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama

Sunucu Hub'ından çağırabilirsiniz bir yöntemi tanımlamak için bir olay işleyicisi Hub proxy için kullanarak eklemek `client` oluşturulan proxy veya arama özelliğini `on` oluşturulan proxy kullanmıyorsanız yöntemi. Parametreleri karmaşık nesneler olabilir.

Arama yapmadan önce olay işleyicisi ekleme `start` bağlantı kurmak için yöntem. (Çağrıldıktan sonra olay işleyicileri eklemek istiyorsanız `start` yöntemi, bkz. Not [bağlantı kurmak nasıl](#establishconnection) daha önce bu belge ve oluşturulan proxy kullanmadan bir yöntemi tanımlamak için sözdizimini kullanın.)

Yöntemi ad eşleştirme büyük küçük harfe duyarlıdır. Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütecek `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, veya `addcontosochatmessagetopage` istemci üzerinde.

**(İle oluşturulan proxy) istemcide yöntemi tanımlayın**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**İstemci (ile oluşturulan proxy) yöntemi tanımlamak için alternatif bir yolu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**İstemci (oluşturulan proxy olmadan veya start yöntemi çağrıldıktan sonra eklerken) yöntemi tanımlayın**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**İstemci yöntemini çağıran sunucu kodu**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Aşağıdaki örnekler yöntemi parametre olarak karmaşık bir nesne içerir.

**Karmaşık bir nesne (ile oluşturulan proxy) alır istemcide yöntemi tanımlayın**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Karmaşık bir nesne (olmadan oluşturulan proxy) alır istemcide yöntemi tanımlayın**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Karmaşık nesne tanımlar sunucu kodu**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Karmaşık bir nesne kullanarak istemci yöntemini çağıran sunucu kodu**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>İstemciden sunucu yöntemleri çağırmak nasıl

İstemciden bir sunucu yöntemi çağırmak için `server` oluşturulan proxy özelliği veya `invoke` yöntemi oluşturulan proxy kullanmıyorsanız Hub proxy. Dönüş değeri veya parametreleri karmaşık nesneler olabilir.

Yöntem adı ortası büyük harf sürümünde hub'da geçirin. SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.

Aşağıdaki örnekler, dönüş değeri sahip olmayan bir sunucu yöntemin nasıl çağrılacağını ve bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.

**Sunucu yöntemiyle HubMethodName özniteliği yok**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Karmaşık nesne tanımlar sunucu kodu içindeki bir parametre geçirildi**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**(İle oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**(Olmadan oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Hub yöntemi ile donatılmış varsa bir `HubMethodName` özniteliği, durum değiştirmeden bu adı kullanabilirsiniz.

**Sunucu yöntemini** HubMethodName özniteliğine sahip

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**(İle oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**(Olmadan oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Önceki örneklerde bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir. Aşağıdaki örnekler bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.

**Dönüş değeri olan bir yöntem için sunucu kodu**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**İçin kullanılan hisse senedi sınıfı** dönüş değeri

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**(İle oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**(Olmadan oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Bağlantı ömür olayları işleme

SignalR aşağıdaki bağlantıyı işleyebilir ömür olayları sağlar:

- `starting`: Herhangi bir veri bağlantısı üzerinden gönderilmeden önce oluşturuldu.
- `received`: Herhangi bir veri bağlantısı alındığında oluşturuldu. Alınan veriler sağlar.
- `connectionSlow`: İstemci yavaş veya sık bırakma bir bağlantı algıladığında oluşturulur.
- `reconnecting`: Temel aktarımı yeniden bağlanmayı başladığında oluşturulur.
- `reconnected`: Temel aktarımı bağlandı tetiklenir.
- `stateChanged`: Bağlantı durumu değiştiğinde oluşturuldu. Eski durum ve yeni bir durum (bağlanma, bağlı, Reconnecting veya bağlantı kesildi) sağlar.
- `disconnected`: Bağlantı kesildi tetiklenir.

Örneğin, fark gecikmeye neden olabilecek bağlantı sorunları olduğunda uyarı iletileri görüntülemek istiyorsanız, işleme `connectionSlow` olay.

**(İle oluşturulan proxy) connectionSlow olayını işle**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**(Olmadan oluşturulan proxy) connectionSlow olayını işle**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Daha fazla bilgi için bkz: [anlama ve SignalR bağlantısı ömrü olaylarını işleme](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Hataların nasıl işleneceğini

SignalR JavaScript istemci sağlayan bir `error` olay işleyicisi ekleyebilirsiniz. Başarısız yöntemi, bir sunucu yöntemi çağrısından kaynaklanan hatalar için bir işleyici eklemek için de kullanabilirsiniz.

Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, SignalR sonra bir hata döndürür özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir. Örneğin, bir çağrı varsa `newContosoChatMessage` başarısız, hata nesnesindeki hata iletisini içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" üretim istemciler için ayrıntılı hata iletileri için ayrıntılı hata iletileri etkinleştirmek isteyip istemediğinizi ancak güvenlik nedeniyle önerilmez gönderme sorun giderme amacıyla, aşağıdaki kodu sunucuda kullanın.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

Aşağıdaki örnek, hata olayı için bir işleyici ekleyin gösterilmektedir.

**Bir hata işleyicisi (ile oluşturulan proxy) ekleyin**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Bir hata işleyicisi (olmadan oluşturulan proxy) ekleyin**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

Aşağıdaki örnek, bir yöntem çağrısının bir hatadan nasıl ele alınacağını gösterir.

**Bir yöntemi çağırma (ile oluşturulan proxy) bir hata işleme**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Bir yöntemi çağırma (olmadan oluşturulan proxy) bir hata işleme**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Bir yöntem çağrısının başarısız olursa, `error` olayı de oluşturulur, bu nedenle, kodunuzda `error` yöntemi işleyici ve `.fail` yöntemi geri çağırma yürütün.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>İstemci-tarafı günlük kaydını etkinleştirme

İstemci tarafı bir bağlantıda günlüğe kaydetmeyi etkinleştirmek üzere ayarlamak `logging` özelliği çağırmadan önce bağlantıyı nesne `start` bağlantı kurmak için yöntem.

**(İle oluşturulan proxy) günlük kaydını etkinleştir**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**(Olmadan oluşturulan proxy) günlük kaydını etkinleştir**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Günlükleri görmek için tarayıcınızın geliştirici araçları açın ve konsol sekmesine gidin. Adım adım yönergeler ve ekran bunu nasıl yapacağınızı görüntülerini gösteren bir öğretici için bkz [Server yayını ASP.NET Signalr - Enable Logging ile](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).
