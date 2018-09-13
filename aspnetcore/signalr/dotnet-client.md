---
title: ASP.NET Core SignalR .NET istemcisi
author: tdykstra
description: ASP.NET Core SignalR .NET istemcisi hakkında bilgi
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: ef84ede2ed45ddc3b64d4ce8f5bd0018a681faf6
ms.sourcegitcommit: 4db337bd47d70c06fff91000c58bc048a491ccec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44749327"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="bf124-103">ASP.NET Core SignalR .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="bf124-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="bf124-104">ASP.NET Core SignalR .NET istemci kitaplığı, .NET uygulamalarından SignalR hub'ları ile iletişim kurmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="bf124-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="bf124-105">Xamarin, Visual Studio sürümü için özel gereksinimleri vardır.</span><span class="sxs-lookup"><span data-stu-id="bf124-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="bf124-106">Daha fazla bilgi için [SignalR istemci 2.1.1 Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="bf124-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="bf124-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bf124-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bf124-108">Bu makalede kod örneği ASP.NET Core SignalR .NET istemcinin kullandığı bir WPF uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="bf124-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="bf124-109">SignalR .NET istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="bf124-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="bf124-110">`Microsoft.AspNetCore.SignalR.Client` Paket, .NET istemcileri için SignalR hub'larını bağlama için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="bf124-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="bf124-111">İstemci kitaplığını yüklemek için aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="bf124-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="bf124-112">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="bf124-112">Connect to a hub</span></span>

<span data-ttu-id="bf124-113">Bir bağlantı kurmak için oluşturma bir `HubConnectionBuilder` ve çağrı `Build`.</span><span class="sxs-lookup"><span data-stu-id="bf124-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="bf124-114">Hub'ı URL'si, protokolü, aktarım türü, günlük düzeyi, üst bilgiler ve diğer seçenekleri bir bağlantı oluşturulurken yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="bf124-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="bf124-115">Gerekli tüm seçenekler ekleyerek herhangi bir yapılandırma `HubConnectionBuilder` yöntemleri `Build`.</span><span class="sxs-lookup"><span data-stu-id="bf124-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="bf124-116">Bağlantıyı başlatmak `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="bf124-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="bf124-117">Tanıtıcı bağlantısı kesildi</span><span class="sxs-lookup"><span data-stu-id="bf124-117">Handle lost connection</span></span>

<span data-ttu-id="bf124-118">Kullanım <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> olay yanıt bağlantı kesildi.</span><span class="sxs-lookup"><span data-stu-id="bf124-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="bf124-119">Örneğin, yeniden bağlanma otomatikleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bf124-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="bf124-120">`Closed` Olay gerektirir döndüren bir temsilci bir `Task`, zaman uyumsuz kod kullanmadan çalıştırılmasına olanak sağlayan `async void`.</span><span class="sxs-lookup"><span data-stu-id="bf124-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="bf124-121">Temsilci imzasında karşılamak için bir `Closed` çalıştırmaları eş döndüren bir olay işleyicisi `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="bf124-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="bf124-122">Zaman uyumsuz desteği için temel nedeni olduğundan, bağlantı yeniden başlatabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bf124-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="bf124-123">Bağlantı bir zaman uyumsuz eylem başlangıcıdır.</span><span class="sxs-lookup"><span data-stu-id="bf124-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="bf124-124">İçinde bir `Closed` bağlantı yeniden işleyicisi aşağıdaki örnekte gösterildiği gibi sunucu aşırı yüklemesini önlemek bazı rastgele gecikme bekleniyor dikkate alın:</span><span class="sxs-lookup"><span data-stu-id="bf124-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="bf124-125">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="bf124-125">Call hub methods from client</span></span>

<span data-ttu-id="bf124-126">`InvokeAsync` hub yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="bf124-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="bf124-127">Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="bf124-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="bf124-128">SignalR zaman uyumsuz, bu nedenle kullanın `async` ve `await` çağrıları yapılırken.</span><span class="sxs-lookup"><span data-stu-id="bf124-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="bf124-129">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="bf124-129">Call client methods from hub</span></span>

<span data-ttu-id="bf124-130">Hub'ı kullanarak çağırdığı yöntemleri tanımlamak `connection.On` yapı sonra ancak bağlantı başlatmadan önce.</span><span class="sxs-lookup"><span data-stu-id="bf124-130">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="bf124-131">Önceki kodda `connection.On` sunucu tarafı kod kullanarak çağırdığında çalıştıran `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bf124-131">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="bf124-132">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="bf124-132">Error handling and logging</span></span>

<span data-ttu-id="bf124-133">Bir try-catch deyiminin hatalarla işleyin.</span><span class="sxs-lookup"><span data-stu-id="bf124-133">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="bf124-134">İnceleme `Exception` nesnesine bir hata gerçekleştikten sonra gerçekleştirilecek uygun eylemi belirleyin.</span><span class="sxs-lookup"><span data-stu-id="bf124-134">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="bf124-135">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bf124-135">Additional resources</span></span>

* [<span data-ttu-id="bf124-136">Merkezler</span><span class="sxs-lookup"><span data-stu-id="bf124-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="bf124-137">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="bf124-137">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="bf124-138">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="bf124-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
