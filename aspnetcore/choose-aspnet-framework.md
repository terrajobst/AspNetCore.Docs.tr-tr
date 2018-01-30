---
title: "ASP.NET ve ASP.NET Core arasında seçim yapma"
author: rick-anderson
description: "ASP.NET ve ASP.NET Core arasında seçim yapma hakkında bilgi edinin."
manager: wpickett
ms.author: riande
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 232e82ed66ff2363230ff09d435db1074c02b53b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="88038-103">ASP.NET ve ASP.NET Core arasında seçim yapma</span><span class="sxs-lookup"><span data-stu-id="88038-103">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="88038-104">ASP.NET web uygulaması oluşturuyorsanız olsun bir çözümü sizin için vardır: Windows Server hedefleme Kurumsal web uygulamalarından küçük mikro Linux kapsayıcıları ve her şeyi arasında hedefleme.</span><span class="sxs-lookup"><span data-stu-id="88038-104">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="88038-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88038-105">ASP.NET Core</span></span>

<span data-ttu-id="88038-106">ASP.NET Core Windows, macOS ya da Linux modern, bulut tabanlı web uygulamaları oluşturmak için açık kaynak, platformlar arası bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="88038-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="88038-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="88038-107">ASP.NET</span></span>

<span data-ttu-id="88038-108">ASP.NET, kurumsal sınıf oluşturmak için gereken tüm hizmetleri Windows server tabanlı web uygulamalarını sağlayan olgun bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="88038-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="88038-109">Hangisinin benim için en uygun mi?</span><span class="sxs-lookup"><span data-stu-id="88038-109">Which one is right for me?</span></span>

| <span data-ttu-id="88038-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88038-110">ASP.NET Core</span></span> | <span data-ttu-id="88038-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="88038-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="88038-112">Windows, macOS ya da Linux derleme</span><span class="sxs-lookup"><span data-stu-id="88038-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="88038-113">Windows için derleme</span><span class="sxs-lookup"><span data-stu-id="88038-113">Build for Windows</span></span>|
|<span data-ttu-id="88038-114">[Razor sayfalarının](xref:mvc/razor-pages/index) ASP.NET Core 2.0 ile bir Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="88038-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="88038-115">Ayrıca bkz. [MVC](xref:mvc/overview) ve [Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="88038-115">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="88038-116">Kullanım [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), veya [Web sayfaları](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="88038-116">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="88038-117">Makine başına birden çok sürüm</span><span class="sxs-lookup"><span data-stu-id="88038-117">Multiple versions per machine</span></span>|<span data-ttu-id="88038-118">Makine başına bir sürüm</span><span class="sxs-lookup"><span data-stu-id="88038-118">One version per machine</span></span>|
|<span data-ttu-id="88038-119">Visual Studio ile geliştirme [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/), veya [Visual Studio Code](https://code.visualstudio.com/) C# veya F # kullanma</span><span class="sxs-lookup"><span data-stu-id="88038-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="88038-120">C#, VB ve F # kullanarak Visual Studio ile geliştirme</span><span class="sxs-lookup"><span data-stu-id="88038-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="88038-121">ASP.NET daha yüksek performans</span><span class="sxs-lookup"><span data-stu-id="88038-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="88038-122">İyi bir performans</span><span class="sxs-lookup"><span data-stu-id="88038-122">Good performance</span></span>|
|[<span data-ttu-id="88038-123">.NET Framework veya .NET çekirdeği çalışma zamanı seçin</span><span class="sxs-lookup"><span data-stu-id="88038-123">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="88038-124">.NET Framework çalışma zamanı kullanın</span><span class="sxs-lookup"><span data-stu-id="88038-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="88038-125">ASP.NET Core senaryoları</span><span class="sxs-lookup"><span data-stu-id="88038-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="88038-126">[Razor sayfalarının](xref:mvc/razor-pages/index) ASP.NET Core 2.0 ile bir Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="88038-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="88038-127">Web siteleri</span><span class="sxs-lookup"><span data-stu-id="88038-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="88038-128">API'leri</span><span class="sxs-lookup"><span data-stu-id="88038-128">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="88038-129">ASP.NET senaryoları</span><span class="sxs-lookup"><span data-stu-id="88038-129">ASP.NET scenarios</span></span>

* [<span data-ttu-id="88038-130">Web siteleri</span><span class="sxs-lookup"><span data-stu-id="88038-130">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="88038-131">API'leri</span><span class="sxs-lookup"><span data-stu-id="88038-131">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="88038-132">Gerçek zamanlı</span><span class="sxs-lookup"><span data-stu-id="88038-132">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="88038-133">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="88038-133">Resources</span></span>

* [<span data-ttu-id="88038-134">ASP.NET giriş</span><span class="sxs-lookup"><span data-stu-id="88038-134">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="88038-135">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="88038-135">Introduction to ASP.NET Core</span></span>](xref:index)
