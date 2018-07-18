---
title: ASP.NET Core SignalR JavaScript istemcisi
author: tdykstra
description: ASP.NET Core SignalR JavaScript istemcisi genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: c13c41b0344b0c880e842f2799d6ee97bd7fff7e
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095430"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript istemcisi

Tarafından [Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR JavaScript istemci kitaplığı, geliştiricilerin, sunucu taraflı hub kodu çağırmak sağlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>SignalR istemci paketini yükle

SignalR JavaScript istemci kitaplığı olarak teslim edilen bir [npm](https://www.npmjs.com/) paket. Visual Studio kullanıyorsanız, çalıştırma `npm install` gelen **Paket Yöneticisi Konsolu** while kök klasöründe. Visual Studio Code için komutu çalıştırın **tümleşik Terminalini**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm paket içeriğini yükler *node_modules\\ @aspnet\signalr\dist\browser*  klasör. Adlı yeni bir klasör oluşturun *signalr* altında *wwwroot\\LIB* klasör. Kopyalama *signalr.js* dosyasını *wwwroot\lib\signalr* klasör.

## <a name="use-the-signalr-javascript-client"></a>SignalR JavaScript istemcisi kullanma

SignalR JavaScript istemci başvuru `<script>` öğesi.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Bir hub'ına bağlama

Aşağıdaki kod oluşturur ve bir bağlantı başlatır. Hub'ının adı büyük/küçük harfe duyarlıdır.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a>Çıkış noktaları arası bağlantılar

Genellikle, tarayıcılar istenen sayfa aynı etki alanında bağlantıları yükleyin. Ancak, başka bir etki alanı bağlantı gerekli olduğunda durumlar vardır.

Kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önlemek için [çıkış noktaları arası bağlantıları](xref:security/cors) varsayılan olarak devre dışıdır. Çıkış noktaları arası istek izin vermek için bunu etkinleştirin `Startup` sınıfı.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

JavaScript istemcileri aramak ortak yöntemler şirket hub'ları tarafından kullanarak `connection.invoke`. `invoke` Yöntemi iki bağımsız değişkeni kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, hub addır `SendMessage`.
* Hub yönteminin içinde tanımlanan tüm bağımsız değişkenler. Aşağıdaki örnekte, bağımsız değişkeni addır `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemleri çağırma

Hub'ından iletiler almak için bir yöntemi kullanarak tanımlarsınız `connection.on` yöntemi.

* JavaScript istemci yöntemin adı. Aşağıdaki örnekte, yöntem adı olan `ReceiveMessage`.
* Bağımsız değişkenleri hub yöntemine geçirir. Aşağıdaki örnekte, bağımsız değişken değeri olan `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Önceki kodda `connection.on` sunucu tarafı kod kullanarak çağırdığında çalıştıran `SendAsync` yöntemi.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR yöntem adını eşleştirerek çağırmak için hangi istemci yöntemini belirler ve bağımsız değişkenler tanımlanan `SendAsync` ve `connection.on`.

> [!NOTE]
> En iyi uygulama, çağrı `connection.start` sonra `connection.on` işleyicilerinizden herhangi iletileri almadan önce kayıtlı olan şekilde.

## <a name="error-handling-and-logging"></a>Hata işleme ve günlüğe kaydetme

Zincir bir `catch` sonuna yöntemi `connection.start` istemci tarafı hataları işlemek için yöntemi. Kullanım `console.error` tarayıcının konsola çıktı hataları.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Bağlantı kurulduğunda oturum için bir Günlükçü ve olay türünü geçirerek istemci tarafı günlük izleme ayarlayın. Belirtilen günlük düzeyi ve üzeri, iletileri günlüğe kaydedilir. Kullanılabilir günlük düzeyleri aşağıdaki gibidir:

* `signalR.LogLevel.Error` : Hata iletileri. Günlükleri `Error` yalnızca iletileri.
* `signalR.LogLevel.Warning` : Olası hatalar hakkında uyarı iletileri. Günlükleri `Warning`, ve `Error` iletileri.
* `signalR.LogLevel.Information` : Hatasız durum iletileri. Günlükleri `Information`, `Warning`, ve `Error` iletileri.
* `signalR.LogLevel.Trace` : İzleme iletileri. Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.

Kullanım `configureLogging` metodunda `HubConnectionBuilder` günlük düzeyi yapılandırmak için. İletileri tarayıcı konsoluna yazılır.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a>İlgili kaynaklar

* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
* [ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme](xref:security/cors)
