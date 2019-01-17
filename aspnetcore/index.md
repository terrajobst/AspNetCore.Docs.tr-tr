---
title: ASP.NET Core’a Giriş
author: rick-anderson
description: Modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, açık kaynak bir çerçeve olan ASP.NET Core’a giriş yapın.
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: index
ms.openlocfilehash: e7c81ff82e5206a5aca217417f6cb1c339d72e89
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341413"
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="d71c7-103">ASP.NET Core’a Giriş</span><span class="sxs-lookup"><span data-stu-id="d71c7-103">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="d71c7-104">[Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Shaun Luttin](https://twitter.com/dicshaunary) tarafından hazırlanmıştır</span><span class="sxs-lookup"><span data-stu-id="d71c7-104">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="d71c7-105">ASP.NET Core, modern, bulut tabanlı, İnternet bağlantılı uygulamalar oluşturmaya yönelik platformlar arası, yüksek performanslı, [açık kaynak](https://github.com/aspnet/home) bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-105">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="d71c7-106">ASP.NET Core ile şunları yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d71c7-106">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="d71c7-107">Web uygulamaları ve hizmetleri, [IoT](https://www.microsoft.com/internet-of-things/) uygulamaları ve mobil arka uçlar oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d71c7-107">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="d71c7-108">Windows, macOS ve Linux üzerinde tercih ettiğiniz geliştirme araçlarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="d71c7-108">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="d71c7-109">Buluta veya şirket içine dağıtın.</span><span class="sxs-lookup"><span data-stu-id="d71c7-109">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="d71c7-110">[.NET Core veya .NET Framework](/dotnet/articles/standard/choosing-core-framework-server) üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d71c7-110">Run on [.NET Core or .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="d71c7-111">Neden ASP.NET Core kullanılmalı?</span><span class="sxs-lookup"><span data-stu-id="d71c7-111">Why use ASP.NET Core?</span></span>

<span data-ttu-id="d71c7-112">Milyonlarca geliştirici, web uygulamaları oluşturmak için [ASP.NET 4.x](/aspnet/overview) kullandı (ve kullanmaya devam ediyor).</span><span class="sxs-lookup"><span data-stu-id="d71c7-112">Millions of developers have used (and continue to use) [ASP.NET 4.x](/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="d71c7-113">ASP.NET Core, ASP.NET 4.x sürümünün daha yalın, daha modüler bir çerçeve elde edilmesini sağlayan mimari değişikliklerle yeniden tasarlanmış halidir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-113">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="d71c7-114">ASP.NET Core MVC kullanarak web API'leri ve web kullanıcı arabirimi oluşturma</span><span class="sxs-lookup"><span data-stu-id="d71c7-114">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="d71c7-115">ASP.NET Core MVC, [web API’leri](xref:tutorials/first-web-api) ve [web uygulamaları](xref:tutorials/razor-pages/index) oluşturmaya yönelik özellikler sağlar:</span><span class="sxs-lookup"><span data-stu-id="d71c7-115">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/first-web-api) and [web apps](xref:tutorials/razor-pages/index):</span></span>

* <span data-ttu-id="d71c7-116">[Model-Görünüm-Denetleyici (MVC) deseni](xref:mvc/overview), web API'lerinin ve web uygulamalarının sınanabilir olmasını sağlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="d71c7-116">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps testable.</span></span>
* <span data-ttu-id="d71c7-117">[Razor Pages](xref:razor-pages/index), web kullanıcı arabirimi oluşturmayı daha kolay ve üretken bir hale getiren, sayfa tabanlı bir programlama modelidir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-117">[Razor Pages](xref:razor-pages/index) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="d71c7-118">[Razor işaretlemesi](xref:mvc/views/razor), [Razor Sayfaları](xref:razor-pages/index) ve [MVC görünümleri](xref:mvc/views/overview) için üretken bir söz dizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d71c7-118">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="d71c7-119">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-119">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="d71c7-120">[Birden çok veri biçimi ve içerik anlaşması](xref:web-api/advanced/formatting) için sunulan yerleşik destek, web API'lerinizin tarayıcılar ve mobil cihazlar dahil olmak üzere birçok çeşit istemciye ulaşmasına imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="d71c7-120">Built-in support for [multiple data formats and content negotiation](xref:web-api/advanced/formatting) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="d71c7-121">[Model bağlama](xref:mvc/models/model-binding), HTTP isteklerinden alınan verileri otomatik olarak eylem metodu parametreleriyle eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-121">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="d71c7-122">[Model doğrulama](xref:mvc/models/validation), otomatik olarak istemci ve sunucu tarafı doğrulama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-122">[Model validation](xref:mvc/models/validation) automatically performs client-side and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="d71c7-123">İstemci tarafı geliştirme</span><span class="sxs-lookup"><span data-stu-id="d71c7-123">Client-side development</span></span>

<span data-ttu-id="d71c7-124">ASP.NET Core, popüler istemci tarafı çerçeveler ve kitaplıklar ([Angular](xref:spa/angular), [React](xref:spa/react) ve [Bootstrap](https://getbootstrap.com/) dahil) ile sorunsuz bir şekilde tümleştirilir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-124">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](https://getbootstrap.com/).</span></span> <span data-ttu-id="d71c7-125">Daha fazla bilgi için bkz. [İstemci tarafı geliştirme](xref:client-side/index).</span><span class="sxs-lookup"><span data-stu-id="d71c7-125">For more information, see [Client-side development](xref:client-side/index).</span></span>

<a name="target-framework"></a>

## <a name="aspnet-core-targeting-net-framework"></a><span data-ttu-id="d71c7-126">.NET Framework'ü hedefleyen ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d71c7-126">ASP.NET Core targeting .NET Framework</span></span>

<span data-ttu-id="d71c7-127">ASP.NET Core 2.x, .NET Core'u veya .NET Framework'ü hedefleyebilir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-127">ASP.NET Core 2.x can target .NET Core or .NET Framework.</span></span> <span data-ttu-id="d71c7-128">.NET Framework'ü hedefleyen ASP.NET Core uygulamaları platformlar arası çalışmaz; bunlar yalnızca Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="d71c7-128">ASP.NET Core apps targeting .NET Framework aren't cross-platform&mdash;they run on Windows only.</span></span> <span data-ttu-id="d71c7-129">Genel olarak, ASP.NET Core 2.x [.NET Standard](/dotnet/standard/net-standard) kitaplıklarından oluşturulmuştur.</span><span class="sxs-lookup"><span data-stu-id="d71c7-129">Generally, ASP.NET Core 2.x is made up of [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="d71c7-130">.NET Standard 2.0 ile yazılmış uygulamalar .NET Standard 2.0'ın desteklendiği her yerde çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="d71c7-130">Apps written with .NET Standard 2.0 run anywhere that .NET Standard 2.0 is supported.</span></span>

<span data-ttu-id="d71c7-131">ASP.NET Core 2.x, .NET Standard 2.0 ile uyumlu .NET Framework sürümlerinde desteklenir:</span><span class="sxs-lookup"><span data-stu-id="d71c7-131">ASP.NET Core 2.x is supported on .NET Framework versions compatible with .NET Standard 2.0:</span></span>

* <span data-ttu-id="d71c7-132">.NET Framework 4.7.1 ve üzeri önemle tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-132">.NET Framework 4.7.1 and later is strongly recommended.</span></span>
* <span data-ttu-id="d71c7-133">.NET Framework 4.6.1 ve üzeri.</span><span class="sxs-lookup"><span data-stu-id="d71c7-133">.NET Framework 4.6.1 and later.</span></span>

<span data-ttu-id="d71c7-134">ASP.NET Core 3.0 ve üzeri yalnızca .NET Core’da çalışır.</span><span class="sxs-lookup"><span data-stu-id="d71c7-134">ASP.NET Core 3.0 and later will only run on .NET Core.</span></span> <span data-ttu-id="d71c7-135">Bu değişiklik hakkında daha fazla bilgi için bkz. [ASP.NET Core 3.0’daki değişikliklere ilk bakış](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span><span class="sxs-lookup"><span data-stu-id="d71c7-135">For more details regarding this change, see [A first look at changes coming in ASP.NET Core 3.0](https://blogs.msdn.microsoft.com/webdev/2018/10/29/a-first-look-at-changes-coming-in-asp-net-core-3-0/).</span></span>

<span data-ttu-id="d71c7-136">.NET Core hedeflemesinin çeşitli avantajları vardır ve bu avantajlar her yeni sürümle birlikte artmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d71c7-136">There are several advantages to targeting .NET Core, and these advantages increase with each release.</span></span> <span data-ttu-id="d71c7-137">.NET Framework'e göre .NET Core'un bazı avantajları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d71c7-137">Some advantages of .NET Core over .NET Framework include:</span></span>

* <span data-ttu-id="d71c7-138">Platformlar arası.</span><span class="sxs-lookup"><span data-stu-id="d71c7-138">Cross-platform.</span></span> <span data-ttu-id="d71c7-139">macOS, Linux ve Windows üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="d71c7-139">Runs on macOS, Linux, and Windows.</span></span>
* <span data-ttu-id="d71c7-140">Geliştirilmiş performans</span><span class="sxs-lookup"><span data-stu-id="d71c7-140">Improved performance</span></span>
* <span data-ttu-id="d71c7-141">Yan yana sürüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="d71c7-141">Side-by-side versioning</span></span>
* <span data-ttu-id="d71c7-142">Yeni API'ler</span><span class="sxs-lookup"><span data-stu-id="d71c7-142">New APIs</span></span>
* <span data-ttu-id="d71c7-143">Açık kaynak</span><span class="sxs-lookup"><span data-stu-id="d71c7-143">Open source</span></span>

<span data-ttu-id="d71c7-144">.NET Framework ile .NET Core arasındaki API açığını kapatmak için çok çalışıyoruz.</span><span class="sxs-lookup"><span data-stu-id="d71c7-144">We're working hard to close the API gap from .NET Framework to .NET Core.</span></span> <span data-ttu-id="d71c7-145">[Windows Uyumluluk Paketi](/dotnet/core/porting/windows-compat-pack), .NET Core'da yalnızca Windows'a yönelik binlerce API'yi kullanıma sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d71c7-145">The [Windows Compatibility Pack](/dotnet/core/porting/windows-compat-pack) made thousands of Windows-only APIs available in .NET Core.</span></span> <span data-ttu-id="d71c7-146">Bu API'ler .NET Core 1.x'te sağlanmamıştı.</span><span class="sxs-lookup"><span data-stu-id="d71c7-146">These APIs weren't available in .NET Core 1.x.</span></span>

## <a name="how-to-download-a-sample"></a><span data-ttu-id="d71c7-147">Örnek indirme</span><span class="sxs-lookup"><span data-stu-id="d71c7-147">How to download a sample</span></span>

<span data-ttu-id="d71c7-148">Çoğu makale ve öğretici örnek koda bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-148">Many of the articles and tutorials include links to sample code.</span></span>

1. <span data-ttu-id="d71c7-149">[ASP.NET depo zip dosyasını indirin](https://codeload.github.com/aspnet/Docs/zip/master).</span><span class="sxs-lookup"><span data-stu-id="d71c7-149">[Download the ASP.NET repository zip file](https://codeload.github.com/aspnet/Docs/zip/master).</span></span>
1. <span data-ttu-id="d71c7-150">*Docs-master.zip* dosyasının sıkıştırmasını açın.</span><span class="sxs-lookup"><span data-stu-id="d71c7-150">Unzip the *Docs-master.zip* file.</span></span>
1. <span data-ttu-id="d71c7-151">Örnek dizinde gezinmek için örnek bağlantıdaki URL’yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="d71c7-151">Use the URL in the sample link to help you navigate to the sample directory.</span></span>

### <a name="preprocessor-directives-in-sample-code"></a><span data-ttu-id="d71c7-152">Örnek kodda ön işlemci yönergeleri</span><span class="sxs-lookup"><span data-stu-id="d71c7-152">Preprocessor directives in sample code</span></span>

<span data-ttu-id="d71c7-153">Örnek uygulamalar birden çok senaryoyu göstermek amacıyla aynı koda ait farklı bölümleri seçmeli olarak derleyip çalıştırmak için `#define` ve `#if-#else/#elif-#endif` C# deyimlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="d71c7-153">To demonstrate multiple scenarios, sample apps use the `#define` and `#if-#else/#elif-#endif` C# statements to selectively compile and run different sections of sample code.</span></span> <span data-ttu-id="d71c7-154">Bu yaklaşımdan yararlanan örneklerde C# dosyalarının üst kısmındaki `#define` deyimini, çalıştırmak istediğiniz senaryoyla ilişkili olan simgeye ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d71c7-154">For those samples that make use of this approach, set the `#define` statement at the top of the C# files to the symbol associated with the scenario that you want to run.</span></span> <span data-ttu-id="d71c7-155">Bazı örnekler senaryoyu çalıştırmak için birden çok dosyanın üst kısmındaki simgenin ayarlanmasını gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-155">Some samples require setting the symbol at the top of multiple files in order to run a scenario.</span></span>

<span data-ttu-id="d71c7-156">Örneğin, aşağıdaki simge listesi `#define` dört senaryonun kullanılabilir olduğunu gösterir (her simge için bir senaryo).</span><span class="sxs-lookup"><span data-stu-id="d71c7-156">For example, the following `#define` symbol list indicates that four scenarios are available (one scenario per symbol).</span></span> <span data-ttu-id="d71c7-157">Geçerli örnek yapılandırması `TemplateCode` senaryosunu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="d71c7-157">The current sample configuration runs the `TemplateCode` scenario:</span></span>

```csharp
#define TemplateCode // or LogFromMain or ExpandDefault or FilterInCode
```

<span data-ttu-id="d71c7-158">Örneği `ExpandDefault` senaryosunu çalıştıracak şekilde değiştirmek için `ExpandDefault` simgesini tanımlayın ve kalan simgeleri açıklama satırı yapılmış şekilde bırakın:</span><span class="sxs-lookup"><span data-stu-id="d71c7-158">To change the sample to run the `ExpandDefault` scenario, define the `ExpandDefault` symbol and leave the remaining symbols commented-out:</span></span>

```csharp
#define ExpandDefault // TemplateCode or LogFromMain or FilterInCode
```

<span data-ttu-id="d71c7-159">Kod bölümlerini seçmeli olarak derlemek üzere [C# ön işlemci yönergelerini](/dotnet/csharp/language-reference/preprocessor-directives/) kullanma hakkında daha fazla bilgi için bkz. [#define (C# Başvurusu)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) ve [#if (C# Başvurusu)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span><span class="sxs-lookup"><span data-stu-id="d71c7-159">For more information on using [C# preprocessor directives](/dotnet/csharp/language-reference/preprocessor-directives/) to selectively compile sections of code, see [#define (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-define) and [#if (C# Reference)](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if).</span></span>

### <a name="regions-in-sample-code"></a><span data-ttu-id="d71c7-160">Örnek kodda bölgeler</span><span class="sxs-lookup"><span data-stu-id="d71c7-160">Regions in sample code</span></span>

<span data-ttu-id="d71c7-161">Bazı örnek uygulamalar, [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) ve [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# deyimleri içine yerleştirilmiş kod bölümleri içerir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-161">Some sample apps contain sections of code surrounded by [#region](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region) and [#endregion](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-endregion) C# statements.</span></span> <span data-ttu-id="d71c7-162">Belge derleme sistemi bu bölgeleri işlenmiş belge konularının içine ekler.</span><span class="sxs-lookup"><span data-stu-id="d71c7-162">The documentation build system injects these regions into the rendered documentation topics.</span></span>  

<span data-ttu-id="d71c7-163">Bölge adları çoğunlukla şu sözcüğü içerir: "snippet."</span><span class="sxs-lookup"><span data-stu-id="d71c7-163">Region names usually contain the word "snippet."</span></span> <span data-ttu-id="d71c7-164">Aşağıdaki örnekte `snippet_FilterInCode` adlı bir bölge gösterilir:</span><span class="sxs-lookup"><span data-stu-id="d71c7-164">The following example shows a region named `snippet_FilterInCode`:</span></span>

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

<span data-ttu-id="d71c7-165">Önündeki C# kod parçacığına, konunun markdown dosyasında aşağıdaki satırla başvurulur:</span><span class="sxs-lookup"><span data-stu-id="d71c7-165">The preceding C# code snippet is referenced in the topic's markdown file with the following line:</span></span>

```
[!code-csharp[](sample/SampleApp/Program.cs?name=snippet_FilterInCode)]
```

<span data-ttu-id="d71c7-166">Kodu çevreleyen `#region` ve `#endregion` deyimlerini rahatça yoksayabilirsiniz (veya kaldırabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="d71c7-166">You may safely ignore (or remove) the `#region` and `#endregion` statements that surround the code.</span></span> <span data-ttu-id="d71c7-167">Konuda açıklanan örnek senaryoları çalıştırmayı planlıyorsanız, bu deyimlerin anasındaki kodu değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="d71c7-167">Don't alter the code within these statements if you plan to run the sample scenarios described in the topic.</span></span> <span data-ttu-id="d71c7-168">Başka senaryolarla denemeler yaparken kodu rahatça değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d71c7-168">Feel free to alter the code when experimenting with other scenarios.</span></span>

<span data-ttu-id="d71c7-169">Daha fazla bilgi için bkz. [ASP.NET belgelerine katkıda bulunma: Kod parçacıkları](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span><span class="sxs-lookup"><span data-stu-id="d71c7-169">For more information, see [Contribute to the ASP.NET documentation: Code snippets](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md#code-snippets).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d71c7-170">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="d71c7-170">Next steps</span></span>

<span data-ttu-id="d71c7-171">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="d71c7-171">For more information, see the following resources:</span></span>

* [<span data-ttu-id="d71c7-172">Razor Sayfaları kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="d71c7-172">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="d71c7-173">ASP.NET Core temelleri</span><span class="sxs-lookup"><span data-stu-id="d71c7-173">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="d71c7-174">[Haftalık ASP.NET topluluğu toplantısında](https://live.asp.net/), takımın ilerleme durumu ve planları ele alınır.</span><span class="sxs-lookup"><span data-stu-id="d71c7-174">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="d71c7-175">Yeni bloglara ve üçüncü taraf yazılıma yer verilir.</span><span class="sxs-lookup"><span data-stu-id="d71c7-175">It features new blogs and third-party software.</span></span>
