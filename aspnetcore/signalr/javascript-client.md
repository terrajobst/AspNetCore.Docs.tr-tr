---
title: ASP.NET Core SignalR JavaScript istemcisi
author: tdykstra
description: ASP.NET Core SignalR JavaScript istemcisi genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 10958c414aa4a285c8a2810bb99e278f719c5b7f
ms.sourcegitcommit: 8bf4dff3069e62972c1b0839a93fb444e502afe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46483055"
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

npm paket içeriğini yükler *node_modules\\@aspnet\signalr\dist\browser* klasör. Adlı yeni bir klasör oluşturun *signalr* altında *wwwroot\\LIB* klasör. Kopyalama *signalr.js* dosyasını *wwwroot\lib\signalr* klasör.

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

JavaScript istemcilerinin şirket hub'ları genel yöntemleri çağırmak [çağırma](/javascript/api/%40aspnet/signalr/hubconnection#invoke) yöntemi [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). `invoke` Yöntemi iki bağımsız değişkeni kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, yöntem hub'ında addır `SendMessage`.
* Hub yönteminin içinde tanımlanan tüm bağımsız değişkenler. Aşağıdaki örnekte, bağımsız değişkeni addır `message`.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemleri çağırma

Hub'ından iletiler almak için bir yöntemi kullanarak tanımlarsınız [üzerinde](/javascript/api/%40aspnet/signalr/hubconnection#on) yöntemi `HubConnection`.

* JavaScript istemci yöntemin adı. Aşağıdaki örnekte, yöntem adı olan `ReceiveMessage`.
* Bağımsız değişkenleri hub yöntemine geçirir. Aşağıdaki örnekte, bağımsız değişken değeri olan `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Önceki kodda `connection.on` sunucu tarafı kod kullanarak çağırdığında çalıştıran [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemi.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR yöntem adını eşleştirerek çağırmak için hangi istemci yöntemini belirler ve bağımsız değişkenler tanımlanan `SendAsync` ve `connection.on`.

> [!NOTE]
> En iyi uygulama, çağrı [Başlat](/javascript/api/%40aspnet/signalr/hubconnection#start) metodunda `HubConnection` sonra `on`. İletileri almadan önce İşleyicileriniz kayıtlı sağlar.

## <a name="error-handling-and-logging"></a>Hata işleme ve günlüğe kaydetme

Zincir bir `catch` sonuna yöntemi `start` istemci tarafı hataları işlemek için yöntemi. Kullanım `console.error` tarayıcının konsola çıktı hataları.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

Bağlantı kurulduğunda oturum için bir Günlükçü ve olay türünü geçirerek istemci tarafı günlük izleme ayarlayın. Belirtilen günlük düzeyi ve üzeri, iletileri günlüğe kaydedilir. Kullanılabilir günlük düzeyleri aşağıdaki gibidir:

* `signalR.LogLevel.Error` &ndash; Hata iletileri. Günlükleri `Error` yalnızca iletileri.
* `signalR.LogLevel.Warning` &ndash; Olası hatalar hakkında uyarı iletileri. Günlükleri `Warning`, ve `Error` iletileri.
* `signalR.LogLevel.Information` &ndash; Hatasız durum iletileri. Günlükleri `Information`, `Warning`, ve `Error` iletileri.
* `signalR.LogLevel.Trace` &ndash; İzleme iletileri. Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.

Kullanım [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metodunda [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) günlük düzeyi yapılandırmak için. İletileri tarayıcı konsoluna yazılır.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="additional-resources"></a>Ek kaynaklar

* [JavaScript API Başvurusu](/javascript/api/?view=signalr-js-latest)
* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
* [ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme](xref:security/cors)
