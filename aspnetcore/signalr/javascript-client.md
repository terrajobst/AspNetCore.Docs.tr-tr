---
title: ASP.NET Core SignalR JavaScript istemcisi
author: bradygaster
description: ASP.NET Core SignalR JavaScript istemcisine genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/javascript-client
ms.openlocfilehash: 926160a41c82853d83890f0d52b14d7d5561a990
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963778"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a>ASP.NET Core SignalR JavaScript istemcisi

, [Oychel Appel](https://twitter.com/rachelappel) tarafından

ASP.NET Core SignalR JavaScript istemci kitaplığı, geliştiricilerin sunucu tarafı hub kodunu çağırmasını sağlar.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-client-package"></a>SignalR istemci paketini yükler

SignalR JavaScript istemci kitaplığı [NPM](https://www.npmjs.com/) paketi olarak dağıtılır. Visual Studio kullanıyorsanız, kök klasörde, **Paket Yöneticisi konsolundan** `npm install` çalıştırın. Visual Studio Code için, **Tümleşik terminalden**komutunu çalıştırın.

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

NPM *node_modules\\@microsoft\signalr\dist\browser* klasöre paket içeriğini yüklüyor. *Wwwroot\\lib* klasörü altında *SignalR* adlı yeni bir klasör oluşturun. *SignalR. js* dosyasını *wwwroot\lib\signalr* klasörüne kopyalayın.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

NPM *node_modules\\@aspnet\signalr\dist\browser* klasöre paket içeriğini yüklüyor. *Wwwroot\\lib* klasörü altında *SignalR* adlı yeni bir klasör oluşturun. *SignalR. js* dosyasını *wwwroot\lib\signalr* klasörüne kopyalayın.

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a>SignalR JavaScript istemcisini kullanma

`<script>` öğesinde JavaScript istemcisine SignalR başvurun.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Bir hub 'a bağlanma

Aşağıdaki kod bir bağlantı oluşturur ve başlatır. Hub 'ın adı büyük/küçük harfe duyarlıdır.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Çapraz kaynak bağlantıları

Genellikle, tarayıcılar istenen sayfayla aynı etki alanındaki bağlantıları yükler. Ancak, başka bir etki alanına bağlantı gerektiğinde bu durumlar vardır.

Kötü amaçlı bir sitenin başka bir siteden hassas verileri okumasını engellemek için, [Çıkış](xref:security/cors) noktaları varsayılan olarak devre dışı bırakılır. Bir çapraz kaynak isteğine izin vermek için `Startup` sınıfında etkinleştirin.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>İstemciden çağrı merkezi yöntemleri

JavaScript istemcileri, [Hubconnection](/javascript/api/%40aspnet/signalr/hubconnection)'un [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) yöntemi aracılığıyla hub 'larda ortak yöntemleri çağırır. `invoke` yöntemi iki bağımsız değişkeni kabul eder:

* Hub yönteminin adı. Aşağıdaki örnekte, hub 'daki Yöntem adı `SendMessage`.
* Hub yönteminde tanımlanan bağımsız değişkenler. Aşağıdaki örnekte, bağımsız değişken adı `message`. Örnek kod, Internet Explorer hariç tüm büyük tarayıcıların güncel sürümlerinde desteklenen ok işlev sözdizimini kullanır.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Azure SignalR hizmetini *sunucusuz modda*kullanıyorsanız, bir istemciden hub yöntemlerini çağıramezsiniz. Daha fazla bilgi için [SignalR hizmeti belgelerine](/azure/azure-signalr/signalr-concept-serverless-development-config)bakın.

`invoke` yöntemi bir JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)döndürür. `Promise`, sunucudaki yöntemi döndürdüğünde döndürülen değer (varsa) ile çözümlenir. Sunucu üzerindeki yöntem bir hata oluşturursa `Promise` hata iletisiyle reddedilir. Bu durumları (veya `await` sözdizimini) işlemek için `Promise` üzerinde `then` ve `catch` yöntemlerini kullanın.

`send` yöntemi bir JavaScript `Promise`döndürür. İleti sunucuya gönderildiğinde `Promise` çözümlenir. İleti gönderilirken bir hata oluşursa `Promise` hata iletisiyle reddedilir. Bu durumları (veya `await` sözdizimini) işlemek için `Promise` üzerinde `then` ve `catch` yöntemlerini kullanın.

> [!NOTE]
> `send` kullanmak sunucu iletiyi almadan önce beklemez. Sonuç olarak, sunucudan veri veya hata döndürülmesi mümkün değildir.

## <a name="call-client-methods-from-hub"></a>Hub 'dan istemci yöntemlerini çağır

Hub 'dan ileti almak için `HubConnection`[on](/javascript/api/%40aspnet/signalr/hubconnection#on) metodunu kullanarak bir yöntemi tanımlayın.

* JavaScript istemci yönteminin adı. Aşağıdaki örnekte, yöntem adı `ReceiveMessage`.
* Hub 'ın yönteme geçirdiği bağımsız değişkenler. Aşağıdaki örnekte, bağımsız değişken değeri `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

`connection.on` önceki kod, sunucu tarafı kodu [Sendadsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemi kullanılarak çağırdığında çalışır.

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR, `SendAsync` ve `connection.on`tanımlanan yöntem adı ve bağımsız değişkenlerle eşleştirerek hangi istemci yönteminin çağrılacağını belirler.

> [!NOTE]
> En iyi uygulama olarak, `on`sonra `HubConnection` [Başlangıç](/javascript/api/%40aspnet/signalr/hubconnection#start) yöntemini çağırın. Bunun yapılması, işleyicilerinin herhangi bir ileti alınmadan önce kaydolmasını sağlar.

## <a name="error-handling-and-logging"></a>Hata işleme ve günlüğe kaydetme

İstemci tarafı hatalarını işlemek için `start` yönteminin sonuna bir `catch` yöntemi zincirleyin. Hataları tarayıcı konsoluna çıkarmak için `console.error` kullanın.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Bağlantı yapıldığında günlüğe bir günlükçü ve olay türü geçirerek istemci tarafı günlük izlemeyi ayarlayın. İletiler, belirtilen günlük düzeyi ve daha yükseği ile günlüğe kaydedilir. Kullanılabilir günlük düzeyleri aşağıdaki gibidir:

* &ndash; hata iletileri `signalR.LogLevel.Error`. Yalnızca `Error` iletileri günlüğe kaydeder.
* olası hatalar hakkında &ndash; uyarı iletileri `signalR.LogLevel.Warning`. `Warning`ve `Error` iletileri günlüğe kaydeder.
* hata olmadan &ndash; durum iletileri `signalR.LogLevel.Information`. `Information`, `Warning`ve `Error` iletileri günlüğe kaydeder.
* Izleme iletilerini &ndash; `signalR.LogLevel.Trace`. Hub ve istemci arasında taşınan veriler de dahil olmak üzere her şeyi günlüğe kaydeder.

Günlük düzeyini yapılandırmak için [Hubconnectionbuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) üzerinde [configurelogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) yöntemini kullanın. İletiler tarayıcı konsoluna kaydedilir.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>İstemcileri yeniden bağla

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Otomatik olarak yeniden bağlan

SignalR için JavaScript istemcisi, [Hubconnectionbuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)'daki `withAutomaticReconnect` yöntemi kullanılarak otomatik olarak yeniden bağlanacak şekilde yapılandırılabilir. Varsayılan olarak otomatik olarak yeniden bağlanmaz.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Hiçbir parametre olmadan, `withAutomaticReconnect()` her bir yeniden bağlanma denemesini denemeden önce, dört başarısız denemeden sonra durdurmadan, istemciyi 0, 2, 10 ve 30 saniye bekleyecek şekilde yapılandırır.

Yeniden bağlanma girişimlerini başlatmadan önce, `HubConnection` `HubConnectionState.Reconnecting` durumuna geçer ve `onclose` geri çağırmaları `Disconnected`, otomatik yeniden bağlanma yapılandırması olmadan `HubConnection` gibi tetikleyerek `onreconnecting` geri çağırmaları tetikler. Bu, kullanıcıların bağlantının kaybedildiği ve Kullanıcı arabirimi öğelerini devre dışı bırakan kullanıcıları uyarma fırsatı sağlar.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

İstemci ilk dört denemeden sonra başarıyla yeniden bağlanırsa, `HubConnection` `Connected` duruma geri dönerek `onreconnected` geri çağırmaları harekete geçer. Bu, kullanıcılara bağlantı yeniden kurulmadığını bildirmek için bir fırsat sağlar.

Bağlantı sunucuya tamamen yeni göründüğünden, `onreconnected` geri çağırması için yeni bir `connectionId` sunulacaktır.

> [!WARNING]
> `HubConnection` [anlaşmayı atlayacak](xref:signalr/configuration#configure-client-options)şekilde yapılandırıldıysa, `onreconnected` geri aramanın `connectionId` parametresi tanımsız olacaktır.

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()`, ilk başlatma başarısızlıklarını yeniden denemek için `HubConnection` yapılandırmaz, bu nedenle başlatma hatalarının el ile işlenmesi gerekir:

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

İstemci ilk dört denemeden sonra başarıyla yeniden bağlanmazsa, `HubConnection` `Disconnected` durumuna geçer ve [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) geri aramalarını harekete geçirebilir. Bu, kullanıcılara bağlantının kalıcı olarak kaybedilmediğini bildirme ve sayfayı yenilemeyi önerme olanağı sağlar:

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Bağlantıyı kesmeden veya yeniden bağlanma zamanlamasını değiştirmeden önce özel sayıda yeniden bağlantı girişimi yapılandırmak için `withAutomaticReconnect`, her bir yeniden bağlanma girişimine başlamadan önce beklenecek gecikme süresi temsil eden bir sayı dizisini kabul eder.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

Yukarıdaki örnek, bağlantı kaybolduktan hemen sonra yeniden bağlanmaya başlamak için `HubConnection` yapılandırır. Bu, varsayılan yapılandırma için de geçerlidir.

İlk yeniden bağlantı girişimi başarısız olursa, ikinci yeniden bağlanma denemesi de varsayılan yapılandırmada olduğu gibi 2 saniye beklemek yerine hemen başlatılır.

İkinci yeniden bağlantı girişimi başarısız olursa, üçüncü yeniden bağlanma denemesi varsayılan yapılandırma gibi 10 saniye içinde başlar.

Özel davranış daha sonra varsayılan şekilde yeniden bağlantı kurmayı denemek yerine, üçüncü yeniden bağlantı girişimi başarısızlığından sonra varsayılan davranıştan sonra yeniden bir kez daha fazla yeniden bağlantı kurmaya çalışır.

Otomatik yeniden bağlanma denemelerinin zamanlaması ve sayısı üzerinde daha fazla denetime sahip olmak istiyorsanız `withAutomaticReconnect`, `nextRetryDelayInMilliseconds`adlı tek bir yönteme sahip `IRetryPolicy` arabirimini uygulayan bir nesneyi kabul eder.

`nextRetryDelayInMilliseconds` `RetryContext`türünde tek bir bağımsız değişken alır. `RetryContext` üç özelliğe sahiptir: `previousRetryCount`, `elapsedMilliseconds` ve `retryReason` `number`, `number` ve `Error`. İlk yeniden bağlanma denemesinden önce, hem `previousRetryCount` hem de `elapsedMilliseconds` sıfır olur ve `retryReason` bağlantının kaybolmasına neden olan hata olacaktır. Her başarısız yeniden deneme denemesinden `previousRetryCount`, `elapsedMilliseconds` bir kez artırılır, bu, şimdiye kadar geçen süre (milisaniye olarak) ve `retryReason` son yeniden bağlanma denemesinin başarısız olmasına neden olacak hata miktarını yansıtacak şekilde güncelleştirilir.

`nextRetryDelayInMilliseconds`, sonraki yeniden bağlanma girişiminden önce beklenecek milisaniye sayısını temsil eden bir sayı döndürmelidir veya `HubConnection` yeniden bağlamayı durdurması gerekiyorsa `null`.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: retryContext => {
          if (retryContext.elapsedMilliseconds < 60000) {
            // If we've been reconnecting for less than 60 seconds so far,
            // wait between 0 and 10 seconds before the next reconnect attempt.
            return Math.random() * 10000;
          } else {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
          }
        })
    .build();
```

Alternatif olarak, [el ile yeniden bağlanma](#manually-reconnect)bölümünde gösterildiği gibi istemcinizi el ile yeniden bağlayacaksınız.

::: moniker-end

### <a name="manually-reconnect"></a>El ile yeniden bağlan

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 3,0 ' den önce SignalR JavaScript istemcisi otomatik olarak yeniden bağlanmaz. İstemcinizi el ile yeniden bağlayacaksınız kodu yazmanız gerekir.

::: moniker-end

Aşağıdaki kod, genel bir el ile yeniden bağlantı yaklaşımını göstermektedir:

1. Bir işlev (Bu durumda `start` işlevi) bağlantıyı başlatmak için oluşturulur.
1. Bağlantının `onclose` olay işleyicisinde `start` işlevini çağırın.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Gerçek dünyada bir uygulama, bir üstel geri kapatmayı kullanır veya vermeden önce belirtilen sayıda kez yeniden dener.

## <a name="additional-resources"></a>Ek kaynaklar

* [JavaScript API'si başvurusu](/javascript/api/?view=signalr-js-latest)
* [JavaScript öğreticisi](xref:tutorials/signalr)
* [WebPack ve TypeScript öğreticisi](xref:tutorials/signalr-typescript-webpack)
* [Merkezler](xref:signalr/hubs)
* [.NET istemcisi](xref:signalr/dotnet-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
* [Çıkış noktaları arası Istekler (CORS)](xref:security/cors)
* [Azure SignalR hizmeti sunucusuz belgeler](/azure/azure-signalr/signalr-concept-serverless-development-config)
