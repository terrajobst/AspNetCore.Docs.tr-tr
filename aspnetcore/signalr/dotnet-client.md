---
title: ASP.NET Core SignalR .NET istemcisi
author: bradygaster
description: ASP.NET Core SignalR .NET istemcisi hakkında bilgi
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 97c553874cb1e4b678fa0e5cd65074f135193861
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153123"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET istemcisi

ASP.NET Core SignalR .NET istemci kitaplığı, .NET uygulamalarından SignalR hub'ları ile iletişim kurmanıza olanak sağlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Bu makalede kod örneği ASP.NET Core SignalR .NET istemcinin kullandığı bir WPF uygulamasıdır.

## <a name="install-the-signalr-net-client-package"></a>SignalR .NET istemci paketini yükle

`Microsoft.AspNetCore.SignalR.Client` Paket, .NET istemcileri için SignalR hub'larını bağlama için gereklidir. İstemci kitaplığını yüklemek için aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresi:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Bir hub'ına bağlama

Bir bağlantı kurmak için oluşturma bir `HubConnectionBuilder` ve çağrı `Build`. Hub'ı URL'si, protokolü, aktarım türü, günlük düzeyi, üst bilgiler ve diğer seçenekleri bir bağlantı oluşturulurken yapılandırılabilir. Gerekli tüm seçenekler ekleyerek herhangi bir yapılandırma `HubConnectionBuilder` yöntemleri `Build`. Bağlantıyı başlatmak `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Tanıtıcı bağlantısı kesildi

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Otomatik olarak yeniden

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> Kullanarak otomatik olarak yeniden yapılandırılabilir `WithAutomaticReconnect` metodunda <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>. Varsayılan olarak otomatik olarak yeniden olmaz.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Herhangi bir parametre olmadan `WithAutomaticReconnect()` dört girişimi başarısız olduktan sonra durduruluyor, her bir yeniden bağlanma denemesi denemeden önce sırasıyla 0, 2, 10 ve 30 saniye bekleyin üzere istemciyi yapılandırır.

Yeniden bağlanma girişimleri başlatmadan önce `HubConnection` geçiş olur `HubConnectionState.Reconnecting` belirtin ve yangın `Reconnecting` olay.  Bu, bağlantısı kesilmiş kullanıcıları uyarın ve UI öğelerini devre dışı bırakmak için bir fırsat sağlar. Etkileşimli olmayan uygulamalar, sıraya alma veya iletileri bırakarak başlatabilirsiniz.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

İstemci ilk dört girişimlerinin içinde başarıyla bağlanırsa `HubConnection` geri geçeceğiyle `Connected` belirtin ve yangın `Reconnected` olay. Bu bağlantı kuruldu ve sıraya alınan iletileri sıradan kullanıcılara bildirmek için bir fırsat sağlar.

Bağlantı tamamen yeni sunucuya baktığı yeni `ConnectionId` için sağlanan `Reconnected` olay işleyicileri.

> [!WARNING]
> `Reconnected` Olay işleyicinin `connectionId` parametresi null olacaktır, `HubConnection` için yapılandırılan [anlaşma atla](xref:signalr/configuration#configure-client-options).

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` Yapılandırma olmayacaktır `HubConnection` başlangıç hataları elle gerek ilk hataları yeniden denemek için:

```csharp
public static async Task<bool> ConnectWithRetryAsync(HubConnection connection, CancellationToken token)
{
    // Keep trying to until we can start or the token is canceled.
    while (true)
    {
        try
        {
            await connection.StartAsync(token);
            Debug.Assert(connection.State == HubConnectionState.Connected);
            return true;
        }
        catch when (token.IsCancellationRequested)
        {
            return false;
        }
        catch
        {
            // Failed to connect, trying again in 5000 ms.
            Debug.Assert(connection.State == HubConnectionState.Disconnected);
            await Task.Delay(5000);
        }
    }
}
```

İstemci ilk dört girişimlerinin içinde başarıyla yeniden değil `HubConnection` geçiş olur `Disconnected` belirtin ve yangın <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> olay. Bu bağlantıyı el ile yeniden başlatmak veya bağlantı kalıcı olarak kesildi kullanıcılara bildirmek deneme fırsatı sağlar.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Özel bir bağlantıyı kesmeden önce yeniden bağlanma denemesi sayısını yapılandırma veya yeniden zamanlamayı değiştirmek için `WithAutomaticReconnect` yeniden bağlanma girişimleri başlatmadan önce beklenecek milisaniye cinsinden gecikme değeri temsil eden sayı dizisi kabul eder.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

Yukarıdaki örnekte yapılandırır `HubConnection` hemen bağlantı kaybedildikten sonra yeniden bağlantılar denemesi başlatılacak. Bu aynı zamanda varsayılan yapılandırması için geçerlidir.

İlk yeniden deneme başarısız olursa, ikinci yeniden bağlanma denemesi de 2 Varsayılan yapılandırmada gibi saniye beklemek yerine başlayacaktır.

İkinci yeniden bağlanma denemesi başarısız olursa, üçüncü yeniden denemede yeniden gibi varsayılan yapılandırması olan 10 saniye içinde başlar.

Özel davranış daha sonra yeniden varsayılan davranışı hatası üçüncü yeniden bağlanma girişimi yapıldıktan sonra durdurarak kareninkinden. Varsayılan yapılandırmasında yok olması bir daha yeniden denemesi başka bir 30 saniye.

Zamanlama ve otomatik sayısı daha fazla denetime yeniden bağlanma girişimleri, isterseniz `WithAutomaticReconnect` kabul eden bir nesneyi uygulama `IRetryPolicy` adlı tek bir yöntem olan arabirimi `NextRetryDelay`.

`NextRetryDelay` tek bir bağımsız değişken alan türüyle `RetryContext`. `RetryContext` Üç özelliğe sahiptir: `PreviousRetryCount`, `ElapsedTime` ve `RetryReason` olduğu bir `long`, `TimeSpan` ve `Exception` sırasıyla. Yeniden bağlanma denemesi ilk önce her ikisini de `PreviousRetryCount` ve `ElapsedTime` sıfır olur ve `RetryReason` bağlantı kaybolmasına neden olan özel durum olacaktır. Başarısız tekrar deneme girişimleri sonra `PreviousRetryCount` biri tarafından artırılacaktır `ElapsedTime` şu ana kadar yeniden bağlanmayı harcanan süreyi yansıtacak şekilde güncelleştirilir ve `RetryReason` özel durum nedeniyle son yeniden bağlanma denemesi başarısız olur.

`NextRetryDelay` bir sonraki yeniden denemeden önce beklenecek süreyi temsil eden ya da bir TimeSpan döndürmelidir veya `null` varsa `HubConnection` yeniden bağlanmayı durdurmanız gerekir.

```csharp
public class RandomRetryPolicy : IRetryPolicy
{
    private readonly Random _random = new Random();

    public TimeSpan? NextRetryDelay(RetryContext retryContext)
    {
        // If we've been reconnecting for less than 60 seconds so far,
        // wait between 0 and 10 seconds before the next reconnect attempt.
        if (retryContext.ElapsedTime < TimeSpan.FromSeconds(60))
        {
            return TimeSpan.FromSeconds(_random.Next() * 10);
        }
        else
        {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
        }
    }
}
```

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

Alternatif olarak, istemci gösterildiği şekilde el ile yeniden bağlanacak kod yazabileceğiniz [el ile yeniden](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>El ile yeniden

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Önce 3.0, SignalR için .NET istemci otomatik olarak yeniden değil. İstemcinizi el ile yeniden kod yazmanız gerekir.

::: moniker-end

Kullanım <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> olay yanıt bağlantı kesildi. Örneğin, yeniden bağlanma otomatikleştirmek isteyebilirsiniz.

`Closed` Olay gerektirir döndüren bir temsilci bir `Task`, zaman uyumsuz kod kullanmadan çalıştırılmasına olanak sağlayan `async void`. Temsilci imzasında karşılamak için bir `Closed` çalıştırmaları eş döndüren bir olay işleyicisi `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

Zaman uyumsuz desteği için temel nedeni olduğundan, bağlantı yeniden başlatabilirsiniz. Bağlantı bir zaman uyumsuz eylem başlangıcıdır.

İçinde bir `Closed` bağlantı yeniden işleyicisi aşağıdaki örnekte gösterildiği gibi sunucu aşırı yüklemesini önlemek bazı rastgele gecikme bekleniyor dikkate alın:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

`InvokeAsync` hub yöntemleri çağırır. Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `InvokeAsync`. SignalR zaman uyumsuz, bu nedenle kullanın `async` ve `await` çağrıları yapılırken.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync` Yöntemi döndürür bir `Task` sunucu yöntem döndürüldüğünde tamamlar. Sonucu olarak dönüş değeri varsa, sağlanan `Task`. Sunucuda yöntemi tarafından oluşturulan özel durumlar bir hatalı üretmek `Task`. Kullanım `await` tamamlamak sunucu yöntemi için beklenecek söz dizimi ve `try...catch` sözdizimi hataları işlemek için.

`SendAsync` Yöntemi döndürür bir `Task` sunucuya ileti gönderildiğinde tamamlar. Dönüş değeri bu sağlanan `Task` Metoda tamamlanana kadar beklemez. İstemcide ileti gönderilirken karşılaşılan özel durumlar bir hatalı üretmek `Task`. Kullanım `await` ve `try...catch` işlemek için söz dizimi hatalarını gönderme.

> [!NOTE]
> Azure SignalR hizmeti kullanıyorsanız, *sunucusuz modu*, bir istemciden hub yöntemlerini çağıramazsınız. Daha fazla bilgi için [SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemleri çağırma

Hub'ı kullanarak çağırdığı yöntemleri tanımlamak `connection.On` yapı sonra ancak bağlantı başlatmadan önce.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Önceki kodda `connection.On` sunucu tarafı kod kullanarak çağırdığında çalıştıran `SendAsync` yöntemi.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Hata işleme ve günlüğe kaydetme

Bir try-catch deyiminin hatalarla işleyin. İnceleme `Exception` nesnesine bir hata gerçekleştikten sonra gerçekleştirilecek uygun eylemi belirleyin.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
* [Sunucusuz Azure SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config)
