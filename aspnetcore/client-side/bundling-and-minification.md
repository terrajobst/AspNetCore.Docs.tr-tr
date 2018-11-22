---
title: Paketleme ve küçültme ASP.NET Core statik varlıkları
author: scottaddie
description: Bir ASP.NET Core web uygulaması, statik kaynakları paketleme ve küçültme tekniklerini uygulayarak en iyi duruma getirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/20/2018
uid: client-side/bundling-and-minification
ms.openlocfilehash: 5d5f0aadb7740c9b2b959d12a585cd8c91758ce8
ms.sourcegitcommit: 4225e2c49a0081e6ac15acff673587201f54b4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282153"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="f445e-103">Paketleme ve küçültme ASP.NET Core statik varlıkları</span><span class="sxs-lookup"><span data-stu-id="f445e-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="f445e-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [David Çam](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="f445e-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="f445e-105">Bu makalede, paketleme ve küçültme, bu özellikler, ASP.NET Core web apps ile nasıl kullanılabileceğini de dahil olmak üzere uygulama avantajları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="f445e-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="f445e-106">Paketleme ve küçültme nedir</span><span class="sxs-lookup"><span data-stu-id="f445e-106">What is bundling and minification</span></span>

<span data-ttu-id="f445e-107">Paketleme ve küçültme, bir web uygulamasında uygulayabileceğiniz iki farklı performans iyileştirmelerini var.</span><span class="sxs-lookup"><span data-stu-id="f445e-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="f445e-108">Birlikte kullanıldığında, paketleme ve küçültme sunucu isteklerinin sayısını azaltmak ve istenen statik varlıkları boyutunu küçültmeyi performansını.</span><span class="sxs-lookup"><span data-stu-id="f445e-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="f445e-109">Paketleme ve küçültme öncelikle ilk sayfa isteği yükleme süresi geliştirmek.</span><span class="sxs-lookup"><span data-stu-id="f445e-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="f445e-110">Bir web sayfası istenen sonra tarayıcıyı statik varlıkları (JavaScript, CSS ve görüntüleri) önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="f445e-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="f445e-111">Sonuç olarak, paketleme ve küçültme aynı sayfa veya sayfalarda aynı varlıkları isteyen aynı sitede isterken performansını yok.</span><span class="sxs-lookup"><span data-stu-id="f445e-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="f445e-112">Varsa expires üst bilgisi varlıklar üzerinde düzgün ayarlanmamış ve paketleme ve küçültme kullanılmaz, tarayıcının güncellik buluşsal yöntemler varlıkları eski birkaç gün sonra işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="f445e-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="f445e-113">Ayrıca, tarayıcı, her varlık için bir doğrulama isteği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="f445e-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="f445e-114">Bu durumda, paketleme ve küçültme, ilk sayfa isteği sonra bile bir performans gelişmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="f445e-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="f445e-115">Paketleme</span><span class="sxs-lookup"><span data-stu-id="f445e-115">Bundling</span></span>

<span data-ttu-id="f445e-116">Paketleme, birden çok dosyayı tek bir dosya halinde birleştirir.</span><span class="sxs-lookup"><span data-stu-id="f445e-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="f445e-117">Paketleme, bir web sayfası gibi bir web varlığı işlemek gerekli olan sunucu isteklerinin sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="f445e-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="f445e-118">Tek paketler herhangi bir sayıda özellikle CSS, JavaScript, vb. oluşturabilirsiniz. Daha az dosya sunucusu tarayıcıya veya uygulamanızı sağlayan hizmet daha az HTTP isteklerini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="f445e-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="f445e-119">Bu sayede, ilk sayfa yükleme performansı geliştirildi.</span><span class="sxs-lookup"><span data-stu-id="f445e-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="f445e-120">Küçültme</span><span class="sxs-lookup"><span data-stu-id="f445e-120">Minification</span></span>

<span data-ttu-id="f445e-121">Küçültme, işlevselliği değiştirmeden koddan gereksiz karakterleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="f445e-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="f445e-122">Sonuç, istenilen varlıkların (CSS, görüntü ve JavaScript dosyaları gibi) önemli boyutta bir düşüş olur.</span><span class="sxs-lookup"><span data-stu-id="f445e-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="f445e-123">Küçültme ortak yan etkilerini kısaltmayı bir karakter, değişken adları ve açıklamaları ve gereksiz boşluk kaldırma içerir.</span><span class="sxs-lookup"><span data-stu-id="f445e-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="f445e-124">Aşağıdaki JavaScript işlevi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="f445e-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="f445e-125">Küçültme işlevi aşağıdaki azaltır:</span><span class="sxs-lookup"><span data-stu-id="f445e-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="f445e-126">Açıklamalar ve gereksiz boşluk kaldırma ek olarak, aşağıdaki parametre ve değişken adları şu şekilde değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="f445e-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="f445e-127">Özgün</span><span class="sxs-lookup"><span data-stu-id="f445e-127">Original</span></span> | <span data-ttu-id="f445e-128">Yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="f445e-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="f445e-129">Paketleme ve küçültme etkisi</span><span class="sxs-lookup"><span data-stu-id="f445e-129">Impact of bundling and minification</span></span>

<span data-ttu-id="f445e-130">Aşağıdaki tabloda, tek tek varlıklar yükleniyor ve paketleme ve küçültme kullanarak arasındaki farklar özetlenmektedir:</span><span class="sxs-lookup"><span data-stu-id="f445e-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="f445e-131">Eylem</span><span class="sxs-lookup"><span data-stu-id="f445e-131">Action</span></span> | <span data-ttu-id="f445e-132">B/dk ile</span><span class="sxs-lookup"><span data-stu-id="f445e-132">With B/M</span></span> | <span data-ttu-id="f445e-133">B/dk</span><span class="sxs-lookup"><span data-stu-id="f445e-133">Without B/M</span></span> | <span data-ttu-id="f445e-134">Değiştir</span><span class="sxs-lookup"><span data-stu-id="f445e-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="f445e-135">Dosya istekleri</span><span class="sxs-lookup"><span data-stu-id="f445e-135">File Requests</span></span>  | <span data-ttu-id="f445e-136">7</span><span class="sxs-lookup"><span data-stu-id="f445e-136">7</span></span>   | <span data-ttu-id="f445e-137">18</span><span class="sxs-lookup"><span data-stu-id="f445e-137">18</span></span>     | <span data-ttu-id="f445e-138">157%</span><span class="sxs-lookup"><span data-stu-id="f445e-138">157%</span></span>
<span data-ttu-id="f445e-139">Aktarılan KB</span><span class="sxs-lookup"><span data-stu-id="f445e-139">KB Transferred</span></span> | <span data-ttu-id="f445e-140">156</span><span class="sxs-lookup"><span data-stu-id="f445e-140">156</span></span> | <span data-ttu-id="f445e-141">264.68</span><span class="sxs-lookup"><span data-stu-id="f445e-141">264.68</span></span> | <span data-ttu-id="f445e-142">70%</span><span class="sxs-lookup"><span data-stu-id="f445e-142">70%</span></span>
<span data-ttu-id="f445e-143">Yükleme süresi (ms)</span><span class="sxs-lookup"><span data-stu-id="f445e-143">Load Time (ms)</span></span> | <span data-ttu-id="f445e-144">885</span><span class="sxs-lookup"><span data-stu-id="f445e-144">885</span></span> | <span data-ttu-id="f445e-145">2360</span><span class="sxs-lookup"><span data-stu-id="f445e-145">2360</span></span>   | <span data-ttu-id="f445e-146">167%</span><span class="sxs-lookup"><span data-stu-id="f445e-146">167%</span></span>

<span data-ttu-id="f445e-147">Tarayıcılar HTTP istek üstbilgilerinin açısından oldukça ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="f445e-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="f445e-148">Toplam bayt ölçüm paketleme sırasında önemli bir azalma gördüğünüz gönderdi.</span><span class="sxs-lookup"><span data-stu-id="f445e-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="f445e-149">Bu örnek yerel olarak çalıştı ancak önemli bir iyileştirme yükleme zamanını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f445e-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="f445e-150">Paketleme ve küçültme ile varlıkları kullanarak bir ağ üzerinden aktarıldığında büyük performans artışı alırlar.</span><span class="sxs-lookup"><span data-stu-id="f445e-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="f445e-151">Paketleme ve küçültme stratejisi seçme</span><span class="sxs-lookup"><span data-stu-id="f445e-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="f445e-152">MVC ve Razor sayfaları proje şablonları, paketleme ve küçültme içeren bir JSON yapılandırma dosyası için kullanıma hazır bir çözüm sağlar.</span><span class="sxs-lookup"><span data-stu-id="f445e-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="f445e-153">Gibi üçüncü taraf araçları [Gulp](xref:client-side/using-gulp) ve [Grunt](xref:client-side/using-grunt) görev çalıştırıcıların, biraz daha karmaşık görevlerin gerçekleştirilmesi.</span><span class="sxs-lookup"><span data-stu-id="f445e-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="f445e-154">Geliştirme iş akışınızın paketleme ve küçültme ötesinde işleme gerektirdiğinde mükemmel bir uyum üçüncü taraf bir araç olan&mdash;linting ve görüntü iyileştirme gibi.</span><span class="sxs-lookup"><span data-stu-id="f445e-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="f445e-155">Tasarım zamanı paketleme ve küçültme kullanarak küçültülmüş dosyaları uygulamanın dağıtımdan önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f445e-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="f445e-156">Paketleme ve küçültme dağıtımdan önce düşük sunucu yükü avantajı sağlar.</span><span class="sxs-lookup"><span data-stu-id="f445e-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="f445e-157">Ancak, bu tasarım zamanı paketleme bilmek önemlidir ve küçültme yapıyı karmaşıklık artar ve yalnızca statik dosyalar ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="f445e-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="f445e-158">Paketleme ve küçültme yapılandırın</span><span class="sxs-lookup"><span data-stu-id="f445e-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f445e-159">ASP.NET Core 2.0 veya daha önceki sürümlerde, MVC ve Razor sayfaları proje şablonları sağlar. bir *bundleconfig.json* her paket için seçenekleri tanımlayan yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="f445e-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f445e-160">ASP.NET Core 2.1 veya daha sonra adlı yeni bir JSON dosyası ekleme *bundleconfig.json*, MVC veya Razor sayfaları proje kök dizini.</span><span class="sxs-lookup"><span data-stu-id="f445e-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="f445e-161">Aşağıdaki JSON dosya başlangıç noktası olarak şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="f445e-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="f445e-162">*Bundleconfig.json* dosyası, her paket için seçenekleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="f445e-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="f445e-163">Önceki örnekte, bir tek bir paket yapılandırmasını özel JavaScript için tanımlanır (*wwwroot/js/site.js*) ve stil sayfası (*wwwroot/css/site.css*) dosyaları.</span><span class="sxs-lookup"><span data-stu-id="f445e-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="f445e-164">Yapılandırma seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f445e-164">Configuration options include:</span></span>

* <span data-ttu-id="f445e-165">`outputFileName`: Çıkış paket dosyasının adı.</span><span class="sxs-lookup"><span data-stu-id="f445e-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="f445e-166">Bir göreli yol içerebilir *bundleconfig.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="f445e-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="f445e-167">**Gerekli**</span><span class="sxs-lookup"><span data-stu-id="f445e-167">**required**</span></span>
* <span data-ttu-id="f445e-168">`inputFiles`: Paket dosyaları bir dizi.</span><span class="sxs-lookup"><span data-stu-id="f445e-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="f445e-169">Yapılandırma dosyası için göreli yollar şunlardır.</span><span class="sxs-lookup"><span data-stu-id="f445e-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="f445e-170">**İsteğe bağlı**, \* bir boş çıkış dosyası boş değer sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="f445e-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="f445e-171">[Genelleştirme](http://www.tldp.org/LDP/abs/html/globbingref.html) desenler desteklenir.</span><span class="sxs-lookup"><span data-stu-id="f445e-171">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="f445e-172">`minify`: Çıkış türü küçültmeye seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="f445e-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="f445e-173">**İsteğe bağlı**, *varsayılan - `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="f445e-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="f445e-174">Çıkış dosyasının türünü yapılandırma seçenekleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f445e-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="f445e-175">CSS Minifier</span><span class="sxs-lookup"><span data-stu-id="f445e-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="f445e-176">JavaScript küçültücü</span><span class="sxs-lookup"><span data-stu-id="f445e-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="f445e-177">HTML Minifier</span><span class="sxs-lookup"><span data-stu-id="f445e-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="f445e-178">`includeInProject`: Sorunlarını giderme proje dosyasına oluşturulan dosyaları eklenip eklenmeyeceğini belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="f445e-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="f445e-179">**İsteğe bağlı**, *varsayılan - yanlış*</span><span class="sxs-lookup"><span data-stu-id="f445e-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="f445e-180">`sourceMap`: Bir kaynak eşlemesi ile birlikte gelen dosyası oluşturulup oluşturulmayacağını belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="f445e-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="f445e-181">**İsteğe bağlı**, *varsayılan - yanlış*</span><span class="sxs-lookup"><span data-stu-id="f445e-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="f445e-182">`sourceMapRootPath`: Oluşturulan kaynak eşleme dosyası depolamak için kök yolu.</span><span class="sxs-lookup"><span data-stu-id="f445e-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="f445e-183">Derleme zamanı yürütülmesini paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="f445e-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="f445e-184">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) yürütme paketleme ve küçültme derleme zamanında NuGet paketi sağlar.</span><span class="sxs-lookup"><span data-stu-id="f445e-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="f445e-185">Paket eklediği [MSBuild hedefleri](/visualstudio/msbuild/msbuild-targets) derleme ve temizleme saatte çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f445e-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="f445e-186">*Bundleconfig.json* tanımlı yapılandırmaya dayanarak çıktı dosyalarını üretmek için derleme işlemi tarafından analiz dosyası.</span><span class="sxs-lookup"><span data-stu-id="f445e-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="f445e-187">Microsoft desteği sağlayan github'daki topluluk odaklı projeye BuildBundlerMinifier aittir.</span><span class="sxs-lookup"><span data-stu-id="f445e-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="f445e-188">Sorun bildirmiş [burada](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="f445e-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f445e-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f445e-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f445e-190">Ekleme *BuildBundlerMinifier* paketini projenize.</span><span class="sxs-lookup"><span data-stu-id="f445e-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="f445e-191">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f445e-191">Build the project.</span></span> <span data-ttu-id="f445e-192">Çıkış penceresinde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="f445e-192">The following appears in the Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="f445e-193">Projeyi temizleyin.</span><span class="sxs-lookup"><span data-stu-id="f445e-193">Clean the project.</span></span> <span data-ttu-id="f445e-194">Çıkış penceresinde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="f445e-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f445e-195">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f445e-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f445e-196">Ekleme *BuildBundlerMinifier* paketini projenize:</span><span class="sxs-lookup"><span data-stu-id="f445e-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="f445e-197">ASP.NET kullanıyorsanız, Core 1.x sürümüne, yeni eklenen paket geri yükleme:</span><span class="sxs-lookup"><span data-stu-id="f445e-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="f445e-198">Proje derleme:</span><span class="sxs-lookup"><span data-stu-id="f445e-198">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="f445e-199">Aşağıdaki seçeneklerle karşılaşırsınız:</span><span class="sxs-lookup"><span data-stu-id="f445e-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="f445e-200">Projeyi temizleyin:</span><span class="sxs-lookup"><span data-stu-id="f445e-200">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="f445e-201">Aşağıdaki çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="f445e-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="f445e-202">Geçici yürütülmesini paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="f445e-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="f445e-203">Proje oluşturmaya gerek kalmadan geçici esasına göre paketleme ve küçültme görevleri çalıştırmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="f445e-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="f445e-204">Ekleme [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet paketini projenize:</span><span class="sxs-lookup"><span data-stu-id="f445e-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="f445e-205">Microsoft desteği sağlayan github'daki topluluk odaklı projeye BundlerMinifier.Core aittir.</span><span class="sxs-lookup"><span data-stu-id="f445e-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="f445e-206">Sorun bildirmiş [burada](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="f445e-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="f445e-207">Bu paket dahil etmek için .NET Core CLI'yı genişletir *dotnet-paket* aracı.</span><span class="sxs-lookup"><span data-stu-id="f445e-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="f445e-208">Aşağıdaki komut, Paket Yöneticisi Konsolu (PMC) penceresinde veya bir komut kabuğu'nda çalıştırılabilir:</span><span class="sxs-lookup"><span data-stu-id="f445e-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="f445e-209">NuGet Paket Yöneticisi \*.csproj dosyaya bağımlılıkları ekler `<PackageReference />` düğümleri.</span><span class="sxs-lookup"><span data-stu-id="f445e-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="f445e-210">`dotnet bundle` Komut .NET Core CLI ile kayıtlı yalnızca bir `<DotNetCliToolReference />` düğümü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f445e-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="f445e-211">\*.Csproj dosyanın uygun şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f445e-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="f445e-212">İş akışı için dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="f445e-212">Add files to workflow</span></span>

<span data-ttu-id="f445e-213">Bir örnekte göz önünde bulundurun ek *custom.css* dosya, aşağıdaki benzeyen eklenir:</span><span class="sxs-lookup"><span data-stu-id="f445e-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="f445e-214">Küçültülecek *custom.css* ve ile paket *site.css* içine bir *site.min.css* göreli yolunu ekleyin *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="f445e-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="f445e-215">Alternatif olarak, aşağıdaki Glob deseni kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="f445e-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> <span data-ttu-id="f445e-216">Bu Glob deseni ile eşleşen tüm CSS dosyaları ve küçültülmüş dosya düzeni dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="f445e-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="f445e-217">Uygulamayı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f445e-217">Build the application.</span></span> <span data-ttu-id="f445e-218">Açık *site.min.css* ve içeriğini fark *custom.css* dosyasının sonuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="f445e-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="f445e-219">Ortam tabanlı paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="f445e-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="f445e-220">En iyi uygulama, paketlenmiş ve küçültülmüş dosyaları uygulamanızı bir üretim ortamında kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f445e-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="f445e-221">Geliştirme sırasında daha kolay uygulama hata ayıklama için özgün dosyaları olun.</span><span class="sxs-lookup"><span data-stu-id="f445e-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="f445e-222">Kullanarak sayfalarınıza eklemek için hangi dosyaların belirtin [ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) görünümlerinizi de.</span><span class="sxs-lookup"><span data-stu-id="f445e-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="f445e-223">Ortam etiketi Yardımcısı yalnızca belirli çalıştırırken içeriğini işler [ortamları](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="f445e-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="f445e-224">Aşağıdaki `environment` etiketi çalıştırıldığında işlenmemiş CSS dosyaları işler `Development` ortam:</span><span class="sxs-lookup"><span data-stu-id="f445e-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="f445e-225">Aşağıdaki `environment` etiketi ile birlikte gelen ve küçültülmüş CSS dosyaları dışındaki bir ortamda çalışan işler `Development`.</span><span class="sxs-lookup"><span data-stu-id="f445e-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="f445e-226">Örneğin, çalışan `Production` veya `Staging` bu stil sayfaları işlenmesi tetikleyen:</span><span class="sxs-lookup"><span data-stu-id="f445e-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="f445e-227">Gulp gelen bundleconfig.JSON kullanma</span><span class="sxs-lookup"><span data-stu-id="f445e-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="f445e-228">Uygulamanın paketleme ve küçültme iş akışı ek işlem gerektiren durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="f445e-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="f445e-229">Görüntü iyileştirme, önbellek busting ve CDN varlık işleme verilebilir.</span><span class="sxs-lookup"><span data-stu-id="f445e-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="f445e-230">Bu gereksinimleri karşılamak için Gulp kullanmak için paketleme ve küçültme iş akışı dönüştürebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f445e-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="f445e-231">Bundler & Minifier uzantısını kullanma</span><span class="sxs-lookup"><span data-stu-id="f445e-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="f445e-232">Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) uzantısını Gulp dönüşümünü işler.</span><span class="sxs-lookup"><span data-stu-id="f445e-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="f445e-233">Microsoft desteği sağlayan github'daki topluluk odaklı projeye Bundler & Minifier uzantısı aittir.</span><span class="sxs-lookup"><span data-stu-id="f445e-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="f445e-234">Sorun bildirmiş [burada](https://github.com/madskristensen/BundlerMinifier/issues).</span><span class="sxs-lookup"><span data-stu-id="f445e-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="f445e-235">Sağ *bundleconfig.json* seçin ve Çözüm Gezgini'nde dosya **Bundler & Minifier** > **dönüştürmek için Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="f445e-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Bağlam menüsü öğesi için Gulp Dönüştür](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="f445e-237">*Gulpfile.js* ve *package.json* dosyalar projeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="f445e-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="f445e-238">Destekleyici [npm](https://www.npmjs.com/) listelenen paketleri *package.json* dosyanın `devDependencies` bölümüne yüklenir.</span><span class="sxs-lookup"><span data-stu-id="f445e-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="f445e-239">Genel bir bağımlılık olarak Gulp CLI'yı yüklemek için PMC penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f445e-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="f445e-240">*Gulpfile.js* dosya okuma *bundleconfig.json* girişler, çıkışlar ve ayarları dosyası.</span><span class="sxs-lookup"><span data-stu-id="f445e-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="f445e-241">El ile Dönüştür</span><span class="sxs-lookup"><span data-stu-id="f445e-241">Convert manually</span></span>

<span data-ttu-id="f445e-242">Visual Studio ve/veya Bundler & Minifier uzantısı yoksa, el ile dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="f445e-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="f445e-243">Ekleme bir *package.json* dosyasıyla aşağıdaki `devDependencies`, proje kök dizini:</span><span class="sxs-lookup"><span data-stu-id="f445e-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="f445e-244">Aynı düzeyde, aşağıdaki komutu çalıştırarak bağımlılıkları yükler *package.json*:</span><span class="sxs-lookup"><span data-stu-id="f445e-244">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="f445e-245">Genel bir bağımlılık olarak Gulp CLI'yı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="f445e-245">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="f445e-246">Kopyalama *gulpfile.js* proje kök dosya aşağıda:</span><span class="sxs-lookup"><span data-stu-id="f445e-246">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="f445e-247">Gulp görevleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="f445e-247">Run Gulp tasks</span></span>

<span data-ttu-id="f445e-248">Visual Studio'da proje derlenmeden önce Gulp küçültme görev tetiklemek için aşağıdaki ekleyin [MSBuild hedefi](/visualstudio/msbuild/msbuild-targets) \*.csproj dosyaya:</span><span class="sxs-lookup"><span data-stu-id="f445e-248">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="f445e-249">Bu örnekte, herhangi bir görev içinde tanımlanan. `MyPreCompileTarget` önceden tanımlanmış önce çalıştırma hedef `Build` hedef.</span><span class="sxs-lookup"><span data-stu-id="f445e-249">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="f445e-250">Visual Studio çıktı penceresinde aşağıdakine benzer bir çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="f445e-250">Output similar to the following appears in Visual Studio's Output window:</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

<span data-ttu-id="f445e-251">Alternatif olarak, Visual Studio'nun görev Çalıştırıcı Gezgini Gulp görevleri için belirli Visual Studio olaylar bağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f445e-251">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="f445e-252">Bkz: [varsayılan görevleri çalıştıran](xref:client-side/using-gulp#running-default-tasks) , bunu ilgili yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="f445e-252">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f445e-253">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f445e-253">Additional resources</span></span>

* [<span data-ttu-id="f445e-254">Gulp kullanma</span><span class="sxs-lookup"><span data-stu-id="f445e-254">Use Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="f445e-255">Grunt kullanma</span><span class="sxs-lookup"><span data-stu-id="f445e-255">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="f445e-256">Birden çok ortam kullanma</span><span class="sxs-lookup"><span data-stu-id="f445e-256">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="f445e-257">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="f445e-257">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
