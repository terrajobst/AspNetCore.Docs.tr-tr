---
title: ASP.NET Core SignalR JavaScript istemcisi
author: bradygaster
description: ASP.NET Core SignalR JavaScript istemcisi genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: 1565aa38a69113781d7c272a1710298cccc1f045
ms.sourcegitcommit: 3eedd6180fbbdcb81a8e1ebdbeb035bf4f2feb92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284511"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript istemcisi

Tarafından [Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR JavaScript istemci kitaplığı, geliştiricilerin, sunucu taraflı hub kodu çağırmak sağlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

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

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Çıkış noktaları arası bağlantılar

Genellikle, tarayıcılar istenen sayfa aynı etki alanında bağlantıları yükleyin. Ancak, başka bir etki alanı bağlantı gerekli olduğunda durumlar vardır.

Kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önlemek için [çıkış noktaları arası bağlantıları](xref:security/cors) varsayılan olarak devre dışıdır. Çıkış noktaları arası istek izin vermek için bunu etkinleştirin `Startup` sınıfı.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

JavaScript istemcilerinin şirket hub'ları genel yöntemleri çağırmak [çağırma](/javascript/api/%40aspnet/signalr/hubconnection#invoke) yöntemi [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). `invoke` Yöntemi iki bağımsız değişkeni kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, yöntem hub'ında addır `SendMessage`.
* Hub yönteminin içinde tanımlanan tüm bağımsız değişkenler. Aşağıdaki örnekte, bağımsız değişkeni addır `message`. Kod örneği, Internet Explorer dışındaki tüm bilinen tarayıcılar'ın geçerli sürümlerinde desteklenen ok işlev sözdizimi kullanır.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Azure SignalR hizmeti kullanıyorsanız, *sunucusuz modu*, bir istemciden hub yöntemlerini çağıramazsınız. Daha fazla bilgi için [SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config).

`invoke` Yöntemi döndürür bir JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise). `Promise` Dönüş değeriyle (varsa) sunucusundaki yöntemi döndüğünde çözümlenir. Yöntem sunucuda bir hata oluşturursa `Promise` hata iletisiyle reddedildi. Kullanım `then` ve `catch` yöntemlerde `Promise` bu durumları idare kendisine (veya `await` sözdizimi).

`send` Yöntemi döndürür bir JavaScript `Promise`. `Promise` Zaman sunucuya ileti gönderildi çözümlenir. İleti gönderilirken bir hata varsa `Promise` hata iletisiyle reddedildi. Kullanım `then` ve `catch` yöntemlerde `Promise` bu durumları idare kendisine (veya `await` sözdizimi).

> [!NOTE]
> Kullanarak `send` sunucunun bir ileti aldı kadar beklemez. Sonuç olarak, veri ya da hataları sunucudan döndürmek mümkün değildir.

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

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Bağlantı kurulduğunda oturum için bir Günlükçü ve olay türünü geçirerek istemci tarafı günlük izleme ayarlayın. Belirtilen günlük düzeyi ve üzeri, iletileri günlüğe kaydedilir. Kullanılabilir günlük düzeyleri aşağıdaki gibidir:

* `signalR.LogLevel.Error` &ndash; Hata iletileri. Günlükleri `Error` yalnızca iletileri.
* `signalR.LogLevel.Warning` &ndash; Olası hatalar hakkında uyarı iletileri. Günlükleri `Warning`, ve `Error` iletileri.
* `signalR.LogLevel.Information` &ndash; Hatasız durum iletileri. Günlükleri `Information`, `Warning`, ve `Error` iletileri.
* `signalR.LogLevel.Trace` &ndash; İzleme iletileri. Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.

Kullanım [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metodunda [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) günlük düzeyi yapılandırmak için. İletileri tarayıcı konsoluna yazılır.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>İstemcileri yeniden

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Otomatik olarak yeniden

SignalR JavaScript istemcisi kullanarak otomatik olarak yeniden yapılandırılabilir `withAutomaticReconnect` metodunda [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Varsayılan olarak otomatik olarak yeniden olmaz.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Herhangi bir parametre olmadan `withAutomaticReconnect()` dört girişimi başarısız olduktan sonra durduruluyor, her bir yeniden bağlanma denemesi denemeden önce sırasıyla 0, 2, 10 ve 30 saniye bekleyin üzere istemciyi yapılandırır.

Yeniden bağlanma girişimleri başlatmadan önce `HubConnection` geçiş olur `HubConnectionState.Reconnecting` belirtin ve yangın kendi `onreconnecting` geçiş yerine geri çağırmaları `Disconnected` durumu ve tetikleme kendi `onclose` geri çağırmaları ister bir `HubConnection`otomatik yeniden olmadan yapılandırılmış. Bu, bağlantısı kesilmiş kullanıcıları uyarın ve UI öğelerini devre dışı bırakmak için bir fırsat sağlar.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

İstemci ilk dört girişimlerinin içinde başarıyla bağlanırsa `HubConnection` geri geçeceğiyle `Connected` belirtin ve yangın kendi `onreconnected` geri çağırmaları. Bu, bağlantı yeniden kurulmadan kullanıcılara bildirmek için bir fırsat sağlar.

Bağlantı tamamen yeni sunucuya baktığı yeni `connectionId` için sağlanan `onreconnected` geri çağırma.

> [!WARNING]
> `onreconnected` Geri çağırma'nın `connectionId` varsa parametresi tanımsız olur `HubConnection` için yapılandırılan [anlaşma atla](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` Yapılandırma olmayacaktır `HubConnection` başlangıç hataları elle gerek ilk hataları yeniden denemek için:

```javascript
async function start() {
    try {
        await connection.start();
        console.assert(connection.state === signalR.HubConnectionState.Connected);
        console.log("connected");
    } catch (err) {
        console.assert(connection.state === signalR.HubConnectionState.Disconnected);
        console.log(err);
        setTimeout(() => start(), 5000);
    }
};
```

İstemci ilk dört girişimlerinin içinde başarıyla yeniden değil `HubConnection` geçiş olur `Disconnected` belirtin ve yangın kendi [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) geri çağırmalar. Bu bağlantıyı kalıcı olarak kesildi ve sayfayı yenilemeyi önerilir kullanıcılara bildirmek için bir fırsat sağlar:

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Özel bir bağlantıyı kesmeden önce yeniden bağlanma denemesi sayısını yapılandırma veya yeniden zamanlamayı değiştirmek için `withAutomaticReconnect` yeniden bağlanma girişimleri başlatmadan önce beklenecek milisaniye cinsinden gecikme değeri temsil eden sayı dizisi kabul eder.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

Yukarıdaki örnekte yapılandırır `HubConnection` hemen bağlantı kaybedildikten sonra yeniden bağlantılar denemesi başlatılacak. Bu aynı zamanda varsayılan yapılandırması için geçerlidir.

İlk yeniden deneme başarısız olursa, ikinci yeniden bağlanma denemesi de 2 Varsayılan yapılandırmada gibi saniye beklemek yerine başlayacaktır.

İkinci yeniden bağlanma denemesi başarısız olursa, üçüncü yeniden denemede yeniden gibi varsayılan yapılandırması olan 10 saniye içinde başlar.

Bir çalışan yerine hata üçüncü yeniden bağlanma girişimi yapıldıktan sonra özel davranış ardından tekrar varsayılan davranış durdurarak kareninkinden daha girişimi 30 başka bir Varsayılan yapılandırmada gibi saniye içinde yeniden.

Zamanlama ve otomatik sayısı daha fazla denetime yeniden bağlanma girişimleri, isterseniz `withAutomaticReconnect` kabul eden bir nesneyi uygulama `IReconnectPolicy` adlı tek bir yöntem olan arabirimi `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` iki bağımsız değişkeni alır `previousRetryCount` ve `elapsedMilliseconds`, her iki numarayı olduğu. Yeniden bağlanma denemesi ilk önce her ikisini de `previousRetryCount` ve `elapsedMilliseconds` sıfır olur. Başarısız tekrar deneme girişimleri sonra `previousRetryCount` birer birer artar ve `elapsedMilliseconds` şimdiye milisaniye cinsinden yeniden bağlanmayı harcanan süreyi yansıtacak şekilde güncelleştirilir.

`nextRetryDelayInMilliseconds` bir sonraki yeniden denemeden önce beklenecek milisaniye sayısını temsil eden bir sayı döndürmelidir veya `null` varsa `HubConnection` yeniden bağlanmayı durdurmanız gerekir.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: (previousRetryCount, elapsedMilliseconds) => {
            if (elapsedMilliseconds < 60000) {
                // If we've been reconnecting for less than 60 seconds so far,
                // wait between 0 and 10 seconds before the next reconnect attempt.
                return Math.random() * 10000;
            } else {
                // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
                return null;
            }
        }
    })
    .build();
```

Alternatif olarak, istemci gösterildiği şekilde el ile yeniden bağlanacak kod yazabileceğiniz [el ile yeniden](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>El ile yeniden

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Önce 3.0, SignalR için JavaScript istemci otomatik olarak yeniden değil. İstemcinizi el ile yeniden kod yazmanız gerekir.

::: moniker-end

Aşağıdaki kod tipik el ile yeniden yaklaşımı gösterir:

1. Bir işlev (Bu durumda, `start` işlevi) bağlantıyı başlatmak için oluşturulur.
1. Çağrı `start` bağlantının işlevinde `onclose` olay işleyicisi.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Gerçek uygulama bir üstel geri alma kullanın veya ancak bir belirtilen sayıda vazgeçmeden önce yeniden deneyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [JavaScript API'si başvurusu](/javascript/api/?view=signalr-js-latest)
* [JavaScript Öğreticisi](xref:tutorials/signalr)
* [Web ve TypeScript Öğreticisi](xref:tutorials/signalr-typescript-webpack)
* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
* [Çıkış noktaları arası istekleri (CORS)](xref:security/cors)
* [Sunucusuz Azure SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config)
