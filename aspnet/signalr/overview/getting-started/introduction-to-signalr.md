---
uid: signalr/overview/getting-started/introduction-to-signalr
title: "SignalR giriş | Microsoft Docs"
author: pfletcher
description: "Bu makalede, SignalR nedir ve oluşturmak için tasarlandığı çözümleri bazıları açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 64ac4e3575f69c164ba053943984ef25f906d7f4
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
<a name="introduction-to-signalr"></a>SignalR giriş
====================
tarafından [CAN Fletcher'dan](https://github.com/pfletcher)

> Bu makalede, SignalR nedir ve oluşturmak için tasarlandığı çözümleri bazıları açıklanmaktadır. 
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


## <a name="what-is-signalr"></a>SignalR nedir?

ASP.NET SignalR uygulamalarını gerçek zamanlı web işlevselliği ekleme işlemini basitleştiren bir ASP.NET geliştiricilerinin kitaplıktır. Gerçek zamanlı web, yeni veri istemek bir istemci için bekleme sunucusunun yerine sunucu kodu anında kullanılabilir hale geldiğinde bağlı içeriği istemcilere hemen göndermesini sağlayan işlevdir.

SignalR, ASP.NET uygulamanız için herhangi bir tür "gerçek zamanlı" web işlevselliği eklemek için kullanılabilir. Sohbet genellikle bir örnek olarak kullanılır, ancak tüm çok fazla yapabilirsiniz. Bir kullanıcının her zaman yeni verileri görmek için bir web sayfasını yeniler veya sayfasını uygulayan [uzun yoklama](http://en.wikipedia.org/wiki/Push_technology#Long_polling) yeni verileri almak için bunu bir SignalR kullanmak için adaydır. Örnekler panolar ve uygulamaların izleme, (eşzamanlı belgelerin düzenleme gibi), işbirliği uygulamaları iş ilerleme güncelleştirmeleri ve gerçek zamanlı forms.

SignalR ayrıca sunucusundan sık güncelleştirmelerini gerektiren web uygulamalarının tamamen yeni türleri sağlar Örneğin, gerçek zamanlı oyun.

SignalR İstemcisi'nde JavaScript işlevleri tarayıcılar (ve diğer istemci platformları) sunucu tarafı .NET kodundan çağıran sunucudan istemciye uzaktan yordam çağrısı (RPC) oluşturmak için basit bir API sağlar. SignalR bağlantı yönetimi için de API içerir (örneğin, bağlanma ve bağlantıyı kesme olayları) ve bağlantıları gruplandırma.

![SignalR ile yöntemleri çağırma](introduction-to-signalr/_static/image1.png)

SignalR bağlantı yönetimi otomatik olarak yönetir ve yayın iletilerini bağlanan tüm istemciler için aynı anda, sohbet odası gibi olanak tanır. Ayrıca, belirli istemcilere iletisi gönderebilir. İstemci ve sunucu arasındaki bağlantının, her iletişimi için yeniden kurulur kurulmaz klasik bir HTTP bağlantısı aksine kalıcıdır.

SignalR, sunucu kodu için istemci kodu uzaktan yordam çağrılarını (RPC), genel istek-yanıt modeli Web'de bugün yerine tarayıcıda çağırabilirsiniz "sunucu itme" işlevlerini destekler.

SignalR uygulamalarını ölçek genişletme için hizmet veri yolu, SQL Server kullanan istemciler binlerce veya [Redis](http://redis.io).

SignalR açık kaynaklı, üzerinden erişilebilir [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR ve WebSocket

SignalR yeni WebSocket aktarımı kullanılabiliyorsa kullanır ve gerektiğinde daha eski taşımalarına geri döner. Uygulamanızı WebSocket kullanarak doğrudan, uygulamanız gereken fazladan işlevsellik çok zaten olacak SignalR yollardan kesinlikle yazabilirsiniz sırada sizin için yapılmış. En önemlisi, bu eski istemcileri için ayrı kod yolu oluşturma hakkında endişelenmeye gerek kalmadan WebSocket yararlanmak için uygulamanızı kodlayabilirsiniz anlamına gelir. SignalR Ayrıca, SignalR temel taşımasına değişiklikleri desteklemek üzere güncelleştirilecek uygulamanızı WebSocket sürümleri arasında tutarlı bir arabirim sağlayan devam edecek beri WebSocket, güncelleştirmeler hakkında endişelenmeniz zorunda kalmaktan ayrıntılarından korur.

SignalR WebSocket tek başına kullanarak bir çözüm kesinlikle oluşturabilirsiniz, ancak tüm diğer aktarım ve güncelleştirmeleri uygulamanızı WebSocket uygulamalarına düzeltilmesi için geri dönüş gibi kendiniz yazmak için gereken işlevleri sağlar.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Taşımalar ve geri dönüşler

SignalR için bir Özet bazı istemci ve sunucu arasında gerçek zamanlı iş yapmak için gerekli olan taşımalar sona erer. Bir SignalR bağlantısı olarak HTTP başlatır ve kullanılabilir durumdaysa WebSocket bağlantı sonra yükseltilir. WebSocket ideal taşıma için SignalR, sunucu belleği en verimli kullanılmasını sağlar, en düşük gecikme süresine sahiptir ve (örneğin, istemci ve sunucu arasındaki tam çift yönlü iletişimi için) en temel özelliklere sahiptir, ancak ayrıca en sıkı sahip olduğu gereksinimleri: WebSocket server'ın Windows Server 2012 veya Windows 8 ve .NET Framework 4.5 kullanılmasını gerektirir. Bu gereksinimler karşılanmazsa, SignalR bağlantıları yapmak için diğer taşımaları kullanmayı dener.

### <a name="html-5-transports"></a>HTML 5 taşımaları

Bu taşıma için destek bağlıdır [HTML 5](http://en.wikipedia.org/wiki/HTML5). İstemci tarayıcısı HTML 5 standart desteklemiyorsa, eski taşımaları kullanılır.

- **WebSocket** (varsa bunlar Websocket destekleyebilmesi hem sunucu hem de tarayıcı gösterir). WebSocket istemci ve sunucu arasında doğru bir kalıcı, çift yönlü bağlantı kurar yalnızca Aktarım ' dir. Bununla birlikte, WebSocket en sıkı gereksinimleri vardır; yalnızca en son sürümlerde ve Microsoft Internet Explorer, Google Chrome, Mozilla Firefox tam olarak desteklenir ve yalnızca Opera ve Safari gibi diğer tarayıcılarda kısmi uygulama vardır.
- **Sunucu gönderilen olayları**, (tarayıcı sunucu gönderilen Internet Explorer dışında tüm tarayıcılar temelde olan olaylarını destekliyorsa.) EventSource olarak da bilinir

### <a name="comet-transports"></a>Comet taşımalar

Aşağıdaki taşımalar dayalı [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) , bir tarayıcı veya diğer istemci tutar sunucu özellikle veri istemcisi olmadan istemciye göndermek için kullanabileceğiniz bir uzun tutulan HTTP isteği, web uygulama modeli Bunu isteme.

- **Devamlı çerçeve** (için yalnızca Internet Explorer). Devamlı çerçeve tamamlanmayan sunucudaki bir uç nokta için bir istek yapan gizli bir IFrame oluşturur. Sunucu sonra sürekli betik hemen çalıştırılır, istemcinin sunucudan istemciye bir tek yönlü gerçek zamanlı bağlantı sağlama gönderir. Sunucu ile istemci arasındaki bağlantı sunucusundan ayrı bir bağlantı istemci bağlantısı için kullanır ve gibi standart bir HTTP isteği, her bir gönderilmesi gereken veri parçası için yeni bir bağlantı oluşturulur.
- **AJAX yoklama uzun**. Uzun yoklama kalıcı bir bağlantı oluşturmaz, ancak bunun yerine sunucu, bu noktada bağlantıyı kapatır yanıt verir ve yeni bir bağlantı hemen istenen kadar açık kalır bir istekle sunucusunu yoklar. Bağlantıyı sıfırlar sırada bu bazı gecikme çıkarabilir.

Hangi aktarımları altında hangi yapılandırmaları desteklenir daha fazla bilgi için bkz: [desteklenen platformlar](supported-platforms.md).

### <a name="transport-selection-process"></a>Taşıma seçimi işlemi

Aşağıdaki listede, hangi aktarım kullanılacak karar vermek için SignalR kullanır adımları gösterir.

1. Tarayıcı Internet Explorer 8 veya daha eski ise, uzun yoklama kullanılır.
2. JSONP yapılandırılmışsa (diğer bir deyişle, `jsonp` parametrenin ayarlanmış `true` bağlantısı başlatıldığında), uzun yoklama kullanılır.
3. (Diğer bir deyişle, SignalR uç nokta barındırma sayfası aynı etki alanında değilse), etki alanları arası bağlantı yapılıyor, WebSocket aşağıdaki ölçütler karşılanıyorsa kullanılacak:

    - İstemci, CORS (çıkış noktaları arası kaynak paylaşımı) destekler. İstemciler üzerinde CORS desteği Ayrıntılar için bkz [CORS caniuse.com adresindeki](http://www.caniuse.com/CORS).
    - İstemci, WebSocket destekler.
    - WebSocket sunucu destekler

    Bu ölçütler hiçbirini karşılanmazsa uzun yoklama kullanılır. Etki alanları arası bağlantılar hakkında daha fazla bilgi için bkz: [etki alanları arası bağlantı kurmak nasıl](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. İstemci ve sunucu destekliyorsa JSONP yapılandırılmamış ve bağlantı etki alanları arası değil, WebSocket kullanılır.
5. İstemci veya sunucu WebSocket desteği yoksa sunucu gönderilen olayları varsa kullanılır.
6. Sunucu gönderilen olaylar kullanılabilir durumda değilse, sonsuza kadar çerçeve denenir.
7. Devamlı çerçeve başarısız olursa, uzun yoklama kullanılır.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Taşımalar izleme

Uygulamanız, hub'ına günlüğe kaydetme ve tarayıcınızda konsol penceresini açma etkinleştirerek kullanarak hangi aktarım belirleyebilirsiniz.

Bir tarayıcıda, hub'ın olayları günlüğe kaydetmeyi etkinleştirmek için istemci uygulamanız için aşağıdaki komutu ekleyin:

`$.connection.hub.logging = true;`

- Internet Explorer'da, geliştirici araçları F12 tuşuna basarak açın ve konsol sekmesini tıklatın.

    ![Console in Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- Chrome'Ctrl + Shift + J tuşlarına basarak konsolunu açın.

    ![Google Chrome konsolunda](introduction-to-signalr/_static/image3.png)

Konsolunu açın ve etkin günlük kaydı ile hangi aktarım SignalR tarafından kullanılan görmeye olacaktır.

![Internet Explorer WebSocket taşıma gösteren konsol](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Bir taşıma belirtme

Bir taşıma anlaşmak belirli miktarda süre ve istemci/sunucu kaynakları alır. İstemci yeteneklerini biliniyorsa, bir taşıma istemci bağlantısı başlatıldığında belirtilebilir. Aşağıdaki kod parçacığını istemci herhangi bir protokol desteklememektedir biliniyordu varsa kullanılacak şekilde Ajax uzun yoklama taşıma kullanarak bağlantı başlatma gösterir:

`connection.start({ transport: 'longPolling' });`

Sırayla belirli aktarımların denemek için bir istemci istiyorsanız, bir geri dönüş sırasını belirleyebilirsiniz. Aşağıdaki kod parçacığını çalışırken WebSocket ve, başarısız olan, uzun yoklama doğrudan giden gösterilir.

`connection.start({ transport: ['webSockets','longPolling'] });`

Dize sabitleri taşımaları belirtmek için şu şekilde tanımlanır:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Bağlantıları ve hub'ları

SignalR API istemciler ve sunucular iletişim için iki modeli içerir: kalıcı bağlantılar ve hub.

Bir bağlantı gönderme tek alıcı, gruplandırılmış veya yayın iletileri için basit bir uç noktasını temsil eder. Geliştirici doğrudan erişim SignalR sunan alt düzey iletişim protokolü için kalıcı bağlantı (.NET kodda PersistentConnection sınıfı tarafından gösterilen) API sağlar. Bağlantıları iletişim modelini kullanarak Windows Communication Foundation gibi bağlantı tabanlı API'ler kullanmış olan geliştiricilere tanıdık gelecektir.

Bir Hub bağlantı yöntemleri birbirine doğrudan çağırmak için istemci ve sunucu veren bir API inşa daha üst düzey bir işlem hattı ' dir. SignalR tarafından magic gibi makine sınırlarında göndermeyi kolayca yerel yöntemleri ve tersi olarak sunucuda yöntemlerini çağırmaya istemcilere izin verme işler. Hub iletişim modelini kullanarak uzaktan çağırma .NET uzaktan iletişim gibi API'leri kullanmış olan geliştiricilere tanıdık gelecektir. Bir hub'ı kullanarak, model bağlama etkinleştirme yöntemleri için kesin türü belirtilmiş parametreleri geçirmek de sağlar.

### <a name="architecture-diagram"></a>mimarisi diyagramı

Aşağıdaki diyagramda hub, kalıcı bağlantılar ve taşımaları için kullanılan temel teknolojileri arasındaki ilişkiyi gösterir.

![SignalR mimarisi API'leri, aktarımları ve istemcilerin gösteren diyagram](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Hub nasıl çalışır

Sunucu tarafı kodu istemcide bir yöntem çağırdığında çağrılacak yöntem parametreleri ve adını içeren etkin taşımanın gönderilen bir paket (bir nesne bir yöntem parametresi olarak gönderildiğinde, bunu kullanarak serileştirilir). İstemci ardından istemci-tarafı kodda tanımlanan yöntemler için yöntem adı ile eşleşir. Bir eşleşme varsa, istemci yöntemi kullanılarak seri durumdan çıkarılmış parametre veri yürütülür.

Yöntem çağrısının gibi araçları kullanarak izlenebilir [Fiddler.](http://fiddler2.com/) Aşağıdaki resimde bir SignalR sunucusundan Fiddler günlükleri bölmesinde web tarayıcısı istemciye gönderilen bir yöntem çağrısı gösterir. Yöntem çağrısının adlı bir hub'dan gönderilen `MoveShapeHub`, çağrılmakta olan yöntemin adı verilen ve `updateShape`.

![SignalR trafiği gösteren Fiddler günlük görünümü](introduction-to-signalr/_static/image6.png)

Bu örnekte, hub'ı adı ile tanımlanır `H` parametresi; yöntemi adı ile tanımlanır `M` parametre ve yöntemine gönderilen verileri ile tanımlanan `A` parametresi. Bu ileti oluşturulan uygulama oluşturulan [yüksek sıklıkta gerçek zamanlı](tutorial-high-frequency-realtime-with-signalr.md) Öğreticisi.

### <a name="choosing-a-communication-model"></a>Bir iletişim modelini seçme

Çoğu uygulama hub API kullanmanız gerekir. Bağlantıları API aşağıdaki durumlarda kullanılabilir:

- Gönderilen gerçek ileti belirtilmesi gerekiyor biçimi.
- Geliştirici uzaktan çağırma modeli yerine bir Mesajlaşma ve dağıtma modeli ile çalışmak tercih eder.
- SignalR kullanmak için bir ileti modeli kullanan bir var olan uygulamayı verilen.
