---
title: ASP.NET Core WebSockets desteği
author: rick-anderson
description: ASP.NET core'da WebSockets kullanmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/06/2018
uid: fundamentals/websockets
ms.openlocfilehash: 6c32269181ea3311c4aea99c08a1c043e7833b05
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341461"
---
# <a name="websockets-support-in-aspnet-core"></a>ASP.NET Core WebSockets desteği

Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Andrew Stanton-Nurse](https://github.com/anurse)

Bu makalede, WebSockets içinde ASP.NET Core ile çalışmaya başlama açıklanmaktadır. [WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür. Sohbet, Pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişim yararlanan uygulamalarda kullanılır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)). Bkz: [sonraki adımlar](#next-steps) bölümünde daha fazla bilgi için.

## <a name="prerequisites"></a>Önkoşullar

* ASP.NET Core 1.1 veya üzeri
* ASP.NET Core destekleyen tüm işletim Sistemleri:
  
  * Windows 7 / Windows Server 2008 veya üstü
  * Linux
  * macOS
  
* IIS ile Windows uygulama çalışıyorsa:

  * Windows 8 / Windows Server 2012 veya üzeri
  * IIS 8 / 8 IIS Express
  * WebSockets etkinleştirilmelidir (bkz [IIS/IIS Express Destek](#iisiis-express-support) bölümüne.).
  
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

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Aşağıdaki ayarlar yapılandırılabilir:

* `KeepAliveInterval` -Nasıl "ping" çerçeveler proxy'leri emin olmak için istemci bağlantıyı açık tutma genellikle gönderilecek. İki dakika varsayılandır.
* `ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu. Gelişmiş kullanıcılar, bu verilerin boyutunu temel alan performans ayarı için değiştirmeniz gerekebilir. Varsayılan değer 4 KB'dir.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Aşağıdaki ayarlar yapılandırılabilir:

* `KeepAliveInterval` -Nasıl "ping" çerçeveler proxy'leri emin olmak için istemci bağlantıyı açık tutma genellikle gönderilecek. İki dakika varsayılandır.
* `ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu. Gelişmiş kullanıcılar, bu verilerin boyutunu temel alan performans ayarı için değiştirmeniz gerekebilir. Varsayılan değer 4 KB'dir.
* `AllowedOrigins` -WebSocket istekleri için izin verilen çıkış noktaları üstbilgi değerleri listesi. Varsayılan olarak, tüm çıkış noktaları kullanılabilir. "WebSocket kaynak kısıtlama" Ayrıntılar için aşağıya bakın.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a>WebSocket isteklerini kabul etmek

İsteği yaşam döngüsünün sonraki yere (daha sonra `Configure` yöntemi veya örneğin bir MVC eylemi) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul edin.

Aşağıdaki örnek daha sonra buna dandır `Configure` yöntemi:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

Web yuvası isteğini herhangi bir URL gelebilir, ancak bu örnek kod, yalnızca isteklerini kabul eder `/ws`.

### <a name="send-and-receive-messages"></a>İleti gönderin ve alın

`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantısı yükseltir ve sağlayan bir [WebSocket](/dotnet/core/api/system.net.websockets.websocket) nesne. Kullanım `WebSocket` ileti göndermek ve almak için nesne.

Web yuvası isteğini kabul eden daha önce gösterilen kod geçirir `WebSocket` nesnesini bir `Echo` yöntemi. Kod, bir ileti alır ve hemen aynı iletiyi gönderir. Gönderilen ve istemci bağlantı kapatana kadar bir döngüde alınan ileti:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

Döngü başlamadan önce WebSocket bağlantısı kabul ederek, ara yazılım ardışık düzenini sona erer. Yuva kapatıldıktan sonra işlem hattını geriye doğru izler. Diğer bir deyişle, işlem hattı, WebSocket kabul edildiğinde ilerletme isteğini durdurur. Döngü tamamlandıktan ve yuva kapalı olduğunda, isteği yeniden işlem hattı devam eder.

::: moniker range=">= aspnetcore-2.2"

### <a name="websocket-origin-restriction"></a>WebSocket kaynak kısıtlama

CORS tarafından sağlanan korumaları WebSockets için geçerli değildir. Tarayıcılar **değil**:

* CORS uçuş öncesi istekler gerçekleştirin.
* Belirtilen kısıtlamalarını dikkate `Access-Control` WebSocket istekleri yaparken üstbilgileri.

Ancak, tarayıcılar gönderebilirsiniz `Origin` WebSocket istekleri gönderirken, üst bilgisi. Uygulamalar, beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için bu üstbilgileri doğrulamak için yapılandırılmalıdır.

Sunucunuz koyduysanız "https://server.com"ve istemci üzerindeki barındırma"https://client.com", Ekle "https://client.com" için `AllowedOrigins` WebSockets doğrulamak için listesi.

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> `Origin` Üst bilgisi, istemcinin ve gibi denetlenir `Referer` başlık sahte. Yapmak **değil** bir kimlik doğrulama mekanizması bu üst bilgi kullan.

::: moniker-end

## <a name="iisiis-express-support"></a>IIS/IIS Express desteği

Windows Server 2012 veya üzeri ve Windows 8 veya üzeri ile IIS/IIS Express 8 veya üzeri, WebSocket protokolü için desteği vardır.

> [!NOTE]
> WebSockets, IIS Express kullanırken her zaman etkindir.

### <a name="enabling-websockets-on-iis"></a>IIS WebSockets etkinleştirme

Windows Server 2012 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:

> [!NOTE]
> Bu adımları IIS Express kullanırken gerekli değildir

1. Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.
1. Seçin **rol tabanlı veya özellik tabanlı yükleme**. **İleri**’yi seçin.
1. (Yerel sunucu varsayılan olarak seçilidir) uygun sunucuyu seçin. **İleri**’yi seçin.
1. Genişletin **Web sunucusu (IIS)** içinde **rolleri** ağacında, genişletme **Web sunucusu**ve ardından **uygulama geliştirme**.
1. Seçin **WebSocket Protokolü**. **İleri**’yi seçin.
1. Ek özelliklere ihtiyaç duyulmayan, seçin **sonraki**.
1. **Yükle**'yi seçin.
1. Yükleme tamamlandığında seçin **Kapat** sihirbazdan çıkmak için.

Windows 8 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:

> [!NOTE]
> Bu adımları IIS Express kullanırken gerekli değildir

1. Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).
1. Aşağıdaki düğümler açın: **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.
1. Seçin **WebSocket Protokolü** özelliği. **Tamam**’ı seçin.

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a>Socket.io, Node.js dosyasını kullanırken WebSocket devre dışı bırak

WebSocket desteği kullanıyorsanız [socket.io](https://socket.io/) üzerinde [Node.js](https://nodejs.org/), varsayılan IIS WebSocket modülünü kullanarak devre dışı bırak `webSocket` öğesinde *web.config* veya *applicationHost.config*. Bu adım yapılamaz ve iletişim WebSocket yerine Node.js uygulaması işlemek IIS WebSocket modülü çalışır.

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a>Sonraki adımlar

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) bu eşlik Yankı uygulama makaledir. WebSocket bağlantılarını sağlayan bir web sayfasına sahip ve sunucu istemciye herhangi bir ileti aldıktan sonra yeniden gönderir. (Bunu ayarlanmadı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden uygulamayı çalıştırın ve gidin http://localhost:5000. Web sayfasının sol üst bağlantı durumunu gösterir:

![Web sayfasının ilk durumu](websockets/_static/start.png)

Seçin **Connect** gösterilen URL'yi bir Web yuvası isteğini göndermek için. Test iletisi girin ve seçin **Gönder**. İşiniz bittiğinde, seçin **Kapat yuva**. **İletişim günlük** bölüm açık, her gönderme bildirir ve kapatma eylemi,'olmuyor.

![Web sayfasının ilk durumu](websockets/_static/end.png)
