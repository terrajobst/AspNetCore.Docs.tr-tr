---
title: .NET Client ASP.NET Core SignalR
author: bradygaster
description: ASP.NET Core SignalR .NET Istemcisiyle ilgili bilgiler
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/14/2020
no-loc:
- SignalR
uid: signalr/dotnet-client
ms.openlocfilehash: a9583c9d6df52ff81a402df03e663ccc3847e51f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660043"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET Istemcisi

ASP.NET Core SignalR .NET istemci kitaplığı, .NET uygulamalarından SignalR hub 'larla iletişim kurmanızı sağlar.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Bu makaledeki kod örneği, ASP.NET Core SignalR .NET istemcisini kullanan bir WPF uygulamasıdır.

## <a name="install-the-signalr-net-client-package"></a>SignalR .NET istemci paketini yükler

.NET istemcilerinin SignalR hub 'larına bağlanması için [Microsoft. AspNetCore. SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) paketi gerekir.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

İstemci kitaplığını yüklemek için, **Paket Yöneticisi konsolu** penceresinde aşağıdaki komutu çalıştırın:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

İstemci kitaplığını yüklemek için komut kabuğu 'nda aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a>Bir hub'ına bağlama

Bir bağlantı kurmak için bir `HubConnectionBuilder` oluşturun ve `Build`çağırın. Hub URL 'SI, protokol, aktarım türü, günlük düzeyi, üst bilgiler ve diğer seçenekler bir bağlantı oluşturulurken yapılandırılabilir. `HubConnectionBuilder` yöntemlerinden herhangi birini `Build`ekleyerek gerekli seçenekleri yapılandırın. Bağlantıyı `StartAsync`başlatın.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Kayıp bağlantıyı işle

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Otomatik olarak yeniden bağlan

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection>, <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>`WithAutomaticReconnect` yöntemi kullanılarak otomatik olarak yeniden bağlanacak şekilde yapılandırılabilir. Varsayılan olarak otomatik olarak yeniden bağlanmaz.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Hiçbir parametre olmadan, `WithAutomaticReconnect()` her bir yeniden bağlanma denemesini denemeden önce, dört başarısız denemeden sonra durdurmadan, istemciyi 0, 2, 10 ve 30 saniye bekleyecek şekilde yapılandırır.

Yeniden bağlanma girişimlerini başlatmadan önce, `HubConnection` `HubConnectionState.Reconnecting` durumuna geçer ve `Reconnecting` olayını harekete geçirebilir.  Bu, kullanıcıların bağlantının kaybedildiği ve Kullanıcı arabirimi öğelerini devre dışı bırakan kullanıcıları uyarma fırsatı sağlar. Etkileşimli olmayan uygulamalar, iletileri sıraya alabilir veya bırakarak başlatabilir.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

İstemci ilk dört deneme süresi içinde başarıyla yeniden bağlanırsa, `HubConnection` `Connected` duruma geçer ve `Reconnected` olayını harekete geçirebilir. Bu, kullanıcılara bağlantı yeniden kurulduğunda ve sıraya alınan tüm iletileri sıradan bildiren bir fırsat sağlar.

Bağlantı sunucuya tamamen yeni göründüğünden, `Reconnected` olay işleyicilerine yeni bir `ConnectionId` sunulacaktır.

> [!WARNING]
> `Reconnected` olay işleyicisinin `connectionId` parametresi, `HubConnection` [anlaşmayı atlayacak](xref:signalr/configuration#configure-client-options)şekilde yapılandırıldıysa null olacaktır.

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()`, ilk başlatma başarısızlıklarını yeniden denemek için `HubConnection` yapılandırmaz, bu nedenle başlatma hatalarının el ile işlenmesi gerekir:

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

İstemci ilk dört denemeden sonra başarıyla yeniden bağlanmazsa, `HubConnection` `Disconnected` durumuna geçer ve <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> olayını harekete geçirebilir. Bu, bağlantıyı el ile yeniden başlatmayı denemek veya bağlantıyı kalıcı olarak kaybettiğini bildirmek için bir fırsat sağlar.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Bağlantıyı kesmeden veya yeniden bağlanma zamanlamasını değiştirmeden önce özel sayıda yeniden bağlantı girişimi yapılandırmak için `WithAutomaticReconnect`, her bir yeniden bağlanma girişimine başlamadan önce beklenecek gecikme süresi temsil eden bir sayı dizisini kabul eder.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

Yukarıdaki örnek, bağlantı kaybolduktan hemen sonra yeniden bağlanmaya başlamak için `HubConnection` yapılandırır. Bu, varsayılan yapılandırma için de geçerlidir.

İlk yeniden bağlantı girişimi başarısız olursa, ikinci yeniden bağlanma denemesi de varsayılan yapılandırmada olduğu gibi 2 saniye beklemek yerine hemen başlatılır.

İkinci yeniden bağlantı girişimi başarısız olursa, üçüncü yeniden bağlanma denemesi varsayılan yapılandırma gibi 10 saniye içinde başlar.

Özel davranış daha sonra, üçüncü yeniden bağlantı girişimi başarısızlığından sonra durarak varsayılan davranıştan daha sonra yeniden ayrılmış. Varsayılan yapılandırmada, 30 saniye içinde bir veya daha fazla yeniden bağlantı denemesi olur.

Otomatik yeniden bağlanma denemelerinin zamanlaması ve sayısı üzerinde daha fazla denetime sahip olmak istiyorsanız `WithAutomaticReconnect`, `NextRetryDelay`adlı tek bir yönteme sahip `IRetryPolicy` arabirimini uygulayan bir nesneyi kabul eder.

`NextRetryDelay` `RetryContext`türünde tek bir bağımsız değişken alır. `RetryContext` üç özelliğe sahiptir: `PreviousRetryCount`, `ElapsedTime` ve `RetryReason``long`, `TimeSpan` ve `Exception`. İlk yeniden bağlanma denemesinden önce, hem `PreviousRetryCount` hem de `ElapsedTime` sıfır olur ve `RetryReason` bağlantının kaybolmasına neden olan özel durum olacaktır. Her başarısız yeniden deneme denemesinden sonra `PreviousRetryCount`, bu ana kadar geçen süreyi yansıtacak şekilde `ElapsedTime` güncelleştirilir ve `RetryReason` son yeniden bağlanma denemesinin başarısız olmasına neden olan özel durum olacaktır.

`NextRetryDelay`, sonraki yeniden bağlanma girişiminden önce beklenecek süreyi temsil eden bir TimeSpan değeri döndürmelidir veya `HubConnection` yeniden bağlamayı durdurması gerekiyorsa `null`.

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
            return TimeSpan.FromSeconds(_random.NextDouble() * 10);
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

Alternatif olarak, [el ile yeniden bağlanma](#manually-reconnect)bölümünde gösterildiği gibi istemcinizi el ile yeniden bağlayacaksınız.

::: moniker-end

### <a name="manually-reconnect"></a>El ile yeniden bağlan

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> 3,0 ' den önce, SignalR .NET istemcisi otomatik olarak yeniden bağlanmaz. İstemcinizi el ile yeniden kod yazmanız gerekir.

::: moniker-end

Kayıp bir bağlantıya yanıt vermek için <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> olayını kullanın. Örneğin, yeniden bağlanmayı otomatik hale getirmek isteyebilirsiniz.

`Closed` olayı, zaman uyumsuz kodun `async void`kullanılmadan çalışmasına izin veren bir `Task`döndüren bir temsilci gerektirir. Zaman uyumlu olarak çalışan bir `Closed` olay işleyicisindeki temsilci imzasını karşılamak için `Task.CompletedTask`döndürün:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

Zaman uyumsuz desteğin ana nedeni, bağlantıyı yeniden başlatabilmeniz için kullanılır. Bir bağlantının başlatılması zaman uyumsuz bir işlemdir.

Bağlantıyı yeniden başlatan bir `Closed` işleyicisinde, aşağıdaki örnekte gösterildiği gibi, sunucunun aşırı yüklenmesini engellemek için bazı rastgele gecikme yapmayı düşünün:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

`InvokeAsync` hub 'daki yöntemleri çağırır. Hub yöntemi adını ve hub yönteminde tanımlanan tüm bağımsız değişkenleri `InvokeAsync`geçirin. SignalR zaman uyumsuzdur, bu nedenle çağrıları yaparken `async` ve `await` kullanın.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync` yöntemi, sunucu yöntemi döndürüldüğünde tamamlayan bir `Task` döndürür. Varsa, dönüş değeri `Task`sonucu olarak sağlanır. Sunucu üzerindeki yöntemi tarafından oluşturulan özel durumlar hatalı bir `Task`üretir. Sunucu yönteminin tamamlanmasını beklemek için `await` sözdizimini kullanın ve hataları işlemek için söz dizimini `try...catch`.

`SendAsync` yöntemi, ileti sunucuya gönderildiğinde tamamlayan bir `Task` döndürür. Bu `Task` sunucu yöntemi tamamlanana kadar beklemediğinden, dönüş değeri sağlanmaz. İletiyi gönderirken istemcide oluşturulan özel durumlar hatalı bir `Task`üretir. Gönderme hatalarını işlemek için `await` ve `try...catch` söz dizimini kullanın.

> [!NOTE]
> Azure SignalR hizmetini *sunucusuz modda*kullanıyorsanız, bir istemciden hub yöntemlerini çağıramezsiniz. Daha fazla bilgi için [SignalR hizmeti belgelerine](/azure/azure-signalr/signalr-concept-serverless-development-config)bakın.

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemleri çağırma

Bağlantıyı başlatmadan önce, derleme sonrasında `connection.On` kullanarak hub çağıran yöntemleri tanımlayın.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

`connection.On` önceki kod, sunucu tarafı kodu `SendAsync` yöntemini kullanarak çağırdığında çalışır.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Hata işleme ve günlüğe kaydetme

Try-catch ifadesiyle hataları işleyin. Bir hata oluştuktan sonra gerçekleştirilecek uygun eylemi öğrenmek için `Exception` nesnesini inceleyin.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Merkezler](xref:signalr/hubs)
* [JavaScript istemcisi](xref:signalr/javascript-client)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
* [Azure SignalR hizmeti sunucusuz belgeler](/azure/azure-signalr/signalr-concept-serverless-development-config)
