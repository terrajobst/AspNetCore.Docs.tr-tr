---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana örnekleri | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: a26cdf144d02f0b429994df9d7863163807ac7b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827278"
---
<a name="katana-samples"></a><span data-ttu-id="c20fe-102">Katana örnekleri</span><span class="sxs-lookup"><span data-stu-id="c20fe-102">Katana Samples</span></span>
====================
<span data-ttu-id="c20fe-103">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c20fe-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="c20fe-104">Katana örnekleri</span><span class="sxs-lookup"><span data-stu-id="c20fe-104">Katana Samples</span></span>

<span data-ttu-id="c20fe-105">**Örnek ASP.NET yönlendiren** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="c20fe-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="c20fe-106">Bazı uygulamalarda, Asp.Net yol tablosundaki OWIN olmayan bileşenleri ile yan yana OWIN bileşenleri denetime isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c20fe-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="c20fe-107">Bu örnek MapOwinPath ve Microsoft.Owin.Host.SystemWeb tarafından sağlanan MapOwinRoute RouteCollection uzantı yöntemlerinin nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c20fe-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="c20fe-108">**İşlem hatları örnek dallanma** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="c20fe-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="c20fe-109">OWIN istek işleme ardışık düzenleri doğrusal olması gerekmez, bunlar farklı şekillerde isteklerini işlemek için dal.</span><span class="sxs-lookup"><span data-stu-id="c20fe-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="c20fe-110">Bu örnek istek yollarını veya üst bilgileri gibi diğer istek verilerini temel alarak bir dallanma ardışık düzenini oluşturmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="c20fe-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="c20fe-111">Bu bileşenler Microsoft.Owin.Mapping nuget paketi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c20fe-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="c20fe-112">**Özel sunucu örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="c20fe-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="c20fe-113">Kendi kendine barındırma, özel bir OWIN sunucusu kullanmayı gösterir OWIN.</span><span class="sxs-lookup"><span data-stu-id="c20fe-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="c20fe-114">**Örnek katıştırılmış** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="c20fe-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="c20fe-115">Bazı OWIN sunucuları içinde kendi işlem çalıştırılabilir (&quot;şirket içinde barındırılan&quot;).</span><span class="sxs-lookup"><span data-stu-id="c20fe-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="c20fe-116">Bu örnek Microsoft.Owin.Hosting nuget paketi tarafından sağlanan araçları kullanarak bir OWIN uygulamanın nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="c20fe-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="c20fe-117">**HelloWorld örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="c20fe-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="c20fe-118">OWIN HTTP sunucusu, çeşitli sunucular arasında uygulama taşınabilirliği sağlayan API soyutlama ' dir.</span><span class="sxs-lookup"><span data-stu-id="c20fe-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="c20fe-119">Bu örnek gösterir kullanarak bir Hello World uygulaması yazma **basit sarmalayıcıları** bir web sunucusu üzerinde ham OWIN soyutlama ve çalışma ASP.NET gibi.</span><span class="sxs-lookup"><span data-stu-id="c20fe-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="c20fe-120">**Merhaba Dünya ham OWIN örneği** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="c20fe-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="c20fe-121">Bu örnek, bir Hello World uygulaması kullanılarak yazılacak gösterilmiştir **ham** OWIN soyutlama ve Asp.Net gibi bir web sunucusunda.</span><span class="sxs-lookup"><span data-stu-id="c20fe-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="c20fe-122">**SignalR örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="c20fe-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="c20fe-123">OWIN kullanarak SignalR barındırma gösterilmektedir / Katana.</span><span class="sxs-lookup"><span data-stu-id="c20fe-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="c20fe-124">Kendi kendine barındırma SignalR hakkında daha fazla bilgi için bkz. [öğretici: SignalR barındırma](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="c20fe-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="c20fe-125">**Statik dosyalar örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="c20fe-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="c20fe-126">OWIN kullanarak statik dosyaları için HTTP isteklerini destekleyecek şekilde nasıl gösterir / Katana.</span><span class="sxs-lookup"><span data-stu-id="c20fe-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="c20fe-127">**Web API** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="c20fe-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="c20fe-128">Bu örnek, IIS'de OWIN barındırma ve Web API OWIN ardışık düzenine eklemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="c20fe-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="c20fe-129">**Web yuvası örnek** | [kaynak kodu](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="c20fe-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="c20fe-130">Web yuvaları OWIN kullanarak desteklemek nasıl gösterir [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c20fe-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
