---
title: ASP.NET Core WebSockets desteği
author: rick-anderson
description: ASP.NET Core WebSockets kullanmaya başlayacağınızı öğrenin.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: ede8064b5e77024b843357d4715869b3495b9147
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153689"
---
# <a name="websockets-support-in-aspnet-core"></a>ASP.NET Core WebSockets desteği

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Barış Stanton-Nurse](https://github.com/anurse)

Bu makalede, ASP.NET Core WebSockets kullanmaya başlama açıklanmaktadır. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)), TCP bağlantıları üzerinden kalıcı iki yönlü iletişim kanalları sağlayan bir protokoldür. Sohbet, Pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişim yararlı uygulamalarda kullanılır.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)). Bkz: [sonraki adımlar](#next-steps) daha fazla bilgi için bölüm.

## <a name="prerequisites"></a>Önkoşullar

* ASP.NET Core 1.1 veya üstü
* ASP.NET Core destekleyen herhangi bir işletim sistemi:
  
  * Windows 7 / Windows Server 2008 veya üzeri
  * Linux
  * MacOS
  
* Uygulama IIS ile Windows üzerinde çalışıyorsa:

  * Windows 8 / Windows Server 2012 veya üzeri
  * IIS 8 / 8 IIS Express
  * WebSockets, IIS'de etkin olması gerekir (bkz [IIS/IIS Express Destek](#iisiis-express-support) bölüm.)
  
* Uygulama çalışıyorsa [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8 / Windows Server 2012 veya üzeri

* Desteklenen tarayıcılar için bkz: https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>WebSockets kullanma zamanı

WebSockets, doğrudan bir yuva bağlantısı ile çalışmak için kullanın. Örneğin, gerçek zamanlı oyun ile olası en iyi performansı için WebSockets kullanın.

[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) daha zengin bir uygulama modeli gerçek zamanlı işlevselliği sağlar, ancak bunun için ASP.NET yalnızca çalışır 4.x, ASP.NET Core. SignalR ASP.NET Core sürümü, ASP.NET Core 2.1 sürümüyle için zamanlandı. Bkz: [ASP.NET Core 2.1 üst düzey planlama](https://github.com/aspnet/Announcements/issues/288).

SignalR Core serbest kadar WebSockets kullanılabilir. Ancak, SignalR sağladığı özellikleri sağlanan ve gerekir geliştirici tarafından desteklenir. Örneğin:

* Geniş bir alternatif taşıma yöntemleri için otomatik geri dönüş kullanarak tarayıcı sürümleri için destek.
* Bir bağlantı düştüğünde otomatik yeniden bağlanma.
* Sunucuda veya yöntemleri çağırma istemciler için destek.
* Birden çok sunucuya ölçeklendirme desteği.

## <a name="how-to-use-it"></a>Nasıl kullanılacağını

* Yükleme [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paket.
* Ara yazılımını yapılandırın.
* WebSocket istekleri kabul edin.
* İleti gönderme ve alma.

### <a name="configure-the-middleware"></a>Ara yazılımını yapılandırma

WebSockets Ara yazılımında eklemek `Configure` yöntemi `Startup` sınıfı:

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Aşağıdaki ayarlar yapılandırılabilir:

* `KeepAliveInterval` -Sık "ping" çerçevelerine proxy'leri emin olmak için istemci bağlantıyı açık tutmak göndermek nasıl.
* `ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu. İleri düzey kullanıcılar, bu veri boyutuna göre performans ayarlaması için değiştirmeniz gerekebilir.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>WebSocket istekleri kabul

İstek yaşam döngüsü devamındaki herhangi bir yerde (daha sonra `Configure` yöntemi veya bir MVC eylem, örneğin) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul et.

Aşağıdaki örnek alanından daha sonra kullanımda `Configure` yöntemi:

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Web yuvası isteğini üzerinde herhangi bir URL adresi gelebilir, ancak bu örnek kod yalnızca isteklerini kabul `/ws`.

### <a name="send-and-receive-messages"></a>İleti gönderme ve alma

`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantı yükseltir ve sağlayan bir [WebSocket](/dotnet/core/api/system.net.websockets.websocket) nesnesi. Kullanım `WebSocket` ileti gönderme ve alma için nesne.

Web yuvası isteğini kabul eden daha önce gösterilen kod iletir `WebSocket` nesnesine bir `Echo` yöntemi. Kod bir ileti alır ve hemen aynı iletiyi gönderir. Gönderilen ve istemci bağlantı kapatana kadar bir döngüde alınan ileti:

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Döngü başlamadan önce WebSocket bağlantısı kabul ederek, ara yazılım ardışık düzenini sona erer. Yuva kapatma sırasında ardışık düzen unwinds. Diğer bir deyişle, istek ardışık düzeninde WebSocket kabul edildiğinde ilerleyen durdurur. Döngü tamamlandı ve yuvanın kapalı olduğunda, isteği yeniden ardışık düzen devam eder.

## <a name="iisiis-express-support"></a>IIS/IIS Express desteği

Windows Server 2012 veya üzeri ve Windows 8 veya üzeri ile IIS/IIS Express 8 veya üzeri WebSocket protokolü için desteği vardır.

Windows Server 2012 veya sonraki sürümlerde WebSocket protokolü için desteği etkinleştirmek için:

1. Kullanım **rol ve Özellik Ekle** gelen Sihirbazı **Yönet** menüsü veya bağlantıyı **Sunucu Yöneticisi'ni**.
1. Seçin **rol tabanlı veya özellik tabanlı yükleme**. Seçin **sonraki**.
1. (Yerel sunucu varsayılan olarak seçilidir) uygun sunucuyu seçin. Seçin **sonraki**.
1. Genişletme **Web sunucusu (IIS)** içinde **rolleri** ağaç, genişletin **Web sunucusu**, genişletin ve ardından **uygulama geliştirme**.
1. Seçin **WebSocket Protokolü**. Seçin **sonraki**.
1. Ek özellikleri olmayan gerekirse seçin **sonraki**.
1. **Yükle**'yi seçin.
1. Yükleme tamamlandığında seçin **Kapat** sihirbazdan çıkmak için.

Windows 8 veya sonraki sürümlerde WebSocket protokolü için desteği etkinleştirmek için:

1. Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).
1. Aşağıdaki düğümler açın: **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.
1. Seçin **WebSocket Protokolü** özelliği. Seçin **Tamam**.

**WebSocket Socket.IO üzerinde node.js kullanırken devre dışı bırak**

WebSocket desteği kullanıyorsanız [Socket.IO](https://socket.io/) üzerinde [Node.js](https://nodejs.org/), varsayılan IIS WebSocket modülü kullanılarak devre dışı bırakma `webSocket` öğesinde *web.config* veya *applicationHost.config*. Bu adımda gerçekleştirilen değil WebSocket iletişim yerine Node.js ve uygulamayı işlemek IIS WebSocket modülü çalışır.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Sonraki adımlar

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) bu eşlik makaledir Yankı uygulama. WebSocket bağlantılar sağlayan bir web sayfasına sahip ve sunucunun aldığı iletileri istemciye yeniden gönderir. (Bunu ayarlanmamış yukarı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden uygulamayı çalıştırın ve gidin http://localhost:5000. Web sayfasının sol üst bağlantı durumunu gösterir:

![Web sayfasının ilk durumu](websockets/_static/start.png)

Seçin **Bağlan** gösterilen URL'yi bir Web yuvası isteğini göndermek için. Sınama iletisi girin ve **Gönder**. İşiniz bittiğinde, seçin **Kapat yuva**. **İletişim günlük** bölüm raporları her açık, gönderme ve kapatma eylemi olarak gerçekleşir.

![Web sayfasının ilk durumu](websockets/_static/end.png)
