---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana örnekleri | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: b8ce2b40a19e0429f1ccedb03b8f829582652d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754452"
---
<a name="katana-samples"></a><span data-ttu-id="e1729-102">Katana örnekleri</span><span class="sxs-lookup"><span data-stu-id="e1729-102">Katana Samples</span></span>
====================
<span data-ttu-id="e1729-103">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e1729-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="e1729-104">Katana örnekleri</span><span class="sxs-lookup"><span data-stu-id="e1729-104">Katana Samples</span></span>

<span data-ttu-id="e1729-105">**Örnek ASP.NET yönlendiren** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="e1729-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="e1729-106">Bazı uygulamalarda, Asp.Net yol tablosundaki OWIN olmayan bileşenleri ile yan yana OWIN bileşenleri denetime isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e1729-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="e1729-107">Bu örnek MapOwinPath ve Microsoft.Owin.Host.SystemWeb tarafından sağlanan MapOwinRoute RouteCollection uzantı yöntemlerinin nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="e1729-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="e1729-108">**İşlem hatları örnek dallanma** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="e1729-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="e1729-109">OWIN istek işleme ardışık düzenleri doğrusal olması gerekmez, bunlar farklı şekillerde isteklerini işlemek için dal.</span><span class="sxs-lookup"><span data-stu-id="e1729-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="e1729-110">Bu örnek istek yollarını veya üst bilgileri gibi diğer istek verilerini temel alarak bir dallanma ardışık düzenini oluşturmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="e1729-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="e1729-111">Bu bileşenler Microsoft.Owin.Mapping nuget paketi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e1729-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="e1729-112">**Özel sunucu örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="e1729-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="e1729-113">Kendi kendine barındırma, özel bir OWIN sunucusu kullanmayı gösterir OWIN.</span><span class="sxs-lookup"><span data-stu-id="e1729-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="e1729-114">**Örnek katıştırılmış** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span><span class="sxs-lookup"><span data-stu-id="e1729-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="e1729-115">Bazı OWIN sunucuları içinde kendi işlem çalıştırılabilir (&quot;şirket içinde barındırılan&quot;).</span><span class="sxs-lookup"><span data-stu-id="e1729-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="e1729-116">Bu örnek Microsoft.Owin.Hosting nuget paketi tarafından sağlanan araçları kullanarak bir OWIN uygulamanın nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="e1729-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="e1729-117">**HelloWorld örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span><span class="sxs-lookup"><span data-stu-id="e1729-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="e1729-118">OWIN HTTP sunucusu, çeşitli sunucular arasında uygulama taşınabilirliği sağlayan API soyutlama ' dir.</span><span class="sxs-lookup"><span data-stu-id="e1729-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="e1729-119">Bu örnek gösterir kullanarak bir Hello World uygulaması yazma **basit sarmalayıcıları** bir web sunucusu üzerinde ham OWIN soyutlama ve çalışma ASP.NET gibi.</span><span class="sxs-lookup"><span data-stu-id="e1729-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="e1729-120">**Merhaba Dünya ham OWIN örneği** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span><span class="sxs-lookup"><span data-stu-id="e1729-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="e1729-121">Bu örnek, bir Hello World uygulaması kullanılarak yazılacak gösterilmiştir **ham** OWIN soyutlama ve Asp.Net gibi bir web sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="e1729-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="e1729-122">**SignalR örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span><span class="sxs-lookup"><span data-stu-id="e1729-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="e1729-123">OWIN kullanarak SignalR barındırma gösterilmektedir / Katana.</span><span class="sxs-lookup"><span data-stu-id="e1729-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="e1729-124">Kendi kendine barındırma SignalR hakkında daha fazla bilgi için bkz. [öğretici: SignalR barındırma](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="e1729-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="e1729-125">**Statik dosyalar örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="e1729-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="e1729-126">OWIN kullanarak statik dosyaları için HTTP isteklerini destekleyecek şekilde nasıl gösterir / Katana.</span><span class="sxs-lookup"><span data-stu-id="e1729-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="e1729-127">**Web API** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span><span class="sxs-lookup"><span data-stu-id="e1729-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="e1729-128">Bu örnek, IIS'de OWIN barındırma ve Web API OWIN ardışık düzenine eklemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e1729-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="e1729-129">**Web yuvası örnek** | [kaynak kodu](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span><span class="sxs-lookup"><span data-stu-id="e1729-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="e1729-130">Web yuvaları OWIN kullanarak desteklemek nasıl gösterir [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e1729-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
