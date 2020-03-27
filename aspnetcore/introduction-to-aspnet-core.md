---
title: ASP.NET Core’a Giriş
author: rick-anderson
description: Modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, açık kaynak bir çerçeve olan ASP.NET Core’a giriş yapın.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: index
ms.openlocfilehash: fd7fa9dd70502f51222e457dd887ef668d377278
ms.sourcegitcommit: 4b166b49ec557a03f99f872dd069ca5e56faa524
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366661"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="cd0c0-103">ASP.NET Core’a Giriş</span><span class="sxs-lookup"><span data-stu-id="cd0c0-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="cd0c0-104">[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Shaun Luttin](https://twitter.com/dicshaunary) tarafından hazırlanmıştır</span><span class="sxs-lookup"><span data-stu-id="cd0c0-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="cd0c0-105">ASP.NET Core, modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, [açık kaynak](https://github.com/aspnet/home) bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="cd0c0-106">ASP.NET Core ile şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="cd0c0-107">Web uygulamaları ve hizmetleri, [IoT](https://www.microsoft.com/internet-of-things/) uygulamaları ve mobil arka uçlar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="cd0c0-108">Windows, macOS ve Linux üzerinde tercih ettiğiniz geliştirme araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="cd0c0-109">Buluta veya şirket içine dağıtın.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="cd0c0-110">[.NET Core veya .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-choose-aspnet-core"></a><span data-ttu-id="cd0c0-111">Neden ASP.NET Core seçmelisiniz?</span><span class="sxs-lookup"><span data-stu-id="cd0c0-111">Why choose ASP.NET Core?</span></span>

<span data-ttu-id="cd0c0-112">Milyonlarca geliştirici, Web uygulamaları oluşturmak için [ASP.NET 4. x](/aspnet/overview) kullanır veya kullandı.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-112">Millions of developers use or have used [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="cd0c0-113">ASP.NET Core, ASP.NET 4.x sürümünün daha yalın, daha modüler bir çerçeve elde edilmesini sağlayan mimari değişikliklerle yeniden tasarlanmış halidir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="cd0c0-114">ASP.NET Core MVC kullanarak web API'leri ve web kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd0c0-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="cd0c0-115">ASP.NET Core MVC, [web API’leri](xref:tutorials/first-web-api) ve [web uygulamaları](xref:tutorials/razor-pages/index) oluşturmaya yönelik özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="cd0c0-116">[Model-Görünüm-Denetleyici (MVC) deseni](xref:mvc/overview), web API'lerinin ve web uygulamalarının sınanabilir olmasını sağlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="cd0c0-117">[Razor Pages](xref:razor-pages/index), web kullanıcı arabirimi oluşturmayı daha kolay ve üretken bir hale getiren, sayfa tabanlı bir programlama modelidir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="cd0c0-118">[Razor işaretlemesi](xref:mvc/views/razor), [Razor Sayfaları](xref:razor-pages/index) ve [MVC görünümleri](xref:mvc/views/overview) için üretken bir söz dizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="cd0c0-119">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="cd0c0-120">[Birden çok veri biçimi ve içerik anlaşması](xref:web-api/advanced/formatting) için sunulan yerleşik destek, web API'lerinizin tarayıcılar ve mobil cihazlar dahil olmak üzere birçok çeşit istemciye ulaşmasına imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="cd0c0-121">[Model bağlama](xref:mvc/models/model-binding), HTTP isteklerinden alınan verileri otomatik olarak eylem metodu parametreleriyle eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="cd0c0-122">[Model doğrulama](xref:mvc/models/validation), otomatik olarak istemci ve sunucu tarafı doğrulama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="cd0c0-123">İstemci tarafı geliştirme</span><span class="sxs-lookup"><span data-stu-id="cd0c0-123">Client-side development</span></span>

<span data-ttu-id="cd0c0-124">ASP.NET Core, [Blazor](xref:blazor/index), [angular](xref:spa/angular), [tepki](xref:spa/react)verme ve [önyükleme](https://getbootstrap.com/)gibi popüler istemci tarafı çerçeveleri ve kitaplıkları ile sorunsuz bir şekilde tümleşir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Blazor](xref:blazor/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="cd0c0-125">Daha fazla bilgi için bkz. *istemci tarafı geliştirme*altındaki <xref:blazor/index> ve ilgili konular.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-125">For more information, see <xref:blazor/index> and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="cd0c0-126">.NET Framework'ü hedefleyen ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cd0c0-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="cd0c0-127">ASP.NET Core 2.x, .NET Core'u veya .NET Framework'ü hedefleyebilir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="cd0c0-128">.NET Framework'ü hedefleyen ASP.NET Core uygulamaları platformlar arası çalışmaz; bunlar yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="cd0c0-129">Genel olarak, ASP.NET Core 2.x [.NET Standard](/dotnet/standard/net-standard) kitaplıklarından oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="cd0c0-130">.NET Standard 2,0 ile yazılan kitaplıklar .NET Standard 2,0 ' i uygulayan herhangi bir [.NET platformunda](/dotnet/standard/net-standard#net-implementation-support)çalışır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-130">Libraries written with .NET Standard 2.0 run on any [.NET platform that implements .NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support).</span></span>

<span data-ttu-id="cd0c0-131">ASP.NET Core 2. x, .NET Standard 2,0 ' i uygulayan .NET Framework sürümlerinde desteklenir:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-131">ASP.NET Core 2.x is supported on .NET Framework versions that implement .NET Standard 2.0:</span></span>

* <span data-ttu-id="cd0c0-132">.NET Framework en son sürüm kesinlikle önerilir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-132">.NET Framework latest version is strongly recommended.</span></span>
* <span data-ttu-id="cd0c0-133">.NET Framework 4.6.1 ve üzeri.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="cd0c0-134">ASP.NET Core 3.0 ve üzeri yalnızca .NET Core’da çalışır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="cd0c0-135">Bu değişiklik hakkında daha fazla bilgi için bkz. [ASP.NET Core 3.0’daki değişikliklere ilk bakış](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="cd0c0-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="cd0c0-136">.NET Core hedeflemesinin çeşitli avantajları vardır ve bu avantajlar her yeni sürümle birlikte artmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="cd0c0-137">.NET Framework'e göre .NET Core'un bazı avantajları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="cd0c0-138">Platformlar arası.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-138">Cross-platform.</span></span> <span data-ttu-id="cd0c0-139">macOS, Linux ve Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="cd0c0-140">Geliştirilmiş performans</span><span class="sxs-lookup"><span data-stu-id="cd0c0-140">Improved performance</span></span>
* [<span data-ttu-id="cd0c0-141">Yan yana sürüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd0c0-141">Side-by-side versioning</span></span>](/dotnet/standard/choosing-core-framework-server#a-need-for-side-by-side-of-net-versions-per-application-level)
* <span data-ttu-id="cd0c0-142">Yeni API'ler</span><span class="sxs-lookup"><span data-stu-id="cd0c0-142">New APIs</span></span>
* <span data-ttu-id="cd0c0-143">Açık kaynak</span><span class="sxs-lookup"><span data-stu-id="cd0c0-143">Open source</span></span>

<span data-ttu-id="cd0c0-144">.NET Framework ile .NET Core arasındaki API açığını kapatmak için çok çalışıyoruz.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="cd0c0-145">[Windows Uyumluluk Paketi](/dotnet/core/porting/windows-compat-pack), .NET Core'da yalnızca Windows'a yönelik binlerce API'yi kullanıma sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="cd0c0-146">Bu API'ler .NET Core 1.x'te sağlanmamıştı.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="cd0c0-147">Önerilen öğrenme yolu</span><span class="sxs-lookup"><span data-stu-id="cd0c0-147">Recommended learning path</span></span>

<span data-ttu-id="cd0c0-148">ASP.NET Core uygulamaları geliştirmeye başlamak için şu öğreticileri ve makaleleri takip etmenizi öneririz:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="cd0c0-149">Geliştirmek veya yönetmek istediğiniz uygulama türüne yönelik bir öğreticiyi takip edin:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="cd0c0-150">Uygulama türü</span><span class="sxs-lookup"><span data-stu-id="cd0c0-150">App type</span></span>  |<span data-ttu-id="cd0c0-151">Senaryo</span><span class="sxs-lookup"><span data-stu-id="cd0c0-151">Scenario</span></span>  |<span data-ttu-id="cd0c0-152">Öğretici</span><span class="sxs-lookup"><span data-stu-id="cd0c0-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="cd0c0-153">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="cd0c0-153">Web app</span></span>                   | <span data-ttu-id="cd0c0-154">Yeni proje geliştirmek için</span><span class="sxs-lookup"><span data-stu-id="cd0c0-154">For new development</span></span>        |[<span data-ttu-id="cd0c0-155">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="cd0c0-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="cd0c0-156">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="cd0c0-156">Web app</span></span>                   | <span data-ttu-id="cd0c0-157">MVC uygulaması yönetmek için</span><span class="sxs-lookup"><span data-stu-id="cd0c0-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="cd0c0-158">MVC ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="cd0c0-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="cd0c0-159">Web API</span><span class="sxs-lookup"><span data-stu-id="cd0c0-159">Web API</span></span>                   |                            |<span data-ttu-id="cd0c0-160">[Web API’si oluşturma](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="cd0c0-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="cd0c0-161">Gerçek zamanlı uygulama</span><span class="sxs-lookup"><span data-stu-id="cd0c0-161">Real-time app</span></span>             |                            |[<span data-ttu-id="cd0c0-162">SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="cd0c0-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |
   |<span data-ttu-id="cd0c0-163">Blazor uygulaması</span><span class="sxs-lookup"><span data-stu-id="cd0c0-163">Blazor app</span></span>                |                            |[<span data-ttu-id="cd0c0-164">Blazor kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="cd0c0-164">Get started with Blazor</span></span>](xref:blazor/get-started) |
   |<span data-ttu-id="cd0c0-165">Uzak yordam çağrısı uygulaması</span><span class="sxs-lookup"><span data-stu-id="cd0c0-165">Remote Procedure Call app</span></span> |                            |[<span data-ttu-id="cd0c0-166">GRPC hizmeti ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="cd0c0-166">Get started with a gRPC service</span></span>](xref:tutorials/grpc/grpc-start) |

1. <span data-ttu-id="cd0c0-167">Temel veri erişiminin nasıl yapıldığını gösteren bir öğreticiyi takip edin:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-167">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="cd0c0-168">Senaryo</span><span class="sxs-lookup"><span data-stu-id="cd0c0-168">Scenario</span></span>  |<span data-ttu-id="cd0c0-169">Öğretici</span><span class="sxs-lookup"><span data-stu-id="cd0c0-169">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="cd0c0-170">Yeni proje geliştirmek için</span><span class="sxs-lookup"><span data-stu-id="cd0c0-170">For new development</span></span>        |[<span data-ttu-id="cd0c0-171">Entity Framework Core ile Razor Pages</span><span class="sxs-lookup"><span data-stu-id="cd0c0-171">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="cd0c0-172">MVC uygulaması yönetmek için</span><span class="sxs-lookup"><span data-stu-id="cd0c0-172">For maintaining an MVC app</span></span> |[<span data-ttu-id="cd0c0-173">Entity Framework Core ile MVC</span><span class="sxs-lookup"><span data-stu-id="cd0c0-173">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="cd0c0-174">Tüm uygulama türleri için geçerli olan ASP.NET Core özelliklerine yönelik genel bakışı okuyun:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-174">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="cd0c0-175">Temel Konular</span><span class="sxs-lookup"><span data-stu-id="cd0c0-175">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="cd0c0-176">İlgilendiğiniz diğer konular için İçindekiler Tablosu’na göz atın.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-176">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="cd0c0-177">Tarayıcıda tamamını takip ettiğiniz ve yerel IDE yüklemesi gerektirmeyen \* yeni bir [web API’si öğreticisi var](https://docs.microsoft.com/learn/modules/build-web-api-net-core).</span><span class="sxs-lookup"><span data-stu-id="cd0c0-177">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="cd0c0-178">Kod [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/)’de çalışır, [curl](https://curl.haxx.se/) ise test için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-178">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="migration-from-the-net-framework"></a><span data-ttu-id="cd0c0-179">.NET Framework geçiş</span><span class="sxs-lookup"><span data-stu-id="cd0c0-179">Migration from the .NET Framework</span></span>

<span data-ttu-id="cd0c0-180">ASP.NET uygulamalarını ASP.NET Core geçirmeye yönelik bir başvuru kılavuzu için, bkz. <xref:migration/proper-to-2x/index>.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-180">For a reference guide to migrating ASP.NET apps to ASP.NET Core, see <xref:migration/proper-to-2x/index>.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="cd0c0-181">Örnek indirme</span><span class="sxs-lookup"><span data-stu-id="cd0c0-181">How to download a sample</span></span>

<span data-ttu-id="cd0c0-182">Çoğu makale ve öğretici örnek koda bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-182">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="cd0c0-183">[ASP.NET depo zip dosyasını indirin](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="cd0c0-183">[Download the ASP.NET repository zip file](https://codeload.github.com/dotnet/AspNetCore.Docs/zip/master).</span></span>
1. <span data-ttu-id="cd0c0-184">*Docs-master.zip* dosyasının sıkıştırmasını açın.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-184">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="cd0c0-185">Örnek dizinde gezinmek için örnek bağlantıdaki URL’yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-185">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="cd0c0-186">Örnek kodda ön işlemci yönergeleri</span><span class="sxs-lookup"><span data-stu-id="cd0c0-186">Preprocessor directives in sample code</span></span>

<span data-ttu-id="cd0c0-187">Birden çok senaryoyu göstermek için örnek uygulamalar, örnek kodun farklı bölümlerini seçmeli olarak derlemek ve çalıştırmak için `#define` ve `#if-#else/#elif-#endif` önişlemci yönergelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-187">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` preprocessor directives to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="cd0c0-188">Bu yaklaşımı kullanan örnekler için, çalıştırmak istediğiniz senaryoyla ilişkili sembolü tanımlamak üzere C# dosyaların en üstündeki `#define` yönergesini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-188">For those samples that make use of this approach, set the `#define` directive at the top of the C# files to define the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="cd0c0-189">Bazı örnekler, bir senaryoyu çalıştırmak için birden fazla dosyanın en üstünde sembolün tanımlanmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-189">Some samples require defining the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="cd0c0-190">Örneğin, aşağıdaki simge listesi `#define` dört senaryonun kullanılabilir olduğunu gösterir (her simge için bir senaryo).</span><span class="sxs-lookup"><span data-stu-id="cd0c0-190">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="cd0c0-191">Geçerli örnek yapılandırması `TemplateCode` senaryosunu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-191">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="cd0c0-192">Örneği `ExpandDefault` senaryosunu çalıştıracak şekilde değiştirmek için `ExpandDefault` simgesini tanımlayın ve kalan simgeleri açıklama satırı yapılmış şekilde bırakın:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-192">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="cd0c0-193">Kod bölümlerini seçmeli olarak derlemek üzere [C# ön işlemci yönergelerini](/dotnet/csharp/language-reference/preprocessor-directives/) kullanma hakkında daha fazla bilgi için bkz. [#define (C# Başvurusu)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) ve [#if (C# Başvurusu)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="cd0c0-193">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="cd0c0-194">Örnek kodda bölgeler</span><span class="sxs-lookup"><span data-stu-id="cd0c0-194">Regions in sample code</span></span>

<span data-ttu-id="cd0c0-195">Bazı örnek uygulamalar [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) ve [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# yönergelerinin çevrelenen kod bölümlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-195">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# directives.</span></span> <span data-ttu-id="cd0c0-196">Belge derleme sistemi bu bölgeleri işlenmiş belge konularının içine ekler.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-196">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="cd0c0-197">Bölge adları çoğunlukla şu sözcüğü içerir: "snippet."</span><span class="sxs-lookup"><span data-stu-id="cd0c0-197">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="cd0c0-198">Aşağıdaki örnekte `snippet_WebHostDefaults` adlı bir bölge gösterilir:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-198">The following example shows a region named `snippet_WebHostDefaults`:</span></span>

```csharp
#region snippet_WebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
    });
#endregion
```

<span data-ttu-id="cd0c0-199">Önündeki C# kod parçacığına, konunun markdown dosyasında aşağıdaki satırla başvurulur:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-199">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```md
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_WebHostDefaults)]
```

<span data-ttu-id="cd0c0-200">Kodu çevreleyen `#region` ve `#endregion` yönergelerini güvenle yoksayabilir (veya kaldırabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="cd0c0-200">You may safely ignore (or remove) the `#region` and `#endregion` directives that surround the code.</span></span> <span data-ttu-id="cd0c0-201">Konusunda açıklanan örnek senaryoları çalıştırmayı planlıyorsanız, bu yönergelerin içindeki kodu değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-201">Don't alter the code within these directives if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="cd0c0-202">Başka senaryolarla denemeler yaparken kodu rahatça değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-202">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="cd0c0-203">Daha fazla bilgi için bkz. [ASP.NET belgelerine katkıda bulunma: Kod parçacıkları](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="cd0c0-203">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/dotnet/AspNetCore.Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd0c0-204">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="cd0c0-204">Next steps</span></span>

<span data-ttu-id="cd0c0-205">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="cd0c0-205">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="cd0c0-206">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="cd0c0-206">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="cd0c0-207">[Haftalık ASP.NET topluluğu toplantısında](https://live.asp.net/), takımın ilerleme durumu ve planları ele alınır.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-207">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="cd0c0-208">Yeni bloglara ve üçüncü taraf yazılıma yer verilir.</span><span class="sxs-lookup"><span data-stu-id="cd0c0-208">It features new blogs and third-party software.</span></span>
