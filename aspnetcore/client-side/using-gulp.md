---
title: ASP.NET çekirdek Gulp kullanın
author: rick-anderson
description: ASP.NET Core Gulp kullanmayı öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
uid: client-side/using-gulp
ms.openlocfilehash: 35f62bf276d3708df0e2c8b56a44c34c178d8ff8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278258"
---
# <a name="use-gulp-in-aspnet-core"></a>ASP.NET çekirdek Gulp kullanın

Tarafından [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), ve [Shayne Boyer](https://twitter.com/spboyer)

Tipik modern web uygulamasında yapı işlemi olabilir:

* Paket ve JavaScript ve CSS dosyaları minify.
* Paketleme ve küçültme görevleri her yapı önce çağrılacak araçlarını çalıştırın.
* CSS daha az derleme veya SASS dosyaları.
* JavaScript CoffeeScript veya TypeScript dosyaları derleyin.

A *görev Çalıştırıcı* , bu yordamı geliştirme görevleri ve daha fazlasını otomatikleştiren bir araçtır. Visual Studio iki popüler JavaScript tabanlı görev koşucular için yerleşik destek sunar: [Gulp](https://gulpjs.com/) ve [Grunt](using-grunt.md).

## <a name="gulp"></a>gulp

Gulp bir JavaScript tabanlı akış derleme için araç seti istemci tarafı kodlar ' dir. Yaygın bir yapı ortamında belirli bir olay tetiklendiğinde işlemleri, bir dizi istemci-tarafı dosyalarıyla akışını sağlamak için kullanılır. Örneğin, Gulp otomatik hale getirmek için kullanılabilir [paketleme ve küçültme](bundling-and-minification.md) veya yeni bir yapı önce bir geliştirme ortamı temizleme.

Bir dizi Gulp görevi tanımlanan *gulpfile.js*. Şu JavaScript Gulp modüller içerir ve gelecek görevlerde başvurulacak dosya yollarını belirtir:

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

Yukarıdaki kod düğümü modüllerine gerekli olduğunu belirtir. `require` İşlevi bağımlı görevler özelliklerine kullanabilir, böylece her modül içeri aktarır. Her içe aktarılmış modüllerin bir değişkene atanır. Modüller, adı veya yolu bulunabilir. Bu örnekte, modüller adlı `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, ve `gulp-uglify` adıyla alınır. Ayrıca, yolları bir dizi oluşturulur; böylece CSS ve JavaScript dosyaları konumlarını yeniden ve görevlerde başvurulan. Aşağıdaki tabloda bulunan modülleri açıklanmakta *gulpfile.js*.

| Modül adı | Açıklama |
| ----------- | ----------- |
| gulp        | Derleme Sistemi akış Gulp. Daha fazla bilgi için bkz: [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Bir düğüm silme modülü. Daha fazla bilgi için bkz: [rimraf](https://www.npmjs.com/package/rimraf). |
| concat gulp | İşletim sisteminin yeni satır karakteri bağlı olarak dosyaları art arda ekler modül. Daha fazla bilgi için bkz: [gulp concat](https://www.npmjs.com/package/gulp-concat). |
| gulp cssmin | CSS dosyaları küçültür modül. Daha fazla bilgi için bkz: [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| gulp uglify | Küçültür bir modül *.js* dosyaları. Daha fazla bilgi için bkz: [gulp uglify](https://www.npmjs.com/package/gulp-uglify). |

Gerekli modülleri alındığında görevleri belirtilebilir. Burada altı görevler vardır kayıtlı, aşağıdaki kod tarafından temsil edilen:

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

Aşağıdaki tabloda Yukarıdaki kod belirtilen görevler bir açıklamasını sağlar:

|Görev adı|Açıklama|
|--- |--- |
|temiz: js|Site.js dosyasının küçültülmüş sürümünü kaldırmak için rimraf düğümü silme modülü kullanan bir görev.|
|temiz: css|Site.css dosyasının küçültülmüş sürümünü kaldırmak için rimraf düğümü silme modülü kullanan bir görev.|
|Temizleme|Çağıran bir görev `clean:js` ve ardından görev `clean:css` görev.|
|Min:js|Küçültür ve js klasördeki tüm .js dosyaları art arda ekler bir görev. . Min.js dosyaları dışlanır.|
|Min:CSS|Küçültür ve css klasördeki tüm .css dosyaları art arda ekler bir görev. . Min.css dosyaları dışlanır.|
|min|Çağıran bir görev `min:js` ve ardından görev `min:css` görev.|

## <a name="running-default-tasks"></a>Varsayılan görevleri çalıştırma

Yeni bir Web uygulaması zaten oluşturmadıysanız, Visual Studio'da yeni bir ASP.NET Web uygulaması projesi oluşturun.

1.  Projeniz için yeni bir JavaScript dosyası ekleyin ve adını *gulpfile.js*, aşağıdaki kodu kopyalayın.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  Açık *package.json* dosyası (ekleyebilir değilse vardır) ve aşağıdakileri ekleyin.

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  İçinde **Çözüm Gezgini**, sağ *gulpfile.js*seçip **görev Çalıştırıcı Gezgini**.
    
    ![Görev Çalıştırıcı Gezgini Çözüm Gezgini'nden açın](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Görev Çalıştırıcı Gezgini** Gulp görevleri listesini gösterir. (Tıklatın gerekebilir **yenileme** proje adı solunda görüntülenen düğmesini.)
    
    ![Görev Çalıştırıcı Gezgini](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > **Görev Çalıştırıcı Gezgini** bağlam menüsü öğesini görünüp görünmeyeceğini yalnızca *gulpfile.js* kök proje dizininde değil.

4.  Altındaki **görevleri** içinde **görev Çalıştırıcı Gezgini**, sağ **temiz**seçip **çalıştırmak** açılır menüden.

    ![Görev Çalıştırıcı Gezgini temiz görevi](using-gulp/_static/04-TaskRunner-clean.png)

    **Görev Çalıştırıcı Gezgini** adlı yeni bir sekme oluşturacak **temiz** ve içinde tanımlanan temiz görev yürütme *gulpfile.js*.

5.  Sağ **temiz** görev ve ardından **bağlamaları** > **önce yapı**.

    ![Görev Çalıştırıcı Gezgini BeforeBuild bağlama](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **Önce yapı** bağlama temiz görevi her proje derleme önce otomatik olarak çalışacak şekilde yapılandırır.

Bağlamaları ile ayarladığınız **görev Çalıştırıcı Gezgini** en üstündeki bir yorum biçiminde depolanır, *gulpfile.js* ve yalnızca Visual Studio'da etkili olur. Visual Studio gerektirmeyen bir alternatif gulp görevleri otomatik olarak yürütülmesini yapılandırmaktır, *.csproj* dosya. Örneğin, bu içine, *.csproj* dosyası:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Visual Studio'da veya bir komut istemini kullanarak proje çalıştırıldığında temiz görev yürütülen artık [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) komutu (çalıştırmak `npm install` ilk).

## <a name="defining-and-running-a-new-task"></a>Tanımlama ve yeni bir görevi çalıştırma

Yeni bir Gulp görev tanımlamak için değiştirme *gulpfile.js*.

1.  Şu JavaScript sonuna ekleme *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    Bu görev adlı `first`, ve yalnızca bir dize görüntüler.

2.  Kaydet *gulpfile.js*.

3.  İçinde **Çözüm Gezgini**, sağ *gulpfile.js*seçip *görev Çalıştırıcı Gezgini*.

4.  İçinde **görev Çalıştırıcı Gezgini**, sağ **ilk**seçip **çalıştırmak**.

    ![Görev Çalıştırıcı Gezgini ilk görevi çalıştırma](using-gulp/_static/06-TaskRunner-First.png)

    Çıktı metin görüntülenir. Ortak senaryolarını temel alarak örneklerini görmek için bkz: [Gulp tarif](#gulp-recipes).

## <a name="defining-and-running-tasks-in-a-series"></a>Tanımlama ve sıralı görevleri çalıştırma

Birden çok görev çalıştırdığınızda görevler eşzamanlı olarak varsayılan olarak çalışır. Belirli bir sırada görevleri çalıştırmanız gerekiyorsa, ancak her görev de, tamamlandığında belirtmeniz gerekir hangi görevleri olarak başka bir görev tamamlandığında bağlıdır.

1.  Bir dizi sırada çalıştırılacak görevleri tanımlamak için Değiştir `first` yukarıda eklediğiniz görev *gulpfile.js* aşağıdaki:

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    Artık üç görev vardır: `series:first`, `series:second`, ve `series`. `series:second` Görevi çalıştırın ve önce tamamlandı için görevleri bir dizi belirten ikinci bir parametresi içerir `series:second` görev çalışır. Kodda yalnızca yukarıda belirtildiği gibi `series:first` görev tamamlandı, önce `series:second` görev çalışır.

2.  Kaydet *gulpfile.js*.

3.  İçinde **Çözüm Gezgini**, sağ *gulpfile.js* seçip **görev Çalıştırıcı Gezgini** zaten açık değilse.

4.  İçinde **görev Çalıştırıcı Gezgini**, sağ **serisi** seçip **çalıştırmak**.

    ![Görev Çalıştırıcı Gezgini serisi görevi çalıştırma](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense kod tamamlama, parametre açıklamaları ve üretkenliğini ve hatalarını azaltmak için diğer özellikleri sağlar. Gulp görevleri JavaScript'te yazılmış; Bu nedenle, IntelliSense geliştirme sırasında Yardım sağlayabilir. JavaScript ile çalışırken, IntelliSense listeler nesneleri, İşlevler, özellikler ve kullanılabilir parametreleri, geçerli bağlamına dayalı. Kod tamamlamak için IntelliSense tarafından sağlanan açılır listeden bir kodlama seçeneği seçin.

![IntelliSense gulp](using-gulp/_static/08-IntelliSense.png)

IntelliSense hakkında daha fazla bilgi için bkz: [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Geliştirme, hazırlama ve üretim ortamları

Hazırlama ve üretim için istemci tarafı dosyaları en iyi duruma getirme Gulp kullanıldığında, işlenen dosyalar yerel bir hazırlama ve üretim konuma kaydedilir. *_Layout.cshtml* dosya kullanır **ortam** CSS dosyaları iki farklı sürümlerini sağlamak için yardımcı etiketi. CSS dosyaları bir sürüm için geliştirme ve başka bir sürüm hazırlama ve üretim için optimize edilmiştir. Visual Studio, değiştirdiğinizde 2017 içinde **ASPNETCORE_ENVIRONMENT** ortam değişkenine `Production`, Visual Studio simge durumuna küçültülmüş CSS dosyalarına bağlantı ve Web uygulaması oluşturacaksınız. Aşağıdaki biçimlendirme gösterildiği **ortam** etiket Yardımcıları için bağlantı etiketlerini içeren `Development` CSS dosyaları ve küçültülmüş `Staging, Production` CSS dosyaları.

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>Ortamlar arasında geçiş yapma

Farklı ortamlar için derleme arasında geçiş yapmak için değiştirme **ASPNETCORE_ENVIRONMENT** ortam değişkeninin değeri.

1.  İçinde **görev Çalıştırıcı Gezgini**, doğrulayın **min** görevi çalıştırmak için ayarlanmış **önce yapı**.

2.  İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve seçin **özellikleri**.

    Web uygulaması için özellik sayfası görüntülenir.

3.  Tıklatın **hata ayıklama** sekmesi.

4.  Değerini **barındırma: ortamı** ortam değişkenine `Production`.

5.  Tuşuna **F5** bir tarayıcıda uygulamayı çalıştırın.

6.  Tarayıcı penceresinde sayfanın sağ tıklatıp **kaynağı görüntüle** sayfasının HTML görüntülemek için.

    Stil sayfası bağlantıları küçültülmüş CSS dosyaları noktası dikkat edin.

7.  Web uygulaması durdurmak için tarayıcıyı kapatın.

8.  Visual Studio'da Web uygulaması için özellik sayfasına dönmek ve değiştirmek **barındırma: ortamı** ortam değişkeni başa `Development`.

9.  Tuşuna **F5** uygulamayı tarayıcıda yeniden çalıştırın.

10. Tarayıcı penceresinde sayfanın sağ tıklatıp **kaynağı görüntüle** sayfasının HTML görmek için.

    CSS dosyaları unminified sürümleri için stil bağlantı noktası dikkat edin.

ASP.NET Core ortamlarda ilgili daha fazla bilgi için bkz: [kullanan birden çok ortamlar](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Görev ve modül ayrıntıları

Gulp görev işlevi adıyla kaydedilir. Diğer görevlerden önce geçerli görevin çalıştırmanız gerekiyorsa, bağımlılıkları belirtebilirsiniz. Ek işlevler izin çalıştırın ve Gulp görevleri izleyin, yanı sıra kaynağı ayarlayın (*src*) ve hedef (*taşınmaya*) değiştirilen dosyaların. Birincil Gulp API işlevleri şunlardır:

|Gulp işlevi|Sözdizimi|Açıklama|
|---   |--- |--- |
|görev  |`gulp.task(name[, deps], fn) { }`|`task` İşlevi bir görev oluşturur. `name` Parametre görevin adını tanımlar. `deps` Parametresi bu görevi çalışmadan önce tamamlanması gereken görevler dizisi içerir. `fn` Parametresi görev işlemlerini gerçekleştiren bir geri çağırma işlevini temsil eder.|
|İzleme |`gulp.watch(glob [, opts], tasks) { }`|`watch` İşlevi bir dosya değişikliği oluştuğunda dosyaları ve çalıştırmalarını görevleri izler. `glob` Parametresi bir `string` veya `array` izlemek için hangi dosyaların belirler. `opts` Parametresi seçenekleri izlemeye ek dosya sağlar.|
|src   |`gulp.src(globs[, options]) { }`|`src` İşlevi glob değerler ile eşleşen dosyaları sağlar. `glob` Parametresi bir `string` veya `array` okumak için hangi dosyaların belirler. `options` Parametresi ek dosya seçenekleri sağlar.|
|Hedef  |`gulp.dest(path[, options]) { }`|`dest` İşlevi için dosyaları yazılabilir bir konuma tanımlar. `path` Parametredir bir dize veya hedef klasör belirleyen işlev. `options` Parametredir çıkış klasörü seçeneklerini belirtir. bir nesne.|

Ek Gulp API başvuru bilgileri için bkz: [Gulp belgeleri API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>Tarif gulp

Gulp Gulp topluluk sağlar [tarif](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Bu tarif ortak senaryolar için Gulp görevleri içerir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Gulp belgeleri](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Paketleme ve ASP.NET Core küçültme](bundling-and-minification.md)
* [ASP.NET çekirdek Grunt kullanın](using-grunt.md)
