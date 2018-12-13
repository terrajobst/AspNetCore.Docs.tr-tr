---
title: Performans Tanılama araçları
author: mjrousos
description: ASP.NET Core uygulamalarında performans sorunlarını tanılamak için faydalı araçlar.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329215"
---
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="7118d-103">Performans Tanılama araçları</span><span class="sxs-lookup"><span data-stu-id="7118d-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="7118d-104">Tarafından [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="7118d-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="7118d-105">Bu makalede, ASP.NET Core performans sorunları tanılamaya yönelik araçları listeler.</span><span class="sxs-lookup"><span data-stu-id="7118d-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="7118d-106">Visual Studio tanılama araçları</span><span class="sxs-lookup"><span data-stu-id="7118d-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="7118d-107">[Profil oluşturma ve tanılama araçları](/visualstudio/profiling) Visual Studio'da yerleşik olarak bulunan performans sorunlarını araştırma başlatmak için iyi bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="7118d-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="7118d-108">Bu araçlar, güçlü ve Visual Studio geliştirme ortamından kullanmak uygun.</span><span class="sxs-lookup"><span data-stu-id="7118d-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="7118d-109">Araç, ASP.NET Core uygulamalarında CPU kullanımı, bellek kullanımı ve performans olayları analiz sağlar.</span><span class="sxs-lookup"><span data-stu-id="7118d-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span>

<span data-ttu-id="7118d-110">Daha fazla bilgi kullanılabilir [Visual Studio belgeleri](/visualstudio/profiling/profiling-overview).</span><span class="sxs-lookup"><span data-stu-id="7118d-110">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="7118d-111">Application Insights</span><span class="sxs-lookup"><span data-stu-id="7118d-111">Application Insights</span></span>

<span data-ttu-id="7118d-112">[Application Insights](/azure/application-insights/app-insights-overview) uygulamanız için ayrıntılı performans verileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="7118d-112">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="7118d-113">Application Insights bilgileri otomatik olarak toplar yanıt hızlarını, hata oranları, bağımlılık yanıt süreleri ve daha fazlası.</span><span class="sxs-lookup"><span data-stu-id="7118d-113">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="7118d-114">Application Insights, uygulamanızın özel olayları ve ölçümleri belirli oturum destekler.</span><span class="sxs-lookup"><span data-stu-id="7118d-114">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="7118d-115">Application Insights, çeşitli ortamlarda kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="7118d-115">Application Insights can be used in a variety environments:</span></span>

* <span data-ttu-id="7118d-116">Azure'da çalışması için en iyi duruma getirilmiş.</span><span class="sxs-lookup"><span data-stu-id="7118d-116">Optimized to work in Azure.</span></span>
* <span data-ttu-id="7118d-117">Üretim, geliştirme ve hazırlama çalışır.</span><span class="sxs-lookup"><span data-stu-id="7118d-117">Works in production, development, and staging.</span></span>
* <span data-ttu-id="7118d-118">Öğesinden yerel olarak çalıştığı [Visual Studio](/azure/application-insights/app-insights-visual-studio) veya diğer barındırma ortamlarında.</span><span class="sxs-lookup"><span data-stu-id="7118d-118">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="7118d-119">Daha fazla bilgi için [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="7118d-119">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="7118d-120">PerfView</span><span class="sxs-lookup"><span data-stu-id="7118d-120">PerfView</span></span>

<span data-ttu-id="7118d-121">[PerfView](https://github.com/Microsoft/perfview) .NET Performans sorunlarını tanılamak için özel olarak .NET ekibi tarafından oluşturulan bir Performans Analizi aracıdır.</span><span class="sxs-lookup"><span data-stu-id="7118d-121">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="7118d-122">PerfView CPU kullanımı, bellek ve GC davranışı, performans olayları ve duvar saati süresi analizini sağlar.</span><span class="sxs-lookup"><span data-stu-id="7118d-122">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="7118d-123">PerfView ve kullanmaya başlama hakkında daha fazla bilgi [PerfView video öğreticiler](http://channel9.msdn.com/Series/PerfView-Tutorial) veya Kullanıcı Kılavuzu Aracı'nda kullanılabilir okuyarak veya [github'da](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="7118d-123">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="perfcollect"></a><span data-ttu-id="7118d-124">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="7118d-124">PerfCollect</span></span>

<span data-ttu-id="7118d-125">PerfView bir .NET senaryoları için faydalı performans çözümleme aracı olsa da, Linux ortamlarında çalışan ASP.NET Core uygulamaları izlemeleri toplamak için kullanamazsınız yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="7118d-125">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="7118d-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) profil oluşturma araçları, yerel Linux kullanan bir bash komut dosyası ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) ve [LTTng](https://lttng.org/)) tarafından PerfView çözümlenebilir Linux'ta izlemeleri toplamak için.</span><span class="sxs-lookup"><span data-stu-id="7118d-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="7118d-127">PerfCollect, PerfView burada doğrudan kullanılamaz Linux ortamlarında performans sorunlarını gösterilmesi yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="7118d-127">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="7118d-128">Bunun yerine, PerfCollect izlemeleri ardından analiz .NET Core uygulamaları PerfView kullanan bir Windows bilgisayara toplayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7118d-128">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="7118d-129">Yükleme ve PerfCollect ile çalışmaya başlama hakkında daha fazla bilgi edinilebilir [github'da](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span><span class="sxs-lookup"><span data-stu-id="7118d-129">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>
