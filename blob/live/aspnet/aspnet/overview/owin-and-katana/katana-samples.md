---
uid: aspnet/overview/owin-and-katana/katana-samples
title: "Katana örnekleri | Microsoft Docs"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 5540164bda8db31c4e78b49ecb7f7c573acca013
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="katana-samples"></a><span data-ttu-id="60290-102">Katana örnekleri</span><span class="sxs-lookup"><span data-stu-id="60290-102">Katana Samples</span></span>
====================
<span data-ttu-id="60290-103">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="60290-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="60290-104">Katana örnekleri</span><span class="sxs-lookup"><span data-stu-id="60290-104">Katana Samples</span></span>

<span data-ttu-id="60290-105">**ASP.NET yönlendirir örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="60290-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="60290-106">Bazı uygulamalarda OWIN olmayan bileşenleri yan yana Asp.Net rota tablosunda OWIN bileşenleri bağlanacağını isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="60290-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="60290-107">Bu örnek MapOwinPath ve Microsoft.Owin.Host.SystemWeb tarafından sağlanan MapOwinRoute RouteCollection genişletme yöntemleri kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="60290-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="60290-108">**Ardışık düzenleri örnek dallanma** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="60290-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="60290-109">OWIN istek işleme ardışık düzenleri doğrusal olması gerekmez, bunlar farklı şekillerde isteklerini işlemek için dal.</span><span class="sxs-lookup"><span data-stu-id="60290-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="60290-110">Bu örnek istek yollarını ya da diğer istek verileri üstbilgileri gibi göre dallanma ardışık düzenini oluşturmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="60290-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="60290-111">Bu bileşenler Microsoft.Owin.Mapping nuget paketi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="60290-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="60290-112">**Özel sunucu örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="60290-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="60290-113">Kendi kendini barındırdığında özel bir OWIN sunucusu kullanmayı gösterir OWIN.</span><span class="sxs-lookup"><span data-stu-id="60290-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="60290-114">**Örnek katıştırılmış** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="60290-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="60290-115">Bazı OWIN sunucuları kendi işlem içinde çalıştırabilirsiniz (&quot;kendi kendini barındıran&quot;).</span><span class="sxs-lookup"><span data-stu-id="60290-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="60290-116">Bu örnek Microsoft.Owin.Hosting nuget paketi tarafından sağlanan araçları kullanarak OWIN uygulamasının başlangıç gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="60290-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="60290-117">**HelloWorld örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="60290-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="60290-118">OWIN HTTP sunucusu çeşitli sunucular arasında uygulama taşınabilirliği sağlayan API soyutlama ' dir.</span><span class="sxs-lookup"><span data-stu-id="60290-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="60290-119">Bu örneği kullanarak bir Hello World uygulamasının nasıl yazılacağını gösterir **basit sarmalayıcıları** ham OWIN soyutlama ve Çalıştır bir web sunucusunda ister ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="60290-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="60290-120">**Hello World ham OWIN örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="60290-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="60290-121">Bu örnek uygulama kullanarak bir Hello World yazmak gösterilmiştir **ham** OWIN soyutlama ve Asp.Net gibi bir web sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="60290-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="60290-122">**SignalR örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="60290-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="60290-123">OWIN kullanarak SignalR kendini barındırma gösterilmektedir / Katana.</span><span class="sxs-lookup"><span data-stu-id="60290-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="60290-124">Kendi kendine barındırma SignalR hakkında daha fazla bilgi için bkz: [Öğreticisi: SignalR kendini barındırma](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="60290-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="60290-125">**Statik dosyaları örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="60290-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="60290-126">HTTP isteklerini OWIN kullanarak statik dosyaları için destek gösterilmektedir / Katana.</span><span class="sxs-lookup"><span data-stu-id="60290-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="60290-127">**Web API** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="60290-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="60290-128">Bu örnek, IIS'de OWIN barındırma ve Web API OWIN ardışık düzenine eklemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="60290-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="60290-129">**Web yuvası örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="60290-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="60290-130">Web yuvalarını kullanarak OWIN içinde desteklemek nasıl gösterir [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="60290-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
