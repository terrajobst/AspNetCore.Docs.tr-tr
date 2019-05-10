---
title: ASP.NET Core SignalR .NET istemcisi
author: bradygaster
description: ASP.NET Core SignalR .NET istemcisi hakkında bilgi
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: b59af0f9c84a008f778709970dba2273abdfcd4f
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087711"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="b657a-103">ASP.NET Core SignalR .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="b657a-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="b657a-104">ASP.NET Core SignalR .NET istemci kitaplığı, .NET uygulamalarından SignalR hub'ları ile iletişim kurmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="b657a-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="b657a-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b657a-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b657a-106">Bu makalede kod örneği ASP.NET Core SignalR .NET istemcinin kullandığı bir WPF uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="b657a-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="b657a-107">SignalR .NET istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="b657a-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="b657a-108">`Microsoft.AspNetCore.SignalR.Client` Paket, .NET istemcileri için SignalR hub'larını bağlama için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b657a-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="b657a-109">İstemci kitaplığını yüklemek için aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="b657a-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="b657a-110">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="b657a-110">Connect to a hub</span></span>

<span data-ttu-id="b657a-111">Bir bağlantı kurmak için oluşturma bir `HubConnectionBuilder` ve çağrı `Build`.</span><span class="sxs-lookup"><span data-stu-id="b657a-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="b657a-112">Hub'ı URL'si, protokolü, aktarım türü, günlük düzeyi, üst bilgiler ve diğer seçenekleri bir bağlantı oluşturulurken yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="b657a-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="b657a-113">Gerekli tüm seçenekler ekleyerek herhangi bir yapılandırma `HubConnectionBuilder` yöntemleri `Build`.</span><span class="sxs-lookup"><span data-stu-id="b657a-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="b657a-114">Bağlantıyı başlatmak `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="b657a-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="b657a-115">Tanıtıcı bağlantısı kesildi</span><span class="sxs-lookup"><span data-stu-id="b657a-115">Handle lost connection</span></span>

<span data-ttu-id="b657a-116">Kullanım <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> olay yanıt bağlantı kesildi.</span><span class="sxs-lookup"><span data-stu-id="b657a-116">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="b657a-117">Örneğin, yeniden bağlanma otomatikleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b657a-117">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="b657a-118">`Closed` Olay gerektirir döndüren bir temsilci bir `Task`, zaman uyumsuz kod kullanmadan çalıştırılmasına olanak sağlayan `async void`.</span><span class="sxs-lookup"><span data-stu-id="b657a-118">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="b657a-119">Temsilci imzasında karşılamak için bir `Closed` çalıştırmaları eş döndüren bir olay işleyicisi `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="b657a-119">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="b657a-120">Zaman uyumsuz desteği için temel nedeni olduğundan, bağlantı yeniden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b657a-120">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="b657a-121">Bağlantı bir zaman uyumsuz eylem başlangıcıdır.</span><span class="sxs-lookup"><span data-stu-id="b657a-121">Starting a connection is an async action.</span></span>

<span data-ttu-id="b657a-122">İçinde bir `Closed` bağlantı yeniden işleyicisi aşağıdaki örnekte gösterildiği gibi sunucu aşırı yüklemesini önlemek bazı rastgele gecikme bekleniyor dikkate alın:</span><span class="sxs-lookup"><span data-stu-id="b657a-122">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="b657a-123">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="b657a-123">Call hub methods from client</span></span>

<span data-ttu-id="b657a-124">`InvokeAsync` hub yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="b657a-124">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="b657a-125">Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="b657a-125">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="b657a-126">SignalR zaman uyumsuz, bu nedenle kullanın `async` ve `await` çağrıları yapılırken.</span><span class="sxs-lookup"><span data-stu-id="b657a-126">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="b657a-127">`InvokeAsync` Yöntemi döndürür bir `Task` sunucu yöntem döndürüldüğünde tamamlar.</span><span class="sxs-lookup"><span data-stu-id="b657a-127">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="b657a-128">Sonucu olarak dönüş değeri varsa, sağlanan `Task`.</span><span class="sxs-lookup"><span data-stu-id="b657a-128">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="b657a-129">Sunucuda yöntemi tarafından oluşturulan özel durumlar bir hatalı üretmek `Task`.</span><span class="sxs-lookup"><span data-stu-id="b657a-129">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="b657a-130">Kullanım `await` tamamlamak sunucu yöntemi için beklenecek söz dizimi ve `try...catch` sözdizimi hataları işlemek için.</span><span class="sxs-lookup"><span data-stu-id="b657a-130">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="b657a-131">`SendAsync` Yöntemi döndürür bir `Task` sunucuya ileti gönderildiğinde tamamlar.</span><span class="sxs-lookup"><span data-stu-id="b657a-131">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="b657a-132">Dönüş değeri bu sağlanan `Task` Metoda tamamlanana kadar beklemez.</span><span class="sxs-lookup"><span data-stu-id="b657a-132">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="b657a-133">İstemcide ileti gönderilirken karşılaşılan özel durumlar bir hatalı üretmek `Task`.</span><span class="sxs-lookup"><span data-stu-id="b657a-133">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="b657a-134">Kullanım `await` ve `try...catch` işlemek için söz dizimi hatalarını gönderme.</span><span class="sxs-lookup"><span data-stu-id="b657a-134">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="b657a-135">Azure SignalR hizmeti kullanıyorsanız, *sunucusuz modu*, bir istemciden hub yöntemlerini çağıramazsınız.</span><span class="sxs-lookup"><span data-stu-id="b657a-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="b657a-136">Daha fazla bilgi için [SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="b657a-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="b657a-137">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="b657a-137">Call client methods from hub</span></span>

<span data-ttu-id="b657a-138">Hub'ı kullanarak çağırdığı yöntemleri tanımlamak `connection.On` yapı sonra ancak bağlantı başlatmadan önce.</span><span class="sxs-lookup"><span data-stu-id="b657a-138">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="b657a-139">Önceki kodda `connection.On` sunucu tarafı kod kullanarak çağırdığında çalıştıran `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b657a-139">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="b657a-140">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="b657a-140">Error handling and logging</span></span>

<span data-ttu-id="b657a-141">Bir try-catch deyiminin hatalarla işleyin.</span><span class="sxs-lookup"><span data-stu-id="b657a-141">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="b657a-142">İnceleme `Exception` nesnesine bir hata gerçekleştikten sonra gerçekleştirilecek uygun eylemi belirleyin.</span><span class="sxs-lookup"><span data-stu-id="b657a-142">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="b657a-143">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b657a-143">Additional resources</span></span>

* [<span data-ttu-id="b657a-144">Merkezler</span><span class="sxs-lookup"><span data-stu-id="b657a-144">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b657a-145">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="b657a-145">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="b657a-146">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="b657a-146">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="b657a-147">Sunucusuz Azure SignalR hizmeti belgeleri</span><span class="sxs-lookup"><span data-stu-id="b657a-147">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
