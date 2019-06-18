---
title: Paketleme ve küçültme ASP.NET Core statik varlıkları
author: scottaddie
description: Bir ASP.NET Core web uygulaması, statik kaynakları paketleme ve küçültme tekniklerini uygulayarak en iyi duruma getirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/17/2019
uid: client-side/bundling-and-minification
ms.openlocfilehash: c3ec67d9f5c2b8eb44b5ced911ccfd0fa40a52a7
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152779"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Paketleme ve küçültme ASP.NET Core statik varlıkları

Tarafından [Scott Addie](https://twitter.com/Scott_Addie) ve [David Çam](https://twitter.com/davidpine7)

Bu makalede, paketleme ve küçültme, bu özellikler, ASP.NET Core web apps ile nasıl kullanılabileceğini de dahil olmak üzere uygulama avantajları açıklanmaktadır.

## <a name="what-is-bundling-and-minification"></a>Paketleme ve küçültme nedir

Paketleme ve küçültme, bir web uygulamasında uygulayabileceğiniz iki farklı performans iyileştirmelerini var. Birlikte kullanıldığında, paketleme ve küçültme sunucu isteklerinin sayısını azaltmak ve istenen statik varlıkları boyutunu küçültmeyi performansını.

Paketleme ve küçültme öncelikle ilk sayfa isteği yükleme süresi geliştirmek. Bir web sayfası istenen sonra tarayıcıyı statik varlıkları (JavaScript, CSS ve görüntüleri) önbelleğe alır. Sonuç olarak, paketleme ve küçültme aynı sayfa veya sayfalarda aynı varlıkları isteyen aynı sitede isterken performansını yok. Varsa expires üst bilgisi varlıklar üzerinde düzgün ayarlanmamış ve paketleme ve küçültme kullanılmaz, tarayıcının güncellik buluşsal yöntemler varlıkları eski birkaç gün sonra işaretleyin. Ayrıca, tarayıcı, her varlık için bir doğrulama isteği gerektirir. Bu durumda, paketleme ve küçültme, ilk sayfa isteği sonra bile bir performans gelişmesi sağlar.

### <a name="bundling"></a>Paketleme

Paketleme, birden çok dosyayı tek bir dosya halinde birleştirir. Paketleme, bir web sayfası gibi bir web varlığı işlemek gerekli olan sunucu isteklerinin sayısını azaltır. Tek paketler herhangi bir sayıda özellikle CSS, JavaScript, vb. oluşturabilirsiniz. Daha az dosya sunucusu tarayıcıya veya uygulamanızı sağlayan hizmet daha az HTTP isteklerini anlamına gelir. Bu sayede, ilk sayfa yükleme performansı geliştirildi.

### <a name="minification"></a>Küçültme

Küçültme, işlevselliği değiştirmeden koddan gereksiz karakterleri kaldırır. Sonuç, istenilen varlıkların (CSS, görüntü ve JavaScript dosyaları gibi) önemli boyutta bir düşüş olur. Küçültme ortak yan etkilerini kısaltmayı bir karakter, değişken adları ve açıklamaları ve gereksiz boşluk kaldırma içerir.

Aşağıdaki JavaScript işlevi göz önünde bulundurun:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Küçültme işlevi aşağıdaki azaltır:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Açıklamalar ve gereksiz boşluk kaldırma ek olarak, aşağıdaki parametre ve değişken adları şu şekilde değiştirilmiştir:

Özgün | Yeniden adlandırıldı
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Paketleme ve küçültme etkisi

Aşağıdaki tabloda, tek tek varlıklar yükleniyor ve paketleme ve küçültme kullanarak arasındaki farklar özetlenmektedir:

Eylem | B/dk ile | B/dk | Değiştir
--- | :---: | :---: | :---:
Dosya istekleri  | 7   | 18     | 157%
Aktarılan KB | 156 | 264.68 | 70%
Yükleme süresi (ms) | 885 | 2360   | 167%

Tarayıcılar HTTP istek üstbilgilerinin açısından oldukça ayrıntılıdır. Toplam bayt ölçüm paketleme sırasında önemli bir azalma gördüğünüz gönderdi. Bu örnek yerel olarak çalıştı ancak önemli bir iyileştirme yükleme zamanını gösterir. Paketleme ve küçültme ile varlıkları kullanarak bir ağ üzerinden aktarıldığında büyük performans artışı alırlar.

## <a name="choose-a-bundling-and-minification-strategy"></a>Paketleme ve küçültme stratejisi seçme

MVC ve Razor sayfaları proje şablonları, paketleme ve küçültme içeren bir JSON yapılandırma dosyası için kullanıma hazır bir çözüm sağlar. Gibi üçüncü taraf araçları [Grunt](xref:client-side/using-grunt) görev Çalıştırıcı, biraz daha karmaşık aynı görevleri gerçekleştirin. Geliştirme iş akışınızın paketleme ve küçültme ötesinde işleme gerektirdiğinde mükemmel bir uyum üçüncü taraf bir araç olan&mdash;linting ve görüntü iyileştirme gibi. Tasarım zamanı paketleme ve küçültme kullanarak küçültülmüş dosyaları uygulamanın dağıtımdan önce oluşturulur. Paketleme ve küçültme dağıtımdan önce düşük sunucu yükü avantajı sağlar. Ancak, bu tasarım zamanı paketleme bilmek önemlidir ve küçültme yapıyı karmaşıklık artar ve yalnızca statik dosyalar ile çalışır.

## <a name="configure-bundling-and-minification"></a>Paketleme ve küçültme yapılandırın

::: moniker range="<= aspnetcore-2.0"

ASP.NET Core 2.0 veya daha önceki sürümlerde, MVC ve Razor sayfaları proje şablonları sağlar. bir *bundleconfig.json* her paket için seçenekleri tanımlayan yapılandırma dosyası:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 veya daha sonra adlı yeni bir JSON dosyası ekleme *bundleconfig.json*, MVC veya Razor sayfaları proje kök dizini. Aşağıdaki JSON dosya başlangıç noktası olarak şunları içerir:

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

*Bundleconfig.json* dosyası, her paket için seçenekleri tanımlar. Önceki örnekte, bir tek bir paket yapılandırmasını özel JavaScript için tanımlanır (*wwwroot/js/site.js*) ve stil sayfası (*wwwroot/css/site.css*) dosyaları.

Yapılandırma seçenekleri şunlardır:

* `outputFileName`: Çıkış için paket dosyasının adı. Bir göreli yol içerebilir *bundleconfig.json* dosya. **Gerekli**
* `inputFiles`: Paket dosyaları dizisi. Yapılandırma dosyası için göreli yollar şunlardır. **İsteğe bağlı**, * bir boş çıkış dosyası boş değer sonuçlanır. [Genelleştirme](http://www.tldp.org/LDP/abs/html/globbingref.html) desenler desteklenir.
* `minify`: Çıktı türü küçültme seçenekleri. **İsteğe bağlı**, *varsayılan - `minify: { enabled: true }`*
  * Çıkış dosyasının türünü yapılandırma seçenekleri kullanılabilir.
    * [CSS Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript küçültücü](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [HTML Minifier](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Proje dosyasına oluşturulan dosyaları eklenip eklenmeyeceğini belirten bayrak. **İsteğe bağlı**, *varsayılan - yanlış*
* `sourceMap`: Bir kaynak eşlemesi ile birlikte gelen dosyası oluşturulup oluşturulmayacağını belirten bayrak. **İsteğe bağlı**, *varsayılan - yanlış*
* `sourceMapRootPath`: Oluşturulan kaynak eşleme dosyası depolamak için kök yolu.

## <a name="build-time-execution-of-bundling-and-minification"></a>Derleme zamanı yürütülmesini paketleme ve küçültme

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) yürütme paketleme ve küçültme derleme zamanında NuGet paketi sağlar. Paket eklediği [MSBuild hedefleri](/visualstudio/msbuild/msbuild-targets) derleme ve temizleme saatte çalıştırın. *Bundleconfig.json* tanımlı yapılandırmaya dayanarak çıktı dosyalarını üretmek için derleme işlemi tarafından analiz dosyası.

> [!NOTE]
> Microsoft desteği sağlayan github'daki topluluk odaklı projeye BuildBundlerMinifier aittir. Sorun bildirmiş [burada](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Ekleme *BuildBundlerMinifier* paketini projenize.

Projeyi oluşturun. Çıkış penceresinde görüntülenir:

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

Projeyi temizleyin. Çıkış penceresinde görüntülenir:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Ekleme *BuildBundlerMinifier* paketini projenize:

```console
dotnet add package BuildBundlerMinifier
```

ASP.NET kullanıyorsanız, Core 1.x sürümüne, yeni eklenen paket geri yükleme:

```console
dotnet restore
```

Proje derleme:

```console
dotnet build
```

Aşağıdaki seçeneklerle karşılaşırsınız:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Projeyi temizleyin:

```console
dotnet clean
```

Aşağıdaki çıktı görünür:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Geçici yürütülmesini paketleme ve küçültme

Proje oluşturmaya gerek kalmadan geçici esasına göre paketleme ve küçültme görevleri çalıştırmak mümkündür. Ekleme [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet paketini projenize:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> Microsoft desteği sağlayan github'daki topluluk odaklı projeye BundlerMinifier.Core aittir. Sorun bildirmiş [burada](https://github.com/madskristensen/BundlerMinifier/issues).

Bu paket dahil etmek için .NET Core CLI'yı genişletir *dotnet-paket* aracı. Aşağıdaki komut, Paket Yöneticisi Konsolu (PMC) penceresinde veya bir komut kabuğu'nda çalıştırılabilir:

```console
dotnet bundle
```

> [!IMPORTANT]
> NuGet Paket Yöneticisi *.csproj dosyaya bağımlılıkları ekler `<PackageReference />` düğümleri. `dotnet bundle` Komut .NET Core CLI ile kayıtlı yalnızca bir `<DotNetCliToolReference />` düğümü kullanılır. *.Csproj dosyanın uygun şekilde değiştirin.

## <a name="add-files-to-workflow"></a>İş akışı için dosyaları Ekle

Bir örnekte göz önünde bulundurun ek *custom.css* dosya, aşağıdaki benzeyen eklenir:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Küçültülecek *custom.css* ve ile paket *site.css* içine bir *site.min.css* göreli yolunu ekleyin *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Alternatif olarak, aşağıdaki Glob deseni kullanılabilir:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> Bu Glob deseni ile eşleşen tüm CSS dosyaları ve küçültülmüş dosya düzeni dahil değildir.

Uygulamayı derleyin. Açık *site.min.css* ve içeriğini fark *custom.css* dosyasının sonuna eklenir.

## <a name="environment-based-bundling-and-minification"></a>Ortam tabanlı paketleme ve küçültme

En iyi uygulama, paketlenmiş ve küçültülmüş dosyaları uygulamanızı bir üretim ortamında kullanılmalıdır. Geliştirme sırasında daha kolay uygulama hata ayıklama için özgün dosyaları olun.

Kullanarak sayfalarınıza eklemek için hangi dosyaların belirtin [ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) görünümlerinizi de. Ortam etiketi Yardımcısı yalnızca belirli çalıştırırken içeriğini işler [ortamları](xref:fundamentals/environments).

Aşağıdaki `environment` etiketi çalıştırıldığında işlenmemiş CSS dosyaları işler `Development` ortam:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

Aşağıdaki `environment` etiketi ile birlikte gelen ve küçültülmüş CSS dosyaları dışındaki bir ortamda çalışan işler `Development`. Örneğin, çalışan `Production` veya `Staging` bu stil sayfaları işlenmesi tetikleyen:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>Gulp gelen bundleconfig.JSON kullanma

Uygulamanın paketleme ve küçültme iş akışı ek işlem gerektiren durumlar vardır. Görüntü iyileştirme, önbellek busting ve CDN varlık işleme verilebilir. Bu gereksinimleri karşılamak için Gulp kullanmak için paketleme ve küçültme iş akışı dönüştürebilirsiniz.

### <a name="use-the-bundler--minifier-extension"></a>Bundler & Minifier uzantısını kullanma

Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) uzantısını Gulp dönüşümünü işler.

> [!NOTE]
> Microsoft desteği sağlayan github'daki topluluk odaklı projeye Bundler & Minifier uzantısı aittir. Sorun bildirmiş [burada](https://github.com/madskristensen/BundlerMinifier/issues).

Sağ *bundleconfig.json* seçin ve Çözüm Gezgini'nde dosya **Bundler & Minifier** > **dönüştürmek için Gulp...** :

![Bağlam menüsü öğesi için Gulp Dönüştür](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js* ve *package.json* dosyalar projeye eklenir. Destekleyici [npm](https://www.npmjs.com/) listelenen paketleri *package.json* dosyanın `devDependencies` bölümüne yüklenir.

Genel bir bağımlılık olarak Gulp CLI'yı yüklemek için PMC penceresinde aşağıdaki komutu çalıştırın:

```console
npm i -g gulp-cli
```

*Gulpfile.js* dosya okuma *bundleconfig.json* girişler, çıkışlar ve ayarları dosyası.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>El ile Dönüştür

Visual Studio ve/veya Bundler & Minifier uzantısı yoksa, el ile dönüştürün.

Ekleme bir *package.json* dosyasıyla aşağıdaki `devDependencies`, proje kök dizini:

> [!WARNING]
> `gulp-uglify` Modülü, ECMAScript (ES) 2015 desteklemiyor / ES6 ve daha sonra. Yükleme [gulp terser](https://www.npmjs.com/package/gulp-terser) yerine `gulp-uglify` ES2015 kullanılacak / ES6 veya üzeri.

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Aynı düzeyde, aşağıdaki komutu çalıştırarak bağımlılıkları yükler *package.json*:

```console
npm i
```

Genel bir bağımlılık olarak Gulp CLI'yı yükleyin:

```console
npm i -g gulp-cli
```

Kopyalama *gulpfile.js* proje kök dosya aşağıda:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Gulp görevleri çalıştırma

Visual Studio'da proje derlenmeden önce Gulp küçültme görev tetiklemek için aşağıdaki ekleyin [MSBuild hedefi](/visualstudio/msbuild/msbuild-targets) *.csproj dosyaya:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

Bu örnekte, herhangi bir görev içinde tanımlanan. `MyPreCompileTarget` önceden tanımlanmış önce çalıştırma hedef `Build` hedef. Visual Studio çıktı penceresinde aşağıdakine benzer bir çıktı görünür:

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


## <a name="additional-resources"></a>Ek kaynaklar

* [Grunt kullanma](xref:client-side/using-grunt)
* [Birden çok ortam kullanma](xref:fundamentals/environments)
* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)
