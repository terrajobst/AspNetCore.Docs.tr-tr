---
title: "ASP.NET Core WebSockets desteği"
author: tdykstra
description: "ASP.NET Core WebSockets kullanmaya başlayacağınızı öğrenin."
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 6f335376c72cd0c68f4667cf0e661a25bf448980
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a>ASP.NET Core WebSockets giriş

Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Barış Stanton-Nurse](https://github.com/anurse)

Bu makalede, ASP.NET Core WebSockets kullanmaya başlama açıklanmaktadır. [WebSocket](https://wikipedia.org/wiki/WebSocket) kalıcı iki yönlü iletişim kanalları üzerinden TCP bağlantıları sağlayan bir protokoldür. Sohbet, yürütebilmektedir gibi uygulamalar için kullanılır, herhangi bir web uygulaması gerçek zamanlı işlevselliği istersiniz.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)). Bkz: [sonraki adımlar](#next-steps) daha fazla bilgi için bölüm.


## <a name="prerequisites"></a>Önkoşullar

* ASP.NET Core 1.1 (1.0 çalışmaz)
* ASP.NET Core üzerinde çalışan herhangi bir işletim sistemi:
  
  * Windows 7 / Windows Server 2008 ve sonraki sürümleri
  * Linux
  * macOS

* **Özel durum**: uygulamanızı IIS ile Windows üzerinde çalışan veya WebListener ile kullanmanız gerekir:

  * Windows 8 / Windows Server 2012 veya üzeri
  * IIS 8 / IIS 8 Express
  * WebSocket IIS'de etkinleştirilmelidir

* Desteklenen tarayıcılar için http://caniuse.com/#feat=websockets bakın.

## <a name="when-to-use-it"></a>Ne zaman kullanılmalı

Bir yuva bağlantısı ile doğrudan çalışmak gerektiğinde WebSockets kullanın. Örneğin, olası en iyi performansı için gerçek zamanlı oyun gerekebilir.

[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) daha zengin sağlayan uygulama modeli gerçek zamanlı işlevselliği, ancak yalnızca ASP.NET üzerinde ASP.NET Core çalıştırır. SignalR Core sürümü geliştirilme aşamasındadır; ilerleme durumunu izlemek için bkz: [SignalR Core için GitHub depo](https://github.com/aspnet/SignalR).

SignalR Core için beklemek istemiyorsanız, WebSockets doğrudan artık kullanabilirsiniz. Ancak SignalR, gibi sağlayacak özellikleri geliştirmek gerekebilir:

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

WebSockets Ara yazılımında eklemek `Configure` yöntemi `Startup` sınıfı.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Aşağıdaki ayarlar yapılandırılabilir:

* `KeepAliveInterval`-Sık "ping" çerçeveler proxy'leri bağlantıyı açık tutmak emin olmak için istemciye göndermek nasıl.
* `ReceiveBufferSize`-Veri almak için kullanılan arabellek boyutu. Yalnızca ileri düzey kullanıcılar, kendi veri boyutuna göre bu, performans ayarlaması için değiştirmek gerekir.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>WebSocket istekleri kabul

İstek yaşam döngüsü devamındaki herhangi bir yerde (daha sonra `Configure` yöntemi veya bir MVC eylem, örneğin) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul et.

Bu örnek daha sonra arasındadır `Configure` yöntemi.

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Web yuvası isteğini üzerinde herhangi bir URL adresi gelebilir, ancak bu örnek kod yalnızca isteklerini kabul `/ws`.

### <a name="send-and-receive-messages"></a>İleti gönderme ve alma

`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantı yükseltir ve verir bir [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) nesnesi. İleti gönderme ve alma için WebSocket nesnesini kullanın.

Web yuvası isteğini kabul eden daha önce gösterilen kod iletir `WebSocket` nesnesine bir `Echo` yöntemi; burada'nın `Echo` yöntemi. Kod bir ileti alır ve hemen aynı iletiyi gönderir. İstemci bağlantısı kapatana kadar bunu bir döngüde kalır. 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Bu döngü başlamadan önce WebSocket kabul ettiğinde, ara yazılım ardışık düzenini sona erer.  Yuva kapatma sırasında ardışık düzen unwinds. Diğer bir deyişle, bir MVC eylem, örneğin isabet zaman olduğu gibi bir WebSocket kabul ardışık düzeninde ilerleyen isteği durdurur olacaktır.  Ancak bu döngü bitirip yuva kapatma isteği geri ardışık düzen işlemi devam eder.

## <a name="next-steps"></a>Sonraki adımlar

[Örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) bu eşlik makaledir basit Yankı uygulama. WebSocket bağlantılar sağlayan bir web sayfasına sahip ve sunucu, istemciye yalnızca aldığı iletileri yeniden gönderir. Çalıştırın (bunu ayarlanmamış yukarı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden ve http://localhost: 5000 için gidin. Web sayfasının sol üst bağlantı durumunu gösterir:

![Web sayfasının ilk durumu](websockets/_static/start.png)

Seçin **Bağlan** gösterilen URL'yi bir Web yuvası isteğini göndermek için.  Sınama iletisi girin ve **Gönder**. İşiniz bittiğinde, seçin **Kapat yuva**. **İletişim günlük** bölüm raporları her açık, gönderme ve kapatma eylemi olarak gerçekleşir.

![Web sayfasının ilk durumu](websockets/_static/end.png)
