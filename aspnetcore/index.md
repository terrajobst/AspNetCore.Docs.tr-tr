---
title: "ASP.NET Core’a Giriş"
author: rick-anderson
description: "ASP.NET Core ile ilgili temel bilgiler sağlanır."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 09/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: a075c63fddb9e28a1da37d4ef6647808a0dcb583
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="59266-104">ASP.NET Core’a Giriş</span><span class="sxs-lookup"><span data-stu-id="59266-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="59266-105">[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Shaun Luttin](https://twitter.com/dicshaunary) tarafından hazırlanmıştır</span><span class="sxs-lookup"><span data-stu-id="59266-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="59266-106">ASP.NET Core, modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, [açık kaynak](https://github.com/aspnet/home) bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="59266-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="59266-107">ASP.NET Core ile şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="59266-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="59266-108">Web uygulamaları ve hizmetleri, [IoT](https://www.microsoft.com/en-us/internet-of-things/) uygulamaları ve mobil arka uçlar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="59266-108">Build web apps and services, [IoT](https://www.microsoft.com/en-us/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="59266-109">Windows, macOS ve Linux üzerinde tercih ettiğiniz geliştirme araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="59266-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="59266-110">Buluta veya şirket içine dağıtın</span><span class="sxs-lookup"><span data-stu-id="59266-110">Deploy to the cloud or on-premises</span></span>
* <span data-ttu-id="59266-111">[.NET Core veya .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="59266-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="59266-112">Neden ASP.NET Core kullanılmalı?</span><span class="sxs-lookup"><span data-stu-id="59266-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="59266-113">Milyonlarca geliştirici, web uygulamaları oluşturmak için ASP.NET’i kullandı (ve kullanmaya devam ediyor).</span><span class="sxs-lookup"><span data-stu-id="59266-113">Millions of developers have used (and continue to use) ASP.NET to create web apps.</span></span> <span data-ttu-id="59266-114">ASP.NET Core, ASP.NET’in daha yalın ve modüler bir çerçeve elde edilmesini sağlayan mimari değişikliklerle yeniden tasarlanmış sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="59266-114">ASP.NET Core is a redesign of ASP.NET, with architectural changes that result in a leaner and modular framework.</span></span>

<span data-ttu-id="59266-115">ASP.NET Core aşağıdaki avantajları sağlar:</span><span class="sxs-lookup"><span data-stu-id="59266-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="59266-116">Web kullanıcı arabirimi ve web API’leri oluşturmak için birleşik bir öykü.</span><span class="sxs-lookup"><span data-stu-id="59266-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="59266-117">[Modern istemci tarafı çerçeveler](xref:client-side/index) ile geliştirme iş akışlarının tümleştirilmesi.</span><span class="sxs-lookup"><span data-stu-id="59266-117">Integration of [modern client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="59266-118">Bulutta kullanıma hazır, ortam tabanlı bir [yapılandırma sistemi](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="59266-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="59266-119">Yerleşik [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="59266-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="59266-120">Basit, yüksek performanslı ve modüler bir HTTP istek işlem hattı.</span><span class="sxs-lookup"><span data-stu-id="59266-120">A lightweight, high-performance, and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="59266-121">IIS’de veya kendi işleminizde barındırma olanağı.</span><span class="sxs-lookup"><span data-stu-id="59266-121">Ability to host on IIS or self-host in your own process.</span></span>
* <span data-ttu-id="59266-122">Gerçek yan yana uygulama sürümü oluşturmayı destekleyen [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="59266-122">Can run on [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server), which supports true side-by-side app versioning.</span></span>
* <span data-ttu-id="59266-123">Modern web geliştirmeyi basitleştiren araçlar.</span><span class="sxs-lookup"><span data-stu-id="59266-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="59266-124">Windows, macOS ve Linux üzerinde derleyip çalıştırma olanağı.</span><span class="sxs-lookup"><span data-stu-id="59266-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="59266-125">Açık kaynak ve topluluk odaklı.</span><span class="sxs-lookup"><span data-stu-id="59266-125">Open-source and community-focused.</span></span>

<span data-ttu-id="59266-126">ASP.NET Core tamamen [NuGet](https://www.nuget.org/) paketleri olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="59266-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="59266-127">Bu, uygulamanızı yalnızca gereksinim duyduğunuz NuGet paketlerini içerecek şekilde en iyi duruma getirmenize imkan sağlar.</span><span class="sxs-lookup"><span data-stu-id="59266-127">This allows you to optimize your app to include just the NuGet packages you need.</span></span> <span data-ttu-id="59266-128">Uygulama yüzeyinin dar olmasının daha sıkı güvenlik, daha az bakım ve gelişmiş performans gibi avantajları vardır.</span><span class="sxs-lookup"><span data-stu-id="59266-128">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="59266-129">ASP.NET Core MVC kullanarak web API'leri ve web kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="59266-129">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="59266-130">ASP.NET Core MVC, [web API'leri](xref:tutorials/index#building-web-apis) ve [web uygulamaları](xref:tutorials/index#building-web-applications) oluşturmanıza yardımcı olan özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="59266-130">ASP.NET Core MVC provides features that help you build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="59266-131">[Model-Görünüm-Denetleyici (MVC) deseni](xref:mvc/overview), web API'lerinin ve web uygulamalarının [sınanabilir](testing/index.md) olmasını sağlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="59266-131">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="59266-132">[Razor Sayfaları](xref:mvc/razor-pages/index) (2.0 sürümünde yeni), web kullanıcı arabirimi oluşturmayı daha kolay ve üretken bir hale getiren, sayfa tabanlı bir programlama modelidir.</span><span class="sxs-lookup"><span data-stu-id="59266-132">[Razor Pages](xref:mvc/razor-pages/index) (new in 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="59266-133">[Razor söz dizimi](xref:mvc/views/razor), [Razor Sayfaları](xref:mvc/razor-pages/index) ve [MVC Görünümleri](xref:mvc/views/overview) için üretken bir dil sağlar.</span><span class="sxs-lookup"><span data-stu-id="59266-133">[Razor syntax](xref:mvc/views/razor) provides a productive language for [Razor Pages](xref:mvc/razor-pages/index) and [MVC Views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="59266-134">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="59266-134">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="59266-135">[Birden çok veri biçimi ve içerik anlaşması](mvc/models/formatting.md) için sunulan yerleşik destek, web API'lerinizin tarayıcılar ve mobil cihazlar dahil olmak üzere birçok çeşit istemciye ulaşmasına imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="59266-135">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="59266-136">[Model Bağlama](xref:mvc/models/model-binding), HTTP isteklerinden alınan verileri otomatik olarak eylem metodu parametreleriyle eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="59266-136">[Model Binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="59266-137">[Model Doğrulama](xref:mvc/models/validation), otomatik olarak istemci ve sunucu taraflı doğrulamayı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="59266-137">[Model Validation](xref:mvc/models/validation) automatically performs client and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="59266-138">İstemci tarafı geliştirme</span><span class="sxs-lookup"><span data-stu-id="59266-138">Client-side development</span></span>

<span data-ttu-id="59266-139">ASP.NET Core ürünü, [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout) ve [Bootstrap](xref:client-side/bootstrap) dahil olmak üzere çeşitli istemci tarafı çerçevelerle sorunsuz olarak tümleştirilebilecek şekilde tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="59266-139">ASP.NET Core is designed to integrate seamlessly with a variety of client-side frameworks, including [AngularJS](xref:client-side/angular), [KnockoutJS](xref:client-side/knockout), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="59266-140">Daha fazla ayrıntı için bkz. [İstemci tarafı geliştirme](client-side/index.md).</span><span class="sxs-lookup"><span data-stu-id="59266-140">See [Client-side development](client-side/index.md) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59266-141">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="59266-141">Next steps</span></span>

<span data-ttu-id="59266-142">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="59266-142">For more information, see the following resources:</span></span>

* [<span data-ttu-id="59266-143">ASP.NET Core öğreticileri</span><span class="sxs-lookup"><span data-stu-id="59266-143">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="59266-144">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="59266-144">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="59266-145">[Haftalık ASP.NET topluluğu toplantısında](https://live.asp.net/), takımın ilerleme durumu ve planlarının ele alınmasının yanı sıra yeni blog gönderileri ve üçüncü taraf yazılımlar sunulur.</span><span class="sxs-lookup"><span data-stu-id="59266-145">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans and features new blogs and third-party software.</span></span>
