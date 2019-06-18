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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="c8de1-103">ASP.NET Core SignalR .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="c8de1-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="c8de1-104">ASP.NET Core SignalR .NET istemci kitaplığı, .NET uygulamalarından SignalR hub'ları ile iletişim kurmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="c8de1-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="c8de1-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c8de1-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c8de1-106">Bu makalede kod örneği ASP.NET Core SignalR .NET istemcinin kullandığı bir WPF uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="c8de1-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="c8de1-107">SignalR .NET istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="c8de1-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="c8de1-108">`Microsoft.AspNetCore.SignalR.Client` Paket, .NET istemcileri için SignalR hub'larını bağlama için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="c8de1-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="c8de1-109">İstemci kitaplığını yüklemek için aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="c8de1-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c8de1-110">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="c8de1-110">Connect to a hub</span></span>

<span data-ttu-id="c8de1-111">Bir bağlantı kurmak için oluşturma bir `HubConnectionBuilder` ve çağrı `Build`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="c8de1-112">Hub'ı URL'si, protokolü, aktarım türü, günlük düzeyi, üst bilgiler ve diğer seçenekleri bir bağlantı oluşturulurken yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="c8de1-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="c8de1-113">Gerekli tüm seçenekler ekleyerek herhangi bir yapılandırma `HubConnectionBuilder` yöntemleri `Build`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="c8de1-114">Bağlantıyı başlatmak `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="c8de1-115">Tanıtıcı bağlantısı kesildi</span><span class="sxs-lookup"><span data-stu-id="c8de1-115">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="c8de1-116">Otomatik olarak yeniden</span><span class="sxs-lookup"><span data-stu-id="c8de1-116">Automatically reconnect</span></span>

<span data-ttu-id="c8de1-117"><xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> Kullanarak otomatik olarak yeniden yapılandırılabilir `WithAutomaticReconnect` metodunda <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span><span class="sxs-lookup"><span data-stu-id="c8de1-117">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="c8de1-118">Varsayılan olarak otomatik olarak yeniden olmaz.</span><span class="sxs-lookup"><span data-stu-id="c8de1-118">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="c8de1-119">Herhangi bir parametre olmadan `WithAutomaticReconnect()` dört girişimi başarısız olduktan sonra durduruluyor, her bir yeniden bağlanma denemesi denemeden önce sırasıyla 0, 2, 10 ve 30 saniye bekleyin üzere istemciyi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="c8de1-119">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="c8de1-120">Yeniden bağlanma girişimleri başlatmadan önce `HubConnection` geçiş olur `HubConnectionState.Reconnecting` belirtin ve yangın `Reconnecting` olay.</span><span class="sxs-lookup"><span data-stu-id="c8de1-120">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="c8de1-121">Bu, bağlantısı kesilmiş kullanıcıları uyarın ve UI öğelerini devre dışı bırakmak için bir fırsat sağlar.</span><span class="sxs-lookup"><span data-stu-id="c8de1-121">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="c8de1-122">Etkileşimli olmayan uygulamalar, sıraya alma veya iletileri bırakarak başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c8de1-122">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="c8de1-123">İstemci ilk dört girişimlerinin içinde başarıyla bağlanırsa `HubConnection` geri geçeceğiyle `Connected` belirtin ve yangın `Reconnected` olay.</span><span class="sxs-lookup"><span data-stu-id="c8de1-123">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="c8de1-124">Bu bağlantı kuruldu ve sıraya alınan iletileri sıradan kullanıcılara bildirmek için bir fırsat sağlar.</span><span class="sxs-lookup"><span data-stu-id="c8de1-124">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="c8de1-125">Bağlantı tamamen yeni sunucuya baktığı yeni `ConnectionId` için sağlanan `Reconnected` olay işleyicileri.</span><span class="sxs-lookup"><span data-stu-id="c8de1-125">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="c8de1-126">`Reconnected` Olay işleyicinin `connectionId` parametresi null olacaktır, `HubConnection` için yapılandırılan [anlaşma atla](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="c8de1-126">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="c8de1-127">`WithAutomaticReconnect()` Yapılandırma olmayacaktır `HubConnection` başlangıç hataları elle gerek ilk hataları yeniden denemek için:</span><span class="sxs-lookup"><span data-stu-id="c8de1-127">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="c8de1-128">İstemci ilk dört girişimlerinin içinde başarıyla yeniden değil `HubConnection` geçiş olur `Disconnected` belirtin ve yangın <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> olay.</span><span class="sxs-lookup"><span data-stu-id="c8de1-128">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="c8de1-129">Bu bağlantıyı el ile yeniden başlatmak veya bağlantı kalıcı olarak kesildi kullanıcılara bildirmek deneme fırsatı sağlar.</span><span class="sxs-lookup"><span data-stu-id="c8de1-129">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="c8de1-130">Özel bir bağlantıyı kesmeden önce yeniden bağlanma denemesi sayısını yapılandırma veya yeniden zamanlamayı değiştirmek için `WithAutomaticReconnect` yeniden bağlanma girişimleri başlatmadan önce beklenecek milisaniye cinsinden gecikme değeri temsil eden sayı dizisi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="c8de1-130">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="c8de1-131">Yukarıdaki örnekte yapılandırır `HubConnection` hemen bağlantı kaybedildikten sonra yeniden bağlantılar denemesi başlatılacak.</span><span class="sxs-lookup"><span data-stu-id="c8de1-131">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="c8de1-132">Bu aynı zamanda varsayılan yapılandırması için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c8de1-132">This is also true for the default configuration.</span></span>

<span data-ttu-id="c8de1-133">İlk yeniden deneme başarısız olursa, ikinci yeniden bağlanma denemesi de 2 Varsayılan yapılandırmada gibi saniye beklemek yerine başlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="c8de1-133">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="c8de1-134">İkinci yeniden bağlanma denemesi başarısız olursa, üçüncü yeniden denemede yeniden gibi varsayılan yapılandırması olan 10 saniye içinde başlar.</span><span class="sxs-lookup"><span data-stu-id="c8de1-134">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="c8de1-135">Özel davranış daha sonra yeniden varsayılan davranışı hatası üçüncü yeniden bağlanma girişimi yapıldıktan sonra durdurarak kareninkinden.</span><span class="sxs-lookup"><span data-stu-id="c8de1-135">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="c8de1-136">Varsayılan yapılandırmasında yok olması bir daha yeniden denemesi başka bir 30 saniye.</span><span class="sxs-lookup"><span data-stu-id="c8de1-136">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="c8de1-137">Zamanlama ve otomatik sayısı daha fazla denetime yeniden bağlanma girişimleri, isterseniz `WithAutomaticReconnect` kabul eden bir nesneyi uygulama `IRetryPolicy` adlı tek bir yöntem olan arabirimi `NextRetryDelay`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-137">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="c8de1-138">`NextRetryDelay` tek bir bağımsız değişken alan türüyle `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-138">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="c8de1-139">`RetryContext` Üç özelliğe sahiptir: `PreviousRetryCount`, `ElapsedTime` ve `RetryReason` olduğu bir `long`, `TimeSpan` ve `Exception` sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="c8de1-139">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="c8de1-140">Yeniden bağlanma denemesi ilk önce her ikisini de `PreviousRetryCount` ve `ElapsedTime` sıfır olur ve `RetryReason` bağlantı kaybolmasına neden olan özel durum olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c8de1-140">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="c8de1-141">Başarısız tekrar deneme girişimleri sonra `PreviousRetryCount` biri tarafından artırılacaktır `ElapsedTime` şu ana kadar yeniden bağlanmayı harcanan süreyi yansıtacak şekilde güncelleştirilir ve `RetryReason` özel durum nedeniyle son yeniden bağlanma denemesi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="c8de1-141">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="c8de1-142">`NextRetryDelay` bir sonraki yeniden denemeden önce beklenecek süreyi temsil eden ya da bir TimeSpan döndürmelidir veya `null` varsa `HubConnection` yeniden bağlanmayı durdurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c8de1-142">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="c8de1-143">Alternatif olarak, istemci gösterildiği şekilde el ile yeniden bağlanacak kod yazabileceğiniz [el ile yeniden](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="c8de1-143">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="c8de1-144">El ile yeniden</span><span class="sxs-lookup"><span data-stu-id="c8de1-144">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="c8de1-145">Önce 3.0, SignalR için .NET istemci otomatik olarak yeniden değil.</span><span class="sxs-lookup"><span data-stu-id="c8de1-145">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="c8de1-146">İstemcinizi el ile yeniden kod yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c8de1-146">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="c8de1-147">Kullanım <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> olay yanıt bağlantı kesildi.</span><span class="sxs-lookup"><span data-stu-id="c8de1-147">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="c8de1-148">Örneğin, yeniden bağlanma otomatikleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c8de1-148">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="c8de1-149">`Closed` Olay gerektirir döndüren bir temsilci bir `Task`, zaman uyumsuz kod kullanmadan çalıştırılmasına olanak sağlayan `async void`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-149">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="c8de1-150">Temsilci imzasında karşılamak için bir `Closed` çalıştırmaları eş döndüren bir olay işleyicisi `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="c8de1-150">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="c8de1-151">Zaman uyumsuz desteği için temel nedeni olduğundan, bağlantı yeniden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c8de1-151">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="c8de1-152">Bağlantı bir zaman uyumsuz eylem başlangıcıdır.</span><span class="sxs-lookup"><span data-stu-id="c8de1-152">Starting a connection is an async action.</span></span>

<span data-ttu-id="c8de1-153">İçinde bir `Closed` bağlantı yeniden işleyicisi aşağıdaki örnekte gösterildiği gibi sunucu aşırı yüklemesini önlemek bazı rastgele gecikme bekleniyor dikkate alın:</span><span class="sxs-lookup"><span data-stu-id="c8de1-153">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c8de1-154">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="c8de1-154">Call hub methods from client</span></span>

<span data-ttu-id="c8de1-155">`InvokeAsync` hub yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="c8de1-155">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="c8de1-156">Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-156">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="c8de1-157">SignalR zaman uyumsuz, bu nedenle kullanın `async` ve `await` çağrıları yapılırken.</span><span class="sxs-lookup"><span data-stu-id="c8de1-157">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="c8de1-158">`InvokeAsync` Yöntemi döndürür bir `Task` sunucu yöntem döndürüldüğünde tamamlar.</span><span class="sxs-lookup"><span data-stu-id="c8de1-158">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="c8de1-159">Sonucu olarak dönüş değeri varsa, sağlanan `Task`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-159">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="c8de1-160">Sunucuda yöntemi tarafından oluşturulan özel durumlar bir hatalı üretmek `Task`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-160">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="c8de1-161">Kullanım `await` tamamlamak sunucu yöntemi için beklenecek söz dizimi ve `try...catch` sözdizimi hataları işlemek için.</span><span class="sxs-lookup"><span data-stu-id="c8de1-161">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="c8de1-162">`SendAsync` Yöntemi döndürür bir `Task` sunucuya ileti gönderildiğinde tamamlar.</span><span class="sxs-lookup"><span data-stu-id="c8de1-162">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="c8de1-163">Dönüş değeri bu sağlanan `Task` Metoda tamamlanana kadar beklemez.</span><span class="sxs-lookup"><span data-stu-id="c8de1-163">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="c8de1-164">İstemcide ileti gönderilirken karşılaşılan özel durumlar bir hatalı üretmek `Task`.</span><span class="sxs-lookup"><span data-stu-id="c8de1-164">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="c8de1-165">Kullanım `await` ve `try...catch` işlemek için söz dizimi hatalarını gönderme.</span><span class="sxs-lookup"><span data-stu-id="c8de1-165">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="c8de1-166">Azure SignalR hizmeti kullanıyorsanız, *sunucusuz modu*, bir istemciden hub yöntemlerini çağıramazsınız.</span><span class="sxs-lookup"><span data-stu-id="c8de1-166">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="c8de1-167">Daha fazla bilgi için [SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="c8de1-167">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c8de1-168">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="c8de1-168">Call client methods from hub</span></span>

<span data-ttu-id="c8de1-169">Hub'ı kullanarak çağırdığı yöntemleri tanımlamak `connection.On` yapı sonra ancak bağlantı başlatmadan önce.</span><span class="sxs-lookup"><span data-stu-id="c8de1-169">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="c8de1-170">Önceki kodda `connection.On` sunucu tarafı kod kullanarak çağırdığında çalıştıran `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c8de1-170">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="c8de1-171">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="c8de1-171">Error handling and logging</span></span>

<span data-ttu-id="c8de1-172">Bir try-catch deyiminin hatalarla işleyin.</span><span class="sxs-lookup"><span data-stu-id="c8de1-172">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="c8de1-173">İnceleme `Exception` nesnesine bir hata gerçekleştikten sonra gerçekleştirilecek uygun eylemi belirleyin.</span><span class="sxs-lookup"><span data-stu-id="c8de1-173">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="c8de1-174">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c8de1-174">Additional resources</span></span>

* [<span data-ttu-id="c8de1-175">Merkezler</span><span class="sxs-lookup"><span data-stu-id="c8de1-175">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c8de1-176">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="c8de1-176">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c8de1-177">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="c8de1-177">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="c8de1-178">Sunucusuz Azure SignalR hizmeti belgeleri</span><span class="sxs-lookup"><span data-stu-id="c8de1-178">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
