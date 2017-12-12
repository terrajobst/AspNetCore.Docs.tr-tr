---
title: "Paketleme ve ASP.NET Core küçültme"
author: scottaddie
description: "Paketleme ve küçültme teknikleri uygulayarak bir ASP.NET Core web uygulamasında statik kaynakları en iyi duruma getirme hakkında bilgi edinin."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/01/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: c271b7ef386bacedbd45fbe9f62c9c486db55b36
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="bundling-and-minification"></a><span data-ttu-id="ced3f-103">Paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="ced3f-103">Bundling and minification</span></span>

<span data-ttu-id="ced3f-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="ced3f-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="ced3f-105">Bu makalede paketleme ve küçültme, bu özellikler ASP.NET Core web apps ile nasıl kullanılabileceğini de dahil olmak üzere uygulama yararlarını açıklar.</span><span class="sxs-lookup"><span data-stu-id="ced3f-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="ced3f-106">Paketleme ve küçültme nedir?</span><span class="sxs-lookup"><span data-stu-id="ced3f-106">What is bundling and minification?</span></span>

<span data-ttu-id="ced3f-107">Paketleme ve küçültme bir web uygulamasını uygulayabilirsiniz iki ayrı performans iyileştirmelerini var.</span><span class="sxs-lookup"><span data-stu-id="ced3f-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="ced3f-108">Birlikte kullanıldığında, paketleme ve küçültme sunucusu isteklerinin sayısını azaltmak ve istenen statik varlıklar boyutunun azaltılması performansı.</span><span class="sxs-lookup"><span data-stu-id="ced3f-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="ced3f-109">Paketleme ve küçültme öncelikle ilk sayfa isteği yükleme süresini artırır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="ced3f-110">Bir web sayfası istenen sonra tarayıcı statik varlıklar (JavaScript, CSS ve görüntüleri) önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="ced3f-111">Sonuç olarak, paketleme ve küçültme aynı sayfa veya sayfaları, aynı varlıklar isteyen aynı sitedeki isterken performansı yok.</span><span class="sxs-lookup"><span data-stu-id="ced3f-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="ced3f-112">Ayarlamazsanız varlıklarınızı doğru başlığındaki süresi dolar ve paketleme ve küçültme kullanmıyorsanız, tarayıcının yenilik buluşsal yöntemler varlıklar eski birkaç gün sonra işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="ced3f-112">If you don't set the expires header correctly on your assets, and if you don’t use bundling and minification, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="ced3f-113">Ayrıca, tarayıcı her varlık için bir doğrulama isteği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="ced3f-114">Bu durumda, paketleme ve küçültme ilk sayfa isteği sonra bile performans geliştirmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ced3f-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="ced3f-115">Paketleme</span><span class="sxs-lookup"><span data-stu-id="ced3f-115">Bundling</span></span>

<span data-ttu-id="ced3f-116">Paketleme birden çok dosya tek bir dosya halinde birleştirir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="ced3f-117">Paketleme, bir web sayfası gibi bir web varlık işlemek için gerekli olan sunucu isteklerinin sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="ced3f-118">Tek tek Paket herhangi bir sayıda özellikle CSS, JavaScript vb. için oluşturabilirsiniz. Daha az sayıda dosya daha az HTTP isteğini tarayıcı sunucuya veya uygulamanızın sağlayan hizmet anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="ced3f-119">Bu sonuçları ilk sayfa yükleme performansı geliştirildi.</span><span class="sxs-lookup"><span data-stu-id="ced3f-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="ced3f-120">Küçültme</span><span class="sxs-lookup"><span data-stu-id="ced3f-120">Minification</span></span>

<span data-ttu-id="ced3f-121">Küçültme işlevselliği değiştirmeden koddan gereksiz karakterleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="ced3f-122">Sonuç istenilen varlıkların (CSS, görüntüler ve JavaScript dosyaları gibi) önemli boyutu azalma olur.</span><span class="sxs-lookup"><span data-stu-id="ced3f-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="ced3f-123">Küçültme ortak yan etkileri değişken adları bir karakter kısaltmak ve açıklamalar ve gereksiz boşluk kaldırma içerir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="ced3f-124">Şu JavaScript işlevi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="ced3f-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="ced3f-125">Küçültme işlevi aşağıdaki azaltır:</span><span class="sxs-lookup"><span data-stu-id="ced3f-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="ced3f-126">Gereksiz boşluk ve açıklamaları kaldırma ek olarak, aşağıdaki parametre ve değişken adları şu şekilde adlandırıldı:</span><span class="sxs-lookup"><span data-stu-id="ced3f-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="ced3f-127">Özgün</span><span class="sxs-lookup"><span data-stu-id="ced3f-127">Original</span></span> | <span data-ttu-id="ced3f-128">Yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="ced3f-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="ced3f-129">Paketleme ve küçültme etkisi</span><span class="sxs-lookup"><span data-stu-id="ced3f-129">Impact of bundling and minification</span></span>

<span data-ttu-id="ced3f-130">Aşağıdaki tabloda tek tek varlıklar yükleme ve paketleme ve küçültme kullanma arasındaki farklar özetlenmektedir:</span><span class="sxs-lookup"><span data-stu-id="ced3f-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="ced3f-131">Eylem</span><span class="sxs-lookup"><span data-stu-id="ced3f-131">Action</span></span> | <span data-ttu-id="ced3f-132">B/M ile</span><span class="sxs-lookup"><span data-stu-id="ced3f-132">With B/M</span></span> | <span data-ttu-id="ced3f-133">B/M</span><span class="sxs-lookup"><span data-stu-id="ced3f-133">Without B/M</span></span> | <span data-ttu-id="ced3f-134">Değiştir</span><span class="sxs-lookup"><span data-stu-id="ced3f-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="ced3f-135">Dosya istekleri</span><span class="sxs-lookup"><span data-stu-id="ced3f-135">File Requests</span></span>  | <span data-ttu-id="ced3f-136">7</span><span class="sxs-lookup"><span data-stu-id="ced3f-136">7</span></span>   | <span data-ttu-id="ced3f-137">18</span><span class="sxs-lookup"><span data-stu-id="ced3f-137">18</span></span>     | <span data-ttu-id="ced3f-138">157%</span><span class="sxs-lookup"><span data-stu-id="ced3f-138">157%</span></span>
<span data-ttu-id="ced3f-139">Aktarılan KB</span><span class="sxs-lookup"><span data-stu-id="ced3f-139">KB Transferred</span></span> | <span data-ttu-id="ced3f-140">156</span><span class="sxs-lookup"><span data-stu-id="ced3f-140">156</span></span> | <span data-ttu-id="ced3f-141">264.68</span><span class="sxs-lookup"><span data-stu-id="ced3f-141">264.68</span></span> | <span data-ttu-id="ced3f-142">70%</span><span class="sxs-lookup"><span data-stu-id="ced3f-142">70%</span></span>
<span data-ttu-id="ced3f-143">Yükleme süresi (ms)</span><span class="sxs-lookup"><span data-stu-id="ced3f-143">Load Time (ms)</span></span> | <span data-ttu-id="ced3f-144">885</span><span class="sxs-lookup"><span data-stu-id="ced3f-144">885</span></span> | <span data-ttu-id="ced3f-145">2360</span><span class="sxs-lookup"><span data-stu-id="ced3f-145">2360</span></span>   | <span data-ttu-id="ced3f-146">167%</span><span class="sxs-lookup"><span data-stu-id="ced3f-146">167%</span></span>

<span data-ttu-id="ced3f-147">Tarayıcılar HTTP istek üstbilgilerinin açısından oldukça ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="ced3f-148">Toplam bayt sayısı, paketleme, önemli ölçüde azalma ölçüm gördüğünüz gönderdi.</span><span class="sxs-lookup"><span data-stu-id="ced3f-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="ced3f-149">Bu örnek yerel olarak çalıştı ancak önemli bir iyileştirme yükleme zamanını gösterir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="ced3f-150">Paketleme ve küçültme varlıklarla kullanarak bir ağ üzerinden aktarıldığında büyük performans artışı alırlar.</span><span class="sxs-lookup"><span data-stu-id="ced3f-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="ced3f-151">Paketleme ve küçültme stratejisi seçme</span><span class="sxs-lookup"><span data-stu-id="ced3f-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="ced3f-152">MVC ve Razor sayfalarının proje şablonları paketleme ve küçültme oluşan bir JSON yapılandırma dosyası için bir giden kutusu çözümü sağlar.</span><span class="sxs-lookup"><span data-stu-id="ced3f-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="ced3f-153">Gibi üçüncü taraf araçları [Gulp](xref:client-side/using-gulp) ve [Grunt](xref:client-side/using-grunt) görev koşucular, biraz daha karmaşık aynı görevleri yerine getirin.</span><span class="sxs-lookup"><span data-stu-id="ced3f-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="ced3f-154">Bir üçüncü taraf aracı, geliştirme iş akışınızı paketleme ve küçültme ötesinde işleme gerektiren harika uygun olduğunda&mdash;linting ve görüntü iyileştirme gibi.</span><span class="sxs-lookup"><span data-stu-id="ced3f-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="ced3f-155">Tasarım zamanı paketleme ve küçültme kullanarak küçültülmüş dosyaları uygulamanın dağıtımdan önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ced3f-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="ced3f-156">Paketleme ve dağıtım öncesinde küçültülmesine azaltılmış sunucu iş yükü avantajı sağlar.</span><span class="sxs-lookup"><span data-stu-id="ced3f-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="ced3f-157">Ancak, bu tasarım zamanı paketleme bilmek önemlidir ve küçültme yapı karmaşıklığını artırır ve yalnızca statik dosyaları ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="ced3f-158">Paketleme ve küçültme yapılandırın</span><span class="sxs-lookup"><span data-stu-id="ced3f-158">Configure bundling and minification</span></span>

<span data-ttu-id="ced3f-159">MVC ve Razor sayfalarının proje şablonları sağlayan bir *bundleconfig.json* her paket için seçenekleri tanımlayan yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="ced3f-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="ced3f-160">Varsayılan olarak, özel JavaScript için tanımlanmış bir tek Paket yapılandırması (*wwwroot/js/site.js*) ve stil sayfası (*wwwroot/css/site.css*) dosyaları:</span><span class="sxs-lookup"><span data-stu-id="ced3f-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="ced3f-161">Paket Seçenekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ced3f-161">Bundle options include:</span></span>

* <span data-ttu-id="ced3f-162">`outputFileName`: Çıkış için paket dosyasının adı.</span><span class="sxs-lookup"><span data-stu-id="ced3f-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="ced3f-163">Bir göreli yolu içerebilir *bundleconfig.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="ced3f-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="ced3f-164">**Gerekli**</span><span class="sxs-lookup"><span data-stu-id="ced3f-164">**required**</span></span>
* <span data-ttu-id="ced3f-165">`inputFiles`: Birlikte paketlemektir dosyaları dizisi.</span><span class="sxs-lookup"><span data-stu-id="ced3f-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="ced3f-166">Bu yapılandırma dosyasının göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="ced3f-167">**İsteğe bağlı**, * bir boş çıkış dosyası boş bir değer sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-167">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="ced3f-168">[genelleme](http://www.tldp.org/LDP/abs/html/globbingref.html) desenleri desteklenir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="ced3f-169">`minify`: Çıkış türü küçültme seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="ced3f-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="ced3f-170">**İsteğe bağlı**, *varsayılan -`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="ced3f-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="ced3f-171">Çıkış dosya türü yapılandırma seçenekleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="ced3f-172">CSS küçültücü</span><span class="sxs-lookup"><span data-stu-id="ced3f-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="ced3f-173">JavaScript küçültücü</span><span class="sxs-lookup"><span data-stu-id="ced3f-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="ced3f-174">HTML küçültücü</span><span class="sxs-lookup"><span data-stu-id="ced3f-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="ced3f-175">`includeInProject`: Proje dosyası için oluşturulan dosyalar eklenip eklenmeyeceğini belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="ced3f-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="ced3f-176">**İsteğe bağlı**, *varsayılan - yanlış*</span><span class="sxs-lookup"><span data-stu-id="ced3f-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="ced3f-177">`sourceMap`: İle birlikte gelen dosyası için kaynak eşlemesi oluşturulup oluşturulmayacağını belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="ced3f-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="ced3f-178">**İsteğe bağlı**, *varsayılan - yanlış*</span><span class="sxs-lookup"><span data-stu-id="ced3f-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="ced3f-179">`sourceMapRootPath`: Oluşturulan kaynak eşleme dosyasını depolayan kök yolu.</span><span class="sxs-lookup"><span data-stu-id="ced3f-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="ced3f-180">Derleme zamanı yürütülmesi paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="ced3f-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="ced3f-181">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet paketi, paketleme, yürütme ve küçültme derleme zamanında sağlar.</span><span class="sxs-lookup"><span data-stu-id="ced3f-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="ced3f-182">Paket yerleştirir [MSBuild hedefleri](/visualstudio/msbuild/msbuild-targets) yapı ve temiz saatte çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ced3f-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="ced3f-183">*Bundleconfig.json* tanımlanan yapılandırmasını temel alarak çıktı dosyaları üretmek için derleme süreci tarafından dosya analiz.</span><span class="sxs-lookup"><span data-stu-id="ced3f-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ced3f-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ced3f-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="ced3f-185">Ekleme *BuildBundlerMinifier* projenize paket.</span><span class="sxs-lookup"><span data-stu-id="ced3f-185">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="ced3f-186">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ced3f-186">Build the project.</span></span> <span data-ttu-id="ced3f-187">Çıktı penceresinde görünür:</span><span class="sxs-lookup"><span data-stu-id="ced3f-187">The following appears in the Output window:</span></span>

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

<span data-ttu-id="ced3f-188">Projeyi temizleyin.</span><span class="sxs-lookup"><span data-stu-id="ced3f-188">Clean the project.</span></span> <span data-ttu-id="ced3f-189">Çıktı penceresinde görünür:</span><span class="sxs-lookup"><span data-stu-id="ced3f-189">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ced3f-190">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="ced3f-190">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="ced3f-191">Ekleme *BuildBundlerMinifier* projenize paketi:</span><span class="sxs-lookup"><span data-stu-id="ced3f-191">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="ced3f-192">ASP.NET kullanıyorsanız 1.x çekirdek, yeni eklenen paket geri yükleme:</span><span class="sxs-lookup"><span data-stu-id="ced3f-192">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="ced3f-193">Projeyi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ced3f-193">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="ced3f-194">Aşağıda görünür:</span><span class="sxs-lookup"><span data-stu-id="ced3f-194">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="ced3f-195">Projeyi temizleyin:</span><span class="sxs-lookup"><span data-stu-id="ced3f-195">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="ced3f-196">Şu çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="ced3f-196">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="ced3f-197">Paketleme ve küçültme geçici yürütülmesi</span><span class="sxs-lookup"><span data-stu-id="ced3f-197">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="ced3f-198">Proje derleme olmadan bir geçici temelinde paketleme ve küçültme görevleri çalıştırmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="ced3f-198">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="ced3f-199">Ekleme [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet paketini projenize:</span><span class="sxs-lookup"><span data-stu-id="ced3f-199">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

<span data-ttu-id="ced3f-200">Bu paket dahil etmek için .NET Core CLI genişletir *dotnet paket* aracı.</span><span class="sxs-lookup"><span data-stu-id="ced3f-200">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="ced3f-201">Aşağıdaki komut, Paket Yöneticisi Konsolu (PMC) penceresinde veya bir komut kabuğu çalıştırılabilir:</span><span class="sxs-lookup"><span data-stu-id="ced3f-201">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="ced3f-202">NuGet Paket Yöneticisi *.csproj dosyaya bağımlılıkları ekler `<PackageReference />` düğümleri.</span><span class="sxs-lookup"><span data-stu-id="ced3f-202">NuGet Package Manager adds dependencies to the *.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="ced3f-203">`dotnet bundle` Komutu ile .NET Core CLI kayıtlı yalnızca bir `<DotNetCliToolReference />` düğümü kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-203">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="ced3f-204">*.Csproj dosyasını uygun şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ced3f-204">Modify the *.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="ced3f-205">İş akışı için dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="ced3f-205">Add files to workflow</span></span>

<span data-ttu-id="ced3f-206">Bir örnekte göz önünde bulundurun ek bir *custom.css* dosya, aşağıdaki benzeyen eklenir:</span><span class="sxs-lookup"><span data-stu-id="ced3f-206">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="ced3f-207">Küçültülecek *custom.css* ve onunla paketini *site.css* içine bir *site.min.css* dosya, göreli yol *bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="ced3f-207">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="ced3f-208">Alternatif olarak, aşağıdaki genelleme düzeni kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="ced3f-208">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="ced3f-209">Bu genelleme deseni tüm CSS dosyaları eşler ve küçültülmüş dosya deseni dışlar.</span><span class="sxs-lookup"><span data-stu-id="ced3f-209">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="ced3f-210">Uygulamayı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ced3f-210">Build the application.</span></span> <span data-ttu-id="ced3f-211">Açık *site.min.css* ve içeriğini fark *custom.css* dosyasının sonuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-211">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="ced3f-212">Ortam tabanlı paketleme ve küçültme</span><span class="sxs-lookup"><span data-stu-id="ced3f-212">Environment-based bundling and minification</span></span>

<span data-ttu-id="ced3f-213">En iyi uygulama, uygulamanızın ile birlikte gelen ve küçültülmüş dosyaları bir üretim ortamında kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-213">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="ced3f-214">Geliştirme sırasında daha kolay uygulama hata ayıklama için özgün dosyaları olun.</span><span class="sxs-lookup"><span data-stu-id="ced3f-214">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="ced3f-215">Sayfalarınızda kullanarak eklemek için hangi dosyaların belirtin [ortam etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) görünümlerinizi de.</span><span class="sxs-lookup"><span data-stu-id="ced3f-215">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="ced3f-216">Ortam etiketi yardımcı yalnızca özel çalıştırırken içeriğini işleyen [ortamları](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="ced3f-216">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="ced3f-217">Aşağıdaki `environment` etiketi çalıştırırken işlenmemiş CSS dosyaları işler `Development` ortamı:</span><span class="sxs-lookup"><span data-stu-id="ced3f-217">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ced3f-218">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="ced3f-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ced3f-219">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="ced3f-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="ced3f-220">Aşağıdaki `environment` etiketi bir ortamda dışında çalıştırırken ile birlikte gelen ve küçültülmüş CSS dosyaları işler `Development`.</span><span class="sxs-lookup"><span data-stu-id="ced3f-220">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="ced3f-221">Örneğin, çalışan `Production` veya `Staging` tetikler bu stil sayfaları oluşturma:</span><span class="sxs-lookup"><span data-stu-id="ced3f-221">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ced3f-222">ASP.NET 2.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="ced3f-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ced3f-223">ASP.NET 1.x çekirdek</span><span class="sxs-lookup"><span data-stu-id="ced3f-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="ced3f-224">Gulp gelen bundleconfig.JSON kullanma</span><span class="sxs-lookup"><span data-stu-id="ced3f-224">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="ced3f-225">Bir uygulamanın paketleme ve küçültme iş akışı ek işlem gerektiren durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="ced3f-225">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="ced3f-226">Görüntüyü iyileştirme, önbellek busting ve CDN varlık işleme örnekleri içerir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-226">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="ced3f-227">Bu gereksinimleri karşılamak için Gulp kullanmak için paketleme ve küçültme iş akışı dönüştürebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ced3f-227">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="ced3f-228">Paketleyici & küçültücü uzantısı kullanma</span><span class="sxs-lookup"><span data-stu-id="ced3f-228">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="ced3f-229">Visual Studio [Paketleyici & küçültücü](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) uzantısını Gulp dönüştürme işler.</span><span class="sxs-lookup"><span data-stu-id="ced3f-229">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

<span data-ttu-id="ced3f-230">Sağ *bundleconfig.json* dosya Çözüm Gezgini'nde ve seçin **Paketleyici & küçültücü** > **dönüştürmek için Gulp...** :</span><span class="sxs-lookup"><span data-stu-id="ced3f-230">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Bağlam menüsü öğesini dönüştürmek için Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="ced3f-232">*Gulpfile.js* ve *package.json* dosyaları projeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-232">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="ced3f-233">Destekleyici [npm](https://www.npmjs.com/) içinde listelenen paketler *package.json* dosyanın `devDependencies` bölüm yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-233">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="ced3f-234">Genel bir bağımlılık olarak Gulp CLI yükleme PMC penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ced3f-234">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="ced3f-235">*Gulpfile.js* dosya okuma *bundleconfig.json* giriş, çıkış ve ayarları dosyası.</span><span class="sxs-lookup"><span data-stu-id="ced3f-235">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="ced3f-236">El ile Dönüştür</span><span class="sxs-lookup"><span data-stu-id="ced3f-236">Convert manually</span></span>

<span data-ttu-id="ced3f-237">Visual Studio ve/veya Paketleyici & küçültücü uzantısı yoksa, el ile dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="ced3f-237">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="ced3f-238">Ekleme bir *package.json* dosyasıyla aşağıdaki `devDependencies`, proje kök:</span><span class="sxs-lookup"><span data-stu-id="ced3f-238">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="ced3f-239">Aynı düzeyde aşağıdaki komutu çalıştırarak bağımlılıkları yükler *package.json*:</span><span class="sxs-lookup"><span data-stu-id="ced3f-239">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="ced3f-240">Gulp CLI genel bağımlılık olarak yükleyin:</span><span class="sxs-lookup"><span data-stu-id="ced3f-240">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="ced3f-241">Kopya *gulpfile.js* proje kök dosya altında:</span><span class="sxs-lookup"><span data-stu-id="ced3f-241">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="ced3f-242">Gulp görevleri Çalıştır</span><span class="sxs-lookup"><span data-stu-id="ced3f-242">Run Gulp tasks</span></span>

<span data-ttu-id="ced3f-243">Visual Studio'da projeyi derlemeler önce Gulp küçültme görev tetiklemek için aşağıdaki ekleyin [MSBuild hedef](/visualstudio/msbuild/msbuild-targets) *.csproj dosyasına:</span><span class="sxs-lookup"><span data-stu-id="ced3f-243">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the *.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="ced3f-244">Bu örnekte, içinde herhangi bir görevi tanımlanan `MyPreCompileTarget` önceden tanımlanmış önce çalıştırmak hedef `Build` hedef.</span><span class="sxs-lookup"><span data-stu-id="ced3f-244">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="ced3f-245">Visual Studio çıktı penceresinde aşağıdakine benzer bir çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="ced3f-245">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

<span data-ttu-id="ced3f-246">Alternatif olarak, Visual Studio'nun görev Çalıştırıcı Gezgini belirli Visual Studio olayları Gulp görevleri bağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ced3f-246">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="ced3f-247">Bkz: [varsayılan görevleri çalıştırma](xref:client-side/using-gulp#running-default-tasks) , bunu ilgili yönergeler için.</span><span class="sxs-lookup"><span data-stu-id="ced3f-247">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ced3f-248">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ced3f-248">Additional resources</span></span>

* [<span data-ttu-id="ced3f-249">Gulp kullanma</span><span class="sxs-lookup"><span data-stu-id="ced3f-249">Using Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="ced3f-250">Grunt kullanma</span><span class="sxs-lookup"><span data-stu-id="ced3f-250">Using Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="ced3f-251">Birden çok ortamı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="ced3f-251">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="ced3f-252">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="ced3f-252">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
