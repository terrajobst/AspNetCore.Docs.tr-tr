---
title: ASP.NET Core SignalR .NET istemcisi
author: tdykstra
description: ASP.NET Core SignalR .NET istemcisi hakkında bilgi
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373325"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="3ba3d-103">ASP.NET Core SignalR .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="3ba3d-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="3ba3d-104">Tarafından [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="3ba3d-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="3ba3d-105">ASP.NET Core SignalR .NET istemci kitaplığı, .NET uygulamalarından SignalR hub'ları ile iletişim kurmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-105">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="3ba3d-106">Xamarin, Visual Studio sürümü için özel gereksinimleri vardır.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-106">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="3ba3d-107">Daha fazla bilgi için [SignalR istemci 2.1.1 Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="3ba3d-107">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="3ba3d-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3ba3d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3ba3d-109">Bu makalede kod örneği ASP.NET Core SignalR .NET istemcinin kullandığı bir WPF uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-109">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="3ba3d-110">SignalR .NET istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="3ba3d-110">Install the SignalR .NET client package</span></span>

<span data-ttu-id="3ba3d-111">`Microsoft.AspNetCore.SignalR.Client` Paket, .NET istemcileri için SignalR hub'larını bağlama için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-111">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="3ba3d-112">İstemci kitaplığını yüklemek için aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="3ba3d-112">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="3ba3d-113">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="3ba3d-113">Connect to a hub</span></span>

<span data-ttu-id="3ba3d-114">Bir bağlantı kurmak için oluşturma bir `HubConnectionBuilder` ve çağrı `Build`.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="3ba3d-115">Hub'ı URL'si, protokolü, aktarım türü, günlük düzeyi, üst bilgiler ve diğer seçenekleri bir bağlantı oluşturulurken yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="3ba3d-116">Gerekli tüm seçenekler ekleyerek herhangi bir yapılandırma `HubConnectionBuilder` yöntemleri `Build`.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="3ba3d-117">Bağlantıyı başlatmak `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="3ba3d-118">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="3ba3d-118">Call hub methods from client</span></span>

<span data-ttu-id="3ba3d-119">`InvokeAsync` hub yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-119">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="3ba3d-120">Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-120">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="3ba3d-121">SignalR zaman uyumsuz, bu nedenle kullanın `async` ve `await` çağrıları yapılırken.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-121">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="3ba3d-122">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="3ba3d-122">Call client methods from hub</span></span>

<span data-ttu-id="3ba3d-123">Hub'ı kullanarak çağırdığı yöntemleri tanımlamak `connection.On` yapı sonra ancak bağlantı başlatmadan önce.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-123">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="3ba3d-124">Önceki kodda `connection.On` sunucu tarafı kod kullanarak çağırdığında çalıştıran `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-124">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="3ba3d-125">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="3ba3d-125">Error handling and logging</span></span>

<span data-ttu-id="3ba3d-126">Bir try-catch deyiminin hatalarla işleyin.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-126">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="3ba3d-127">İnceleme `Exception` nesnesine bir hata gerçekleştikten sonra gerçekleştirilecek uygun eylemi belirleyin.</span><span class="sxs-lookup"><span data-stu-id="3ba3d-127">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="3ba3d-128">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3ba3d-128">Additional resources</span></span>

* [<span data-ttu-id="3ba3d-129">Merkezler</span><span class="sxs-lookup"><span data-stu-id="3ba3d-129">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3ba3d-130">JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="3ba3d-130">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="3ba3d-131">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="3ba3d-131">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
