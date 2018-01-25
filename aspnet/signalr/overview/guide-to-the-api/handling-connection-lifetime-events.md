---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: "Anlama ve SignalR bağlantısı ömrü olayları işleme | Microsoft Docs"
author: pfletcher
description: "Bu makalede, hub API'si tarafından kullanıma sunulan olayları kullanmayı açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 2fd9cafd8d7706807998793c3c39377fe9604266
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Anlama ve SignalR bağlantısı ömrü olayları işleme
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)

> Bu makalede işleyebilir SignalR bağlantısı, yeniden bağlanma ve bağlantıyı kesme olayları ve yapılandırabileceğiniz zaman aşımı ve canlı tutma ayarlarını genel bakış sağlar.
> 
> Makalede zaten SignalR ve bağlantı ömür olayları bazı bilgiye sahip olduğunuz varsayılmaktadır. SignalR giriş için bkz: [SignalR giriş](../getting-started/introduction-to-signalr.md). Bağlantı ömrü olaylarının bir listesi için aşağıdaki kaynaklara bakın:
> 
> - [Bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını](hubs-api-guide-server.md#connectionlifetime)
> - [Bağlantı ömür olayları JavaScript istemcilerinin nasıl ele alınacağını](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [Bağlantı ömür olayları .NET istemcileri nasıl ele alınacağını](hubs-api-guide-net-client.md#connectionlifetime)
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

Bu makalede aşağıdaki bölümleri içerir:

- [Bağlantı ömrü terminoloji ve senaryolar](#terminology)

    - [SignalR bağlantıları, aktarım bağlantıları ve fiziksel bağlantıları](#signalrvstransport)
    - [Taşıma bağlantısı kesme senaryoları](#transportdisconnect)
    - [İstemci bağlantısının kesilmesine senaryoları](#clientdisconnect)
    - [Sunucu bağlantı kesme senaryoları](#serverdisconnect)
- [Zaman aşımı ve canlı tutma ayarları](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Zaman aşımı ve canlı tutma ayarlarını değiştirme](#changetimeout)
- [Bağlantı kesilmeleri hakkında kullanıcıya bildirim nasıl](#notifydisconnect)
- [Sürekli olarak yeniden bağlama](#continuousreconnect)
- [Bir istemci sunucu kodunda kesme hakkında](#disconnectclientfromserver)
- [Bağlantı kesme nedeni algılama](#detectingreasonfordisconnection)

API başvuru konuları API'si .NET 4.5 sürümüne bağlantılardır. .NET 4 kullanıyorsanız, bkz: [API konuları .NET 4 sürümü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Bağlantı ömrü terminoloji ve senaryolar

`OnReconnected` Bir SignalR hub'ı olay işleyicisini hemen sonra yürütebilir `OnConnected` ancak sonra değil `OnDisconnected` belirtilen istemci için. Bağlantı kesme olmadan yeniden bağlanmayı olabilir "bağlantı" word SignalR öğesinde kullanılan birkaç yolu vardır nedenidir.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>SignalR bağlantıları, aktarım bağlantıları ve fiziksel bağlantıları

Bu makalede birbirinden *SignalR bağlantıları*, *taşıma bağlantıları*, ve *fiziksel bağlantıları*:

- **Bir SignalR bağlantısı** bir istemci ve sunucu URL'si, SignalR API'si tarafından korunur ve benzersiz bir bağlantı kimliği ile tanımlanan arasında mantıksal bir ilişki başvurur Bu ilişki hakkındaki verileri SignalR tarafından korunur ve aktarım bağlantı kurmak için kullanılır. İlişki Uçları ve SignalR istemci çağırdığında verilerini siler `Stop` yöntemi ve bir zaman aşımı sınırı SignalR kayıp aktarım yeniden bağlantı girişimi sırasında ulaşıldığında.
- **Taşıma bağlantısı** dört aktarım API'lerini biri tarafından korunan bir sunucu ile istemci arasındaki mantıksal ilişkisi başvuruyor: WebSockets, sunucu tarafından gönderilen olayları, geri getirilemeyecek biçimde çerçeve veya uzun yoklama. Taşıma bağlantısı oluşturmak için Aktarım API SignalR kullanır ve aktarım bağlantı oluşturmak için bir fiziksel ağ bağlantı varlığını aktarım API bağlıdır. SignalR sonlandırıldığında veya API taşıma fiziksel bağlantı kesildiğinde algıladığında taşıma bağlantısını sonlandırır.
- **Fiziksel bağlantı** --kablo, fiziksel ağ bağlantıları kablosuz sinyalleri, yönlendiriciler, istemci bilgisayar ve bir sunucu bilgisayar arasındaki iletişimi kolaylaştırmak vb.--başvuruyor. Fiziksel bağlantı taşıma bağlantısı kurabilmek için mevcut olması gerekir ve bir SignalR bağlantısı kurabilmek için taşıma bağlantı kurulamıyor. Bu konunun ilerleyen bölümlerinde açıklandığı gibi ancak fiziksel bağlantısı kesiliyor her zaman hemen taşıma veya SignalR bağlantısı sonlanmıyor.

Aşağıdaki diyagramda bir SignalR bağlantısı PersistentConnection API SignalR katman ve hub API tarafından temsil edilen, Aktarım katmanı bağlantısı taşımaları katmanı tarafından temsil edilen ve fiziksel bağlantı sunucusu arasındaki çizgilerle gösterilir ve istemciler.

![SignalR mimarisi diyagramı](handling-connection-lifetime-events/_static/image1.png)

Çağırdığınızda `Start` yöntemi SignalR İstemcisi'nde, sağlamayla SignalR istemci kodu fiziksel bir sunucuya bağlantı kurmak için gereken tüm bilgileri. SignalR istemci kodu, bir HTTP isteği yapmak ve dört taşıma yöntemleri birini kullanan fiziksel bir ağ bağlantısı kurmak için bu bilgileri kullanır. İstemci hala otomatik olarak yeni bir aktarım bağlantı SignalR URL'ye yeniden oluşturmak için gereken bilgileri içerdiğinden taşıma bağlantısı başarısız veya sunucu başarısız olursa bir SignalR bağlantısı hemen azalttıktan değil. Bu senaryoda, kullanıcı uygulamasından herhangi bir müdahalesi söz konusu ve SignalR istemci kodu yeni bir aktarım bağlantı kurduğunda, yeni bir SignalR bağlantı başlatılmaz. Bir SignalR bağlantısı sürekliliği aslında yansıtılır, çağırdığınızda oluşturulan bağlantı kimliği `Start` yöntemi, değiştirmez.

`OnReconnected` Olay işleyicisi Hub'ında taşıma bağlantısı kesilmiş sonra otomatik olarak yeniden kurulduğunda yürütür. `OnDisconnected` Olay işleyicisi bir SignalR bağlantısı sonunda yürütür. Bir SignalR bağlantısı aşağıdaki yollardan biriyle sona erdirilebilir:

- İstemci çağırırsa `Stop` yöntemi, bir Dur iletisi sunucuya gönderilir ve istemci ve sunucunun son bir SignalR bağlantısı hemen.
- İstemci ve sunucu arasında bağlantı kaybedildikten sonra istemci yeniden bağlanmayı dener ve istemci yeniden bağlanmayı sunucu bekler. Yeniden bağlanma girişimleri başarısız olur ve bağlantı kesme zaman aşımı süresi sona erer, istemci ve sunucu bir SignalR bağlantısı bitiş. İstemci yeniden bağlanmayı denemeden durdurur ve sunucunun kendi SignalR bağlantısı gösterimi siler.
- İstemci çağırmak için bir fırsat gerek kalmadan çalıştıran durduğunda `Stop` yöntemi, sunucunun beklediği istemci yeniden bağlanmak ve sonra bağlantı kesme zaman aşımı süresinden sonra da SignalR bağlantıyı sonlandırır.
- Çalıştıran, sunucusu vermiyor bağlanmayı istemci çalışır (taşıma bağlantısını yeniden oluşturursanız) ve sonra bağlantı kesme zaman aşımı süresinden sonra da SignalR bağlantıyı sonlandırır.

Ne zaman bağlantı sorunu olmadığından ve çağırarak kullanıcı uygulaması SignalR bağlantısı sona erer `Stop` yöntemi, bir SignalR bağlantısı ve aktarım bağlantı başlar ve aynı anda hakkında son adresindeki. Aşağıdaki bölümlerde daha ayrıntılı olarak diğer senaryolar açıklanmaktadır.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Taşıma bağlantısı kesme senaryoları

Fiziksel bağlantı yavaş olabilir veya bağlantı kesintiler olabilir. Taşıma bağlantısı kesinti uzunluğu gibi etkenlere bağlı olarak bırakılabilir. SignalR sonra aktarım yeniden bağlantı kurmayı dener. Bazen taşıma bağlantısı API kesinti algılar ve taşıma bağlantısını keser ve SignalR hemen bağlantısı kesildiği öğrenir. Diğer senaryolarda ne taşıma bağlantısı API ne de SignalR hemen bağlantısı kaybedildi uyumlu olur. Uzun yoklama dışındaki tüm taşımaları için SignalR istemcisi adlı bir işlev kullanan *keepalive* API taşıma algılayamıyor bağlantı kaybı için denetlemek için. Uzun yoklama bağlantılar hakkında daha fazla bilgi için bkz: [zaman aşımı ve canlı tutma ayarlarını](#timeoutkeepalive) bu konuda daha sonra.

Düzenli aralıklarla bir bağlantısı etkin olduğunda, sunucu istemciye bir canlı tutma paketi gönderir. Bu makalenin yazıldığı tarih itibariyle varsayılan sıklık her 10 saniyedir. Bu paketler için dinleyerek, istemciler bir bağlantı sorunu olup olmadığını söyleyebilir. Keepalive paket beklenirken alınmazsa, kısa bir süre sonra istemci bağlantısı sorunlarını yavaşlığı veya kesintilerinin gibi olduğunu varsayar. Keepalive daha uzun bir süre sonra hala alınmazsa, istemci bağlantı kesildi ve yeniden bağlanmayı denemeden başlar varsayar.

Aşağıdaki diyagram tipik bir senaryoda hemen aktarım API tarafından tanınmayan fiziksel bağlantı sorunları olduğunda oluşan istemci ve sunucu olayları gösterir. Diyagram için aşağıdaki koşullar geçerlidir:

- Taşıma WebSockets, sonsuza kadar çerçeve veya sunucu tarafından gönderilen olayları ' dir.
- Fiziksel ağ bağlantısını kesinti değişen dönemlerini vardır.
- SignalR bunları algılamak için keepalive işlevselliğini kullanır şekilde API taşıma kesintiler haberdar olmaz.

![Taşıma bağlantısının kesilmesi](handling-connection-lifetime-events/_static/image2.png)

İstemci modu yeniden bağlanmayı içine gider, ancak bağlantı kesme zaman aşımı sınırı içinde taşıma bağlantısı kurulamıyor, sunucunun SignalR bağlantıyı sonlandırır. Bu durum oluştuğunda sunucunun Hub'ın yürütür `OnDisconnected` yöntemi ve daha sonra istemci yönetir durumda istemciye gönderilecek bir bağlantı kesme iletisi sıralarını. İstemci daha sonra yeniden bağlanma, bağlantıyı kesme komutu ve çağrıları aldığı `Stop` yöntemi. Bu senaryoda, `OnReconnected` istemci bağlandığında yürütülmedi ve `OnDisconnected` istemci çağırdığında yürütülmedi `Stop`. Aşağıdaki diyagramda, bu senaryo gösterilmektedir.

![Aktarım kesintilerini - sunucu zaman aşımı](handling-connection-lifetime-events/_static/image3.png)

İstemci üzerinde gerçekleştirilen SignalR bağlantısı ömür olayları şunlardır:

- `ConnectionSlow`İstemci olay.

    Keepalive zaman aşımı süresi önceden belirlenmiş bir kısmının bu yana en son iletinin geçti veya keepalive ping alındı tetiklenir. Varsayılan keepalive uyarı zaman aşımını 2/3 keepalive zaman aşımı olur. Uyarı hakkında 13 saniyede oluşmaz canlı tutma zaman aşımı 20 saniye kullanılır.

    Varsayılan olarak, sunucu, 10 saniyede keepalive ping gönderir ve istemci keepalive ping 2 saniyede (üçte keepalive zaman aşımı değeri keepalive zaman aşımı uyarısı değeri arasındaki farkı) hakkında denetler.

    API taşıma bir bağlantısının kesilmesi uyumlu hale gelirse, keepalive uyarı zaman aşımını geçirmeden önce SignalR bağlantısının kesilmesi bilgilendirilmek. Bu durumda, `ConnectionSlow` değil olayı ve SignalR Git doğrudan `Reconnecting` olay.
- `Reconnecting`İstemci olay.

    (A) API taşıma bağlantısı kesildiği veya bu yana en son iletinin (b keepalive zaman aşımı süresi geçmiş veya keepalive ping alındı algıladığında oluşturulur. SignalR istemci kodu yeniden denemeye başlar. Taşıma bağlantısı kesildiğinde bazı işlemler yapması için uygulamanızın istiyorsanız bu olay işleyebilir. Varsayılan keepalive zaman aşımı süresi şu anda 20 saniye kullanılır.

    SignalR modu yeniden bağlanmayı olsa da bir Hub yöntemini çağırmak, istemci kodunuzun çalışırsa, SignalR komutu göndermeyi dener. Çoğu zaman, bu tür denemeleri başarısız olur, ancak bazı durumlarda bunlar başarılı olabilir. Sunucu tarafından gönderilen olaylar, sonsuza kadar çerçeve ve uzun yoklama taşıma için SignalR iki iletişim kanalı, iletileri göndermek için istemcinin kullandığı diğeri iletileri almak için kullandığı kullanır. Alma için kullanılan kalıcı olarak açık bir kanaldır, ve fiziksel bağlantısı kesildiğinde, kapalı bir. Fiziksel bağlantı geri yüklediyseniz, alma kanal yeniden kurulur kurulmaz önce yöntem çağrısı istemciden sunucuya başarılı olabilir kullanılabilir kalır göndermek için kullanılan kanalı. SignalR alma için kullanılan kanal yeniden açar kadar dönüş değeri alınmadı.
- `Reconnected`İstemci olay.

    Taşıma bağlantısı yeniden kurulduğunda oluşturulur. `OnReconnected` Olay işleyicisi hub yürütür.
- `Closed`İstemci olayı (`disconnected` JavaScript olayda).

    SignalR istemci kodu taşıma bağlantısını kaybettikten sonra yeniden yapmaya çalışırken bağlantı kesme zaman aşımı süresi sona erdiğinde oluşturuldu. Varsayılan bağlantı kesme zaman aşımı olan 30 saniye. (Çünkü bağlantı sona erdiğinde de bu olay tetiklenir `Stop` yöntemi çağrılır.)

Aktarım API tarafından algılanmayan ve keepalive ping keepalive uyarı zaman aşımını daha uzun süre sunucudan alınması beklemeyin aktarım bağlantı kesintileri herhangi bir bağlantı çıkarılmasına ömür olayları neden.

Bazı ağ ortamları kasıtlı olarak boşta kalma bağlantıları kapatın ve başka bir keepalive paketlerin bu bu ağlar bir SignalR bağlantısı kullanımda olduğunu biliyor veren tarafından önlemeye yardımcı olmak için işlevdir. Olağanüstü durumlarda keepalive ping varsayılan sıklığını kapalı bağlantıları önlemek için yeterli olmayabilir. Bu durumda daha sık gönderilmesini keepalive ping yapılandırabilirsiniz. Daha fazla bilgi için bkz: [zaman aşımı ve canlı tutma ayarlarını](#timeoutkeepalive) bu konuda daha sonra.

> [!NOTE] 
> 
> **Önemli**: Burada açıklanan olayların sırası garanti edilmez. SignalR bağlantısı ömür olayları bu düzene göre tahmin edilebilir bir şekilde yükseltmek için her girişiminde bulunur, ancak ağ olayları çeşitli türleri ve hangi aktarım API'leri gibi temel iletişimleri çerçeveleri bunları işlemek birçok yolu vardır. Örneğin, `Reconnected` istemci yeniden bağlandığında, olay oluşmayabilir veya `OnConnected` bağlantı girişimi başarısız olduğunda, sunucuda işleyici çalıştırabilirsiniz. Bu konu, normalde normal belirli koşullar tarafından üretilen etkilerini açıklar.


<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>İstemci bağlantısının kesilmesine senaryoları

Bir tarayıcı istemci bir SignalR bağlantısı tutar SignalR istemci kodu bir web sayfasında JavaScript bağlamında çalışır. Sahip birinden gittiğinizde sonlandırmak SignalR bağlantısı olan neden sayfasında üzere başka bir ve o kullanıcının neden varsa birden çok bağlantı kimlikleri ile birden çok bağlantı birden çok tarayıcı pencerelerini veya sekmeler bağlanın. Kullanıcı bir tarayıcı penceresi veya sekmesinde kapatır veya yeni bir sayfasına gider veya sayfa yenilendikten, siz ve çağrıları için SignalR istemci kodu tarayıcı olay işleme için SignalR bağlantısı hemen sona erer `Stop` yöntemi. Bu senaryolar veya uygulamanız çağırdığında tüm istemci platformu `Stop` yöntemi, `OnDisconnected` olay işleyicisi yürütür hemen sunucuda ve istemci başlatır `Closed` olay (olay adlı `disconnected` içinde JavaScript için).

Bir istemci uygulaması veya üzerinde çalıştığı bilgisayar kilitlenmesine veya (örneğin, kullanıcının dizüstü bilgisayar kapandığında) uyku moduna geçer, sunucunun hakkında ne olduğu bilgisi yok. Sunucu bilir kadar istemci kaybı bağlantı kesintisi nedeniyle olabilir ve istemci yeniden çalışıyor olabilir. Bu nedenle, sunucunun beklediği istemci yeniden bağlanmak için bir fırsat vermek için bu senaryolarda ve `OnDisconnected` (yaklaşık 30 saniye varsayılan olarak) bağlantı kesme zaman aşımı süresi sona erene kadar yürütmez. Aşağıdaki diyagramda, bu senaryo gösterilmektedir.

![İstemci bilgisayar hatası](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Sunucu bağlantı kesme senaryoları

Bir sunucu çevrimdışı--olduğunda yeniden başlatır, başarısız, uygulama etki alanı dönüştürür, vb.--nedeni bağlantı kesildi benzer olabilir veya SignalR ve aktarım API hemen sunucu kayboluyor ve SignalR başlamak olmadan yeniden bağlanmaya biliyor olabilirsiniz yükseltme `ConnectionSlow` olay. İstemci, istemci modu yeniden bağlanmayı içine kalırsa ve sunucuyu kurtarır veya bağlantı kesme zaman aşımı süresi sona ermeden önce yeniden başlatma veya yeni bir sunucu çevrimiçi duruma geri yüklenen veya yeni bir sunucuya yeniden bağlanır. Bu durumda, istemci üzerinde bir SignalR bağlantısı devam eder ve `Reconnected` olayı oluşturulur. İlk sunucuda `OnDisconnected` hiçbir zaman yürütülür ve yeni sunucudaki `OnReconnected` rağmen yürütülen `OnConnected` hiçbir zaman önce o sunucuda istemci için yürütüldü. (Aynı sunucu onu yeniden başlatıldığında olduğundan istemci bir yeniden başlatma veya uygulama etki alanı geri dönüşüm sonra aynı sunucuya bağlanırsa önceki bağlantı etkinlik belleğe sahip değil etkili olur.) Aşağıdaki diyagramda API taşıma hemen bağlantı kesildi uyumlu hale varsayar böylece `ConnectionSlow` olayı oluşmaz.

![Sunucu hatası ve yeniden bağlanma](handling-connection-lifetime-events/_static/image5.png)

Bir sunucu bağlantı kesme zaman aşımı süresi içinde kullanılabilir olmaz, SignalR bağlantıyı sonlandırır. Bu senaryoda, `Closed` olayı (`disconnected` JavaScript istemcilerinin) istemci üzerinde oluşturulur ancak `OnDisconnected` sunucu üzerinde hiçbir zaman çağrılır. Aşağıdaki diyagramda SignalR keepalive işlevsellik tarafından algılandığı şekilde API aktarım bağlantı kesildi, farkında olmaz olduğunu varsayar ve `ConnectionSlow` olayı oluşturulur.

![Sunucu hatası ve zaman aşımı](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Zaman aşımı ve canlı tutma ayarları

Varsayılan `ConnectionTimeout`, `DisconnectTimeout`, ve `KeepAlive` değerleri çoğu senaryosu için uygun olan ancak ortamınıza özel gereksinimleri varsa değiştirilebilir. Örneğin, ağ ortamınızı 5 saniye süreyle boşta olan bağlantıların kapatırsa, keepalive değerini azaltın gerekebilir.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Bu ayar, bir taşıma bağlantısı açık ve kapatıp yeni bir bağlantı açarak önce bir yanıtı beklenirken bırakın süreyi temsil eder. Varsayılan değer 110 saniyedir.

Bu ayar, yalnızca zaman keepalive işlevselliği, normalde geçerli olduğu yalnızca uzun süre devre dışı bırakıldı geçerlidir yoklama taşıması. Aşağıdaki diyagramda bir uzun üzerinde bu ayarın etkisi gösterilmektedir yoklama taşıma bağlantısı.

![Uzun yoklama taşıma bağlantısı](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Bu ayar tetiklenmeden önce taşıma bağlantısı kaybedildi sonra beklenecek süreyi temsil eden `Disconnected` olay. Varsayılan değer 30 saniyedir. Ayarladığınızda `DisconnectTimeout`, `KeepAlive` otomatik olarak 1/3'e ayarlanır `DisconnectTimeout` değeri.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Bu ayar boştaki bir bağlantı üzerinden canlı tutma paketi göndermeden önce beklenecek süreyi temsil eder. Varsayılan değer 10 saniyedir. Bu değer birden fazla 1/3 / olmamalıdır `DisconnectTimeout` değeri.

Her ikisi de ayarlamak istiyorsanız `DisconnectTimeout` ve `KeepAlive`ayarlayın `KeepAlive` sonra `DisconnectTimeout`. Aksi takdirde, `KeepAlive` ayarı olacaktır üzerine ne zaman `DisconnectTimeout` otomatik olarak ayarlar `KeepAlive` zaman aşımı değeri 1/3.

Keepalive işlevini devre dışı bırakmak istiyorsanız, `KeepAlive` null. KeepAlive işlevselliği otomatik olarak uzun süre devre dışı yoklama taşıması.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Zaman aşımı ve canlı tutma ayarlarını değiştirme

Bu ayarlar için varsayılan değerleri değiştirmek için bunları kümesinde `Application_Start` içinde *Global.asax* , aşağıdaki örnekte gösterildiği gibi dosya. Örnek kodda gösterilen değerleri varsayılan değerleri ile aynıdır.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Bağlantı kesilmeleri hakkında kullanıcıya bildirim nasıl

Bazı uygulamalarda bağlantısı sorunları olduğunda kullanıcıya bir ileti görüntülemek isteyebilirsiniz. Nasıl için çeşitli seçenekler ve bunu yapmak ne zaman var. Aşağıdaki kod örnekleri oluşturulan proxy kullanan bir JavaScript istemci için ' dir.

- Tanıtıcı `connectionSlow` olay modu yeniden bağlanmayı içine geçmeden önce SignalR, bağlantı sorunların farkında hemen bir ileti görüntüler.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Tanıtıcı `reconnecting` olay SignalR bağlantı kesme farkındadır ve modu yeniden bağlanmayı içine gittiği zaman bir ileti görüntüler.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- İşleme `disconnected` bir ileti yeniden bağlanma girişimi zaman görüntülemek için olay zaman aşımına uğradı. Bu senaryoda, sunucu ile bir bağlantı yeniden yeniden oluşturmak için tek yolu bir SignalR bağlantısı çağırarak yeniden başlatmaktır `Start` yeni bir bağlantı kimliği oluşturacak yöntemi Aşağıdaki kod örneği yalnızca yeniden bağlanan bir zaman aşımından sonra bildirim değil çağırarak neden SignalR bağlantısı normal sonuna sonra sorun emin olmak için bir bayrak kullanır `Stop` yöntemi.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Sürekli olarak yeniden bağlama

Bazı uygulamalarda otomatik olarak bağlantı kaybı oldu ve yeniden bağlanma girişimi zaman aşımına uğradı sonra yeniden oluşturmak isteyebilirsiniz. Bunu yapmak için çağırabilirsiniz `Start` yönteminden, `Closed` olay işleyicisi (`disconnected` olay işleyicisi JavaScript istemcilerde). Çağırmadan önce bir süre bekleyin isteyebilirsiniz `Start` çok Bunu önlemek için sık sunucu ya da fiziksel bağlantısı olduğunda kullanılamaz. Aşağıdaki kod örneği oluşturulan proxy kullanan bir JavaScript istemci için ' dir.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Mobil istemcilerin dikkat edilmesi gereken olası bir sorunu, sunucu ya da fiziksel bağlantı kullanılamadığında sürekli yeniden bağlanma denemesi gereksiz pil boşaltma neden olabilecek ' dir.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Bir istemci sunucu kodunda kesme hakkında

SignalR sürüm 2 istemcileri kesme için yerleşik bir sunucu API yok. Vardır [bu işlevselliği gelecekte ekleme planları](https://github.com/SignalR/SignalR/issues/2101). Geçerli SignalR sürümü, bir istemci sunucu bağlantısını kesmek için basit bir bağlantıyı kes yöntemi istemcide uygulamak ve sunucudan bu yöntemi çağırabilmeniz için yoldur. Aşağıdaki kod örneği, oluşturulan proxy kullanan bir JavaScript istemci için bir bağlantı kesme yöntemi gösterilir.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Güvenlik - ne istemcileri kesme için bu yöntem ne de önerilen yerleşik API adres kötü amaçlı kod istemcileri yeniden bağlanma veya kullanılan kod kaldırabilir beri çalıştırılan kullanılan istemcileri senaryo `stopClient` yöntemi veya değiştirme bunu yapar. Durum bilgisi olan hizmet reddi (DOS) koruması uygulamak için uygun framework veya sunucu katman değil, ön uç altyapısı yerdir.


<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Bağlantı kesme nedeni algılama

SignalR 2.1 sunucuya bir aşırı ekler `OnDisconnect` zaman aşımına uğramadan yerine istemci kasıtlı olarak bağlantısı olmadığını belirten bir olay. `StopCalled` Parametredir istemci açıkça bağlantı kapalı olduğunda true. İstemcinin bağlantısını kesmek için bir sunucu hatası neden olursa, JavaScript'te hata bilgilerini istemci olarak geçirilecektir `$.connection.hub.lastError`.

**C# sunucu kodu: `stopCalled` parametresi**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**JavaScript istemci kodu: erişme `lastError` içinde `disconnect` olay.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
