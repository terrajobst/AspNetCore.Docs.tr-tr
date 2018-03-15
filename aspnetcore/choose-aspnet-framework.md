---
title: "ASP.NET ve ASP.NET Core arasında seçim yapma"
author: rick-anderson
description: "ASP.NET ve ASP.NET Core arasında seçim yapma hakkında bilgi edinin."
manager: wpickett
ms.author: riande
ms.date: 03/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 3986e904d6670c451edc5c9338dc07e18d3c207d
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="70674-103">ASP.NET ve ASP.NET Core arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="70674-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="70674-104">ASP.NET web uygulaması olsun, oluşturduğunuz bir çözümü sizin için vardır: Windows Server hedefleme Kurumsal web uygulamaları ' küçük mikro Linux kapsayıcıları ve her şeyi arasında hedefleme.</span><span class="sxs-lookup"><span data-stu-id="70674-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="70674-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70674-105">ASP.NET Core</span></span>

<span data-ttu-id="70674-106">ASP.NET Core Windows, macOS ya da Linux modern, bulut tabanlı web uygulamaları oluşturmak için açık kaynak, platformlar arası bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="70674-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="70674-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="70674-107">ASP.NET</span></span>

<span data-ttu-id="70674-108">ASP.NET, kurumsal sınıf oluşturmak için gereken tüm hizmetleri Windows server tabanlı web uygulamalarında sağlayan olgun bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="70674-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web apps on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="70674-109">Hangisinin benim için en uygun mi?</span><span class="sxs-lookup"><span data-stu-id="70674-109">Which one is right for me?</span></span>

| <span data-ttu-id="70674-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70674-110">ASP.NET Core</span></span> | <span data-ttu-id="70674-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="70674-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="70674-112">Windows, macOS ya da Linux derleme</span><span class="sxs-lookup"><span data-stu-id="70674-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="70674-113">Windows için derleme</span><span class="sxs-lookup"><span data-stu-id="70674-113">Build for Windows</span></span>|
|<span data-ttu-id="70674-114">[Razor sayfalarının](xref:mvc/razor-pages/index) ASP.NET Core itibariyle bir Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır 2.x.</span><span class="sxs-lookup"><span data-stu-id="70674-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="70674-115">Ayrıca bkz. [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), ve [SignalR](xref:signalr/introduction-signalr-core).</span><span class="sxs-lookup"><span data-stu-id="70674-115">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction-signalr-core).</span></span>|<span data-ttu-id="70674-116">Kullanım [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), veya [Web sayfaları](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="70674-116">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="70674-117">Makine başına birden çok sürüm</span><span class="sxs-lookup"><span data-stu-id="70674-117">Multiple versions per machine</span></span>|<span data-ttu-id="70674-118">Makine başına bir sürüm</span><span class="sxs-lookup"><span data-stu-id="70674-118">One version per machine</span></span>|
|<span data-ttu-id="70674-119">Visual Studio ile geliştirme [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/), veya [Visual Studio Code](https://code.visualstudio.com/) C# veya F # kullanma</span><span class="sxs-lookup"><span data-stu-id="70674-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="70674-120">C#, VB ve F # kullanarak Visual Studio ile geliştirme</span><span class="sxs-lookup"><span data-stu-id="70674-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="70674-121">ASP.NET daha yüksek performans</span><span class="sxs-lookup"><span data-stu-id="70674-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="70674-122">İyi bir performans</span><span class="sxs-lookup"><span data-stu-id="70674-122">Good performance</span></span>|
|[<span data-ttu-id="70674-123">.NET Framework veya .NET çekirdeği çalışma zamanı seçin</span><span class="sxs-lookup"><span data-stu-id="70674-123">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="70674-124">.NET Framework çalışma zamanı kullanın</span><span class="sxs-lookup"><span data-stu-id="70674-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="70674-125">ASP.NET Core senaryoları</span><span class="sxs-lookup"><span data-stu-id="70674-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="70674-126">[Razor sayfalarının](xref:mvc/razor-pages/index) ASP.NET Core itibariyle bir Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır 2.x.</span><span class="sxs-lookup"><span data-stu-id="70674-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="70674-127">Web siteleri</span><span class="sxs-lookup"><span data-stu-id="70674-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="70674-128">API'leri</span><span class="sxs-lookup"><span data-stu-id="70674-128">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="70674-129">Gerçek zamanlı</span><span class="sxs-lookup"><span data-stu-id="70674-129">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="70674-130">ASP.NET senaryoları</span><span class="sxs-lookup"><span data-stu-id="70674-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="70674-131">Web siteleri</span><span class="sxs-lookup"><span data-stu-id="70674-131">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="70674-132">API'leri</span><span class="sxs-lookup"><span data-stu-id="70674-132">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="70674-133">Gerçek zamanlı</span><span class="sxs-lookup"><span data-stu-id="70674-133">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="70674-134">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="70674-134">Resources</span></span>

* [<span data-ttu-id="70674-135">ASP.NET giriş</span><span class="sxs-lookup"><span data-stu-id="70674-135">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="70674-136">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="70674-136">Introduction to ASP.NET Core</span></span>](xref:index)
