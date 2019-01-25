---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Signalr'a giriş | Microsoft Docs
author: bradygaster
description: Bu makalede, SignalR nedir ve bazı çözümler oluşturmak için tasarlandığı açıklanır.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 7b9dae3e5d79319a9fefee41f4525a59f950746a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837773"
---
<a name="introduction-to-signalr"></a>SignalR’a Giriş
====================

tarafından [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]


> Bu makalede, SignalR nedir ve bazı çözümler oluşturmak için tasarlandığı açıklanır. 
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>SignalR nedir?

ASP.NET SignalR uygulamalarına gerçek zamanlı web işlevselliği ekleme işlemini basitleştiren bir kitaplık ASP.NET geliştiricileri için ' dir. Gerçek zamanlı web işlevselliği yeni veri istemek bir istemci için bekleyin sunucusuna sahip olmak yerine, kullanılabilir olduğu anda bağlı istemcilere içerik sunucusu kod gönderimi sağlamak için yeteneğidir.

SignalR, ASP.NET uygulamanızı herhangi bir tür "gerçek zamanlı" web işlevselliği eklemek için kullanılabilir. Sohbet, genellikle bir örnek olarak kullanılır, ancak çok daha fazlasını yapabilirsiniz. Bir kullanıcının dilediğiniz zaman yeni verileri görmek için bir web sayfasını yeniler veya sayfa uygulayan [uzun yoklama](http://en.wikipedia.org/wiki/Push_technology#Long_polling) yeni verileri almak için bir aday SignalR için kullanmaktır. Örnekler, panolar ve uygulamaların izlenmesi, işbirliğine dayalı uygulamalar (örneğin, aynı anda belgelerin düzenleme), iş devam eden güncelleştirmelerin ve gerçek zamanlı form.

SignalR, tamamen yeni sunucusundan yüksek sıklıkta güncelleştirme yapılması gereken web uygulaması türlerini de sağlar, gerçek zamanlı oyun.

SignalR İstemcisi'nde JavaScript işlevleri tarayıcılar (ve diğer istemci platformları) sunucu tarafı .NET kodundan çağıran sunucu istemciye uzaktan yordam çağrılarını (RPC) oluşturmak için basit bir API sağlar. SignalR bağlantı yönetimi için de API içerir (örneğin, bağlayın ve bağlantıyı kesme olayları) ve bağlantıları gruplandırma.

![SignalR ile yöntemlerini çağırma](introduction-to-signalr/_static/image1.png)

SignalR bağlantı yönetimi otomatik olarak işler ve bağlanan tüm istemciler için yayın iletilerini eşzamanlı olarak gibi bir sohbet odası olanak tanır. Ayrıca, belirli istemciler için iletileri gönderebilir. İstemci ve sunucu arasındaki bağlantıyı, her iletişim için yeniden kurulur klasik bir HTTP bağlantısı aksine kalıcıdır.

SignalR, sunucu kodu tarayıcıyı ortak istek-yanıt modeli yerine uzak yordam çağrılarını (RPC) Bugün Web'de istemci koduna çağırabilirsiniz "sunucu gönderimi" işlevlerini destekler.

SignalR uygulamalarına ölçeğini binlerce istemciden Service Bus, SQL Server'ı kullanarak için veya [Redis](http://redis.io).

SignalR, açık kaynaklı, erişilebilir [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR ve WebSocket

SignalR, mevcut olduğunda yeni WebSocket taşıma kullanır ve gerektiğinde daha eski taşımalarına geri döner. Olsa da kesinlikle WebSocket kullanarak doğrudan, uygulamanız gereken fazladan işlevsellik birçok zaten sizin yerinize yapılır SignalR yol kullanarak uygulamanızı yazabilirsiniz. En önemlisi, bu eski istemciler için ayrı bir kod yolu oluşturma hakkında endişelenmenize gerek kalmadan WebSocket yararlanmak için uygulamanızın kod anlamına gelir. SignalR Ayrıca, SignalR değişiklikler temel alınan aktarımda içinde desteklemek üzere uygulamanızı WebSocket sürümleri arasında tutarlı bir arabirim sağlayan güncelleştirilse WebSocket, güncelleştirmeleri hakkında endişelenmenize gerek kalmamasını ayrıntılarından korur.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Aktarım ve geri dönüşler

SignalR, bir Özet, bazı istemci ve sunucu arasında gerçek zamanlı iş yapmak için gerekli olan taşımalar sona erer. Bir SignalR bağlantısı HTTP başlatır ve sonra WebSocket bağlantısı varsa yükseltilir. WebSocket ideal taşıma için SignalR, sunucu bellek kullanımını en verimli hale getirir, en düşük gecikme süresine sahip ve (örneğin, istemci ve sunucu arasında tam çift yönlü iletişimi için) en temel özelliklere sahip, ancak ayrıca en katı sahip olduğu Gereksinimler: WebSocket sunucu Windows Server 2012 veya Windows 8 ve .NET Framework 4.5 kullanılmasını gerektirir. Bu gereksinimler karşılanmazsa, SignalR bağlantılarından olmak için diğer aktarımlar'ı kullanmayı dener.

### <a name="html-5-transports"></a>HTML 5 taşır

Desteği bu taşımalar bağımlı [HTML 5](http://en.wikipedia.org/wiki/HTML5). İstemci tarayıcısı HTML 5 standart desteklemiyorsa, eski aktarımları kullanılır.

- **WebSocket** (varsa bunlar Websocket desteği hem sunucu hem de tarayıcı gösterir). WebSocket istemci ve sunucu arasında doğru kalıcı, çift yönlü bağlantı kuran yalnızca Aktarım ' dir. Ancak, WebSocket Ayrıca, en katı gereksinimleri vardır; yalnızca en son sürümlerinde Microsoft Internet Explorer, Google Chrome ve Mozilla Firefox tam olarak desteklenir ve yalnızca kısmi bir uygulamasını Opera ve Safari gibi diğer tarayıcılar vardır.
- **Sunucu tarafından gönderilen olaylarla**EventSource (tarayıcı sunucu gönderilen Internet Explorer dışındaki tüm tarayıcılar temel olan olayları destekler. varsa) olarak da bilinen

### <a name="comet-transports"></a>Comet taşımalar

Aşağıdaki taşımalar dayalı [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) içinde bir tarayıcı veya diğer istemci tutar sunucu özellikle veri istemcisi olmadan bir istemciye göndermek için kullanabileceğiniz bir uzun tutulan HTTP isteği, web uygulama modeli Bunu isteniyor.

- **Sonsuza kadar çerçeve** (için yalnızca Internet Explorer). Sonsuza kadar çerçeve tamamlandığı sunucuda bir uç noktaya bir istek getiren gizli bir IFrame oluşturur. Sunucusu sürekli olarak betik sağlayan bir tek yönlü gerçek zamanlı bağlantı sunucudan istemciye hangi hemen yürütüldüğünde, istemciye gönderir. İstemciden sunucuya bağlantı istemci bağlantısı için sunucudan ayrı bir bağlantı kullanır ve benzer standart bir HTTP isteği, her bir gönderilmesi gereken veri parçası için yeni bir bağlantı oluşturulur.
- **AJAX uzun yoklama**. Uzun yoklama kalıcı bir bağlantı oluşturmaz, ancak bunun yerine sunucu, bu noktada bağlantıyı kapatır yanıt verir ve yeni bir bağlantı hemen istenen kadar açık kalır. bir istek sunucusuyla yoklar. Bağlantıyı sıfırlar sırada bu bazı gecikmelere neden olabilir.

Hangi aktarımları altında hangi yapılandırmaları desteklenir daha fazla bilgi için bkz: [desteklenen platformlar](supported-platforms.md).

### <a name="transport-selection-process"></a>Aktarım seçim işlemi

Aşağıdaki liste, hangi aktarım kullanılacak karar vermek için SignalR kullanan adımları gösterir.

1. Internet Explorer 8 veya önceki tarayıcı ise uzun yoklama kullanılır.
2. JSONP yapılandırılmışsa (diğer bir deyişle, `jsonp` parametrenin ayarlanmış `true` bağlantısı başlatıldığında), uzun yoklama kullanılır.
3. (Diğer bir deyişle, SignalR uç nokta barındırma sayfası aynı etki alanında değilse), etki alanları arası bağlantısı yapılıyor, WebSocket aşağıdaki ölçütler karşılandığında kullanılacaktır:

   - İstemci, CORS (çıkış noktaları arası kaynak paylaşımı) destekler. İstemciler üzerinde CORS desteği için bilgi [caniuse.com, CORS](http://www.caniuse.com/CORS).
   - İstemci, WebSocket destekler.
   - WebSocket sunucu destekler

     Aşağıdaki ölçütleri karşılanmazsa uzun yoklama kullanılır. Etki alanları arası bağlantılar hakkında daha fazla bilgi için bkz. [etki alanları arası bağlantı kurmak nasıl](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. İstemci ve sunucu destekliyorsa JSONP yapılandırılmamış ve etki alanları arası bağlantı değil, WebSocket kullanılır.
5. İstemci veya sunucu WebSocket desteklemiyorsa, sunucu tarafından gönderilen olaylarla varsa kullanılır.
6. Sunucu gönderilen olayları kullanılabilir durumda değilse, sonsuza kadar çerçeve denenir.
7. Sonsuza kadar çerçeve başarısız olursa, uzun yoklama kullanılır.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Taşımalar izleme

Hub'ınızın günlüğe kaydetme ve tarayıcınızda konsol penceresini açma olanak sağlayarak uygulamanızın kullanarak hangi aktarım belirleyebilirsiniz.

Bir tarayıcıda, hub'ın olayları günlüğe kaydetmeyi etkinleştirmek için istemci uygulamanız için aşağıdaki komutu ekleyin:

`$.connection.hub.logging = true;`

- Internet Explorer'da, F12 tuşuna basarak geliştirici araçlarını açın ve konsolu sekmesine tıklayın.

    ![Microsoft Internet Explorer konsolunda](introduction-to-signalr/_static/image2.png)
- Chrome'da, Ctrl + Shift + J tuşlarına basarak konsolunu açın.

    ![Google Chrome konsolunda](introduction-to-signalr/_static/image3.png)

Konsolunu açın ve günlüğe kaydetme etkin hangi aktarım SignalR tarafından kullanıldığını görmek mümkün olacaktır.

![Internet Explorer WebSocket aktarım gösteren konsol](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Bir taşıma belirtme

Bir taşıma anlaşması belirli miktarda zaman ve istemci/sunucu kaynakları alır. İstemci yeteneklerini biliniyorsa, aktarım istemci bağlantısı başlatıldığında belirtilebilir. Aşağıdaki kod parçacığı, istemci herhangi bir protokolünü desteklemiyor biliniyordu, kullanılan Ajax uzun yoklama taşıma kullanarak bağlantı başlatma göstermektedir:

`connection.start({ transport: 'longPolling' });`

Belirli aktarımların sırayla denemek için bir istemci istiyorsanız, bir geri dönüş düzeni belirtebilirsiniz. Aşağıdaki kod parçacığı çalışırken WebSocket ve başarısız, uzun yoklama doğrudan giderek gösterir.

`connection.start({ transport: ['webSockets','longPolling'] });`

Dize sabitleri taşımalar belirtmek için şu şekilde tanımlanır:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Bağlantıları ve hub'ları

SignalR API, istemciler ve sunucular arasında iletişim kurmak için iki modeli içerir: Kalıcı bağlantılarını ve hub.

Bir bağlantı, tek alıcı, gruplandırılmış veya yayın ileti gönderme için basit bir uç noktasını temsil eder. Geliştirici doğrudan erişim SignalR sunan alt düzey iletişim protokolü için kalıcı bağlantı (.NET kodda PersistentConnection sınıfı tarafından temsil edilen) API sağlar. Bağlantı iletişim modelini kullanarak Windows Communication Foundation gibi bağlantı tabanlı API'leri kullanan geliştiriciler için tanıdık gelecektir.

Bir hub'ı, istemci ve sunucunun doğrudan birbirleri üzerinde yöntemleri çağırmak verir bağlantı API üzerinde derlenmiş daha üst düzey bir işlem hattı ' dir. SignalR tarafından magic gibi makine sınırlarında gönderme istemcilerinin yerel yöntemler olarak kolayca ve tersi olarak, sunucu üzerinde yöntemleri çağırmak işler. Hub'ları iletişim modelini kullanarak uzaktan çağırma .NET uzaktan iletişim gibi API'leri kullanan geliştiriciler için tanıdık gelecektir. Bir hub'ı kullanarak, model bağlama etkinleştirme yöntemleri için türü kesin belirlenmiş parametreleri geçirmek de sağlar.

### <a name="architecture-diagram"></a>Mimari diyagramı

Aşağıdaki diyagramda, hub'ları, kalıcı bağlantılar ve taşımalar için kullanılan temel teknolojileri arasındaki ilişkiyi gösterir.

![SignalR mimarisi API'leri, aktarımları ve istemcilerin gösteren diyagram](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Hub nasıl çalışır

Sunucu tarafı kodu istemcide bir yöntemi çağırdığında, çağrılacak yöntem parametreleri ve adını içeren etkin aktarım arasında gönderilen bir paket (bir nesne bir yöntem parametresi olarak gönderildiğinde, onu kullanarak JSON serileştirilmiş). İstemci, istemci tarafı kod içinde tanımlanan yöntemler için yöntem adını ardından eşleşir. Bir eşleşme varsa, istemci yöntemi kullanılarak seri durumdan çıkarılmış parametre veri yürütülür.

Yöntem çağrısının gibi araçları kullanarak izlenebilir [fiddler'ı.](http://fiddler2.com/) Aşağıdaki resimde, bir web tarayıcı istemcisine Fiddler günlükleri bölmesinde bir SignalR sunucusundan onlara gönderilen bir yöntem çağrısının gösterir. Yöntem çağrısının adlı bir hub'ından gönderilen `MoveShapeHub`, ve çağrılmakta olan yöntemin çağrıldığı `updateShape`.

![SignalR trafiği gösteren Fiddler günlük görünümü](introduction-to-signalr/_static/image6.png)

Bu örnekte, hub'ı adı ile tanımlanır `H` parametre; yöntem adı ile tanımlanan `M` parametresi ve yönteme gönderilen tüm veriler ile tanımlanan `A` parametresi. Bu iletiyi oluşturan uygulama oluşturulur [yüksek sıklıkta gerçek zamanlı](tutorial-high-frequency-realtime-with-signalr.md) öğretici.

### <a name="choosing-a-communication-model"></a>Bir iletişim modelini seçme

Çoğu uygulama, hub'ları API kullanmanız gerekir. Aşağıdaki durumlarda bağlantıları API kullanılabilir:

- Biçim, gönderilen gerçek ileti belirtilmesi gerekiyor.
- Geliştirici, bir uzak çağrı modeli yerine bir Mesajlaşma ve dispatching modeli ile çalışmak tercih eder.
- SignalR kullanmak için bir Mesajlaşma modeli kullanan mevcut bir uygulama Taşınmakta.
