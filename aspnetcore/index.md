---
title: ASP.NET Core’a Giriş
author: rick-anderson
description: Modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, açık kaynak bir çerçeve olan ASP.NET Core’a giriş yapın.
ms.author: riande
ms.date: 9/28/2018
uid: index
ms.openlocfilehash: 3bb86fa255548ff66592ac14c1020e0c6b47959c
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391163"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="be3c3-103">ASP.NET Core’a Giriş</span><span class="sxs-lookup"><span data-stu-id="be3c3-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="be3c3-104">[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Shaun Luttin](https://twitter.com/dicshaunary) tarafından hazırlanmıştır</span><span class="sxs-lookup"><span data-stu-id="be3c3-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="be3c3-105">ASP.NET Core, modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, [açık kaynak](https://github.com/aspnet/home) bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="be3c3-106">ASP.NET Core ile şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="be3c3-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="be3c3-107">Web uygulamaları ve hizmetleri, [IoT](https://www.microsoft.com/internet-of-things/) uygulamaları ve mobil arka uçlar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="be3c3-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="be3c3-108">Windows, macOS ve Linux üzerinde tercih ettiğiniz geliştirme araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="be3c3-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="be3c3-109">Buluta veya şirket içine dağıtın.</span><span class="sxs-lookup"><span data-stu-id="be3c3-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="be3c3-110">[.NET Core veya .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="be3c3-110">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="be3c3-111">Neden ASP.NET Core kullanılmalı?</span><span class="sxs-lookup"><span data-stu-id="be3c3-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="be3c3-112">Milyonlarca geliştirici, web uygulamaları oluşturmak için [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) kullandı (ve kullanmaya devam ediyor).</span><span class="sxs-lookup"><span data-stu-id="be3c3-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="be3c3-113">ASP.NET Core, ASP.NET 4.x sürümünün daha yalın, daha modüler bir çerçeve elde edilmesini sağlayan mimari değişikliklerle yeniden tasarlanmış halidir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="be3c3-114">ASP.NET Core MVC kullanarak web API'leri ve web kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="be3c3-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="be3c3-115">ASP.NET Core MVC, [web API’leri](xref:tutorials/index#build-web-apis) ve [web uygulamaları](xref:tutorials/index#build-web-apps) oluşturmaya yönelik özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="be3c3-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#build-web-apis) and [web apps](xref:tutorials/index#build-web-apps):</span></span>

* <span data-ttu-id="be3c3-116">[Model-Görünüm-Denetleyici (MVC) deseni](xref:mvc/overview), web API'lerinin ve web uygulamalarının [sınanabilir](xref:test/index) olmasını sağlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="be3c3-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](xref:test/index).</span></span>
* <span data-ttu-id="be3c3-117">[Razor Sayfaları](xref:razor-pages/index) (ASP.NET Core 2.0 sürümünde yeni), web kullanıcı arabirimi oluşturmayı daha kolay ve üretken bir hale getiren, sayfa tabanlı bir programlama modelidir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-117">[Razor Pages](xref:razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="be3c3-118">[Razor işaretlemesi](xref:mvc/views/razor), [Razor Sayfaları](xref:razor-pages/index) ve [MVC görünümleri](xref:mvc/views/overview) için üretken bir söz dizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="be3c3-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="be3c3-119">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="be3c3-120">[Birden çok veri biçimi ve içerik anlaşması](xref:web-api/advanced/formatting) için sunulan yerleşik destek, web API'lerinizin tarayıcılar ve mobil cihazlar dahil olmak üzere birçok çeşit istemciye ulaşmasına imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="be3c3-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="be3c3-121">[Model bağlama](xref:mvc/models/model-binding), HTTP isteklerinden alınan verileri otomatik olarak eylem metodu parametreleriyle eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="be3c3-122">[Model doğrulama](xref:mvc/models/validation), otomatik olarak istemci ve sunucu tarafı doğrulama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="be3c3-123">İstemci tarafı geliştirme</span><span class="sxs-lookup"><span data-stu-id="be3c3-123">Client-side development</span></span>

<span data-ttu-id="be3c3-124">ASP.NET Core, popüler istemci tarafı çerçeveler ve kitaplıklar ([Angular](xref:spa/angular), [React](xref:spa/react) ve [Bootstrap](https://getbootstrap.com/) dahil) ile sorunsuz bir şekilde tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="be3c3-125">Daha fazla bilgi için bkz. [İstemci tarafı geliştirme](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="be3c3-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="be3c3-126">.NET Framework'ü hedefleyen ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be3c3-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="be3c3-127">ASP.NET Core, .NET Core'u veya .NET Framework'ü hedefleyebilir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-127">ASP.NET Core can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="be3c3-128">.NET Framework'ü hedefleyen ASP.NET Core uygulamaları platformlar arası çalışmaz; bunlar yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="be3c3-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="be3c3-129">ASP.NET Core'da .NET Framework hedeflemesi desteğini kaldırmaya yönelik bir plan yoktur.</span><span class="sxs-lookup"><span data-stu-id="be3c3-129">There are no plans to remove support for targeting .NET Framework in ASP.NET Core.</span></span> <span data-ttu-id="be3c3-130">Genel olarak, ASP.NET Core [.NET Standard](/dotnet/standard/net-standard) kitaplıklarından oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="be3c3-130">Generally, ASP.NET Core is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="be3c3-131">.NET Standard 2.0 ile yazılmış uygulamalar .NET Standard 2.0'ın desteklendiği her yerde çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="be3c3-131">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="be3c3-132">ASP.NET Core 2.x, .NET Standard 2.0 ile uyumlu .NET Framework sürümlerinde desteklenir:</span><span class="sxs-lookup"><span data-stu-id="be3c3-132">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="be3c3-133">.NET Framework 4.7.1 ve üzeri önemle tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-133">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="be3c3-134">.NET Framework 4.6.1 ve üzeri.</span><span class="sxs-lookup"><span data-stu-id="be3c3-134">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="be3c3-135">.NET Core hedeflemesinin çeşitli avantajları vardır ve bu avantajlar her yeni sürümle birlikte artmaktadır.</span><span class="sxs-lookup"><span data-stu-id="be3c3-135">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="be3c3-136">.NET Framework'e göre .NET Core'un bazı avantajları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="be3c3-136">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="be3c3-137">Platformlar arası.</span><span class="sxs-lookup"><span data-stu-id="be3c3-137">Cross-platform.</span></span> <span data-ttu-id="be3c3-138">macOS, Linux ve Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="be3c3-138">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="be3c3-139">Geliştirilmiş performans</span><span class="sxs-lookup"><span data-stu-id="be3c3-139">Improved performance</span></span>
* <span data-ttu-id="be3c3-140">Yan yana sürüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="be3c3-140">Side-by-side versioning</span></span>
* <span data-ttu-id="be3c3-141">Yeni API'ler</span><span class="sxs-lookup"><span data-stu-id="be3c3-141">New APIs</span></span>
* <span data-ttu-id="be3c3-142">Açık kaynak</span><span class="sxs-lookup"><span data-stu-id="be3c3-142">Open source</span></span>

<span data-ttu-id="be3c3-143">.NET Framework ile .NET Core arasındaki API açığını kapatmak için çok çalışıyoruz.</span><span class="sxs-lookup"><span data-stu-id="be3c3-143">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="be3c3-144">[Windows Uyumluluk Paketi](/dotnet/core/porting/windows-compat-pack), .NET Core'da yalnızca Windows'a yönelik binlerce API'yi kullanıma sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="be3c3-144">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="be3c3-145">Bu API'ler .NET Core 1.x'te sağlanmamıştı.</span><span class="sxs-lookup"><span data-stu-id="be3c3-145">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be3c3-146">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="be3c3-146">Next steps</span></span>

<span data-ttu-id="be3c3-147">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="be3c3-147">For more information, see the following resources:</span></span>

* [<span data-ttu-id="be3c3-148">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="be3c3-148">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="be3c3-149">ASP.NET Core öğreticileri</span><span class="sxs-lookup"><span data-stu-id="be3c3-149">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="be3c3-150">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="be3c3-150">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="be3c3-151">[Haftalık ASP.NET topluluğu toplantısında](https://live.asp.net/), takımın ilerleme durumu ve planları ele alınır.</span><span class="sxs-lookup"><span data-stu-id="be3c3-151">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="be3c3-152">Yeni bloglara ve üçüncü taraf yazılıma yer verilir.</span><span class="sxs-lookup"><span data-stu-id="be3c3-152">It features new blogs and third-party software.</span></span>
