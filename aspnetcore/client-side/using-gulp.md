---
title: ASP.NET Core Gulp kullanma
author: rick-anderson
description: ASP.NET Core Gulp kullanmayı öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 4f383be0498b5b861bd43cc0f0685b1e62c7571b
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795531"
---
# <a name="use-gulp-in-aspnet-core"></a>ASP.NET Core Gulp kullanma

Tarafından [Scott Addie](https://scottaddie.com), [Shayne boyer'ın](https://twitter.com/spboyer), ve [David Çam](https://twitter.com/davidpine7)

Tipik bir modern web uygulamasında, yapı işlemi olabilir:

* Ve JavaScript ve CSS dosyalarına küçültme.
* Paketleme ve küçültme görevleri her yapıdan önce çağrılacak araçları çalıştırın.
* CSS daha az derleme veya SASS dosyalarına.
* CoffeeScript veya TypeScript dosyaları JavaScript'e derleyin.

A *görev Çalıştırıcı* , bu yordamı geliştirme görevlerini ve daha fazlasını otomatikleştiren bir araçtır. Visual Studio, iki popüler JavaScript tabanlı görev çalıştırıcıların için yerleşik destek sunar: [Gulp](https://gulpjs.com/) ve [Grunt](using-grunt.md).

## <a name="gulp"></a>Gulp

Gulp bir JavaScript tabanlı akış derleme araç için istemci tarafı kod setidir. Ayrıca, bir derleme ortamında belirli bir olayı tetiklendiğinde süreçlerini bir dizi istemci-tarafı dosyaları akışla aktarma için yaygın olarak kullanılır. Örneğin, Gulp otomatikleştirmek için kullanılabilir [paketleme ve küçültme](bundling-and-minification.md) veya yeni bir yapıdan önce bir geliştirme ortamı temizleme.

Gulp görev kümesini tanımlanan *gulpfile.js*. Aşağıdaki JavaScript Gulp modüllerini içerir ve gelecek görevlerde başvurulabilmesi için dosya yollarını belirtir:

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

Yukarıdaki kod, düğüm modüllerine gerekli olduğunu belirtir. `require` İşlevi özelliklerine bağımlı görevler kullanabilir, böylece her bir modülü içeri aktarır. Her bir içeri aktarılan modül bir değişkene atanır. Modül adı veya yolu yer alabilir. Bu örnekte, modülleri adlı `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, ve `gulp-uglify` adına göre alınır. Ayrıca, yolları bir dizi oluşturulur, CSS ve JavaScript dosyalarının konumları yeniden ve görevlerde başvurulan. Aşağıdaki tabloda bulunan modüllerinin açıklamaları verilmiştir *gulpfile.js*.

| Modül adı | Açıklama |
| ----------- | ----------- |
| gulp        | Akış Gulp derleme sistemi. Daha fazla bilgi için [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Bir düğüm silme modülü. Daha fazla bilgi için [rimraf](https://www.npmjs.com/package/rimraf). |
| gulp Birleştir | İşletim sisteminin yeni satır karakterine bağlı dosyaları birleştiren bir modül. Daha fazla bilgi için [gulp concat](https://www.npmjs.com/package/gulp-concat). |
| gulp cssmin | CSS dosyaları küçültür modülü. Daha fazla bilgi için [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| gulp uglify | Küçültür bir modül *.js* dosyaları. Daha fazla bilgi için [gulp uglify](https://www.npmjs.com/package/gulp-uglify). |

Gerekli modülleri alındıktan sonra görevleri belirtilebilir. Burada altı görevler vardır kayıtlı, aşağıdaki kodun gösterdiği:

```javascript
gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

gulp.task("min:js", () => {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", () => {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", gulp.series(["min:js", "min:css"]));
    
// A 'default' task is required by Gulp v4
gulp.task("default", gulp.series(["min"]));
```

---

Aşağıdaki tabloda, yukarıdaki kodda belirtilen görevlerin bir açıklama sağlar:

|Görev adı|Açıklama|
|--- |--- |
|temiz: js|Site.js dosyasını küçültülmüş sürümünü kaldırmak için rimraf düğüm silme modülü kullanan bir görev.|
|temiz: css|Site.css dosya küçültülmüş sürümünü kaldırmak için rimraf düğüm silme modülü kullanan bir görev.|
|Temizleme|Çağıran göreve `clean:js` görev, ardından `clean:css` görev.|
|Min:js|Küçültür ve js klasördeki tüm .js dosyaları birleştiren bir görev. . Min.js dosyaları hariç tutulur.|
|Min:CSS|Bir görev, küçültür ve css klasördeki tüm .css dosyaları art arda ekler. . Min.css dosyaları hariç tutulur.|
|min|Çağıran göreve `min:js` görev, ardından `min:css` görev.|

## <a name="running-default-tasks"></a>Varsayılan görevleri çalıştırma

Yeni bir Web uygulaması oluşturmadıysanız, Visual Studio'da yeni bir ASP.NET Web uygulaması projesi oluşturun.

1.  Açık *package.json* dosyası (ekleyin Aksi halde var) ve aşağıdakileri ekleyin.

    ```json
    {
      "devDependencies": {
        "gulp": "^4.0.0",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.2.0",
        "gulp-uglify": "3.0.0",
        "rimraf": "2.6.1"
      }
    }
    ```

2.  Projenize yeni bir JavaScript dosyası ekleyin ve adlandırın *gulpfile.js*, ardından aşağıdaki kodu kopyalayın.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    const gulp = require("gulp"),
          rimraf = require("rimraf"),
          concat = require("gulp-concat"),
          cssmin = require("gulp-cssmin"),
          uglify = require("gulp-uglify");
    
    const paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
    gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
    gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

    gulp.task("min:js", () => {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });

    gulp.task("min:css", () => {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });

    gulp.task("min", gulp.series(["min:js", "min:css"]));
    
    // A 'default' task is required by Gulp v4
    gulp.task("default", gulp.series(["min"]));
    ```

3.  İçinde **Çözüm Gezgini**, sağ *gulpfile.js*seçip **görev Çalıştırıcı Gezgini**.
    
    ![Görev Çalıştırıcı Gezgini Çözüm Gezgini'nden açın](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Görev Çalıştırıcı Gezgini** Gulp görev listesini gösterir. ('ye tıklamanız gerekebilir **Yenile** proje adının solunda görünen düğme.)
    
    ![Görev Çalıştırıcı Gezgini](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > **Görev Çalıştırıcı Gezgini** bağlam menüsü öğesi, yalnızca görünür *gulpfile.js* kök proje dizininde olduğu.

4.  Altındaki **görevleri** içinde **görev Çalıştırıcı Gezgini**, sağ **temiz**seçip **çalıştırma** açılır menüden.

    ![Görev Çalıştırıcı Gezgini temizleme görevini](using-gulp/_static/04-TaskRunner-clean.png)

    **Görev Çalıştırıcı Gezgini** adlı yeni bir sekme oluşturacak **temiz** ve içinde tanımlanan temizleme görevini yürütün *gulpfile.js*.

5.  Sağ **temiz** görev ve ardından **bağlamaları** > **önce yapı**.

    ![Görev Çalıştırıcı Gezgini BeforeBuild bağlama](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **Önce yapı** bağlama temizleme görevini her yapı projenin önce otomatik olarak çalışacak şekilde yapılandırır.

Bağlamaları ile ayarladığınız **görev Çalıştırıcı Gezgini** en üstündeki açıklama biçiminde depolanır, *gulpfile.js* ve yalnızca Visual Studio'da etkili olur. Gulp görev otomatik yürütme yapılandırmak için Visual Studio gerektirmeyen bir alternatif olan, *.csproj* dosya. Örneğin, bu yerleştirecek, *.csproj* dosyası:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Artık Visual Studio'da veya bir komut istemi kullanarak proje çalıştırıldığında temizleme görevini yürütülür [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) komut (çalıştırma `npm install` ilk).

## <a name="defining-and-running-a-new-task"></a>Tanımlama ve yeni bir görevi çalıştırma

Yeni Gulp görev tanımlamak için değiştirme *gulpfile.js*.

1.  Aşağıdaki JavaScript sonuna ekleyin *gulpfile.js*:

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    Bu görev adlı `first`, ve yalnızca bir dize görüntüler.

2.  Kaydet *gulpfile.js*.

3.  İçinde **Çözüm Gezgini**, sağ *gulpfile.js*seçip *görev Çalıştırıcı Gezgini*.

4.  İçinde **görev Çalıştırıcı Gezgini**, sağ **ilk**seçip **çalıştırma**.

    ![Görev Çalıştırıcı Gezgini ilk görevi çalıştır](using-gulp/_static/06-TaskRunner-First.png)

    Çıkış metni görüntülenir. Yaygın senaryoları tabanlı örnekler görmek için bkz: [Gulp tarifler](#gulp-recipes).

## <a name="defining-and-running-tasks-in-a-series"></a>Tanımlama ve sıralı görevleri çalıştırma

Görevler, eşzamanlı olarak birden çok görev çalıştırdığınızda, varsayılan olarak çalışır. Belirli bir sırada görevleri çalıştırmak ihtiyacınız varsa, ancak her görevi de, tamamlandığında belirtmelisiniz hangi görevleri olarak başka bir görev öğesinin tamamlanmasına bağlıdır.

1.  Bir sırada çalıştırılacak görev dizisini tanımlamak için değiştirin `first` , yukarıda eklediğiniz görev *gulpfile.js* aşağıdaki:

    ```javascript
    gulp.task('series:first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    gulp.task('series:second', done => {
      console.log('second task! <-----');
      done(); // signal completion
    });

    gulp.task('series', gulp.series(['series:first', 'series:second']), () => { });

    // A 'default' task is required by Gulp v4
    gulp.task('default', gulp.series('series'));
    ```
 
    Artık üç görev vardır: `series:first`, `series:second`, ve `series`. `series:second` Görevi çalıştırın ve önce tamamlanan görevleri bir dizi belirtir, ikinci bir parametre içeren `series:second` görev çalıştırır. Kodu yalnızca yukarıda belirtildiği gibi `series:first` görevi tamamlandı, önce `series:second` görev çalıştırır.

2.  Kaydet *gulpfile.js*.

3.  İçinde **Çözüm Gezgini**, sağ *gulpfile.js* seçip **görev Çalıştırıcı Gezgini** zaten açık değilse.

4.  İçinde **görev Çalıştırıcı Gezgini**, sağ **serisi** seçip **çalıştırma**.

    ![Görev Çalıştırıcı Gezgini serisi görevini Çalıştır](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense kod tamamlama, parametre açıklamaları ve verimliliğini artırın ve hatalarını azaltmak için diğer özellikler sağlar. Gulp görev JavaScript'te yazılmış; Bu nedenle, IntelliSense, geliştirme sırasında Yardım sağlayabilir. JavaScript ile çalışırken, IntelliSense, nesneleri, işlevleri, özellikleri listeler ve kullanılabilir parametreleri geçerli Bağlamınızı temel alarak. Kodunuzu tamamlayabilirsiniz IntelliSense'in sağladığı açılan listeden bir kodlama seçeneğini seçin.

![IntelliSense gulp](using-gulp/_static/08-IntelliSense.png)

IntelliSense hakkında daha fazla bilgi için bkz: [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Geliştirme, hazırlama ve üretim ortamları

Gulp, istemci tarafı dosyaları hazırlama ve üretim için en iyi duruma getirmek için kullanıldığında, işlenen dosyalar yerel bir hazırlama ve üretim konuma kaydedilir. *_Layout.cshtml* dosya kullandığı **ortam** etiket Yardımcısı CSS dosyaları iki farklı sürümleri sağlar. Bir CSS dosyaları için geliştirme sürümüdür ve başka bir sürüm, hazırlama ve üretim için optimize edilmiştir. Visual Studio değiştirdiğinizde 2017, **ASPNETCORE_ENVIRONMENT** ortam değişkenine `Production`, Visual Studio bağlantı azaltılmış CSS dosyaları ve Web uygulaması oluşturacaksınız. Aşağıdaki biçimlendirme gösterildiği **ortam** etiket Yardımcıları için bağlantı etiketlerini içeren `Development` CSS dosyaları ve küçültülmüş `Staging, Production` CSS dosyaları.

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

Derleme için farklı ortamlar arasında geçiş yapmak için değişiklik **ASPNETCORE_ENVIRONMENT** ortam değişkenin değeri.

1.  İçinde **görev Çalıştırıcı Gezgini**, doğrulayın **min** görev olarak çalıştırmak için ayarlandı **önce yapı**.

2.  İçinde **Çözüm Gezgini**, proje adını sağ tıklatın ve seçin **özellikleri**.

    Web uygulaması için özellik sayfası görüntülenir.

3.  Tıklayın **hata ayıklama** sekmesi.

4.  Değerini **barındırma: ortam** ortam değişkenine `Production`.

5.  Tuşuna **F5** uygulamayı tarayıcıda çalıştırın.

6.  Tarayıcı penceresinde, sayfanın sağ tıklayıp **kaynağı görüntüle** sayfanın HTML görüntülemek için.

    Stil sayfası bağlantıları küçültülmüş CSS dosyalarının olduğu noktaya dikkat edin.

7.  Web uygulamasını Durdur için tarayıcıyı kapatın.

8.  Visual Studio'da Web uygulaması için özellik sayfasına dönmek ve değiştirmek **barındırma: ortam** ortam değişkeni başa `Development`.

9.  Tuşuna **F5** uygulamayı bir tarayıcıda yeniden çalıştırın.

10. Tarayıcı penceresinde, sayfanın sağ tıklayıp **kaynağı görüntüle** sayfanın HTML görmek için.

    CSS dosyaları unminified sürümleri için stil sayfası bağlantı noktası dikkat edin.

ASP.NET Core ortamlarda ilgili daha fazla bilgi için bkz. [birden fazla ortam kullanayım](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Görev ve modül ayrıntıları

Gulp görev, bir işlev adı ile kaydedilir. Diğer görevlerden önce geçerli görevin çalıştırmanız gerekiyorsa, bağımlılıkları belirtebilirsiniz. Ek işlevler izin çalıştırın ve Gulp görevleri izleyin, hem de kaynak ayarlayın (*src*) ve hedef (*dest*) değiştirilen dosyaları. Birincil Gulp API işlevleri şunlardır:

|Gulp işlevi|Sözdizimi|Açıklama|
|---   |--- |--- |
|görev  |`gulp.task(name[, deps], fn) { }`|`task` İşlevi, bir görev oluşturur. `name` Parametre görev adını tanımlar. `deps` Parametresi, bu görevi çalışmadan önce tamamlanması için görev dizisi içerir. `fn` Parametre görevin işlemleri gerçekleştiren bir geri çağırma işlevini temsil eder.|
|İzleme |`gulp.watch(glob [, opts], tasks) { }`|`watch` İşlevi, bir dosya değişikliği oluştuğunda dosyaları ve çalışan görevleri izler. `glob` Parametresi bir `string` veya `array` izlemek için hangi dosyaların belirler. `opts` Parametresi ek dosya izleme seçenekleri sağlar.|
|src   |`gulp.src(globs[, options]) { }`|`src` İşlevi glob değerleri eşleşen dosyaları sağlar. `glob` Parametresi bir `string` veya `array` okumak için hangi dosyaların belirler. `options` Parametresi ek dosya seçenekleri sağlar.|
|Hedef  |`gulp.dest(path[, options]) { }`|`dest` İşlevi olduğu dosyaları yazılabilir bir konuma tanımlar. `path` Parametresi, bir dize veya hedef klasör belirleyen işlev. `options` Parametredir çıkış klasör seçenekleri belirten bir nesne.|

Ek Gulp API başvuru bilgileri için bkz: [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>Gulp tarifleri

Gulp Gulp topluluk sağlar [tarifler](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Bu tarif içeren yaygın senaryolara Gulp görevleri içerir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Gulp belgeleri](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Paketleme ve küçültme ASP.NET core'da](bundling-and-minification.md)
* [ASP.NET Core Grunt kullanma](using-grunt.md)
