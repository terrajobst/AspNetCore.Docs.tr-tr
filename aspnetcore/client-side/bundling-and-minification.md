---
title: ASP.NET Core statik varlıkları paketleyin ve azın
author: scottaddie
description: Bir ASP.NET Core Web uygulamasındaki statik kaynakları paketleme ve küçültmeye yönelik teknikleri uygulayarak nasıl iyileştirileyeceğinizi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: a7a5c40d6c31c4416212c02c1b491dd794f2a1d3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658272"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a><span data-ttu-id="6fedd-103">ASP.NET Core statik varlıkları paketleyin ve azın</span><span class="sxs-lookup"><span data-stu-id="6fedd-103">Bundle and minify static assets in ASP.NET Core</span></span>

<span data-ttu-id="6fedd-104">[Scott Ade](https://twitter.com/Scott_Addie) ve [David çam](https://twitter.com/davidpine7) tarafından</span><span class="sxs-lookup"><span data-stu-id="6fedd-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="6fedd-105">Bu makalede, bu özelliklerin ASP.NET Core Web uygulamalarıyla nasıl kullanılabileceği da dahil olmak üzere paketleme ve küçültmeye yönelik uygulama avantajları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="6fedd-106">Paketlemeyi ve küçültmeye göre</span><span class="sxs-lookup"><span data-stu-id="6fedd-106">What is bundling and minification</span></span>

<span data-ttu-id="6fedd-107">Paketleme ve küçültme, bir Web uygulamasında uygulayabileceğiniz iki ayrı performans iyileştirmesidir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="6fedd-108">Birlikte kullanıldığında, paketleme ve minbirleştirime, sunucu isteklerinin sayısını azaltarak ve istenen statik varlıkların boyutunu azaltarak performansı geliştirir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="6fedd-109">Paketleme ve minbirleştirmesi birincil olarak ilk sayfa isteği yükleme süresini geliştirir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="6fedd-110">Bir Web sayfası istendiğinde, tarayıcı statik varlıkları (JavaScript, CSS ve görüntüler) önbelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="6fedd-111">Sonuç olarak, aynı sayfada veya sayfalarda aynı varlıkları talep eden aynı siteye veya sayfalara istekte bulunduğunda, paketleme ve minlıme performansı artırmayın.</span><span class="sxs-lookup"><span data-stu-id="6fedd-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="6fedd-112">Süre sonu üst bilgisi varlıklarda doğru ayarlanmamışsa ve paketleme ve küçültme kullanılmıyorsa, tarayıcının yenilik buluşsal yöntemleri, varlıkları birkaç günden daha eski bir süre sonra işaretler.</span><span class="sxs-lookup"><span data-stu-id="6fedd-112">If the expires header isn't set correctly on the assets and if bundling and minification isn't used, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="6fedd-113">Ayrıca, tarayıcı her varlık için bir doğrulama isteği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="6fedd-114">Bu durumda, paketleme ve minbirleşme, ilk sayfa isteğinden sonra bile bir performans geliştirmesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6fedd-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="6fedd-115">Paketleme</span><span class="sxs-lookup"><span data-stu-id="6fedd-115">Bundling</span></span>

<span data-ttu-id="6fedd-116">Paketleme, birden çok dosyayı tek bir dosya halinde birleştirir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="6fedd-117">Paketleme, Web sayfası gibi bir Web varlığını işlemek için gerekli olan sunucu isteği sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-117">Bundling reduces the number of server requests that are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="6fedd-118">Özellikle CSS, JavaScript vb. için istediğiniz sayıda ayrı paket oluşturabilirsiniz. Daha az dosya tarayıcıdan sunucuya veya uygulamanızı sağlayan hizmetten daha az HTTP isteğinin olması anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="6fedd-119">Bu, geliştirilmiş ilk sayfa yükü performansına neden olur.</span><span class="sxs-lookup"><span data-stu-id="6fedd-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="6fedd-120">Küçültme</span><span class="sxs-lookup"><span data-stu-id="6fedd-120">Minification</span></span>

<span data-ttu-id="6fedd-121">Minbirleşme işlevleri işlevselliği değiştirmeden koddan gereksiz karakterleri kaldırır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="6fedd-122">Sonuç, istenen varlıklarda (CSS, görüntüler ve JavaScript dosyaları gibi) önemli bir boyut azalmasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="6fedd-123">Yaygın olarak kullanılan yan etkileri, değişken adlarını tek bir karaktere kısaltmayı ve açıklamaları ve gereksiz boşlukları kaldırmayı içerir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="6fedd-124">Aşağıdaki JavaScript işlevini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="6fedd-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="6fedd-125">Minbirleşme işlevi şu şekilde azaltır:</span><span class="sxs-lookup"><span data-stu-id="6fedd-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="6fedd-126">Açıklamaları ve gereksiz boşlukları kaldırmanın yanı sıra aşağıdaki parametre ve değişken adları şu şekilde yeniden adlandırıldı:</span><span class="sxs-lookup"><span data-stu-id="6fedd-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="6fedd-127">Özgün</span><span class="sxs-lookup"><span data-stu-id="6fedd-127">Original</span></span> | <span data-ttu-id="6fedd-128">İsim</span><span class="sxs-lookup"><span data-stu-id="6fedd-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="6fedd-129">Paketleme ve küçültmeye yönelik etkileri</span><span class="sxs-lookup"><span data-stu-id="6fedd-129">Impact of bundling and minification</span></span>

<span data-ttu-id="6fedd-130">Aşağıdaki tabloda, tek tek yükleme varlıkları ve paketleme ve küçültme kullanımı arasındaki farklılıklar özetlenmektedir:</span><span class="sxs-lookup"><span data-stu-id="6fedd-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="6fedd-131">Eylem</span><span class="sxs-lookup"><span data-stu-id="6fedd-131">Action</span></span> | <span data-ttu-id="6fedd-132">B/M ile</span><span class="sxs-lookup"><span data-stu-id="6fedd-132">With B/M</span></span> | <span data-ttu-id="6fedd-133">B/M olmadan</span><span class="sxs-lookup"><span data-stu-id="6fedd-133">Without B/M</span></span> | <span data-ttu-id="6fedd-134">Değiştir</span><span class="sxs-lookup"><span data-stu-id="6fedd-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="6fedd-135">Dosya Istekleri</span><span class="sxs-lookup"><span data-stu-id="6fedd-135">File Requests</span></span>  | <span data-ttu-id="6fedd-136">7</span><span class="sxs-lookup"><span data-stu-id="6fedd-136">7</span></span>   | <span data-ttu-id="6fedd-137">18</span><span class="sxs-lookup"><span data-stu-id="6fedd-137">18</span></span>     | <span data-ttu-id="6fedd-138">157%</span><span class="sxs-lookup"><span data-stu-id="6fedd-138">157%</span></span>
<span data-ttu-id="6fedd-139">KB aktarıldı</span><span class="sxs-lookup"><span data-stu-id="6fedd-139">KB Transferred</span></span> | <span data-ttu-id="6fedd-140">156</span><span class="sxs-lookup"><span data-stu-id="6fedd-140">156</span></span> | <span data-ttu-id="6fedd-141">264.68</span><span class="sxs-lookup"><span data-stu-id="6fedd-141">264.68</span></span> | <span data-ttu-id="6fedd-142">%70</span><span class="sxs-lookup"><span data-stu-id="6fedd-142">70%</span></span>
<span data-ttu-id="6fedd-143">Yükleme süresi (MS)</span><span class="sxs-lookup"><span data-stu-id="6fedd-143">Load Time (ms)</span></span> | <span data-ttu-id="6fedd-144">885</span><span class="sxs-lookup"><span data-stu-id="6fedd-144">885</span></span> | <span data-ttu-id="6fedd-145">2360</span><span class="sxs-lookup"><span data-stu-id="6fedd-145">2360</span></span>   | <span data-ttu-id="6fedd-146">167%</span><span class="sxs-lookup"><span data-stu-id="6fedd-146">167%</span></span>

<span data-ttu-id="6fedd-147">Tarayıcılar HTTP istek üst bilgileriyle ilgili oldukça ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="6fedd-148">Gönderilen toplam bayt ölçümü, paketleme sırasında önemli bir düşüş gördük.</span><span class="sxs-lookup"><span data-stu-id="6fedd-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="6fedd-149">Yükleme zamanı önemli bir geliştirme gösterir, ancak bu örnek yerel olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="6fedd-150">Ağ üzerinden aktarılan varlıklarla paketleme ve küçültmeye karşı kullanım sırasında daha fazla performans artışı gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="6fedd-151">Bir paketleme ve küçültmeye karşı bir strateji seçin</span><span class="sxs-lookup"><span data-stu-id="6fedd-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="6fedd-152">MVC ve Razor Pages proje şablonları, bir JSON yapılandırma dosyasından oluşan paketleme ve küçültmeye yönelik kullanıma hazır bir çözüm sunar.</span><span class="sxs-lookup"><span data-stu-id="6fedd-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="6fedd-153">[Grdalar](xref:client-side/using-grunt) görev Çalıştırıcısı gibi üçüncü taraf araçlar, aynı görevleri biraz daha karmaşıklıkla yerine getirmiş.</span><span class="sxs-lookup"><span data-stu-id="6fedd-153">Third-party tools, such as the [Grunt](xref:client-side/using-grunt) task runner, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="6fedd-154">Geliştirme iş akışınız, bağlama ve görüntü iyileştirmesi gibi&mdash;, paket oluşturma ve küçültmeye göre işleme gerektirdiğinde, üçüncü taraf bir araç harika bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="6fedd-155">Tasarım zamanı paketleme ve küçültme kullanarak, küçültülmüş dosyalar uygulamanın dağıtımından önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6fedd-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="6fedd-156">Dağıtımdan önce paketleme ve küçültme, azaltılmış sunucu yükünün avantajlarından faydalanabilmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6fedd-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="6fedd-157">Bununla birlikte, tasarım zamanı paketleme ve küçültme, derleme karmaşıklığını artırır ve yalnızca statik dosyalarla birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="6fedd-158">Paketlemeyi ve küçültmeye göre yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6fedd-158">Configure bundling and minification</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="6fedd-159">ASP.NET Core 2,0 veya önceki sürümlerde, MVC ve Razor Pages proje şablonları her bir paket için seçenekleri tanımlayan bir *paketleme liconfig. JSON* yapılandırma dosyası sağlar:</span><span class="sxs-lookup"><span data-stu-id="6fedd-159">In ASP.NET Core 2.0 or earlier, the MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file that defines the options for each bundle:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6fedd-160">ASP.NET Core 2,1 veya sonraki sürümlerde, MVC veya Razor Pages proje köküne *paketleme liconfig. JSON*adlı yenı bir JSON dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6fedd-160">In ASP.NET Core 2.1 or later, add a new JSON file, named *bundleconfig.json*, to the MVC or Razor Pages project root.</span></span> <span data-ttu-id="6fedd-161">Aşağıdaki JSON 'yi bir başlangıç noktası olarak bu dosyaya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6fedd-161">Include the following JSON in that file as a starting point:</span></span>

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="6fedd-162">*Paketleme liconfig. JSON* dosyası her bir paket için seçenekleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6fedd-162">The *bundleconfig.json* file defines the options for each bundle.</span></span> <span data-ttu-id="6fedd-163">Yukarıdaki örnekte, özel JavaScript (*Wwwroot/js/site. js*) ve stil sayfası (*Wwwroot/CSS/site. css*) dosyaları için tek bir paket yapılandırması tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-163">In the preceding example, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files.</span></span>

<span data-ttu-id="6fedd-164">Yapılandırma seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6fedd-164">Configuration options include:</span></span>

* <span data-ttu-id="6fedd-165">`outputFileName`: çıkış yapılacak paket dosyasının adı.</span><span class="sxs-lookup"><span data-stu-id="6fedd-165">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="6fedd-166">, *Paketleme liconfig. JSON* dosyasından göreli bir yol içerebilir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-166">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="6fedd-167">**Gerekli**</span><span class="sxs-lookup"><span data-stu-id="6fedd-167">**required**</span></span>
* <span data-ttu-id="6fedd-168">`inputFiles`: birlikte paketedilecek dosya dizisi.</span><span class="sxs-lookup"><span data-stu-id="6fedd-168">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="6fedd-169">Bunlar yapılandırma dosyasına yönelik göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-169">These are relative paths to the configuration file.</span></span> <span data-ttu-id="6fedd-170">**isteğe bağlı**\* boş bir değer boş bir çıktı dosyasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-170">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="6fedd-171">[Glob](https://www.tldp.org/LDP/abs/html/globbingref.html) desenleri desteklenir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-171">[globbing](https://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="6fedd-172">`minify`: çıkış türü için minbirleşme seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="6fedd-172">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="6fedd-173">**isteğe bağlı**, *varsayılan `minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="6fedd-173">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="6fedd-174">Yapılandırma seçenekleri çıkış dosyası türü başına kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-174">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="6fedd-175">CSS minifier</span><span class="sxs-lookup"><span data-stu-id="6fedd-175">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="6fedd-176">JavaScript minifier</span><span class="sxs-lookup"><span data-stu-id="6fedd-176">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="6fedd-177">HTML minifier</span><span class="sxs-lookup"><span data-stu-id="6fedd-177">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="6fedd-178">`includeInProject`: oluşturulan dosyaların proje dosyasına eklenip eklenmeyeceğini belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="6fedd-178">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="6fedd-179">**isteğe bağlı**, *Varsayılan-yanlış*</span><span class="sxs-lookup"><span data-stu-id="6fedd-179">**optional**, *default - false*</span></span>
* <span data-ttu-id="6fedd-180">`sourceMap`: paketlenmiş dosya için bir kaynak eşlemesi oluşturulup oluşturulmayacağını belirten bayrak.</span><span class="sxs-lookup"><span data-stu-id="6fedd-180">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="6fedd-181">**isteğe bağlı**, *Varsayılan-yanlış*</span><span class="sxs-lookup"><span data-stu-id="6fedd-181">**optional**, *default - false*</span></span>
* <span data-ttu-id="6fedd-182">`sourceMapRootPath`: oluşturulan kaynak eşleme dosyasını depolamak için kök yolu.</span><span class="sxs-lookup"><span data-stu-id="6fedd-182">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="6fedd-183">Paketleme ve küçültmeye yönelik derleme zamanı yürütme</span><span class="sxs-lookup"><span data-stu-id="6fedd-183">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="6fedd-184">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet paketi, derleme zamanında paketleme ve küçültmeye karşı yürütmeyi mümkün bir şekilde sunar.</span><span class="sxs-lookup"><span data-stu-id="6fedd-184">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="6fedd-185">Paket, derleme ve Temizleme zamanında çalıştırılan [MSBuild hedeflerini](/visualstudio/msbuild/msbuild-targets) çıkartır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-185">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="6fedd-186">*Paketleme liconfig. JSON* dosyası, tanımlanan yapılandırmaya göre çıkış dosyalarını oluşturmak için yapı işlemi tarafından çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-186">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="6fedd-187">BuildBundlerMinifier, GitHub 'da Microsoft 'un destek sunmamakta olan topluluk odaklı bir projeye aittir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-187">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="6fedd-188">Sorunlar [burada](https://github.com/madskristensen/BundlerMinifier/issues)dosyalanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-188">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6fedd-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fedd-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6fedd-190">*BuildBundlerMinifier* paketini projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6fedd-190">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="6fedd-191">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6fedd-191">Build the project.</span></span> <span data-ttu-id="6fedd-192">Çıkış penceresinde aşağıdakiler görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="6fedd-192">The following appears in the Output window:</span></span>

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

<span data-ttu-id="6fedd-193">Projeyi temizleyin.</span><span class="sxs-lookup"><span data-stu-id="6fedd-193">Clean the project.</span></span> <span data-ttu-id="6fedd-194">Çıkış penceresinde aşağıdakiler görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="6fedd-194">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-cli"></a>[<span data-ttu-id="6fedd-195">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6fedd-195">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6fedd-196">*BuildBundlerMinifier* paketini projenize ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6fedd-196">Add the *BuildBundlerMinifier* package to your project:</span></span>

```dotnetcli
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="6fedd-197">ASP.NET Core 1. x kullanıyorsanız, yeni eklenen paketi geri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="6fedd-197">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```dotnetcli
dotnet restore
```

<span data-ttu-id="6fedd-198">Projeyi derleyin:</span><span class="sxs-lookup"><span data-stu-id="6fedd-198">Build the project:</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="6fedd-199">Aşağıdakiler görünür:</span><span class="sxs-lookup"><span data-stu-id="6fedd-199">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="6fedd-200">Projeyi temizle:</span><span class="sxs-lookup"><span data-stu-id="6fedd-200">Clean the project:</span></span>

```dotnetcli
dotnet clean
```

<span data-ttu-id="6fedd-201">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="6fedd-201">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="6fedd-202">Paketlemeyi ve küçültmeye karşı geçici yürütme</span><span class="sxs-lookup"><span data-stu-id="6fedd-202">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="6fedd-203">Paketleme ve küçültmeye yönelik görevleri, projeyi oluşturmadan geçici olarak çalıştırmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6fedd-203">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="6fedd-204">[BundlerMinifier. Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet paketini projenize ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6fedd-204">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="6fedd-205">BundlerMinifier. Core, GitHub 'da Microsoft 'un destek sunmamakta olan topluluk odaklı bir projeye aittir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-205">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="6fedd-206">Sorunlar [burada](https://github.com/madskristensen/BundlerMinifier/issues)dosyalanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-206">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="6fedd-207">Bu paket, .NET Core CLI *DotNet-demeti* aracını içerecek şekilde genişletir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-207">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="6fedd-208">Aşağıdaki komut, Paket Yöneticisi Konsolu (PMC) penceresinde veya bir komut kabuğunda çalıştırılabilir:</span><span class="sxs-lookup"><span data-stu-id="6fedd-208">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```dotnetcli
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="6fedd-209">NuGet Paket Yöneticisi, \*. csproj dosyasına `<PackageReference />` düğümleri olarak bağımlılıklar ekler.</span><span class="sxs-lookup"><span data-stu-id="6fedd-209">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="6fedd-210">`dotnet bundle` komutu, yalnızca bir `<DotNetCliToolReference />` düğümü kullanıldığında .NET Core CLI kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-210">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="6fedd-211">\*. Csproj dosyasını uygun şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6fedd-211">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="6fedd-212">İş akışına dosya ekleme</span><span class="sxs-lookup"><span data-stu-id="6fedd-212">Add files to workflow</span></span>

<span data-ttu-id="6fedd-213">Aşağıdakilere benzer bir ek *özel. css* dosyası eklendiğini bir örnek düşünün:</span><span class="sxs-lookup"><span data-stu-id="6fedd-213">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="6fedd-214">*Custom. css* dosyasını küçültmeye ve *site. css* ' yi bir *site. min. css* dosyasına paketlemek için, ilgili yolu *paketleme liconfig. JSON*öğesine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6fedd-214">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="6fedd-215">Alternatif olarak, aşağıdaki glob deseninin kullanılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="6fedd-215">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/!(*.min).css" ]
> ```
>
> <span data-ttu-id="6fedd-216">Bu glob model tüm CSS dosyalarıyla eşleşir ve küçültülmüş dosya modelini dışlar.</span><span class="sxs-lookup"><span data-stu-id="6fedd-216">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="6fedd-217">Uygulamayı derleyin.</span><span class="sxs-lookup"><span data-stu-id="6fedd-217">Build the application.</span></span> <span data-ttu-id="6fedd-218">*Site. min. css* dosyasını açın ve *Custom. css* içeriğinin dosyanın sonuna ekleneceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="6fedd-218">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="6fedd-219">Ortam tabanlı paketleme ve minbirleşme</span><span class="sxs-lookup"><span data-stu-id="6fedd-219">Environment-based bundling and minification</span></span>

<span data-ttu-id="6fedd-220">En iyi uygulama olarak, uygulamanızın paketlenmiş ve küçültülmüş dosyaları bir üretim ortamında kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-220">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="6fedd-221">Geliştirme sırasında, özgün dosyalar uygulamanın hata ayıklamasını daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-221">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="6fedd-222">Görünümlerinizde [ortam etiketi yardımcısını](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) kullanarak sayfalarınıza hangi dosyaların ekleneceğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="6fedd-222">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="6fedd-223">Ortam etiketi Yardımcısı yalnızca belirli [ortamlarda](xref:fundamentals/environments)çalışırken içeriğini işler.</span><span class="sxs-lookup"><span data-stu-id="6fedd-223">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="6fedd-224">Aşağıdaki `environment` etiketi, `Development` ortamında çalışırken işlenmemiş CSS dosyalarını işler:</span><span class="sxs-lookup"><span data-stu-id="6fedd-224">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

<span data-ttu-id="6fedd-225">Aşağıdaki `environment` etiketi, `Development`dışında bir ortamda çalışırken paketlenmiş ve küçültülmüş CSS dosyalarını işler.</span><span class="sxs-lookup"><span data-stu-id="6fedd-225">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="6fedd-226">Örneğin, `Production` veya `Staging` içinde çalıştırmak, bu stil sayfaları işlemesini tetikler:</span><span class="sxs-lookup"><span data-stu-id="6fedd-226">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="6fedd-227">Gulp adresinden paketleme liconfig. JSON kullanın</span><span class="sxs-lookup"><span data-stu-id="6fedd-227">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="6fedd-228">Uygulamanın paketleme ve küçültmeye yönelik iş akışının ek işlem gerektirdiği durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-228">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="6fedd-229">Örnek görüntü iyileştirmesi, önbellek performansı ve CDN varlık işleme sayılabilir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-229">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="6fedd-230">Bu gereksinimleri karşılamak için, paketleme ve küçültmeye yönelik iş akışını Gulp kullanacak şekilde dönüştürebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6fedd-230">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="6fedd-231">Paketleme & minifier uzantısını kullanma</span><span class="sxs-lookup"><span data-stu-id="6fedd-231">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="6fedd-232">Visual Studio [Paketcisi & minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) uzantısı, Gulp 'e dönüştürmeyi işler.</span><span class="sxs-lookup"><span data-stu-id="6fedd-232">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="6fedd-233">Paketleme & minifier uzantısı, Microsoft 'un hiçbir destek sunmamakta olan GitHub 'daki topluluk odaklı bir projeye aittir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-233">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="6fedd-234">Sorunlar [burada](https://github.com/madskristensen/BundlerMinifier/issues)dosyalanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-234">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="6fedd-235">Çözüm Gezgini *paketleme liconfig. JSON* dosyasına sağ tıklayın ve **paketler & minifier** ' ı seçin > **Gulp 'e Dönüştür...** :</span><span class="sxs-lookup"><span data-stu-id="6fedd-235">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![Gulp bağlam menüsü öğesine Dönüştür](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="6fedd-237">*Gulpfile. js* ve *Package. JSON* dosyaları projeye eklenir.</span><span class="sxs-lookup"><span data-stu-id="6fedd-237">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="6fedd-238">*Package. JSON* dosyasının `devDependencies` bölümünde listelenen [NPM](https://www.npmjs.com/) paketlerinin desteklenmesi yüklüdür.</span><span class="sxs-lookup"><span data-stu-id="6fedd-238">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="6fedd-239">Gulp CLı 'yi genel bağımlılık olarak yüklemek için PMC penceresinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6fedd-239">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="6fedd-240">*Gulpfile. js* dosyası, girdiler, çıktılar ve ayarlar için *paketleme liconfig. JSON* dosyasını okur.</span><span class="sxs-lookup"><span data-stu-id="6fedd-240">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="6fedd-241">El ile Dönüştür</span><span class="sxs-lookup"><span data-stu-id="6fedd-241">Convert manually</span></span>

<span data-ttu-id="6fedd-242">Visual Studio ve/veya paketleme & minifier uzantısı kullanılamıyorsa, el ile dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="6fedd-242">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="6fedd-243">Proje köküne aşağıdaki `devDependencies`sahip bir *Package. JSON* dosyası ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6fedd-243">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

> [!WARNING]
> <span data-ttu-id="6fedd-244">`gulp-uglify` modülü ECMAScript (ES) 2015/ES6 ve üstünü desteklemez.</span><span class="sxs-lookup"><span data-stu-id="6fedd-244">The `gulp-uglify` module doesn't support ECMAScript (ES) 2015 / ES6 and later.</span></span> <span data-ttu-id="6fedd-245">ES2015/ES6 veya sonraki bir sürümü kullanmak için `gulp-uglify` yerine [Gulp-Terser](https://www.npmjs.com/package/gulp-terser) 'i yükler.</span><span class="sxs-lookup"><span data-stu-id="6fedd-245">Install [gulp-terser](https://www.npmjs.com/package/gulp-terser) instead of `gulp-uglify` to use ES2015 / ES6 or later.</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="6fedd-246">Aşağıdaki komutu *Package. JSON*ile aynı düzeyde çalıştırarak bağımlılıkları yükler:</span><span class="sxs-lookup"><span data-stu-id="6fedd-246">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="6fedd-247">Gulp CLı 'yı genel bağımlılık olarak yükler:</span><span class="sxs-lookup"><span data-stu-id="6fedd-247">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="6fedd-248">Aşağıdaki *gulpfile. js* dosyasını proje köküne kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="6fedd-248">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="6fedd-249">Gulp görevlerini Çalıştır</span><span class="sxs-lookup"><span data-stu-id="6fedd-249">Run Gulp tasks</span></span>

<span data-ttu-id="6fedd-250">Proje Visual Studio 'da yapılandırmadan önce Gulp minbirleşme görevini tetiklemek için, \*. csproj dosyasına aşağıdaki [MSBuild hedefini](/visualstudio/msbuild/msbuild-targets) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="6fedd-250">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="6fedd-251">Bu örnekte, `MyPreCompileTarget` hedefi içinde tanımlanan tüm görevler önceden tanımlanmış `Build` hedeften önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="6fedd-251">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="6fedd-252">Visual Studio 'nun çıkış penceresinde aşağıdakine benzer bir çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="6fedd-252">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="6fedd-253">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6fedd-253">Additional resources</span></span>

* [<span data-ttu-id="6fedd-254">Grunt kullanma</span><span class="sxs-lookup"><span data-stu-id="6fedd-254">Use Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="6fedd-255">Birden çok ortam kullanma</span><span class="sxs-lookup"><span data-stu-id="6fedd-255">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="6fedd-256">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="6fedd-256">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
