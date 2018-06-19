---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET SignalR hub'ları API Kılavuzu - .NET istemci (C#) | Microsoft Docs
author: pfletcher
description: Bu belge SignalR sürüm 2'in Windows Mağazası (WinRT), WPF, Silverlight ve simgeler gibi .NET istemcileri için hub API'sini kullanmaya tanıtılmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: c52a02291e18b1dd8a9d95b33fe466d17aae835f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043944"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>ASP.NET SignalR hub'ları API Kılavuzu - .NET istemci (C#)
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)

> Bu belge SignalR sürüm 2'in Windows Mağazası (WinRT), WPF, Silverlight ve konsol uygulamaları gibi .NET istemcileri için hub API kullanarak giriş bilgileri sağlar.
> 
> SignalR hub'ları API bir sunucuya bağlanan istemciler ve sunucu istemcilerine uzaktan yordam çağrılarını (RPC) yapmanızı sağlar. Sunucu kodu, istemciler tarafından çağrılabilir yöntemlerini tanımlama ve istemci üzerinde çalışan yöntemlerini çağırın. İstemci kodu sunucudan çağrılabilir yöntemlerini tanımlama ve sunucu üzerinde çalışan yöntemlerini çağırın. SignalR, istemci-sunucu tesisat tümünün ilgilenir.
> 
> SignalR kalıcı bağlantılar olarak adlandırılan bir alt düzey API de sunar. Giriş SignalR, hub'lar ve kalıcı bağlantılar için ya da tam bir SignalR uygulamasının nasıl oluşturulacağını gösteren bir öğretici için bkz: [Başlarken SignalR -](../getting-started/index.md).
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

- [İstemci Kurulumu](#clientsetup)
- [Bir bağlantı kurmak nasıl](#establishconnection)

    - [Silverlight istemcilerden etki alanları arası bağlantıları](#slcrossdomain)
- [Bağlantı yapılandırma](#configureconnection)

    - [WPF istemcileri en fazla eşzamanlı bağlantı sayısını ayarlama](#maxconnections)
    - [Sorgu dizesi parametreleri belirtme](#querystring)
    - [Taşıma yöntemini belirleme](#transport)
    - [HTTP üst bilgilerini belirtme](#httpheaders)
    - [İstemci sertifikalarını belirtme](#clientcertificate)
- [Hub proxy oluşturma](#proxy)
- [Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama](#callclient)

    - [Parametresiz yöntemleri](#clientmethodswithoutparms)
    - [Parametre türleri belirtme parametrelerle yöntemleri](#clientmethodswithparmtypes)
    - [Dinamik nesneler parametre belirterek parametrelerle yöntemleri](#clientmethodswithdynamparms)
    - [Bir işleyici kaldırma](#removehandler)
- [İstemciden sunucu yöntemleri çağırmak nasıl](#callserver)
- [Bağlantı ömür olayları işleme](#connectionlifetime)
- [Hataların nasıl işleneceğini](#handleerrors)
- [İstemci-tarafı günlük kaydını etkinleştirme](#logging)
- [WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod](#wpfsl)

Örnek .NET istemci projeleri için aşağıdaki kaynaklara bakın:

- [Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) Github.com'u (WinRT, Silverlight, konsol uygulama örnekler) üzerinde.
- [DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) Github.com'u (WPF örnek) üzerinde.
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).

Sunucu veya JavaScript istemcilerinin program konusunda daha fazla belgeler için aşağıdaki kaynaklara bakın:

- [SignalR hub'ları API Kılavuzu - sunucu](hubs-api-guide-server.md)
- [SignalR hub'ları API Kılavuzu - JavaScript istemci](hubs-api-guide-javascript-client.md)

API başvuru konuları API'si .NET 4.5 sürümüne bağlantılardır. .NET 4 kullanıyorsanız, bkz: [API konuları .NET 4 sürümü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>İstemci Kurulumu

Yükleme [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet paketi (değil [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) paketi). Bu paket, .NET 4 ve .NET 4.5 için WinRT, Silverlight, WPF, konsol uygulaması veya Windows Phone istemcileri destekler.

İstemcide sahip SignalR sürümü sunucuda yüklü sürümü farklıdır, SignalR genellikle fark uyum mümkün olur. Örneğin, SignalR sürüm 2 çalıştıran bir sunucuda yüklü 1.1.x sahip istemciler ve bunun yanı sıra sürüm 2'in yüklü olan istemcileri destekler. Sunucusundaki sürümü ve istemcinin sürümü arasındaki farkı çok fazla olabilir veya istemci sunucunun yeni olan, SignalR döndürürse bir `InvalidOperationException` istemci bağlantısı kurmaya çalıştığında özel durum. Hata iletisi "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Bir bağlantı kurmak nasıl

Bir bağlantı kurmadan önce oluşturmak zorunda bir `HubConnection` nesne ve bir proxy oluşturun. Bağlantı kurmak için çağrı `Start` yöntemi `HubConnection` nesnesi.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> JavaScript istemciler için en az bir olay işleyicisi çağırmadan önce kaydetmek zorunda `Start` bağlantı kurmak için yöntem. Bu .NET istemcileri için gerekli değildir. JavaScript istemciler için oluşturulan proxy kodu otomatik olarak mevcut tüm hub'ları için proxy sunucusu üzerinde oluşturur ve bir işleyici kaydetme olduğundan hangi hub belirtmek nasıl kullanmak istemci amaçlamaktadır. Ancak, bir proxy oluşturmak hub'ı kullanacak SignalR varsayar böylece için bir .NET istemci Hub proxy'leri el ile oluşturduğunuz.


Varsayılan örnek kod kullanır "/ signalr" SignalR hizmete bağlanmak için URL. Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).

`Start` Yöntemini zaman uyumsuz olarak yürütür. Bağlantı kurulduktan sonra kod sonraki satırların kadar yürütme yok emin olmak için `await` ASP.NET 4.5 zaman uyumsuz bir yöntem olarak veya `.Wait()` zaman uyumlu bir yöntem. Kullanmayan `.Wait()` WinRT istemcisinde.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Silverlight istemcilerden etki alanları arası bağlantıları

Silverlight istemcilerden etki alanları arası bağlantıları etkinleştirme hakkında daha fazla bilgi için bkz: [bir hizmet kullanılabilir etki alanı sınırlar boyunca yapma](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Bağlantı yapılandırma

Bir bağlantı kurmadan önce aşağıdaki seçeneklerden birini belirtebilirsiniz:

- Eşzamanlı bağlantı sayısı sınırı.
- Sorgu dizesi parametreleri.
- Taşıma yöntemi.
- HTTP üstbilgileri.
- İstemci sertifikaları.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>WPF istemcileri en fazla eşzamanlı bağlantı sayısını ayarlama

WPF istemcileri en fazla 2'in varsayılan değerini eşzamanlı bağlantı sayısını artırmak olabilir. Önerilen değer 10'dur.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Daha fazla bilgi için bkz: [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Sorgu dizesi parametreleri belirtme

İstemci bağlandığında sunucuya veri göndermek istiyorsanız, sorgu dizesi parametreleri bağlantı nesnesine ekleyebilirsiniz. Aşağıdaki örnek, bir sorgu dizesi parametresi istemci kodu ayarlamak gösterilmiştir.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

Aşağıdaki örnek, bir sorgu dizesi parametresi sunucu kodu okuma gösterilmektedir.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Taşıma yöntemini belirleme

Bağlama işleminin bir parçası olarak, bir SignalR istemci sunucuyla desteklenen en iyi aktarım belirlemek için sunucu ve istemci tarafından normalde görüşür. Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, bu anlaşma işlemi devre dışı bırakabilir. Taşıma yöntemini belirtmek için bir taşıyıcı nesnesi başlangıç yönteme geçirin. Aşağıdaki örnek, aktarım yöntemi istemci kodda belirtme gösterilmektedir.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) ad alanı taşımayı belirtmek için kullanabileceğiniz aşağıdaki sınıflar içerir.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (kullanılabilir. yalnızca sunucu ve istemci .NET 4.5 kullandığınızda)
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (istemci ve sunucu tarafından desteklenen en iyi aktarım otomatik olarak seçer. Varsayılan taşımayı burasıdır. Bu konuda geçirme `Start` yöntemi her şeyi geçirme değil aynı etkiye sahiptir.)

Yalnızca tarayıcılar tarafından kullanıldığından ForeverFrame aktarım bu listeye dahil edilmez.

Sunucu kodu aktarım yönteminde denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - bağlam özelliğinden istemcisi hakkında bilgi almak nasıl](hubs-api-guide-server.md#contextproperty). Taşımalar ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarımları ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>HTTP üst bilgilerini belirtme

HTTP üstbilgileri ayarlamak için kullanın `Headers` özelliği değerinin bağlantı nesnesindeki. Aşağıdaki örnek, bir HTTP üstbilgisi Ekle gösterilmektedir.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>İstemci sertifikalarını belirtme

İstemci sertifikalarını eklemek için kullanın `AddClientCertificate` bağlantı nesnesi üzerinde yöntemi.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Hub proxy oluşturma

Bir Hub sunucudan çağırabilirsiniz istemci üzerinde yöntemleri tanımlamak için ve sunucudaki Hub yöntemlerini çağırma, Hub için bir proxy çağırarak oluşturun `CreateHubProxy` değerinin bağlantı nesnesindeki. Dize için ilettiğiniz `CreateHubProxy` Hub sınıfın adını ya da tarafından belirtilen adını `HubName` bir sunucu üzerinde kullanıldıysa özniteliği. Ad eşleştirme büyük küçük harfe duyarlıdır.

**Sunucudaki hub sınıfı**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**İstemci proxy Hub sınıfı için oluşturma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Hub sınıfıyla tasarlamanız varsa bir `HubName` özniteliği, bu adı kullanabilirsiniz.

**Sunucudaki hub sınıfı**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**İstemci proxy Hub sınıfı için oluşturma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

Çağırırsanız `HubConnection.CreateHubProxy` verilerle birden çok kez aynı `hubName`, aynı önbelleğe alma `IHubProxy` nesnesi.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama

Sunucu çağırabilirsiniz bir yöntemi tanımlamak için proxy's kullanın `On` olay işleyicisi kaydetmek için yöntem.

Yöntemi ad eşleştirme büyük küçük harfe duyarlıdır. Örneğin, `Clients.All.UpdateStockPrice` sunucuda yürütecek `updateStockPrice`, `updatestockprice`, veya `UpdateStockPrice` istemci üzerinde.

Farklı istemci platformları UI güncelleştirmek için nasıl yöntemi kodu yazdığınız farklı gereksinimleri vardır. Gösterilen örnek WinRT (Windows mağazası .NET) istemciler için verilebilir. WPF, Silverlight ve konsol uygulaması örnekleri verilmiştir [bu konunun ilerleyen bölümlerinde ayrı bir bölüm](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Parametresiz yöntemleri

İşleme yöntemi parametrelerini yoksa genel olmayan kullanın `On` yöntemi:

**Sunucu kodu parametresiz istemci yöntemi çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Yöntem için WinRT istemci kodu adlı parametresiz sunucusundan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Parametre türleri belirtme parametrelerle yöntemleri

İşleme yöntemi parametrelere sahipse, parametre türleri genel türleri belirtin `On` yöntemi. Genel aşırı `On` yöntemi, 8 adete kadar parametreleri (Windows Phone 7 4) belirtmenize olanak verir. Aşağıdaki örnekte, bir parametre gönderilen `UpdateStockPrice` yöntemi.

**Sunucu kodu parametresi olan istemci yöntemi çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**Parametresi için kullanılan hisse senedi sınıfı**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Bir yöntem için WinRT istemci kodu adlı bir parametre olan sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Dinamik nesneler parametre belirterek parametrelerle yöntemleri

Alternatif genel tür parametreleri belirtme olarak `On` yöntemi, dinamik nesneler olarak parametreleri belirtebilirsiniz:

**Sunucu kodu parametresi olan istemci yöntemi çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**Parametresi için kullanılan hisse senedi sınıfı**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Bir yöntem için WinRT istemci kodu adlı bir dinamik Nesne parametresi için kullanarak, bir parametre olan sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Bir işleyici kaldırma

Bir işleyici kaldırmak için arama kendi `Dispose` yöntemi.

**Sunucudan adlı bir yöntem için istemci kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**İşleyici kaldırmak için istemci kodu**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>İstemciden sunucu yöntemleri çağırmak nasıl

Sunucuda bir yöntemi çağırmak için `Invoke` Hub proxy yöntemi.

Sunucu yönteminin dönüş değeri yoksa, genel olmayan kullanın `Invoke` yöntemi.

**Bir dönüş değerine sahip bir yöntem için sunucu kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**İstemci kodu bir dönüş değerine sahip bir yöntemi çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Sunucu yönteminin dönüş değeri varsa, dönüş türü genel türü belirtin `Invoke` yöntemi.

**Dönüş değeri olan ve bir karmaşık tür parametresi alan bir yöntem için sunucu kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**Parametre ve dönüş değeri için kullanılan hisse senedi sınıfı**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**İstemci kodu dönüş değeri yok ve bir ASP.NET 4.5 async yöntemi bir karmaşık tür parametresi alan bir yöntemi çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**İstemci kodu dönüş değeri yok ve bir zaman uyumlu yöntemi bir karmaşık tür parametresi alan bir yöntemi çağırma**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke` Yöntemi zaman uyumsuz olarak yürütür ve döndürür bir `Task` nesnesi. Belirtmediyseniz `await` veya `.Wait()`, kodun sonraki satırında, çağırmayı yöntemi yürütülmesi tamamlandı önce yürütülür.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Bağlantı ömür olayları işleme

SignalR aşağıdaki bağlantıyı işleyebilir ömür olayları sağlar:

- `Received`: Herhangi bir veri bağlantısı alındığında oluşturuldu. Alınan veriler sağlar.
- `ConnectionSlow`: İstemci yavaş veya sık bırakma bir bağlantı algıladığında oluşturulur.
- `Reconnecting`: Temel aktarımı yeniden bağlanmayı başladığında oluşturulur.
- `Reconnected`: Temel aktarımı bağlandı tetiklenir.
- `StateChanged`: Bağlantı durumu değiştiğinde oluşturuldu. Eski durum ve yeni durum sağlar. Durum değerleri bağlantısı hakkında bilgi için bkz [ConnectionState numaralandırma](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Bağlantı kesildi tetiklenir.

Örneğin, önemli değildir ancak aralıklı bağlantısı sorunlarına neden hataları için uyarı iletileri görüntülemek istiyorsanız, gibi yavaşlığı veya sık bağlantı, bırakarak işlemek `ConnectionSlow` olay.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Daha fazla bilgi için bkz: [anlama ve SignalR bağlantısı ömrü olaylarını işleme](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Hataların nasıl işleneceğini

Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, SignalR sonra bir hata döndürür özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir. Örneğin, bir çağrı varsa `newContosoChatMessage` başarısız, hata nesnesindeki hata iletisini içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" üretim istemciler için ayrıntılı hata iletileri için ayrıntılı hata iletileri etkinleştirmek isteyip istemediğinizi ancak güvenlik nedeniyle önerilmez gönderme sorun giderme amacıyla, aşağıdaki kodu sunucuda kullanın.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

SignalR başlatır hataları işlemek için bir işleyici ekleyebilirsiniz `Error` değerinin bağlantı nesnesindeki olay.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Yöntem çağrılarını hataları işlemek için bir try-catch bloğu içinde kodu alın.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>İstemci-tarafı günlük kaydını etkinleştirme

İstemci-tarafı günlük kaydını etkinleştirmek için ayarlanmış `TraceLevel` ve `TraceWriter` bağlantı nesne üzerindeki özellikleri.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod

Sunucu çağırabilirsiniz istemci yöntemleri tanımlamak için daha önce gösterilen kod örnekleri WinRT istemcileri için geçerlidir. Aşağıdaki örnekler, WPF, Silverlight ve konsol uygulaması istemciler için eşdeğer kodunu gösterir.

### <a name="methods-without-parameters"></a>Parametresiz yöntemleri

**WPF istemci kodunu parametresiz sunucusundan adlı yöntemi**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Parametresiz sunucusundan adlı bir yöntem için Silverlight istemci kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Parametresiz sunucusundan konsol uygulaması istemci kodu yöntemi için çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Parametre türleri belirtme parametrelerle yöntemleri

**WPF istemci kodu bir yöntem için parametre sunucusuyla çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Bir parametre olan sunucudan adlı bir yöntem için Silverlight istemci kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Konsol uygulaması istemci kodu bir yöntem için parametre sunucusuyla çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Dinamik nesneler parametre belirterek parametrelerle yöntemleri

**Dinamik Nesne parametresi için kullanarak, bir parametre olan sunucudan adlı bir yöntem için WPF istemci kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Dinamik Nesne parametresi için kullanarak, bir parametre olan sunucudan adlı bir yöntem için Silverlight istemci kodu**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Sunucu parametresi için bir dinamik nesnesi kullanılarak bir parametre ile bir yöntem için konsol uygulaması istemci kodu çağrılır**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
