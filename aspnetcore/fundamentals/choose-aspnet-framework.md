---
title: Seçin arasında ASP.NET 4.x ve ASP.NET Core
author: rick-anderson
description: ASP.NET Core vs açıklar. ASP.NET 4.x ve bunlar arasında seçim yapma.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/15/2019
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 9e093e83a1f6367cbb244076a8351644244f9874
ms.sourcegitcommit: 7e00e8236ca4eabf058f07020a5a3882daf7564f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68239221"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="7323d-103">Seçin arasında ASP.NET 4.x ve ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7323d-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="7323d-104">ASP.NET Core, ASP.NET'in yeniden 4.x.</span><span class="sxs-lookup"><span data-stu-id="7323d-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="7323d-105">Bu makalede, aralarındaki farklılıklar listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="7323d-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="7323d-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7323d-106">ASP.NET Core</span></span>

<span data-ttu-id="7323d-107">ASP.NET Core Windows, macOS veya Linux'ta modern, bulut tabanlı web uygulamaları oluşturmaya yönelik açık kaynaklı, platformlar arası bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="7323d-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="7323d-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="7323d-108">ASP.NET 4.x</span></span>

<span data-ttu-id="7323d-109">ASP.NET 4.x, kurumsal sınıf, oluşturmak için gereken hizmetleri Windows server tabanlı web apps'de sağlayan olgun bir altyapısıdır.</span><span class="sxs-lookup"><span data-stu-id="7323d-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="7323d-110">Framework seçimi</span><span class="sxs-lookup"><span data-stu-id="7323d-110">Framework selection</span></span>

<span data-ttu-id="7323d-111">Aşağıdaki tabloda karşılaştırılmıştır ASP.NET Core, ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="7323d-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="7323d-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7323d-112">ASP.NET Core</span></span> | <span data-ttu-id="7323d-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="7323d-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="7323d-114">Windows, macOS veya Linux için derleme</span><span class="sxs-lookup"><span data-stu-id="7323d-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="7323d-115">Windows için derleme</span><span class="sxs-lookup"><span data-stu-id="7323d-115">Build for Windows</span></span>|
|<span data-ttu-id="7323d-116">[Razor sayfaları](xref:razor-pages/index) itibarıyla ASP.NET Core Web kullanıcı Arabirimi oluşturmak için önerilen yaklaşımdır 2.x.</span><span class="sxs-lookup"><span data-stu-id="7323d-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="7323d-117">Ayrıca bkz: [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), ve [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="7323d-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="7323d-118">Kullanım [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [Web kancaları](/aspnet/webhooks/), veya [Web sayfaları](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="7323d-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="7323d-119">Makine başına birden çok sürümü</span><span class="sxs-lookup"><span data-stu-id="7323d-119">Multiple versions per machine</span></span>|<span data-ttu-id="7323d-120">Makine başına bir sürümü</span><span class="sxs-lookup"><span data-stu-id="7323d-120">One version per machine</span></span>|
|<span data-ttu-id="7323d-121">Geliştirme [Visual Studio](https://visualstudio.microsoft.com/vs/), [Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/), veya [Visual Studio Code](https://code.visualstudio.com/) kullanarak C# veyaF#</span><span class="sxs-lookup"><span data-stu-id="7323d-121">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="7323d-122">Geliştirme [Visual Studio](https://visualstudio.microsoft.com/vs/) kullanarak C#, VB, veyaF#</span><span class="sxs-lookup"><span data-stu-id="7323d-122">Develop with [Visual Studio](https://visualstudio.microsoft.com/vs/) using C#, VB, or F#</span></span>|
|<span data-ttu-id="7323d-123">Daha yüksek performansa ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="7323d-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="7323d-124">İyi bir performans</span><span class="sxs-lookup"><span data-stu-id="7323d-124">Good performance</span></span>|
|[<span data-ttu-id="7323d-125">.NET Core çalışma zamanı kullanın</span><span class="sxs-lookup"><span data-stu-id="7323d-125">Use .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="7323d-126">.NET Framework çalışma zamanını kullanma</span><span class="sxs-lookup"><span data-stu-id="7323d-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="7323d-127">Bkz: [.NET Framework'ü hedefleyen ASP.NET Core](xref:index#target-framework) .NET Framework üzerinde ASP.NET Core 2.x desteği hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="7323d-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="7323d-128">ASP.NET Core senaryoları</span><span class="sxs-lookup"><span data-stu-id="7323d-128">ASP.NET Core scenarios</span></span>

* [<span data-ttu-id="7323d-129">Web siteleri</span><span class="sxs-lookup"><span data-stu-id="7323d-129">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="7323d-130">API'ler</span><span class="sxs-lookup"><span data-stu-id="7323d-130">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="7323d-131">Gerçek zamanlı</span><span class="sxs-lookup"><span data-stu-id="7323d-131">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="7323d-132">Azure'da ASP.NET Core uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="7323d-132">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="7323d-133">ASP.NET 4.x senaryoları</span><span class="sxs-lookup"><span data-stu-id="7323d-133">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="7323d-134">Web siteleri</span><span class="sxs-lookup"><span data-stu-id="7323d-134">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="7323d-135">API'ler</span><span class="sxs-lookup"><span data-stu-id="7323d-135">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="7323d-136">Gerçek zamanlı</span><span class="sxs-lookup"><span data-stu-id="7323d-136">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="7323d-137">Azure'da ASP.NET 4.x web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7323d-137">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="7323d-138">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7323d-138">Additional resources</span></span>

* [<span data-ttu-id="7323d-139">ASP.NET'e giriş</span><span class="sxs-lookup"><span data-stu-id="7323d-139">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="7323d-140">ASP.NET Core'a giriş</span><span class="sxs-lookup"><span data-stu-id="7323d-140">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
