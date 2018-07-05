---
title: ASP.NET Core WebSockets desteği
author: rick-anderson
description: ASP.NET core'da WebSockets kullanmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: a9fe13ef7895ea3ab43257dbbaf4521f883c0804
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433993"
---
# <a name="websockets-support-in-aspnet-core"></a>ASP.NET Core WebSockets desteği

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Andrew Stanton-Nurse](https://github.com/anurse)

Bu makalede, WebSockets içinde ASP.NET Core ile çalışmaya başlama açıklanmaktadır. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür. Sohbet, Pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişim yararlanan uygulamalarda kullanılır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)). Bkz: [sonraki adımlar](#next-steps) bölümünde daha fazla bilgi için.

## <a name="prerequisites"></a>Önkoşullar

* ASP.NET Core 1.1 veya üzeri
* ASP.NET Core destekleyen tüm işletim Sistemleri:
  
  * Windows 7 / Windows Server 2008 veya üstü
  * Linux
  * MacOS
  
* IIS ile Windows uygulama çalışıyorsa:

  * Windows 8 / Windows Server 2012 veya üzeri
  * IIS 8 / 8 IIS Express
  * IIS WebSockets etkinleştirildi (bkz [IIS/IIS Express Destek](#iisiis-express-support) bölümüne.)
  
* Uygulama çalışıyorsa [HTTP.sys](xref:fundamentals/servers/httpsys):

  * Windows 8 / Windows Server 2012 veya üzeri

* Desteklenen tarayıcılar için bkz: https://caniuse.com/#feat=websockets.

## <a name="when-to-use-websockets"></a>Ne zaman WebSockets kullanılır?

WebSockets, doğrudan bir yuva bağlantı ile çalışması için kullanın. Örneğin, gerçek zamanlı oyun ile mümkün olan en iyi performansı için WebSockets kullanın.

[ASP.NET Core SignalR](xref:signalr/introduction) , uygulamalara gerçek zamanlı web işlevselliği ekleme basitleştiren bir kitaplık. Mümkün olduğunda WebSockets kullanır.

## <a name="how-to-use-websockets"></a>WebSockets kullanma

* Yükleme [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paket.
* Ara yazılımını yapılandırın.
* WebSocket isteklerini kabul etmek.
* İletileri gönderip yeniden açın.

### <a name="configure-the-middleware"></a>Ara yazılımını yapılandırma

WebSockets Ara yazılımında ekleme `Configure` yöntemi `Startup` sınıfı:

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

Aşağıdaki ayarlar yapılandırılabilir:

* `KeepAliveInterval` -Nasıl "ping" çerçeveler proxy'leri emin olmak için istemci bağlantıyı açık tutma genellikle gönderilecek.
* `ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu. Gelişmiş kullanıcılar, bu verilerin boyutunu temel alan performans ayarı için değiştirmeniz gerekebilir.

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a>WebSocket isteklerini kabul etmek

İsteği yaşam döngüsünün sonraki yere (daha sonra `Configure` yöntemi veya örneğin bir MVC eylemi) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul edin.

Aşağıdaki örnek daha sonra buna dandır `Configure` yöntemi:

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

Web yuvası isteğini herhangi bir URL gelebilir, ancak bu örnek kod, yalnızca isteklerini kabul eder `/ws`.

### <a name="send-and-receive-messages"></a>İleti gönderin ve alın

`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantısı yükseltir ve sağlayan bir [WebSocket](/dotnet/core/api/system.net.websockets.websocket) nesne. Kullanım `WebSocket` ileti göndermek ve almak için nesne.

Web yuvası isteğini kabul eden daha önce gösterilen kod geçirir `WebSocket` nesnesini bir `Echo` yöntemi. Kod, bir ileti alır ve hemen aynı iletiyi gönderir. Gönderilen ve istemci bağlantı kapatana kadar bir döngüde alınan ileti:

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

Döngü başlamadan önce WebSocket bağlantısı kabul ederek, ara yazılım ardışık düzenini sona erer. Yuva kapatıldıktan sonra işlem hattını geriye doğru izler. Diğer bir deyişle, işlem hattı, WebSocket kabul edildiğinde ilerletme isteğini durdurur. Döngü tamamlandıktan ve yuva kapalı olduğunda, isteği yeniden işlem hattı devam eder.

## <a name="iisiis-express-support"></a>IIS/IIS Express desteği

Windows Server 2012 veya üzeri ve Windows 8 veya üzeri ile IIS/IIS Express 8 veya üzeri, WebSocket protokolü için desteği vardır.

Windows Server 2012 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:

1. Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.
1. Seçin **rol tabanlı veya özellik tabanlı yükleme**. Seçin **sonraki**.
1. (Yerel sunucu varsayılan olarak seçilidir) uygun sunucuyu seçin. Seçin **sonraki**.
1. Genişletin **Web sunucusu (IIS)** içinde **rolleri** ağacında, genişletme **Web sunucusu**ve ardından **uygulama geliştirme**.
1. Seçin **WebSocket Protokolü**. Seçin **sonraki**.
1. Ek özelliklere ihtiyaç duyulmayan, seçin **sonraki**.
1. **Yükle**'yi seçin.
1. Yükleme tamamlandığında seçin **Kapat** sihirbazdan çıkmak için.

Windows 8 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:

1. Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).
1. Aşağıdaki düğümler açın: **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.
1. Seçin **WebSocket Protokolü** özelliği. Seçin **Tamam**.

**Socket.io, node.js dosyasını kullanırken WebSocket devre dışı bırak**

WebSocket desteği kullanıyorsanız [socket.io](https://socket.io/) üzerinde [Node.js](https://nodejs.org/), varsayılan IIS WebSocket modülünü kullanarak devre dışı bırak `webSocket` öğesinde *web.config* veya *applicationHost.config*. Bu adım yapılamaz ve iletişim WebSocket yerine Node.js uygulaması işlemek IIS WebSocket modülü çalışır.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Sonraki adımlar

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) bu eşlik Yankı uygulama makaledir. WebSocket bağlantılarını sağlayan bir web sayfasına sahip ve sunucu istemciye herhangi bir ileti aldıktan sonra yeniden gönderir. (Bunu ayarlanmadı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden uygulamayı çalıştırın ve gidin http://localhost:5000. Web sayfasının sol üst bağlantı durumunu gösterir:

![Web sayfasının ilk durumu](websockets/_static/start.png)

Seçin **Connect** gösterilen URL'yi bir Web yuvası isteğini göndermek için. Test iletisi girin ve seçin **Gönder**. İşiniz bittiğinde, seçin **Kapat yuva**. **İletişim günlük** bölüm açık, her gönderme bildirir ve kapatma eylemi,'olmuyor.

![Web sayfasının ilk durumu](websockets/_static/end.png)
