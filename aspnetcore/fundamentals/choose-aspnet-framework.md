---
title: ASP.NET 4. x ve ASP.NET Core arasında seçim yapın
author: rick-anderson
description: ASP.NET Core vs. ASP.NET 4. x ve aralarında nasıl seçim yapılacağını açıklar.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/12/2019
no-loc:
- SignalR
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 8b1681476f96e8613f9461c507fbb7696f888cbc
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963619"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="a68f8-103">ASP.NET 4. x ve ASP.NET Core arasında seçim yapın</span><span class="sxs-lookup"><span data-stu-id="a68f8-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="a68f8-104">ASP.NET Core, ASP.NET 4. x ' in bir tasarlayadır.</span><span class="sxs-lookup"><span data-stu-id="a68f8-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="a68f8-105">Bu makalede aralarındaki farklar listelenir.</span><span class="sxs-lookup"><span data-stu-id="a68f8-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="a68f8-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a68f8-106">ASP.NET Core</span></span>

<span data-ttu-id="a68f8-107">ASP.NET Core, Windows, macOS veya Linux 'ta modern, bulut tabanlı Web uygulamaları oluşturmaya yönelik açık kaynaklı, platformlar arası bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="a68f8-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="a68f8-108">ASP.NET 4. x</span><span class="sxs-lookup"><span data-stu-id="a68f8-108">ASP.NET 4.x</span></span>

<span data-ttu-id="a68f8-109">ASP.NET 4. x, Windows 'ta kurumsal düzeyde, sunucu tabanlı Web uygulamaları oluşturmak için gereken hizmetleri sağlayan, yetişkinlere yönelik bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="a68f8-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="a68f8-110">Çerçeve seçimi</span><span class="sxs-lookup"><span data-stu-id="a68f8-110">Framework selection</span></span>

<span data-ttu-id="a68f8-111">Aşağıdaki tabloda ASP.NET Core ASP.NET 4. x ile karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="a68f8-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="a68f8-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a68f8-112">ASP.NET Core</span></span> | <span data-ttu-id="a68f8-113">ASP.NET 4. x</span><span class="sxs-lookup"><span data-stu-id="a68f8-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="a68f8-114">Windows, macOS veya Linux için derleme</span><span class="sxs-lookup"><span data-stu-id="a68f8-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="a68f8-115">Windows için derleme</span><span class="sxs-lookup"><span data-stu-id="a68f8-115">Build for Windows</span></span>|
|<span data-ttu-id="a68f8-116">[Razor Pages](xref:razor-pages/index) , bir Web kullanıcı arabirimini ASP.NET Core 2. x olarak oluşturmak için önerilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="a68f8-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="a68f8-117">Ayrıca bkz. [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api)ve [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="a68f8-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="a68f8-118">[Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), Web [kancaları](/aspnet/webhooks/)veya [Web sayfaları](/aspnet/web-pages) kullanın</span><span class="sxs-lookup"><span data-stu-id="a68f8-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="a68f8-119">Makine başına birden çok sürüm</span><span class="sxs-lookup"><span data-stu-id="a68f8-119">Multiple versions per machine</span></span>|<span data-ttu-id="a68f8-120">Makine başına bir sürüm</span><span class="sxs-lookup"><span data-stu-id="a68f8-120">One version per machine</span></span>|
|<span data-ttu-id="a68f8-121">C# Veya kullanarak [Visual Studio](https://visualstudio.microsoft.com/vs/), [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)veya [Visual Studio Code](https://code.visualstudio.com/) geliştirinF#</span><span class="sxs-lookup"><span data-stu-id="a68f8-121">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="a68f8-122">, Vb veya kullanarak C# [Visual Studio](https://visualstudio.microsoft.com/vs/) ile geliştirmeF#</span><span class="sxs-lookup"><span data-stu-id="a68f8-122">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/) using C#, VB, or F#</span></span>|
|<span data-ttu-id="a68f8-123">ASP.NET 4. x 'ten daha yüksek performans</span><span class="sxs-lookup"><span data-stu-id="a68f8-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="a68f8-124">İyi performans</span><span class="sxs-lookup"><span data-stu-id="a68f8-124">Good performance</span></span>|
|[<span data-ttu-id="a68f8-125">.NET Core çalışma zamanı kullan</span><span class="sxs-lookup"><span data-stu-id="a68f8-125">Use .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="a68f8-126">.NET Framework çalışma zamanını kullan</span><span class="sxs-lookup"><span data-stu-id="a68f8-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="a68f8-127">.NET Framework üzerinde ASP.NET Core 2. x desteği hakkında bilgi için bkz. [ASP.NET Core .NET Framework Hedefleme](xref:index#target-framework) .</span><span class="sxs-lookup"><span data-stu-id="a68f8-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="a68f8-128">ASP.NET Core senaryolar</span><span class="sxs-lookup"><span data-stu-id="a68f8-128">ASP.NET Core scenarios</span></span>

* [<span data-ttu-id="a68f8-129">Siteleriniz</span><span class="sxs-lookup"><span data-stu-id="a68f8-129">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="a68f8-130">GetVersionEx</span><span class="sxs-lookup"><span data-stu-id="a68f8-130">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="a68f8-131">Gerçek zamanlı</span><span class="sxs-lookup"><span data-stu-id="a68f8-131">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="a68f8-132">Azure 'a ASP.NET Core uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="a68f8-132">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="a68f8-133">ASP.NET 4. x senaryoları</span><span class="sxs-lookup"><span data-stu-id="a68f8-133">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="a68f8-134">Siteleriniz</span><span class="sxs-lookup"><span data-stu-id="a68f8-134">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="a68f8-135">GetVersionEx</span><span class="sxs-lookup"><span data-stu-id="a68f8-135">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="a68f8-136">Gerçek zamanlı</span><span class="sxs-lookup"><span data-stu-id="a68f8-136">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="a68f8-137">Azure 'da bir ASP.NET 4. x Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="a68f8-137">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="a68f8-138">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a68f8-138">Additional resources</span></span>

* [<span data-ttu-id="a68f8-139">ASP.NET 'e giriş</span><span class="sxs-lookup"><span data-stu-id="a68f8-139">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="a68f8-140">ASP.NET Core giriş</span><span class="sxs-lookup"><span data-stu-id="a68f8-140">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
