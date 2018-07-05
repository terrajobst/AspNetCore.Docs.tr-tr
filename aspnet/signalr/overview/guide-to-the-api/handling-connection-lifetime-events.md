---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: Signalr'da bağlantı ömrü olaylarını işleme ve anlama | Microsoft Docs
author: pfletcher
description: Bu makalede, hub'ları API'si tarafından kullanıma sunulan olayları kullanmayı açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: cc7725858057c6313b3b4210199c5bbb636f4592
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366909"
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Anlama ve signalr'da bağlantı ömrü olaylarını işleme
====================
tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Bu makalede başa çıkabilir SignalR bağlantı yeniden bağlanma ve bağlantıyı kesme olaylarını ve yapılandırabileceğiniz zaman aşımı ve keepalive ayarları genel bir bakış sağlar.
> 
> SignalR ve bağlantı ömrü olaylarını biraz bilgi zaten sahip olduğunuz varsayılır. Signalr'a giriş için bkz [signalr'a giriş](../getting-started/introduction-to-signalr.md). Bağlantı ömrü olaylarını bir listesi için aşağıdaki kaynaklara bakın:
> 
> - [Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl](hubs-api-guide-server.md#connectionlifetime)
> - [JavaScript istemcilerinin bağlantı ömrü olaylarını işlemek nasıl](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [.NET istemcileri bağlantı ömrü olaylarını işlemek nasıl](hubs-api-guide-net-client.md#connectionlifetime)
> 
> ## <a name="software-versions-used-in-this-topic"></a>Bu konu başlığında kullanılan yazılım sürümleri
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
> SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Genel Bakış

Bu makalede, aşağıdaki bölümleri içerir:

- [Bağlantı ömrü terminoloji ve senaryolar](#terminology)

    - [SignalR bağlantıları, aktarım bağlantılarının ve fiziksel bağlantıları](#signalrvstransport)
    - [Taşıma bağlantısı kesme senaryoları](#transportdisconnect)
    - [İstemci bağlantı kesilmesi senaryoları](#clientdisconnect)
    - [Sunucu bağlantı kesilmesi senaryoları](#serverdisconnect)
- [Zaman aşımı ve keepalive ayarları](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Zaman aşımı ve keepalive ayarlarını değiştirme](#changetimeout)
- [Kullanıcı bağlantı kesilmesi hakkında bilgilendirme](#notifydisconnect)
- [Sürekli olarak yeniden bağlama](#continuousreconnect)
- [Bir istemci sunucu kodunda kesme hakkında](#disconnectclientfromserver)
- [Bağlantı kesilmesi nedeni algılama](#detectingreasonfordisconnection)

API başvuru konularına bağlar API .NET 4.5 sürümü var. .NET 4 kullanıyorsanız, bkz. [API konuları .NET 4 sürümünü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Bağlantı ömrü terminoloji ve senaryolar

`OnReconnected` Olay işleyicisinde bir SignalR hub'ı hemen sonra yürütebilir `OnConnected` ancak sonra değil `OnDisconnected` belirli bir istemci için. Bir yeniden olmadan bağlantı kesilmesi olabilir "bağlantısı" sözcüğü SignalR öğesinde kullanılan çeşitli yolları vardır nedenidir.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>SignalR bağlantıları, aktarım bağlantılarının ve fiziksel bağlantıları

Bu makalede birbirinden ayırt *SignalR bağlantıları*, *taşıma bağlantıları*, ve *fiziksel bağlantıları*:

- **SignalR bağlantı** bir istemci ve sunucu URL'si, SignalR API'sı tarafından bakımı yapılan ve benzersiz bir bağlantı kimliği tarafından tanımlanan arasında mantıksal bir ilişki başvurur Bu ilişki hakkındaki verileri SignalR tarafından korunur ve aktarım bağlantı kurmak için kullanılır. İlişki sona erer ve SignalR istemci çağırdığında verilerini siler `Stop` yöntemi ve bir zaman aşımı sınırı SignalR kayıp taşıma bağlantısını yeniden kurmaya çalışıyor durumdayken ulaşıldığında.
- **Aktarım bağlantı** dört aktarım API'lerini biri tarafından korunan bir sunucu ile istemci arasındaki mantıksal ilişkisi başvuruyor: WebSockets, sunucu tarafından gönderilen olaylar, sonsuza kadar çerçeve veya uzun yoklama. Aktarım bağlantısı oluşturmak için API taşıma SignalR kullanır ve aktarım API Aktarım bağlantısı oluşturmak için bir fiziksel ağ bağlantısı varlığı üzerinde bağlıdır. SignalR sonlandığında veya taşıma API'si fiziksel bağlantının bozuk olduğunu algıladığında taşıma bağlantısını sonlandırır.
- **Fiziksel bağlantı** --kablo, fiziksel ağ bağlantıları kablosuz sinyalleri, yönlendiriciler, bir istemci bilgisayarı ile sunucu bilgisayarı arasındaki iletişimi kolaylaştırır vb.--başvuruyor. Fiziksel bağlantı Aktarım bağlantısı kurabilmek için mevcut olması gerekir ve bir SignalR bağlantısı kurabilmek için taşıma bağlantı kurulması gerekir. Bu konunun ilerleyen kısımlarında açıklandığı gibi ancak fiziksel bağlantı kesme her zaman hemen taşıma veya SignalR bağlantısı sonlanmıyor.

Aşağıdaki diyagramda, SignalR bağlantı hub'ları API ve PersistentConnection API SignalR katmanı tarafından temsil edilen, Aktarım bağlantısı taşımalar katmanı tarafından temsil edilir ve fiziksel bağlantı sunucusu arasındaki çizgilerle gösterilir ve istemciler.

![SignalR mimarisi diyagramı](handling-connection-lifetime-events/_static/image1.png)

Çağırdığınızda `Start` SignalR istemci yönteminde, sağlamaya SignalR istemci kodu fiziksel bir sunucuya bağlantı kurmak için gereken tüm bilgileri. SignalR istemci kodu, bir HTTP istek ve dört aktarım yöntemi kullanan fiziksel bir ağ bağlantısı kurmak için bu bilgileri kullanır. İstemci, otomatik olarak yeni bir aktarım bağlantısı aynı SignalR URL'ye yeniden oluşturmak için gereken bilgileri yine de sahip olduğu Aktarım bağlantısı başarısız veya sunucu başarısız olursa, SignalR bağlantı hemen hemen gideceği anlamına gelmez. Bu senaryoda, kullanıcı uygulamadaki herhangi bir müdahalesi söz konusu ve SignalR istemci kodu yeni bir aktarım bağlantı kurduğunda, yeni bir SignalR bağlantısı başlamaz. Sürekliliği SignalR bağlantısının bulgusunda yansıtılır, çağırdığınızda oluşturulduğu bağlantı kimliği `Start` yöntemi değiştirmez.

`OnReconnected` Olay işleyicisi Hub'ında bir aktarım bağlantısı kesilmiş sonra otomatik olarak yeniden kurulduğunda yürütür. `OnDisconnected` Olay işleyicisi bir SignalR bağlantısının sonunda yürütür. Bir SignalR bağlantısının aşağıdaki yollardan biriyle sona erdirilebilir:

- İstemci çağırırsa `Stop` yöntemi, bir Dur iletisi sunucuya gönderilir ve hem istemci hem de sunucu son SignalR bağlantı hemen.
- İstemci ve sunucu arasında bağlantı kaybedildikten sonra istemci yeniden dener ve istemci yeniden bağlanmak sunucunun bekler. Yeniden bağlanma girişimleri başarısız olur ve bağlantı kesme zaman aşımı süresi sona erer, istemci ve sunucu SignalR bağlantı sonlandır. İstemci yeniden bağlanmayı denemeden durdurur ve sunucunun SignalR bağlantı gösterimine siler.
- İstemci çağırmak için bir fırsat zorunda kalmadan çalıştıran durduğunda `Stop` yöntemi, sunucunun beklediği istemci yeniden bağlamak ve sonra bağlantı kesme zaman aşımı süresi sonrasında SignalR bağlantıyı sonlandırır.
- Çalıştırmayı, sunucu durdurur, istemci yeniden denediğinde (Aktarım bağlantısı yeniden oluşturursanız) ve sonra bağlantı kesme zaman aşımı süresi sonrasında SignalR bağlantıyı sonlandırır.

Ne zaman bağlantı sorunu olmadığından ve kullanıcı uygulaması çağırarak SignalR bağlantı sona erer `Stop` yöntemi, SignalR bağlantı ve taşıma bağlantısını başlayıp bitmelidir aynı zamanda hakkında. Aşağıdaki bölümlerde, diğer senaryolar daha ayrıntılı olarak açıklanmaktadır.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Taşıma bağlantısı kesme senaryoları

Fiziksel bağlantı yavaş olabilir veya bağlantı kesintiler olabilir. Taşıma bağlantısını kesinti uzunluğu gibi faktörlere bağlı olarak bırakılabilir. SignalR, ardından taşıma bağlantısını yeniden kurmaya çalışır. Bazen taşıma bağlantısını API kesinti algılar ve taşıma bağlantısını keser ve SignalR hemen bağlantısı kesildiği öğrenir. Diğer senaryolarda ne taşıma bağlantısını API ve SignalR hemen bağlantı kaybedildi haberdar olur. Uzun yoklama dışındaki tüm aktarımları için çağrılan bir işlev SignalR istemcinin *keepalive* taşıma API'si algılayamaz bağlantı kaybı için denetlenecek. Uzun yoklama bağlantılar hakkında daha fazla bilgi için bkz: [zaman aşımı ve keepalive ayarları](#timeoutkeepalive) bu konuda.

Düzenli aralıklarla bir bağlantı etkin olduğunda sunucu istemciye keepalive paket gönderir. Bu makalenin yazıldığı tarih itibarıyla varsayılan sıklık her 10 saniyedir. Bu paketler için dinleyerek istemciler bir bağlantı sorunu olmadığını söyleyebilir. Keepalive paket beklendiğinde alınmazsa, kısa bir süre sonra istemci yavaşlık veya kesintiler gibi bağlantı sorunlarını olduğunu varsayar. Keepalive uzun bir süre sonra hala alınmazsa, bağlantı kesildi ve yeniden bağlanmayı deniyor başlar istemci varsayar.

Aşağıdaki diyagram tipik bir senaryoda hemen taşıma API'si tarafından tanınmayan fiziksel bağlantı sorunları olduğunda oluşan istemci ve sunucu olayları gösterir. Diyagram için aşağıdaki koşullar geçerlidir:

- Taşıma WebSockets, sonsuza kadar çerçeve veya sunucu tarafından gönderilen olayların ' dir.
- Fiziksel ağ bağlantısını kesinti değişen dönemlerini vardır.
- SignalR bunları algılamak için keepalive işlevselliğini kullanır. böylece taşıma API'si kesintileri farkında olmaz.

![Aktarım bağlantı kesilmesi](handling-connection-lifetime-events/_static/image2.png)

İstemci modu yeniden bağlanmayı içine geçer, ancak bağlantı kesme zaman aşımı sınırı içinde aktarım bağlantı kuramıyor. sunucu SignalR bağlantıyı sonlandırır. Bu durum oluştuğunda, Hub'ın sunucu yürütür `OnDisconnected` yöntemi ve daha sonra bağlanmak istemci yöneten olasılığına istemciye gönderilecek bir bağlantı kesme iletisi sıralarını. İstemci, daha sonra yeniden bağlarsanız, çağrıları ve bağlantıyı kesme komutu aldığı `Stop` yöntemi. Bu senaryoda, `OnReconnected` istemci bağlandığında yürütülmez ve `OnDisconnected` istemci çağırdığında yürütülmez `Stop`. Aşağıdaki diyagramda, bu senaryo gösterilmektedir.

![Aktarım kesintileri - sunucu zaman aşımı](handling-connection-lifetime-events/_static/image3.png)

İstemcide oluşturulabilir SignalR bağlantı ömrü olaylarını aşağıda verilmiştir:

- `ConnectionSlow` İstemci olay.

    Keepalive ping alındı veya önceden belirlenmiş bir keepalive zaman aşımı süresi oranını son yapılacak itibaren geçen zaman oluşturulur. Varsayılan keepalive uyarı zaman aşımını 2/3 keepalive zaman aşımı olur. Keepalive zaman aşımı süresi 20 saniye olduğundan yaklaşık 13 saniyede uyarı oluşur.

    Varsayılan olarak, sunucu, 10 saniyede keepalive ping gönderir ve istemci keepalive ping her 2 saniyede (keepalive zaman aşımı değeri keepalive zaman aşımı uyarısı değeri arasındaki farkı üçüncü bir) ilgili olup olmadığını denetler.

    Taşıma API'si bir bağlantısının kesilmesi uyumlu hale gelirse keepalive uyarı zaman aşımını geçirmeden önce SignalR bağlantısının kesilmesi haberdar olması. Bu durumda, `ConnectionSlow` olay değil yükseltilebilir ve SignalR Git doğrudan `Reconnecting` olay.
- `Reconnecting` İstemci olay.

    Bağlantı kaybedilirse veya (b) keepalive zaman aşımı süresi son yapılacak itibaren geçen veya keepalive ping alındı (a) aktarım API tespit ettiğinde oluşturulur. SignalR istemci kodu yeniden bağlanmaya çalışılırken başlar. Taşıma bağlantısı kesildiğinde, bazı işlemler yapması için uygulamanızı isterseniz bu olay işleyebilir. Varsayılan keepalive zaman aşımı süresi şu anda 20 saniyedir.

    İstemci kodunuz SignalR modu yeniden bağlanmayı de olsa bir Hub yöntemini çağırmaya çalışırsa, SignalR komutu göndermeyi yeniden deneyecek. Çoğu zaman, bu tür denemeleri başarısız olur, ancak bazı durumlarda bunlar başarılı olabilir. Sunucu tarafından gönderilen olayları, sonsuza kadar çerçeve ve uzun yoklama taşıma için iki iletişim kanalı, istemcinin göndermek için kullandığı diğeri iletileri almak için kullandığı SignalR kullanır. Kanal alma için kullanılan kalıcı olarak açık olur ve fiziksel bağlantı kesildiğinde kapalı bir. Fiziksel bağlantısı geri alma kanal yeniden kurulur önce istemciden sunucuya bir yöntem çağrısının başarılı olabilir kullanılabilir kalır göndermek için kullanılan kanal. SignalR alma için kullanılan kanal yeniden açılana kadar dönüş değeri alınmayan.
- `Reconnected` İstemci olay.

    Aktarım bağlantısı yeniden kurulduğunda oluşturulur. `OnReconnected` Hub olay işleyicisinde yürütür.
- `Closed` İstemci olayı (`disconnected` JavaScript olay).

    SignalR istemci kodu Aktarım bağlantısı kaybettikten sonra yeniden bağlanmaya çalışırken bağlantı kesme zaman aşımı süresi dolduğunda oluşturulur. Varsayılan bağlantı kesme zaman aşımı olan 30 saniye. (Bu olay için bağlantı sona erdiğinde de tetiklenir `Stop` yöntemi çağrılır.)

Taşıma API'si tarafından algılanmayan ve keepalive ping keepalive zaman aşımı uyarısı süresinden daha uzun bir süre sunucudan alımını beklemeyin aktarım bağlantı kesintilerini herhangi bir bağlantı oluşturulması yaşam süresi olayları neden.

Bazı ağ ortamları boşta kalan bağlantıların kasıtlı olarak kapatın ve bu ağların bir SignalR bağlantısı kullanımda almayacağınızı tarafından bu önlemeye yardımcı olmak için keepalive paketlerinin başka bir işlev. Aşırı durumlarda varsayılan keepalive ping sıklığını kapalı bağlantılarını önlemek için yeterli olmayabilir. Bu durumda, daha sık gönderilmesini keepalive ping yapılandırabilirsiniz. Daha fazla bilgi için [zaman aşımı ve keepalive ayarları](#timeoutkeepalive) bu konuda.

> [!NOTE] 
> 
> **Önemli**: Burada açıklanan olayların sırasını garanti edilmez. SignalR bağlantı ömrü olaylarını bu düzene göre öngörülebilir bir şekilde yükseltmek için her girişimlerde bulunur, ancak ağ olayları çeşitli kullanımları ve hangi aktarım API'leri gibi temel iletişim çerçeveleri bunları işlemesi birçok yolu vardır. Örneğin, `Reconnected` istemci bağlandığında, olay oluşmayabilir veya `OnConnected` işleyici sunucudaki bağlantı girişimi başarısız olduğunda çalıştırabilirsiniz. Bu konuda, genellikle bazı tipik durumlarda tarafından üretilen etkileri açıklanmaktadır.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>İstemci bağlantı kesilmesi senaryoları

Bir tarayıcı istemcisinin bir SignalR bağlantısının tutan SignalR istemci kodu bir web sayfasının JavaScript bağlamında çalıştırılır. Sahip bir gittiğinizde sonlandırmak SignalR bağlantısı neden sayfa için başka bir ve söz konusu bilgisayarın neden varsa birden çok bağlantı kimlikleri birden fazla bağlantıyla birden çok tarayıcı pencerelerini veya sekmeleri bağlanın. Kullanıcı bir tarayıcı penceresi veya sekmesi kapatır veya yeni bir sayfaya gider veya sayfa yenilendikten sonra ve aramalar için SignalR istemci kodu, tarayıcı olayını işler çünkü SignalR bağlantı hemen sona `Stop` yöntemi. Bu senaryolar ya da uygulamanız çağırdığında istemci platformu `Stop` yöntemi `OnDisconnected` olay işleyicisi sunucuda hemen yürütür ve istemcinin oluşturduğu `Closed` olay (olay adlı `disconnected` içinde JavaScript için).

Bir istemci uygulaması veya üzerinde çalıştığı bilgisayar kilitleniyor veya (örneğin, kullanıcının bir dizüstü bilgisayar kapandığında) uyku moduna geçer, sunucunun ne hakkında bilgisi verilmediği için. Sunucu bilir kadar istemci kaybı nedeniyle bağlantı kesintisi olabilir ve istemciyi yeniden çalışıyor olabilir. Bu nedenle, sunucunun beklediği istemci yeniden bağlanmak için bir fırsat vermek için bu senaryolarda ve `OnDisconnected` (yaklaşık 30 saniye varsayılan olarak) bağlantı kesme zaman aşımı süresi dolana kadar yürütmez. Aşağıdaki diyagramda, bu senaryo gösterilmektedir.

![İstemci bilgisayar hatası](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Sunucu bağlantı kesilmesi senaryoları

Bir sunucu çevrimdışı--olduğunda yeniden başlatır, başarısız olursa uygulama etki alanı geri dönüştürme işlemleri, vb.--sonuç bir bağlantı kesildi benzer olabilir veya aktarım API ve SignalR hemen sunucu kalktığını ve SignalR başlayın olmadan yeniden bağlanmaya çalışılırken biliyor olabilirsiniz yükseltme `ConnectionSlow` olay. İstemci, istemci modu yeniden bağlanmayı içine aşması durumunda ve sunucu kurtarır ya da bağlantı kesme zaman aşımı süresi dolmadan önce yeniden başlatma veya yeni bir sunucu çevrimiçi duruma geri yüklendi veya yeni sunucuya yeniden bağlanır. Bu durumda, istemcide SignalR bağlantısı devam eder ve `Reconnected` olayı oluşturulur. İlk sunucusunda `OnDisconnected` asla yürütülmez ve yeni sunucudaki `OnReconnected` rağmen yürütülür `OnConnected` hiçbir zaman önce bu sunucuda istemci için yürütüldü. (Aynı sunucu da başlatıldığında çünkü istemci bir yeniden başlatma veya uygulama etki alanı dönüşüm sonra aynı sunucuya bağlanırsa önceki bağlantı etkinlik belleğe sahip değil etkili olur.) Aşağıdaki diyagramda taşıma API'si hemen bağlantı kesildi haberdar olur varsayar. Bu nedenle `ConnectionSlow` olayı oluşmaz.

![Sunucu hatası ve yeniden bağlanma](handling-connection-lifetime-events/_static/image5.png)

Bir sunucu bağlantı kesme zaman aşımı süresi içinde kullanılabilir hale gelmezse, SignalR bağlantısı ile sona erer. Bu senaryoda, `Closed` olay (`disconnected` JavaScript istemcilerinin) istemcide oluşturulur ancak `OnDisconnected` sunucuda hiçbir zaman çağrılır. Aşağıdaki diyagramda, SignalR keepalive işlevi tarafından algılandığı şekilde taşıma API'si bağlantı kesildi, farkında haline gelmediğinden emin varsayar ve `ConnectionSlow` olayı oluşturulur.

![Sunucu hatası ve zaman aşımı](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Zaman aşımı ve keepalive ayarları

Varsayılan `ConnectionTimeout`, `DisconnectTimeout`, ve `KeepAlive` değerler çoğu senaryo için uygun olan ancak ortamınıza özel gereksinimleri varsa değiştirilebilir. Örneğin, ağ ortamınızda 5 saniye boyunca boşta bağlantılar kapanıyorsa keepalive değerini azaltın gerekebilir.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Bu ayarı açık ve yeni bir bağlantı açmak ve kapatmadan önce yanıt bekleyen bir aktarım bağlantısı kalma süresini temsil eder. Varsayılan değer 110 saniyedir.

Bu ayar, yalnızca zaman keepalive işlevi, normalde yalnızca uzun olarak uygulandığı devre dışı geçerlidir yoklama taşıması. Aşağıdaki diyagram bu ayarı uzun etkisini gösterir. yoklama taşıma bağlantısı.

![Uzun yoklama taşıma bağlantısı](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Bu ayar tetiklenmeden önce bir aktarım bağlantısı kayboldu sonra beklenecek süreyi temsil eden `Disconnected` olay. Varsayılan değer 30 saniyedir. Ayarladığınızda `DisconnectTimeout`, `KeepAlive` 1/3'e ayarlandığında otomatik olarak `DisconnectTimeout` değeri.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Bu ayar, boştaki bir bağlantı üzerinden canlı tutma paketi göndermeden önce beklenecek süreyi temsil eder. Varsayılan değer 10 saniyedir. Bu değer, birden fazla 1/3 / olmamalıdır `DisconnectTimeout` değeri.

Hem ayarlamak istiyorsanız `DisconnectTimeout` ve `KeepAlive`ayarlayın `KeepAlive` sonra `DisconnectTimeout`. Aksi takdirde, `KeepAlive` ayar olacak üzerine ne zaman `DisconnectTimeout` otomatik olarak ayarlar `KeepAlive` zaman aşımı değeri 1/3.

Keepalive işlevini devre dışı bırakmak istiyorsanız, `KeepAlive` null. KeepAlive işlevleri otomatik olarak uzun süre devre dışı yoklama taşıması.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Zaman aşımı ve keepalive ayarlarını değiştirme

Bu ayarlar için varsayılan değerleri değiştirmek için bunları kümesinde `Application_Start` içinde *Global.asax* , aşağıdaki örnekte gösterildiği gibi dosya. Örnek kodda gösterilen değerleri varsayılan değerleri ile aynıdır.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Kullanıcı bağlantı kesilmesi hakkında bilgilendirme

Bazı uygulamalarda bağlantı sorunları olduğunda, kullanıcıya bir ileti görüntülemek isteyebilirsiniz. Çeşitli seçenekler nasıl ve ne zaman bunu yapmak var. Aşağıdaki kod örnekleri, oluşturulan proxy kullanmak için bir JavaScript istemcisi olan.

- Tanıtıcı `connectionSlow` modu yeniden bağlanmayı içine geçmeden önce SignalR, bağlantı sorunların farkında hemen sonra bir ileti görüntülemek için olay.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Tanıtıcı `reconnecting` SignalR bağlantı kesilmesi farkındadır ve modu yeniden bağlanmayı uygulamasına gidip bir ileti görüntülemek için olay.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Tanıtıcı `disconnected` yeniden bağlanma girişimi olduğunda bir ileti görüntülemek için olay zaman aşımına uğradı. Bu senaryoda, bir sunucu ile yeniden yeniden bağlantı kurmayı tek yolu çağırarak SignalR bağlantı yeniden başlatmaktır `Start` yeni bir bağlantı kimliği oluşturur, yöntemi Aşağıdaki kod örneği, yalnızca yeniden bağlanan bir zaman aşımından sonra bildirim çağırarak neden SignalR bağlantı normal bir ucuna değil sonra sorun emin olmak için bir bayrak kullanır. `Stop` yöntemi.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Sürekli olarak yeniden bağlama

Bazı uygulamalarda otomatik olarak kaybolmuş olabilir ve yeniden bağlanma girişimi zaman aşımına uğradı sonra yeniden bağlantı isteyebilirsiniz. Bunu yapmak için çağırabilirsiniz `Start` yönteminden, `Closed` olay işleyicisi (`disconnected` JavaScript istemcilerde olay işleyicisi). Çağırmadan önce bir süre bekleyin isteyebileceğiniz `Start` çok Bunu önlemek için sık sunucu veya fiziksel bağlantısı olduğunda kullanılabilir. Aşağıdaki kod örneği, oluşturulan proxy kullanarak JavaScript istemcisi için ' dir.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Mobil istemciler dikkat edilmesi gereken olası bir sorunu, sunucu ya da fiziksel bağlantı kullanılamadığında sürekli yeniden bağlanma denemesi gereksiz pil boşaltma neden olabilecek ' dir.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Bir istemci sunucu kodunda kesme hakkında

SignalR sürüm 2, istemcilerin bağlantıları kesiliyor için yerleşik bir sunucu API yok. Vardır [gelecekte bu işlevsellik eklemek için planlar](https://github.com/SignalR/SignalR/issues/2101). Geçerli SignalR, en basit yolu, bir istemcinin sunucudan bağlantısını kesmek için istemcide bir bağlantıyı kes yöntemi uygulaması ve sunucudan bu yöntemi çağırmak için sürümündedir. Aşağıdaki kod örneği oluşturulan proxy kullanarak JavaScript istemcisi için bir bağlantı kesme yöntemi gösterir.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Güvenlik - ne istemcilerin bağlantıları kesiliyor için bu yöntem ya da önerilen yerleşik API adres kötü amaçlı kod çalıştıran istemcilerin adresine yeniden bağlanılamadı veya ele geçirilen kodu kaldırabilir kullanılan istemcilerin senaryo `stopClient` yöntemi veya değiştirme ne işe yarar. Durum bilgisi olan hizmet reddi (DOS) koruması uygulamak için en uygun framework veya sunucusu katmanı değil, ön uç altyapısı yerdir.


<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Bağlantı kesilmesi nedeni algılama

SignalR 2.1 sunucuya aşırı ekler `OnDisconnect` aşımından yerine istemci kasıtlı olarak bağlı olmadığını gösteren bir olay. `StopCalled` Parametredir istemci açıkça bağlantı kapalı değilse true. İstemci bağlantısını kesmek için bir sunucu hatası neden olursa JavaScript'te hata bilgilerini istemci olarak geçirilecek `$.connection.hub.lastError`.

**C# sunucu kodu: `stopCalled` parametresi**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**JavaScript istemci kodu: erişme `lastError` içinde `disconnect` olay.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
