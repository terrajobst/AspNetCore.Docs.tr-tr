---
title: ASP.NET Core paket ve minifiy statik varlıkları
author: scottaddie
description: Paketleme ve küçültme teknikleri uygulayarak bir ASP.NET Core web uygulamasında statik kaynakları en iyi duruma getirme hakkında bilgi edinin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: a155422c0fd638f46fe4a9d8a77faebc0b2a5681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a>ASP.NET Core paket ve minifiy statik varlıkları

Tarafından [Scott Addie](https://twitter.com/Scott_Addie)

Bu makalede paketleme ve küçültme, bu özellikler ASP.NET Core web apps ile nasıl kullanılabileceğini de dahil olmak üzere uygulama yararlarını açıklar.

## <a name="what-is-bundling-and-minification"></a>Paketleme ve küçültme nedir?

Paketleme ve küçültme bir web uygulamasını uygulayabilirsiniz iki ayrı performans iyileştirmelerini var. Birlikte kullanıldığında, paketleme ve küçültme sunucusu isteklerinin sayısını azaltmak ve istenen statik varlıklar boyutunun azaltılması performansı.

Paketleme ve küçültme öncelikle ilk sayfa isteği yükleme süresini artırır. Bir web sayfası istenen sonra tarayıcı statik varlıklar (JavaScript, CSS ve görüntüleri) önbelleğe alır. Sonuç olarak, paketleme ve küçültme aynı sayfa veya sayfaları, aynı varlıklar isteyen aynı sitedeki isterken performansı yok. Varsa süresi üstbilgi değil doğru ayarladığınızdan varlıklar ve paketleme ve küçültme değil kullandıysanız, tarayıcının yenilik buluşsal yöntemler varlıklar eski birkaç gün sonra işaretleyin. Ayrıca, tarayıcı her varlık için bir doğrulama isteği gerektirir. Bu durumda, paketleme ve küçültme ilk sayfa isteği sonra bile performans geliştirmesi sağlar.

### <a name="bundling"></a>Paketleme

Paketleme birden çok dosya tek bir dosya halinde birleştirir. Paketleme, bir web sayfası gibi bir web varlık işlemek için gerekli olan sunucu isteklerinin sayısını azaltır. Tek tek Paket herhangi bir sayıda özellikle CSS, JavaScript vb. için oluşturabilirsiniz. Daha az sayıda dosya daha az HTTP isteğini tarayıcı sunucuya veya uygulamanızın sağlayan hizmet anlamına gelir. Bu sonuçları ilk sayfa yükleme performansı geliştirildi.

### <a name="minification"></a>Küçültme

Küçültme işlevselliği değiştirmeden koddan gereksiz karakterleri kaldırır. Sonuç istenilen varlıkların (CSS, görüntüler ve JavaScript dosyaları gibi) önemli boyutu azalma olur. Küçültme ortak yan etkileri değişken adları bir karakter kısaltmak ve açıklamalar ve gereksiz boşluk kaldırma içerir.

Şu JavaScript işlevi göz önünde bulundurun:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Küçültme işlevi aşağıdaki azaltır:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Gereksiz boşluk ve açıklamaları kaldırma ek olarak, aşağıdaki parametre ve değişken adları şu şekilde adlandırıldı:

Özgün | Yeniden adlandırıldı
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Paketleme ve küçültme etkisi

Aşağıdaki tabloda tek tek varlıklar yükleme ve paketleme ve küçültme kullanma arasındaki farklar özetlenmektedir:

Eylem | B/M ile | B/M | Değiştir
--- | :---: | :---: | :---:
Dosya istekleri  | 7   | 18     | 157%
Aktarılan KB | 156 | 264.68 | 70%
Yükleme süresi (ms) | 885 | 2360   | 167%

Tarayıcılar HTTP istek üstbilgilerinin açısından oldukça ayrıntılıdır. Toplam bayt sayısı, paketleme, önemli ölçüde azalma ölçüm gördüğünüz gönderdi. Bu örnek yerel olarak çalıştı ancak önemli bir iyileştirme yükleme zamanını gösterir. Paketleme ve küçültme varlıklarla kullanarak bir ağ üzerinden aktarıldığında büyük performans artışı alırlar.

## <a name="choose-a-bundling-and-minification-strategy"></a>Paketleme ve küçültme stratejisi seçme

MVC ve Razor sayfalarının proje şablonları paketleme ve küçültme oluşan bir JSON yapılandırma dosyası için bir giden kutusu çözümü sağlar. Gibi üçüncü taraf araçları [Gulp](xref:client-side/using-gulp) ve [Grunt](xref:client-side/using-grunt) görev koşucular, biraz daha karmaşık aynı görevleri yerine getirin. Bir üçüncü taraf aracı, geliştirme iş akışınızı paketleme ve küçültme ötesinde işleme gerektiren harika uygun olduğunda&mdash;linting ve görüntü iyileştirme gibi. Tasarım zamanı paketleme ve küçültme kullanarak küçültülmüş dosyaları uygulamanın dağıtımdan önce oluşturulur. Paketleme ve dağıtım öncesinde küçültülmesine azaltılmış sunucu iş yükü avantajı sağlar. Ancak, bu tasarım zamanı paketleme bilmek önemlidir ve küçültme yapı karmaşıklığını artırır ve yalnızca statik dosyaları ile çalışır.

## <a name="configure-bundling-and-minification"></a>Paketleme ve küçültme yapılandırın

MVC ve Razor sayfalarının proje şablonları sağlayan bir *bundleconfig.json* her paket için seçenekleri tanımlayan yapılandırma dosyası. Varsayılan olarak, özel JavaScript için tanımlanmış bir tek Paket yapılandırması (*wwwroot/js/site.js*) ve stil sayfası (*wwwroot/css/site.css*) dosyaları:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Yapılandırma seçenekleri şunlardır:

* `outputFileName`: Çıkış için paket dosyasının adı. Bir göreli yolu içerebilir *bundleconfig.json* dosya. **Gerekli**
* `inputFiles`: Birlikte paketlemektir dosyaları dizisi. Bu yapılandırma dosyasının göreli yollardır. **İsteğe bağlı**, * bir boş çıkış dosyası boş bir değer sonuçlanır. [genelleme](http://www.tldp.org/LDP/abs/html/globbingref.html) desenleri desteklenir.
* `minify`: Çıkış türü küçültme seçenekleri. **İsteğe bağlı**, *varsayılan - `minify: { enabled: true }`*
  * Çıkış dosya türü yapılandırma seçenekleri kullanılabilir.
    * [CSS Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript küçültücü](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [HTML küçültücü](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Proje dosyası için oluşturulan dosyalar eklenip eklenmeyeceğini belirten bayrak. **İsteğe bağlı**, *varsayılan - yanlış*
* `sourceMap`: İle birlikte gelen dosyası için kaynak eşlemesi oluşturulup oluşturulmayacağını belirten bayrak. **İsteğe bağlı**, *varsayılan - yanlış*
* `sourceMapRootPath`: Oluşturulan kaynak eşleme dosyasını depolayan kök yolu.

## <a name="build-time-execution-of-bundling-and-minification"></a>Derleme zamanı yürütülmesi paketleme ve küçültme

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet paketi, paketleme, yürütme ve küçültme derleme zamanında sağlar. Paket yerleştirir [MSBuild hedefleri](/visualstudio/msbuild/msbuild-targets) yapı ve temiz saatte çalıştırın. *Bundleconfig.json* tanımlanan yapılandırmasını temel alarak çıktı dosyaları üretmek için derleme süreci tarafından dosya analiz.

> [!NOTE]
> Microsoft destek sağlayan GitHub topluluk odaklı bir projede BuildBundlerMinifier ait. Sorunları Dosyalanan [burada](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Ekleme *BuildBundlerMinifier* projenize paket.

Projeyi oluşturun. Çıktı penceresinde görünür:

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

Projeyi temizleyin. Çıktı penceresinde görünür:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

Ekleme *BuildBundlerMinifier* projenize paketi:

```console
dotnet add package BuildBundlerMinifier
```

ASP.NET kullanıyorsanız 1.x çekirdek, yeni eklenen paket geri yükleme:

```console
dotnet restore
```

Projeyi oluşturun:

```console
dotnet build
```

Aşağıda görünür:

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

Şu çıktı görünür:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Paketleme ve küçültme geçici yürütülmesi

Proje derleme olmadan bir geçici temelinde paketleme ve küçültme görevleri çalıştırmak mümkündür. Ekleme [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet paketini projenize:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> Microsoft destek sağlayan GitHub topluluk odaklı bir projede BundlerMinifier.Core ait. Sorunları Dosyalanan [burada](https://github.com/madskristensen/BundlerMinifier/issues).

Bu paket dahil etmek için .NET Core CLI genişletir *dotnet paket* aracı. Aşağıdaki komut, Paket Yöneticisi Konsolu (PMC) penceresinde veya bir komut kabuğu çalıştırılabilir:

```console
dotnet bundle
```

> [!IMPORTANT]
> NuGet Paket Yöneticisi *.csproj dosyaya bağımlılıkları ekler `<PackageReference />` düğümleri. `dotnet bundle` Komutu ile .NET Core CLI kayıtlı yalnızca bir `<DotNetCliToolReference />` düğümü kullanılır. *.Csproj dosyasını uygun şekilde değiştirin.

## <a name="add-files-to-workflow"></a>İş akışı için dosyaları Ekle

Bir örnekte göz önünde bulundurun ek bir *custom.css* dosya, aşağıdaki benzeyen eklenir:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Küçültülecek *custom.css* ve onunla paketini *site.css* içine bir *site.min.css* dosya, göreli yol *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> Alternatif olarak, aşağıdaki genelleme düzeni kullanılabilir:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> Bu genelleme deseni tüm CSS dosyaları eşler ve küçültülmüş dosya deseni dışlar.

Uygulamayı oluşturun. Açık *site.min.css* ve içeriğini fark *custom.css* dosyasının sonuna eklenir.

## <a name="environment-based-bundling-and-minification"></a>Ortam tabanlı paketleme ve küçültme

En iyi uygulama, uygulamanızın ile birlikte gelen ve küçültülmüş dosyaları bir üretim ortamında kullanılması gerekir. Geliştirme sırasında daha kolay uygulama hata ayıklama için özgün dosyaları olun.

Sayfalarınızda kullanarak eklemek için hangi dosyaların belirtin [ortam etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) görünümlerinizi de. Ortam etiketi yardımcı yalnızca özel çalıştırırken içeriğini işleyen [ortamları](xref:fundamentals/environments).

Aşağıdaki `environment` etiketi çalıştırırken işlenmemiş CSS dosyaları işler `Development` ortamı:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

* * *
Aşağıdaki `environment` etiketi bir ortamda dışında çalıştırırken ile birlikte gelen ve küçültülmüş CSS dosyaları işler `Development`. Örneğin, çalışan `Production` veya `Staging` tetikler bu stil sayfaları oluşturma:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

* * *
## <a name="consume-bundleconfigjson-from-gulp"></a>Gulp gelen bundleconfig.JSON kullanma

Bir uygulamanın paketleme ve küçültme iş akışı ek işlem gerektiren durumlar vardır. Görüntüyü iyileştirme, önbellek busting ve CDN varlık işleme örnekleri içerir. Bu gereksinimleri karşılamak için Gulp kullanmak için paketleme ve küçültme iş akışı dönüştürebilirsiniz.

### <a name="use-the-bundler--minifier-extension"></a>Paketleyici & küçültücü uzantısı kullanma

Visual Studio [Paketleyici & küçültücü](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) uzantısını Gulp dönüştürme işler.

> [!NOTE]
> Microsoft destek sağlayan GitHub topluluk odaklı bir projede Paketleyici & küçültücü uzantısı ait. Sorunları Dosyalanan [burada](https://github.com/madskristensen/BundlerMinifier/issues).

Sağ *bundleconfig.json* dosya Çözüm Gezgini'nde ve seçin **Paketleyici & küçültücü** > **dönüştürmek için Gulp...** :

![Bağlam menüsü öğesini dönüştürmek için Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js* ve *package.json* dosyaları projeye eklenir. Destekleyici [npm](https://www.npmjs.com/) içinde listelenen paketler *package.json* dosyanın `devDependencies` bölüm yüklenir.

Genel bir bağımlılık olarak Gulp CLI yükleme PMC penceresinde aşağıdaki komutu çalıştırın:

```console
npm i -g gulp-cli
```

*Gulpfile.js* dosya okuma *bundleconfig.json* giriş, çıkış ve ayarları dosyası.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>El ile Dönüştür

Visual Studio ve/veya Paketleyici & küçültücü uzantısı yoksa, el ile dönüştürün.

Ekleme bir *package.json* dosyasıyla aşağıdaki `devDependencies`, proje kök:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Aynı düzeyde aşağıdaki komutu çalıştırarak bağımlılıkları yükler *package.json*:

```console
npm i
```

Gulp CLI genel bağımlılık olarak yükleyin:

```console
npm i -g gulp-cli
```

Kopya *gulpfile.js* proje kök dosya altında:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Gulp görevleri Çalıştır

Visual Studio'da projeyi derlemeler önce Gulp küçültme görev tetiklemek için aşağıdaki ekleyin [MSBuild hedef](/visualstudio/msbuild/msbuild-targets) *.csproj dosyasına:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

Bu örnekte, içinde herhangi bir görevi tanımlanan `MyPreCompileTarget` önceden tanımlanmış önce çalıştırmak hedef `Build` hedef. Visual Studio çıktı penceresinde aşağıdakine benzer bir çıktı görünür:

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

Alternatif olarak, Visual Studio'nun görev Çalıştırıcı Gezgini belirli Visual Studio olayları Gulp görevleri bağlamak için kullanılabilir. Bkz: [varsayılan görevleri çalıştırma](xref:client-side/using-gulp#running-default-tasks) , bunu ilgili yönergeler için.

## <a name="additional-resources"></a>Ek kaynaklar

* [Gulp kullanma](xref:client-side/using-gulp)
* [Grunt kullanma](xref:client-side/using-grunt)
* [Birden çok ortamı ile çalışma](xref:fundamentals/environments)
* [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)
