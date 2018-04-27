---
title: ASP.NET Core SignalR JavaScript istemci
author: rachelappel
description: ASP.NET Core SignalR JavaScript istemci genel bakış.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/06/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/javascript-client
ms.openlocfilehash: cca1a523bd536f4365e54c196f6c9024779d5d76
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript istemci

Tarafından [Rachel Appel](http://twitter.com/rachelappel)

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

ASP.NET Core SignalR JavaScript istemci kitaplığını, sunucu taraflı hub kod geliştiricilerin sağlar.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>SignalR istemci paketini yükle

SignalR JavaScript istemci kitaplığını olarak teslim edilen bir [npm](https://www.npmjs.com/) paket. Visual Studio kullanıyorsanız, çalıştırmak `npm install` gelen **Paket Yöneticisi Konsolu** while kök klasöründe. Visual Studio Code komutu Çalıştır **tümleşik Terminal**.

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

Npm yükler ve paket içeriklerinin *node_modules\\ @aspnet\signalr\dist\browser*  klasör. Adlı yeni bir klasör oluşturun *signalr* altında *wwwroot\\lib* klasör. Kopya *signalr.js* dosya *wwwroot\lib\signalr* klasör.

## <a name="use-the-signalr-javascript-client"></a>SignalR JavaScript istemci kullanın

SignalR JavaScript istemci başvuru `<script>` öğesi.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Bir hub'a bağlanması

Aşağıdaki kod oluşturur ve bir bağlantı başlatır. Hub'ın adı büyük küçük harfe duyarlı değil.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=1-2,18)]

### <a name="cross-origin-connections"></a>Çıkış noktaları arası bağlantılar

Genellikle, tarayıcılar, istenen sayfa aynı etki alanından bağlantıları yükler. Ancak, başka bir etki alanıyla bağlantı gerekli olduğunda durumlar vardır.

Kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önlemek için [çıkış noktaları arası bağlantılar](xref:security/cors) varsayılan olarak devre dışıdır. Çıkış noktaları arası istek izin vermek için içinde etkinleştirmek `Startup` sınıfı.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-34,55)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırın

JavaScript istemcileri genel yöntemler hub tarafından üzerinde kullanarak Çağır `connection.invoke`. `invoke` Yöntemi iki bağımsız değişken kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, hub addır `SendMessage`.
* Hub yönteminin tanımlanan herhangi bir bağımsız değişken. Aşağıdaki örnekte, bağımsız değişkeni addır `message`.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=14)]

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemlerini çağırın

Hub'dan iletileri almak için bir yöntemi kullanarak tanımlarsınız `connection.on` yöntemi.

* JavaScript istemci yöntemin adı. Aşağıdaki örnekte, yöntem addır `ReceiveMessage`.
* Bağımsız değişkenleri hub için yöntem geçirir. Aşağıdaki örnekte, bağımsız değişken değeri olan `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=4-9)]

Önceki kod `connection.on` sunucu tarafı kodu kullanarak çağırdığında çalışır `SendAsync` yöntemi.

[!code-javascript[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR belirler yöntem adı eşleştirerek çağırmak için hangi istemci yöntemi ve bağımsız değişkenler tanımlanan `SendAsync` ve `connection.on`.

> [!NOTE]
> En iyi uygulama, arama `connection.start` sonra `connection.on` herhangi bir ileti almadan önce işleyicileri kaydedilir şekilde.

## <a name="error-handling-and-logging"></a>Hata işleme ve günlüğe kaydetme

Zincir bir `catch` yöntemi sonuna `connection.start` istemci-tarafı hataları işlemek için yöntem. Kullanım `console.error` tarayıcının konsola çıktı hataları.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=18)]

İstemci-tarafı günlük izleme Günlükçü ve olay türü bağlantı yapıldığında oturum geçirerek ayarlayın. Belirtilen günlük düzeyi ve daha yüksek iletileri günlüğe kaydedilir. Kullanılabilir günlük düzeyleri aşağıdaki gibidir:

* `signalR.LogLevel.Error` : Hata iletileri. Günlükleri `Error` yalnızca iletiler.
* `signalR.LogLevel.Warning` : Olası hatalar hakkında uyarı iletileri. Günlükleri `Warning`, ve `Error` iletileri.
* `signalR.LogLevel.Information` : Hatasız durum iletileri. Günlükleri `Information`, `Warning`, ve `Error` iletileri.
* `signalR.LogLevel.Trace` : İletileri izleme. Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.

Günlükçü kaydetmeye başlamak için bağlantı geçirin. Tarayıcının geliştirici araçları genellikle iletileri görüntüleyen bir konsol içerir.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=1-2)]

## <a name="related-resources"></a>İlgili kaynaklar

* [ASP.NET Core SignalR hub'ları](xref:signalr/hubs)
* [ASP.NET Core çıkış noktaları arası istekleri (CORS) etkinleştir](xref:security/cors)