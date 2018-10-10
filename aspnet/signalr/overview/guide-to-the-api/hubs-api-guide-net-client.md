---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET SignalR Hubs API Kılavuzu - .NET istemcisi (C#) | Microsoft Docs
author: pfletcher
description: Bu belge, SignalR sürüm 2 (WinRT) Windows Store, WPF, Silverlight ve simgeler gibi .NET istemcileri için hub'ları API kullanarak bir giriş sağlar...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 2d7dd1480694eacffc0cfa60ac0179b16348488d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913001"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>ASP.NET SignalR Hubs API Kılavuzu - .NET istemcisi (C#)
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Bu belge, SignalR sürüm 2 (WinRT) Windows Store, WPF, Silverlight ve konsol uygulamaları gibi .NET istemcileri için hub'ları API kullanmaya giriş bilgileri sağlar.
>
> SignalR hub'ları API, bir sunucuya bağlanan istemcilerin ve istemcilerin sunucuya uzaktan yordam çağrısı (RPC) oluşturmanıza olanak sağlar. Sunucu kodu, istemciler tarafından çağrılabilen yöntemleri tanımlamak ve bir istemcide çalışmasına yöntemler çağırır. İstemci kodu sunucudan çağıran yöntemleri tanımlamak ve sunucu üzerinde çalışan yöntemleri çağırın. SignalR tüm istemci-sunucu tesisat sizin için üstlenir.
>
> SignalR kalıcı bağlantı adlı bir alt düzey API'si de sunar. SignalR hub'ları ve kalıcı bağlantılar için giriş veya tam bir SignalR uygulamanın nasıl oluşturulacağını gösteren bir öğretici için bkz: [SignalR çalışmaya başlama -](../getting-started/index.md).
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

- [İstemci Kurulumu](#clientsetup)
- [Nasıl bir bağlantı kurmak için](#establishconnection)

    - [Silverlight istemcileri etki alanları arası bağlantılar](#slcrossdomain)
- [Bağlantı yapılandırma](#configureconnection)

    - [WPF istemcileri en fazla eş zamanlı bağlantı sayısını ayarlama](#maxconnections)
    - [Sorgu dizesi parametrelerini belirtme](#querystring)
    - [Taşıma yöntemini belirleme](#transport)
    - [HTTP üst bilgilerini belirtme](#httpheaders)
    - [İstemci sertifikalarını belirtmek nasıl](#clientcertificate)
- [Hub proxy oluşturma](#proxy)
- [Sunucu çağıran istemciye yöntemleri tanımlama](#callclient)

    - [Parametresiz yöntemleri](#clientmethodswithoutparms)
    - [Parametre türleri belirtme parametrelere sahip yöntemleri](#clientmethodswithparmtypes)
    - [Parametreler için dinamik nesneleri belirterek parametrelere sahip yöntemleri](#clientmethodswithdynamparms)
    - [Bir işleyici kaldırma](#removehandler)
- [İstemciden sunucu yöntemleri çağırma](#callserver)
- [Bağlantı ömrü olaylarını işlemek nasıl](#connectionlifetime)
- [Hatalarını işleme](#handleerrors)
- [İstemci tarafı günlük kaydını etkinleştirme](#logging)
- [WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod](#wpfsl)

Örnek .NET istemci projeleri için aşağıdaki kaynaklara bakın:

- [Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) GitHub.com (WinRT, Silverlight, konsol uygulaması örnekleri) üzerinde.
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) üzerinde GitHub.com (WPF örnek).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) üzerinde GitHub.com (konsol uygulaması örnek).

Sunucu veya JavaScript istemcilerinin program hakkında daha fazla belge için aşağıdaki kaynaklara bakın:

- [SignalR hub API Kılavuzu - sunucu](hubs-api-guide-server.md)
- [SignalR hub API Kılavuzu - JavaScript istemcisi](hubs-api-guide-javascript-client.md)

API başvuru konularına bağlar API .NET 4.5 sürümü var. .NET 4 kullanıyorsanız, bkz. [API konuları .NET 4 sürümünü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>İstemci Kurulumu

Yükleme [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet paketini (değil [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) paketi). Bu paket, .NET 4 ve .NET 4.5 için WinRT, Silverlight, WPF, konsol uygulaması ve Windows Phone istemcileri destekler.

İstemcide sahip SignalR sürümü sunucuda yüklü sürümden farklı ise, SignalR genellikle farkı uyum sağlamak kullanabilirsiniz. Örneğin, SignalR sürüm 2 çalıştıran bir sunucuya 1.1.x yüklü olan istemciler ve bunun yanı sıra sürüm 2'in yüklü olan istemcileri destekler. Sunucu sürümünde ve istemci sürümü arasındaki fark çok fazla olabilir veya istemcinin sunucudan daha yeniyse, SignalR oluşturur bir `InvalidOperationException` istemci bağlantısı kurmaya çalıştığında bir özel durum. Hata iletisi "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Nasıl bir bağlantı kurmak için

Bir bağlantı kurabilmesi için önce oluşturmak sahip bir `HubConnection` nesnesi ve bir ara sunucu oluşturun. Bağlantı kurmak için çağrı `Start` metodunda `HubConnection` nesne.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> JavaScript istemciler için çağırmadan önce en az bir olay işleyicisi kaydetmek zorunda `Start` bağlantı kurmak için yöntemi. Bu, .NET istemcileri için gerekli değildir. JavaScript istemciler için oluşturulan proxy kodu otomatik olarak mevcut tüm hub'ları proxy'lerini sunucuda oluşturur ve bir işleyici kaydetmektir hangi hub'ları nasıl belirttiğiniz istemcinizi kullanmayı düşünüyor. Ancak SignalR kabul eder, proxy için oluşturduğunuz hub'ı kullanacak şekilde için .NET istemci Hub proxy el ile oluşturduğunuz.


Varsayılan örnek kodu kullanır "/ signalr" SignalR hizmetinize bağlanmak için URL. Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - /signalr URL](hubs-api-guide-server.md#signalrurl).

`Start` Yöntemi zaman uyumsuz olarak yürütür. Bağlantı kurulduktan sonra sonraki kod satırlarını kadar yürütmek yoksa emin olmak için `await` ASP.NET 4.5 zaman uyumsuz yönteminde veya `.Wait()` zaman uyumlu bir yöntem içinde. Kullanmayın `.Wait()` bir WinRT istemcisinde.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Silverlight istemcileri etki alanları arası bağlantılar

Silverlight istemcilerden etki alanları arası bağlantıları etkinleştirme hakkında daha fazla bilgi için bkz: [bir hizmet üzerinden etki alanı sınırlarında kullanılabilir hale getirme](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Bağlantı yapılandırma

Bir bağlantı kurmadan önce aşağıdaki seçeneklerden birini belirtebilirsiniz:

- Eş zamanlı bağlantı sayısı sınırı.
- Dize parametreleri sorgulayın.
- Taşıma yöntemi.
- HTTP üstbilgileri.
- İstemci sertifikaları.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>WPF istemcileri en fazla eş zamanlı bağlantı sayısını ayarlama

WPF istemcileri 2 varsayılan değerini eşzamanlı bağlantıların maksimum sayısını artırmanız gerekebilir. Önerilen değer 10'dur.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Daha fazla bilgi için [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Sorgu dizesi parametrelerini belirtme

İstemci bağlandığında sunucuya veri göndermek istiyorsanız, bağlantı nesnesi için sorgu dizesi parametreleri ekleyebilirsiniz. Aşağıdaki örnek bir sorgu dizesi parametresi istemci kodu ayarlama işlemini gösterir.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Aşağıdaki örnekte, sunucu kodu bir sorgu dizesi parametresi okunacak gösterilmektedir.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Taşıma yöntemini belirleme

Bağlama işleminin bir parçası, SignalR istemci ile sunucu hem sunucu hem de istemci tarafından desteklenen en iyi aktarım belirlemek için normalde görüşür. Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, bu anlaşma işlemi devre dışı bırakabilir. Aktarım yöntemi belirtmek için başlangıç yöntemine bir transport nesnesi içinde geçirin. Aşağıdaki örnek, aktarım yöntemi istemci kodu belirtmek gösterilmektedir.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) ad alanı taşıma belirtmek için kullanabileceğiniz aşağıdaki sınıfları içerir.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (kullanılabilir. yalnızca, .NET 4.5 hem sunucu hem de istemci kullandığınızda)
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (istemci ve sunucu tarafından desteklenen en iyi aktarım otomatik olarak seçer. Varsayılan aktarım budur. Bu konuda geçirme `Start` yöntemi her şeyi geçmiyor aynı etkiye sahiptir.)

Yalnızca tarayıcı tarafından kullanıldığından ForeverFrame taşıma bu listede dahil edilmez.

Sunucu kodu aktarım yöntemi denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - bağlam özelliği istemci hakkında bilgi almak nasıl](hubs-api-guide-server.md#contextproperty). Aktarım ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarım ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>HTTP üst bilgilerini belirtme

HTTP üst bilgilerini ayarlayacak şekilde kullanmak `Headers` bağlantı nesnesindeki özelliği. Aşağıdaki örnek, bir HTTP üstbilgisi Ekle gösterilmektedir.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>İstemci sertifikalarını belirtmek nasıl

İstemci sertifikaları eklemek için `AddClientCertificate` bağlantı nesnesi üzerinde yöntemi.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Hub proxy oluşturma

Sunucudan bir Hub çağıran istemci üzerinde yöntemleri tanımlamak için ve sunucudaki Hub yöntemlerini çağırma, Hub için bir proxy çağırarak oluşturma `CreateHubProxy` bağlantı nesnesindeki. Dize, için geçirdiğiniz `CreateHubProxy` Hub sınıfınıza adını veya tarafından belirtilen adı `HubName` sunucuda kullanılan bir özniteliği. Ad eşleştirme büyük küçük harfe duyarlıdır.

**Sunucudaki hub sınıfı**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Hub sınıfı için istemci proxy oluşturma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Hub sınıfınıza tasarlamanız, bir `HubName` özniteliği, bu adı kullanın.

**Sunucudaki hub sınıfı**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Hub sınıfı için istemci proxy oluşturma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Eğer `HubConnection.CreateHubProxy` birden çok kez ile aynı `hubName`, aynı önbelleğe alma `IHubProxy` nesne.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Sunucu çağıran istemciye yöntemleri tanımlama

Proxy sunucu çağırabilen bir yöntemi tanımlamak için kullanmak `On` bir olay işleyicisi kaydetmek için yöntemi.

Yöntem adı ile eşleşen büyük küçük harfe duyarlıdır. Örneğin, `Clients.All.UpdateStockPrice` sunucuda yürütülür `updateStockPrice`, `updatestockprice`, veya `UpdateStockPrice` istemci üzerinde.

Farklı istemci platformları UI'yi güncellemeye yöntemine kodu yazarsınız nasıl farklı gereksinimlere sahip. Gösterilen WinRT (Windows Store .NET) istemciler için verilebilir. İçinde WPF, Silverlight ve konsol uygulaması örnekleri verilmiştir [ayrı bir bölümde bu konunun ilerleyen bölümlerinde](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Parametresiz yöntemleri

İşleme yöntemi parametrelerine sahip değildir, genel olmayan aşırı yüklemesini kullanın. `On` yöntemi:

**Sunucu kodu parametresiz istemci yöntemi çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**WinRT istemci kodu yöntemi için çağrılır parametresiz sunucusundan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Parametre türleri belirtme parametrelere sahip yöntemleri

İşleme yöntemi parametrelere sahipse, parametre türleri genel türlerini belirtmek `On` yöntemi. Genel aşırı yükleme `On` yöntemi en fazla 8 parametreleri (Windows Phone 7 4) belirtmenize olanak verir. Aşağıdaki örnekte, bir parametre için gönderilen `UpdateStockPrice` yöntemi.

**Sunucu kodu istemci yöntemi parametresi ile çağırılıyor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Parametresi için kullanılan stok sınıfı**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**WinRT istemci kodu için bir yöntem olarak adlandırılan bir parametresi olan bir sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Parametreler için dinamik nesneleri belirterek parametrelere sahip yöntemleri

Alternatif genel tür parametrelerini belirtme olarak `On` yöntemi parametrelerini dinamik nesneler olarak belirtebilirsiniz:

**Sunucu kodu istemci yöntemi parametresi ile çağırılıyor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Parametresi için kullanılan stok sınıfı**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Bir yöntem için WinRT istemci kodu adlı bir parametresiyle dinamik Nesne parametresi için kullanarak sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Bir işleyici kaldırma

Bir işleyici kaldırmak için arama, `Dispose` yöntemi.

**Sunucudan adlı bir yöntem için istemci kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**İstemci kodu, işleyici kaldırmak için**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>İstemciden sunucu yöntemleri çağırma

Sunucu üzerinde bir yöntemi çağırmak için `Invoke` Hub proxy yöntemi.

Sunucu yönteminin dönüş değeri varsa, genel olmayan aşırı yüklemesini kullanın `Invoke` yöntemi.

**Dönüş değeri içeren bir yöntem için sunucu kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**İstemci kodu, dönüş değeri olmayan bir yöntem çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Sunucu yönteminin dönüş değeri varsa, dönüş türü genel türü olarak belirtin. `Invoke` yöntemi.

**Bir değer döndürmez ve karmaşık tür parametre alan bir yöntem için sunucu kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Stok sınıfı parametresi için kullanılan ve dönüş değeri**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**İstemci kodu bir değer döndürmez ve bir ASP.NET 4.5 zaman uyumsuz yöntemde bir karmaşık tür parametre alan bir yöntem çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**İstemci kodu bir değer döndürmez ve zaman uyumlu bir yöntemde bir karmaşık tür parametre alan bir yöntem çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke` Yöntemi zaman uyumsuz olarak yürütür ve döndürür bir `Task` nesne. Belirtmezseniz `await` veya `.Wait()`, sonraki kod satırına, çağırma yöntemi yürütmeyi bitirmeden önce yürütülür.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Bağlantı ömrü olaylarını işlemek nasıl

SignalR işleyebilirsiniz ömür olayları aşağıdaki bağlantı sağlar:

- `Received`: Herhangi bir veri bağlantısı alındığında oluşturulur. Alınan veriler sağlar.
- `ConnectionSlow`: Bir istemci yavaş veya sık bırakma bağlantı algıladığında oluşturulur.
- `Reconnecting`: Temel alınan aktarımda yeniden bağlanmayı başladığında oluşturulur.
- `Reconnected`: Temel alınan aktarımda bağlandığınızda oluşturulur.
- `StateChanged`: Bağlantı durumu değiştiğinde oluşturulur. Eski durum ve yeni durum sağlar. Durum değerleri bağlantısı hakkında bilgi için bkz [ConnectionState numaralandırma](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Bağlantı kesildiğinde oluşturulur.

Örneğin, önemli değildir ancak aralıklı bağlantı sorunlarına neden bir hata için uyarı iletileri görüntülemek istiyorsanız, gibi yavaşlık ya da sık sık bağlantısı, bırakarak işlemek `ConnectionSlow` olay.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Daha fazla bilgi için [anlama ve signalr'da bağlantı ömrü olaylarını işleme](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Hatalarını işleme

Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, bir hatanın ardından SignalR döndüren özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir. Örneğin, bir çağrı `newContosoChatMessage` başarısız, hata iletisi hata nesnesi içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" güvenlik nedeniyle, ayrıntılı hata iletileri için etkinleştirmek istiyorsanız ancak üretimde istemciler için ayrıntılı hata iletileri önerilmez gönderme sorun giderme amacıyla sunucu üzerinde aşağıdaki kodu kullanın.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

SignalR oluşturan hataları işlemek için bir işleyici ekleyebilirsiniz `Error` bağlantı nesnesindeki olay.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Yöntem çağrıları hatalarını işlemek için bir try-catch bloğu içinde kod alın.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>İstemci tarafı günlük kaydını etkinleştirme

İstemci tarafı günlük kaydını etkinleştirmek için ayarlanmış `TraceLevel` ve `TraceWriter` bağlantı nesnesindeki özellikleri.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod

Sunucu çağırabilirsiniz istemci yöntemleri tanımlamak için daha önce gösterilen kod örnekleri, WinRT istemcileri için geçerlidir. Aşağıdaki örnekler, WPF, Silverlight ve konsol uygulaması istemciler için eşdeğer kod gösterir.

### <a name="methods-without-parameters"></a>Parametresiz yöntemleri

**Parametre olmadan sunucu WPF istemci kodu yöntemi için çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Parametresiz sunucusundan Silverlight istemci kodu yöntemi için çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Parametre olmadan sunucu konsol uygulaması istemci kodu yöntemi için çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Parametre türleri belirtme parametrelere sahip yöntemleri

**WPF istemci kodu bir yöntem için parametre sunucusuyla çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Sunucu parametresi olan bir yöntem için Silverlight istemci kodu çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Konsol uygulaması istemci kodu bir yöntem için parametre sunucusuyla çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Parametreler için dinamik nesneleri belirterek parametrelere sahip yöntemleri

**WPF sunucusundan dinamik Nesne parametresi için kullanarak, bir parametre olarak adlandırılan bir yöntem için istemci kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Dinamik Nesne parametresi için kullanarak, bir parametre ile sunucusundan adlı bir yöntem için Silverlight istemci kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Konsol uygulaması istemci kodu için bir yöntem parametresi için bir dinamik Nesne kullanarak, bir parametre sunucusuyla çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
