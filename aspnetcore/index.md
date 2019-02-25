---
title: ASP.NET Core’a Giriş
author: rick-anderson
description: 'Modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, açık kaynak bir çerçeve olan ASP.NET Core’a giriş yapın.'
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: index
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="e9a4d-103">ASP.NET Core’a Giriş</span><span class="sxs-lookup"><span data-stu-id="e9a4d-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="e9a4d-104">[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Shaun Luttin](https://twitter.com/dicshaunary) tarafından hazırlanmıştır</span><span class="sxs-lookup"><span data-stu-id="e9a4d-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="e9a4d-105">ASP.NET Core, modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, [açık kaynak](https://github.com/aspnet/home) bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="e9a4d-106">ASP.NET Core ile şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="e9a4d-107">Web uygulamaları ve hizmetleri, [IoT](https://www.microsoft.com/internet-of-things/) uygulamaları ve mobil arka uçlar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="e9a4d-108">Windows, macOS ve Linux üzerinde tercih ettiğiniz geliştirme araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="e9a4d-109">Buluta veya şirket içine dağıtın.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="e9a4d-110">[.NET Core veya .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="e9a4d-111">Neden ASP.NET Core kullanılmalı?</span><span class="sxs-lookup"><span data-stu-id="e9a4d-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="e9a4d-112">Milyonlarca geliştirici, web uygulamaları oluşturmak için [ASP.NET 4.x](/aspnet/overview) kullandı (ve kullanmaya devam ediyor).</span><span class="sxs-lookup"><span data-stu-id="e9a4d-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="e9a4d-113">ASP.NET Core, ASP.NET 4.x sürümünün daha yalın, daha modüler bir çerçeve elde edilmesini sağlayan mimari değişikliklerle yeniden tasarlanmış halidir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="e9a4d-114">ASP.NET Core MVC kullanarak web API'leri ve web kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e9a4d-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="e9a4d-115">ASP.NET Core MVC, [web API’leri](xref:tutorials/first-web-api) ve [web uygulamaları](xref:tutorials/razor-pages/index) oluşturmaya yönelik özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="e9a4d-116">[Model-Görünüm-Denetleyici (MVC) deseni](xref:mvc/overview), web API'lerinin ve web uygulamalarının sınanabilir olmasını sağlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="e9a4d-117">[Razor Pages](xref:razor-pages/index), web kullanıcı arabirimi oluşturmayı daha kolay ve üretken bir hale getiren, sayfa tabanlı bir programlama modelidir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="e9a4d-118">[Razor işaretlemesi](xref:mvc/views/razor), [Razor Sayfaları](xref:razor-pages/index) ve [MVC görünümleri](xref:mvc/views/overview) için üretken bir söz dizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="e9a4d-119">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="e9a4d-120">[Birden çok veri biçimi ve içerik anlaşması](xref:web-api/advanced/formatting) için sunulan yerleşik destek, web API'lerinizin tarayıcılar ve mobil cihazlar dahil olmak üzere birçok çeşit istemciye ulaşmasına imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="e9a4d-121">[Model bağlama](xref:mvc/models/model-binding), HTTP isteklerinden alınan verileri otomatik olarak eylem metodu parametreleriyle eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="e9a4d-122">[Model doğrulama](xref:mvc/models/validation), otomatik olarak istemci ve sunucu tarafı doğrulama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="e9a4d-123">İstemci tarafı geliştirme</span><span class="sxs-lookup"><span data-stu-id="e9a4d-123">Client-side development</span></span>

<span data-ttu-id="e9a4d-124">ASP.NET Core, aralarında [Razor Components](xref:razor-components/index), [Angular](xref:spa/angular), [React](xref:spa/react) ve [Bootstrap](https://getbootstrap.com/)’in bulunduğu popüler istemci tarafı çerçeve ve kitaplıklarla sorunsuz bir şekilde tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Razor Components](xref:razor-components/index), [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="e9a4d-125">Daha fazla bilgi için [Razor Components](xref:razor-components/index) ve *İstemci tarafı geliştirme* altındaki ilgili konulara bakın.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-125">For more information, see [Razor Components](xref:razor-components/index) and related topics under *Client-side development*.</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="e9a4d-126">.NET Framework'ü hedefleyen ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e9a4d-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="e9a4d-127">ASP.NET Core 2.x, .NET Core'u veya .NET Framework'ü hedefleyebilir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="e9a4d-128">.NET Framework'ü hedefleyen ASP.NET Core uygulamaları platformlar arası çalışmaz; bunlar yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="e9a4d-129">Genel olarak, ASP.NET Core 2.x [.NET Standard](/dotnet/standard/net-standard) kitaplıklarından oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="e9a4d-130">.NET Standard 2.0 ile yazılmış uygulamalar .NET Standard 2.0'ın desteklendiği her yerde çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="e9a4d-131">ASP.NET Core 2.x, .NET Standard 2.0 ile uyumlu .NET Framework sürümlerinde desteklenir:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="e9a4d-132">.NET Framework 4.7.1 ve üzeri önemle tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="e9a4d-133">.NET Framework 4.6.1 ve üzeri.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="e9a4d-134">ASP.NET Core 3.0 ve üzeri yalnızca .NET Core’da çalışır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="e9a4d-135">Bu değişiklik hakkında daha fazla bilgi için bkz. [ASP.NET Core 3.0’daki değişikliklere ilk bakış](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="e9a4d-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="e9a4d-136">.NET Core hedeflemesinin çeşitli avantajları vardır ve bu avantajlar her yeni sürümle birlikte artmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="e9a4d-137">.NET Framework'e göre .NET Core'un bazı avantajları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="e9a4d-138">Platformlar arası.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-138">Cross-platform.</span></span> <span data-ttu-id="e9a4d-139">macOS, Linux ve Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="e9a4d-140">Geliştirilmiş performans</span><span class="sxs-lookup"><span data-stu-id="e9a4d-140">Improved performance</span></span>
* <span data-ttu-id="e9a4d-141">Yan yana sürüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="e9a4d-141">Side-by-side versioning</span></span>
* <span data-ttu-id="e9a4d-142">Yeni API'ler</span><span class="sxs-lookup"><span data-stu-id="e9a4d-142">New APIs</span></span>
* <span data-ttu-id="e9a4d-143">Açık kaynak</span><span class="sxs-lookup"><span data-stu-id="e9a4d-143">Open source</span></span>

<span data-ttu-id="e9a4d-144">.NET Framework ile .NET Core arasındaki API açığını kapatmak için çok çalışıyoruz.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="e9a4d-145">[Windows Uyumluluk Paketi](/dotnet/core/porting/windows-compat-pack), .NET Core'da yalnızca Windows'a yönelik binlerce API'yi kullanıma sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="e9a4d-146">Bu API'ler .NET Core 1.x'te sağlanmamıştı.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="recommended-learning-path"></a><span data-ttu-id="e9a4d-147">Önerilen öğrenme yolu</span><span class="sxs-lookup"><span data-stu-id="e9a4d-147">Recommended learning path</span></span>

<span data-ttu-id="e9a4d-148">ASP.NET Core uygulamaları geliştirmeye başlamak için şu öğreticileri ve makaleleri takip etmenizi öneririz:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-148">We recommend the following sequence of tutorials and articles for an introduction to developing ASP.NET Core apps:</span></span>

1. <span data-ttu-id="e9a4d-149">Geliştirmek veya yönetmek istediğiniz uygulama türüne yönelik bir öğreticiyi takip edin:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-149">Follow a tutorial for the type of app you want to develop or maintain:</span></span>

   |<span data-ttu-id="e9a4d-150">Uygulama türü</span><span class="sxs-lookup"><span data-stu-id="e9a4d-150">App type</span></span>  |<span data-ttu-id="e9a4d-151">Senaryo</span><span class="sxs-lookup"><span data-stu-id="e9a4d-151">Scenario</span></span>  |<span data-ttu-id="e9a4d-152">Eğitmen</span><span class="sxs-lookup"><span data-stu-id="e9a4d-152">Tutorial</span></span>  |
   |----------|----------|----------|
   |<span data-ttu-id="e9a4d-153">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="e9a4d-153">Web app</span></span>       | <span data-ttu-id="e9a4d-154">Yeni proje geliştirmek için</span><span class="sxs-lookup"><span data-stu-id="e9a4d-154">For new development</span></span>        |[<span data-ttu-id="e9a4d-155">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e9a4d-155">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start) |
   |<span data-ttu-id="e9a4d-156">Web uygulaması</span><span class="sxs-lookup"><span data-stu-id="e9a4d-156">Web app</span></span>       | <span data-ttu-id="e9a4d-157">MVC uygulaması yönetmek için</span><span class="sxs-lookup"><span data-stu-id="e9a4d-157">For maintaining an MVC app</span></span> |[<span data-ttu-id="e9a4d-158">MVC ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e9a4d-158">Get started with MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)|
   |<span data-ttu-id="e9a4d-159">Web API</span><span class="sxs-lookup"><span data-stu-id="e9a4d-159">Web API</span></span>       |                            |<span data-ttu-id="e9a4d-160">[Web API’si oluşturma](xref:tutorials/first-web-api)\*</span><span class="sxs-lookup"><span data-stu-id="e9a4d-160">[Create a web API](xref:tutorials/first-web-api)\*</span></span>  |
   |<span data-ttu-id="e9a4d-161">Gerçek zamanlı uygulama</span><span class="sxs-lookup"><span data-stu-id="e9a4d-161">Real-time app</span></span> |                            |[<span data-ttu-id="e9a4d-162">SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e9a4d-162">Get started with SignalR</span></span>](xref:tutorials/signalr) |

1. <span data-ttu-id="e9a4d-163">Temel veri erişiminin nasıl yapıldığını gösteren bir öğreticiyi takip edin:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-163">Follow a tutorial that shows how to do basic data access:</span></span>

   |<span data-ttu-id="e9a4d-164">Senaryo</span><span class="sxs-lookup"><span data-stu-id="e9a4d-164">Scenario</span></span>  |<span data-ttu-id="e9a4d-165">Eğitmen</span><span class="sxs-lookup"><span data-stu-id="e9a4d-165">Tutorial</span></span>  |
   |----------|----------|
   | <span data-ttu-id="e9a4d-166">Yeni proje geliştirmek için</span><span class="sxs-lookup"><span data-stu-id="e9a4d-166">For new development</span></span>        |[<span data-ttu-id="e9a4d-167">Entity Framework Core ile Razor Pages</span><span class="sxs-lookup"><span data-stu-id="e9a4d-167">Razor Pages with Entity Framework Core</span></span>](xref:data/ef-rp/intro) |
   | <span data-ttu-id="e9a4d-168">MVC uygulaması yönetmek için</span><span class="sxs-lookup"><span data-stu-id="e9a4d-168">For maintaining an MVC app</span></span> |[<span data-ttu-id="e9a4d-169">Entity Framework Core ile MVC</span><span class="sxs-lookup"><span data-stu-id="e9a4d-169">MVC with Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

1. <span data-ttu-id="e9a4d-170">Tüm uygulama türleri için geçerli olan ASP.NET Core özelliklerine yönelik genel bakışı okuyun:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-170">Read an overview of ASP.NET Core features that apply to all app types:</span></span>

   * [<span data-ttu-id="e9a4d-171">Temeller</span><span class="sxs-lookup"><span data-stu-id="e9a4d-171">Fundamentals</span></span>](xref:fundamentals/index)

1. <span data-ttu-id="e9a4d-172">İlgilendiğiniz diğer konular için İçindekiler Tablosu’na göz atın.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-172">Browse the Table of Contents for other topics of interest.</span></span>

<span data-ttu-id="e9a4d-173">Tarayıcıda tamamını takip ettiğiniz ve yerel IDE yüklemesi gerektirmeyen \* yeni bir [web API’si öğreticisi var](https://docs.microsoft.com/learn/modules/build-web-api-net-core).</span><span class="sxs-lookup"><span data-stu-id="e9a4d-173">\* There is a new [web API tutorial that you follow entirely in the browser](https://docs.microsoft.com/learn/modules/build-web-api-net-core), no local IDE installation required.</span></span>  <span data-ttu-id="e9a4d-174">Kod [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/)’de çalışır, [curl](https://curl.haxx.se/) ise test için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-174">The code runs in an [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), and [curl](https://curl.haxx.se/) is used for testing.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="e9a4d-175">Örnek indirme</span><span class="sxs-lookup"><span data-stu-id="e9a4d-175">How to download a sample</span></span>

<span data-ttu-id="e9a4d-176">Çoğu makale ve öğretici örnek koda bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-176">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="e9a4d-177">[ASP.NET depo zip dosyasını indirin](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="e9a4d-177">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="e9a4d-178">*Docs-master.zip* dosyasının sıkıştırmasını açın.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-178">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="e9a4d-179">Örnek dizinde gezinmek için örnek bağlantıdaki URL’yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-179">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="e9a4d-180">Örnek kodda ön işlemci yönergeleri</span><span class="sxs-lookup"><span data-stu-id="e9a4d-180">Preprocessor directives in sample code</span></span>

<span data-ttu-id="e9a4d-181">Örnek uygulamalar birden çok senaryoyu göstermek amacıyla aynı koda ait farklı bölümleri seçmeli olarak derleyip çalıştırmak için `#define` ve `#if-#else/#elif-#endif` C# deyimlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-181">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="e9a4d-182">Bu yaklaşımdan yararlanan örneklerde C# dosyalarının üst kısmındaki `#define` deyimini, çalıştırmak istediğiniz senaryoyla ilişkili olan simgeye ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-182">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="e9a4d-183">Bazı örnekler senaryoyu çalıştırmak için birden çok dosyanın üst kısmındaki simgenin ayarlanmasını gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-183">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="e9a4d-184">Örneğin, aşağıdaki simge listesi `#define` dört senaryonun kullanılabilir olduğunu gösterir (her simge için bir senaryo).</span><span class="sxs-lookup"><span data-stu-id="e9a4d-184">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="e9a4d-185">Geçerli örnek yapılandırması `TemplateCode` senaryosunu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-185">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="e9a4d-186">Örneği `ExpandDefault` senaryosunu çalıştıracak şekilde değiştirmek için `ExpandDefault` simgesini tanımlayın ve kalan simgeleri açıklama satırı yapılmış şekilde bırakın:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-186">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="e9a4d-187">Kod bölümlerini seçmeli olarak derlemek üzere [C# ön işlemci yönergelerini](/dotnet/csharp/language-reference/preprocessor-directives/) kullanma hakkında daha fazla bilgi için bkz. [#define (C# Başvurusu)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) ve [#if (C# Başvurusu)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="e9a4d-187">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="e9a4d-188">Örnek kodda bölgeler</span><span class="sxs-lookup"><span data-stu-id="e9a4d-188">Regions in sample code</span></span>

<span data-ttu-id="e9a4d-189">Bazı örnek uygulamalar, [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) ve [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# deyimleri içine yerleştirilmiş kod bölümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-189">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="e9a4d-190">Belge derleme sistemi bu bölgeleri işlenmiş belge konularının içine ekler.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-190">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="e9a4d-191">Bölge adları çoğunlukla şu sözcüğü içerir: "snippet."</span><span class="sxs-lookup"><span data-stu-id="e9a4d-191">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="e9a4d-192">Aşağıdaki örnekte `snippet_FilterInCode` adlı bir bölge gösterilir:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-192">The following example shows a region named `snippet_FilterInCode`:</span></span>

```csharp
#region snippet_FilterInCode
WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
        logging.AddFilter("System", LogLevel.Debug)
            .AddFilter<DebugLoggerProvider>("Microsoft", LogLevel.Trace))
            .Build();
#endregion
```

<span data-ttu-id="e9a4d-193">Önündeki C# kod parçacığına, konunun markdown dosyasında aşağıdaki satırla başvurulur:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-193">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="e9a4d-194">Kodu çevreleyen `#region` ve `#endregion` deyimlerini rahatça yoksayabilirsiniz (veya kaldırabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="e9a4d-194">You may safely ignore (or remove) the `#region` and `#endregion` statements that surround the code.</span></span> <span data-ttu-id="e9a4d-195">Konuda açıklanan örnek senaryoları çalıştırmayı planlıyorsanız, bu deyimlerin anasındaki kodu değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-195">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="e9a4d-196">Başka senaryolarla denemeler yaparken kodu rahatça değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-196">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="e9a4d-197">Daha fazla bilgi için bkz. [ASP.NET belgelerine katkıda bulunma: Kod parçacıkları](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="e9a4d-197">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9a4d-198">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="e9a4d-198">Next steps</span></span>

<span data-ttu-id="e9a4d-199">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="e9a4d-199">For more information, see the following resources:</span></span>

* <xref:getting-started>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="e9a4d-200">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="e9a4d-200">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="e9a4d-201">[Haftalık ASP.NET topluluğu toplantısında](https://live.asp.net/), takımın ilerleme durumu ve planları ele alınır.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-201">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="e9a4d-202">Yeni bloglara ve üçüncü taraf yazılıma yer verilir.</span><span class="sxs-lookup"><span data-stu-id="e9a4d-202">It features new blogs and third-party software.</span></span>
