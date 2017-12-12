---
title: "ASP.NET ve ASP.NET Core arasında seçim yapma"
author: rick-anderson
description: "ASP.NET ve ASP.NET Core arasında seçim yapma hakkında bilgi edinin."
keywords: "ASP.NET Core, temelleri, genel bakış"
ms.author: riande
manager: wpickett
ms.date: 09/30/2017
ms.topic: article
ms.assetid: f0d17abf-3c69-413e-87fc-30780805e33f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 875064bd3437acc4e2a53220e19e86431d8c159b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="00dea-104">ASP.NET ve ASP.NET Core arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="00dea-104">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="00dea-105">ASP.NET web uygulaması oluşturuyorsanız olsun bir çözümü sizin için vardır: Windows Server hedefleme Kurumsal web uygulamalarından küçük mikro Linux kapsayıcıları ve her şeyi arasında hedefleme.</span><span class="sxs-lookup"><span data-stu-id="00dea-105">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="00dea-106">ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="00dea-106">ASP.NET Core</span></span>

<span data-ttu-id="00dea-107">ASP.NET Core Windows, macOS ya da Linux modern, bulut tabanlı web uygulamaları oluşturmak için açık kaynak, platformlar arası bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="00dea-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="00dea-108">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="00dea-108">ASP.NET</span></span>

<span data-ttu-id="00dea-109">ASP.NET, kurumsal sınıf oluşturmak için gereken tüm hizmetleri Windows server tabanlı web uygulamalarını sağlayan olgun bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="00dea-109">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="00dea-110">Hangisinin benim için en uygun mi?</span><span class="sxs-lookup"><span data-stu-id="00dea-110">Which one is right for me?</span></span>

| <span data-ttu-id="00dea-111">ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="00dea-111">ASP.NET Core</span></span> | <span data-ttu-id="00dea-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="00dea-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="00dea-113">Windows, macOS ya da Linux derleme</span><span class="sxs-lookup"><span data-stu-id="00dea-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="00dea-114">Windows için derleme</span><span class="sxs-lookup"><span data-stu-id="00dea-114">Build for Windows</span></span>|
|<span data-ttu-id="00dea-115">[Razor sayfalarının](xref:mvc/razor-pages/index) ASP.NET Core 2.0 ile bir Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="00dea-115">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="00dea-116">Ayrıca bkz. [MVC](xref:mvc/overview) ve [Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="00dea-116">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="00dea-117">Kullanım [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), veya [Web sayfaları](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="00dea-117">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="00dea-118">Makine başına birden çok sürüm</span><span class="sxs-lookup"><span data-stu-id="00dea-118">Multiple versions per machine</span></span>|<span data-ttu-id="00dea-119">Makine başına bir sürüm</span><span class="sxs-lookup"><span data-stu-id="00dea-119">One version per machine</span></span>|
|<span data-ttu-id="00dea-120">Visual Studio ile geliştirme [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/), veya [Visual Studio Code](https://code.visualstudio.com/) C# veya F # kullanma</span><span class="sxs-lookup"><span data-stu-id="00dea-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="00dea-121">C#, VB ve F # kullanarak Visual Studio ile geliştirme</span><span class="sxs-lookup"><span data-stu-id="00dea-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="00dea-122">ASP.NET daha yüksek performans</span><span class="sxs-lookup"><span data-stu-id="00dea-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="00dea-123">İyi bir performans</span><span class="sxs-lookup"><span data-stu-id="00dea-123">Good performance</span></span>|
|[<span data-ttu-id="00dea-124">.NET Framework veya .NET çekirdeği çalışma zamanı seçin</span><span class="sxs-lookup"><span data-stu-id="00dea-124">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="00dea-125">.NET Framework çalışma zamanı kullanın</span><span class="sxs-lookup"><span data-stu-id="00dea-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="00dea-126">ASP.NET Core senaryoları</span><span class="sxs-lookup"><span data-stu-id="00dea-126">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="00dea-127">[Razor sayfalarının](xref:mvc/razor-pages/index) ASP.NET Core 2.0 ile bir Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="00dea-127">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="00dea-128">Web siteleri</span><span class="sxs-lookup"><span data-stu-id="00dea-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="00dea-129">API'leri</span><span class="sxs-lookup"><span data-stu-id="00dea-129">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="00dea-130">ASP.NET senaryoları</span><span class="sxs-lookup"><span data-stu-id="00dea-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="00dea-131">Web siteleri</span><span class="sxs-lookup"><span data-stu-id="00dea-131">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="00dea-132">API'leri</span><span class="sxs-lookup"><span data-stu-id="00dea-132">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="00dea-133">Gerçek zamanlı</span><span class="sxs-lookup"><span data-stu-id="00dea-133">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="00dea-134">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="00dea-134">Resources</span></span>

* [<span data-ttu-id="00dea-135">ASP.NET giriş</span><span class="sxs-lookup"><span data-stu-id="00dea-135">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="00dea-136">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="00dea-136">Introduction to ASP.NET Core</span></span>](xref:index)
