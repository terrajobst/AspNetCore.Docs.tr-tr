---
title: ASP.NET Core SignalR .NET istemcisi
author: rachelappel
description: ASP.NET Core SignalR .NET istemcisi hakkında bilgi
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: faa4368988971a3e7fcdcd1b044971e16d70f19a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273301"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="8a773-103">ASP.NET Core SignalR .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="8a773-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="8a773-104">Tarafından [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="8a773-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="8a773-105">ASP.NET Core SignalR .NET istemci Xamarin, WPF, Windows Forms, konsol ve .NET Core uygulamalar tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8a773-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="8a773-106">Gibi [JavaScript istemci](xref:signalr/javascript-client), .NET istemci almak ve göndermek ve gerçek zamanlı olarak hub'ına iletileri almasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="8a773-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

<span data-ttu-id="8a773-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8a773-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8a773-108">Bu makaledeki kod örneği ASP.NET Core SignalR .NET istemcinin kullandığı bir WPF uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="8a773-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="8a773-109">SignalR .NET istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="8a773-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="8a773-110">`Microsoft.AspNetCore.SignalR.Client` Paket .NET istemcileri için SignalR hub'larını bağlamak gereklidir.</span><span class="sxs-lookup"><span data-stu-id="8a773-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="8a773-111">İstemci Kitaplığı'nı yüklemek için aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="8a773-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="8a773-112">Bir hub'a bağlanması</span><span class="sxs-lookup"><span data-stu-id="8a773-112">Connect to a hub</span></span>

<span data-ttu-id="8a773-113">Bir bağlantı kurmak için oluşturma bir `HubConnectionBuilder` ve arama `Build`.</span><span class="sxs-lookup"><span data-stu-id="8a773-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="8a773-114">Hub URL'si, protokolü, aktarım türü, günlük düzeyi, üstbilgiler ve diğer seçenekleri bağlantı oluşturulurken yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="8a773-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="8a773-115">Gerekli tüm seçenekler herhangi birini ekleyerek yapılandırma `HubConnectionBuilder` yöntemlerin içine `Build`.</span><span class="sxs-lookup"><span data-stu-id="8a773-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="8a773-116">Bağlantı ile başlangıç `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a773-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="8a773-117">İstemciden hub yöntemlerini çağırın</span><span class="sxs-lookup"><span data-stu-id="8a773-117">Call hub methods from client</span></span>

<span data-ttu-id="8a773-118">`InvokeAsync` hub'ına yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="8a773-118">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="8a773-119">Hub yönteminin adını ve hub yönteminin tanımlanan herhangi bir bağımsız değişken geçirmek `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a773-119">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="8a773-120">SignalR zaman uyumsuz ve bu nedenle kullanacağını `async` ve `await` çağrıları yapılırken.</span><span class="sxs-lookup"><span data-stu-id="8a773-120">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="8a773-121">İstemci hub'ından yöntemlerini çağırın</span><span class="sxs-lookup"><span data-stu-id="8a773-121">Call client methods from hub</span></span>

<span data-ttu-id="8a773-122">Hub'ı çağırır kullanarak yöntemlerini tanımlama `connection.On` yapı sonra ancak bağlantı başlatmadan önce.</span><span class="sxs-lookup"><span data-stu-id="8a773-122">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="8a773-123">Önceki kod `connection.On` sunucu tarafı kodu kullanarak çağırdığında çalışır `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8a773-123">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="8a773-124">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="8a773-124">Error handling and logging</span></span>

<span data-ttu-id="8a773-125">Try-catch deyimi hatalarla işleyin.</span><span class="sxs-lookup"><span data-stu-id="8a773-125">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="8a773-126">İnceleme `Exception` hata oluştuktan sonra gerçekleştirilecek uygun eylemi belirlemek için nesne.</span><span class="sxs-lookup"><span data-stu-id="8a773-126">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="8a773-127">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8a773-127">Additional resources</span></span>

* [<span data-ttu-id="8a773-128">Merkezler</span><span class="sxs-lookup"><span data-stu-id="8a773-128">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8a773-129">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="8a773-129">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="8a773-130">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="8a773-130">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)