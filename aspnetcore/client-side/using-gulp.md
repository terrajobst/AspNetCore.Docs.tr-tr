---
title: ASP.NET çekirdek Gulp kullanın
author: rick-anderson
description: ASP.NET Core Gulp kullanmayı öğrenin.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: f776b2025b6ebfeff28d3903aaeac4d7d89665b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="b34e7-103">ASP.NET çekirdek Gulp kullanın</span><span class="sxs-lookup"><span data-stu-id="b34e7-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="b34e7-104">Tarafından [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), ve [Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="b34e7-104">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="b34e7-105">Tipik modern web uygulamasında yapı işlemi olabilir:</span><span class="sxs-lookup"><span data-stu-id="b34e7-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="b34e7-106">Paket ve JavaScript ve CSS dosyaları minify.</span><span class="sxs-lookup"><span data-stu-id="b34e7-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="b34e7-107">Paketleme ve küçültme görevleri her yapı önce çağrılacak araçlarını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b34e7-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="b34e7-108">CSS daha az derleme veya SASS dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b34e7-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="b34e7-109">JavaScript CoffeeScript veya TypeScript dosyaları derleyin.</span><span class="sxs-lookup"><span data-stu-id="b34e7-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="b34e7-110">A *görev Çalıştırıcı* , bu yordamı geliştirme görevleri ve daha fazlasını otomatikleştiren bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="b34e7-111">Visual Studio iki popüler JavaScript tabanlı görev koşucular için yerleşik destek sunar: [Gulp](https://gulpjs.com/) ve [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="b34e7-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="b34e7-112">gulp</span><span class="sxs-lookup"><span data-stu-id="b34e7-112">Gulp</span></span>

<span data-ttu-id="b34e7-113">Gulp bir JavaScript tabanlı akış derleme için araç seti istemci tarafı kodlar ' dir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="b34e7-114">Yaygın bir yapı ortamında belirli bir olay tetiklendiğinde işlemleri, bir dizi istemci-tarafı dosyalarıyla akışını sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="b34e7-115">Örneğin, Gulp otomatik hale getirmek için kullanılabilir [paketleme ve küçültme](bundling-and-minification.md) veya yeni bir yapı önce bir geliştirme ortamı temizleme.</span><span class="sxs-lookup"><span data-stu-id="b34e7-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="b34e7-116">Bir dizi Gulp görevi tanımlanan *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b34e7-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="b34e7-117">Şu JavaScript Gulp modüller içerir ve gelecek görevlerde başvurulacak dosya yollarını belirtir:</span><span class="sxs-lookup"><span data-stu-id="b34e7-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="b34e7-118">Yukarıdaki kod düğümü modüllerine gerekli olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="b34e7-119">`require` İşlevi bağımlı görevler özelliklerine kullanabilir, böylece her modül içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="b34e7-120">Her içe aktarılmış modüllerin bir değişkene atanır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="b34e7-121">Modüller, adı veya yolu bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="b34e7-122">Bu örnekte, modüller adlı `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, ve `gulp-uglify` adıyla alınır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="b34e7-123">Ayrıca, yolları bir dizi oluşturulur; böylece CSS ve JavaScript dosyaları konumlarını yeniden ve görevlerde başvurulan.</span><span class="sxs-lookup"><span data-stu-id="b34e7-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="b34e7-124">Aşağıdaki tabloda bulunan modülleri açıklanmakta *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b34e7-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="b34e7-125">Modül adı</span><span class="sxs-lookup"><span data-stu-id="b34e7-125">Module Name</span></span> | <span data-ttu-id="b34e7-126">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b34e7-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="b34e7-127">gulp</span><span class="sxs-lookup"><span data-stu-id="b34e7-127">gulp</span></span>        | <span data-ttu-id="b34e7-128">Derleme Sistemi akış Gulp.</span><span class="sxs-lookup"><span data-stu-id="b34e7-128">The Gulp streaming build system.</span></span> <span data-ttu-id="b34e7-129">Daha fazla bilgi için bkz: [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="b34e7-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="b34e7-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="b34e7-130">rimraf</span></span>      | <span data-ttu-id="b34e7-131">Bir düğüm silme modülü.</span><span class="sxs-lookup"><span data-stu-id="b34e7-131">A Node deletion module.</span></span> <span data-ttu-id="b34e7-132">Daha fazla bilgi için bkz: [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="b34e7-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="b34e7-133">concat gulp</span><span class="sxs-lookup"><span data-stu-id="b34e7-133">gulp-concat</span></span> | <span data-ttu-id="b34e7-134">İşletim sisteminin yeni satır karakteri bağlı olarak dosyaları art arda ekler modül.</span><span class="sxs-lookup"><span data-stu-id="b34e7-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="b34e7-135">Daha fazla bilgi için bkz: [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="b34e7-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="b34e7-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="b34e7-136">gulp-cssmin</span></span> | <span data-ttu-id="b34e7-137">CSS dosyaları küçültür modül.</span><span class="sxs-lookup"><span data-stu-id="b34e7-137">A module that minifies CSS files.</span></span> <span data-ttu-id="b34e7-138">Daha fazla bilgi için bkz: [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="b34e7-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="b34e7-139">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="b34e7-139">gulp-uglify</span></span> | <span data-ttu-id="b34e7-140">Küçültür bir modül *.js* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b34e7-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="b34e7-141">Daha fazla bilgi için bkz: [gulp uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="b34e7-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="b34e7-142">Gerekli modülleri alındığında görevleri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="b34e7-143">Burada altı görevler vardır kayıtlı, aşağıdaki kod tarafından temsil edilen:</span><span class="sxs-lookup"><span data-stu-id="b34e7-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="b34e7-144">Aşağıdaki tabloda Yukarıdaki kod belirtilen görevler bir açıklamasını sağlar:</span><span class="sxs-lookup"><span data-stu-id="b34e7-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="b34e7-145">Görev adı</span><span class="sxs-lookup"><span data-stu-id="b34e7-145">Task Name</span></span>|<span data-ttu-id="b34e7-146">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b34e7-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="b34e7-147">clean:js</span><span class="sxs-lookup"><span data-stu-id="b34e7-147">clean:js</span></span>|<span data-ttu-id="b34e7-148">Site.js dosyasının küçültülmüş sürümünü kaldırmak için rimraf düğümü silme modülü kullanan bir görev.</span><span class="sxs-lookup"><span data-stu-id="b34e7-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="b34e7-149">clean:css</span><span class="sxs-lookup"><span data-stu-id="b34e7-149">clean:css</span></span>|<span data-ttu-id="b34e7-150">Site.css dosyasının küçültülmüş sürümünü kaldırmak için rimraf düğümü silme modülü kullanan bir görev.</span><span class="sxs-lookup"><span data-stu-id="b34e7-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="b34e7-151">Temizleme</span><span class="sxs-lookup"><span data-stu-id="b34e7-151">clean</span></span>|<span data-ttu-id="b34e7-152">Çağıran bir görev `clean:js` ve ardından görev `clean:css` görev.</span><span class="sxs-lookup"><span data-stu-id="b34e7-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="b34e7-153">min:js</span><span class="sxs-lookup"><span data-stu-id="b34e7-153">min:js</span></span>|<span data-ttu-id="b34e7-154">Küçültür ve js klasördeki tüm .js dosyaları art arda ekler bir görev.</span><span class="sxs-lookup"><span data-stu-id="b34e7-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="b34e7-155">. Min.js dosyaları dışlanır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="b34e7-156">min:css</span><span class="sxs-lookup"><span data-stu-id="b34e7-156">min:css</span></span>|<span data-ttu-id="b34e7-157">Küçültür ve css klasördeki tüm .css dosyaları art arda ekler bir görev.</span><span class="sxs-lookup"><span data-stu-id="b34e7-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="b34e7-158">. Min.css dosyaları dışlanır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="b34e7-159">min</span><span class="sxs-lookup"><span data-stu-id="b34e7-159">min</span></span>|<span data-ttu-id="b34e7-160">Çağıran bir görev `min:js` ve ardından görev `min:css` görev.</span><span class="sxs-lookup"><span data-stu-id="b34e7-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="b34e7-161">Varsayılan görevleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b34e7-161">Running default tasks</span></span>

<span data-ttu-id="b34e7-162">Yeni bir Web uygulaması zaten oluşturmadıysanız, Visual Studio'da yeni bir ASP.NET Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b34e7-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="b34e7-163">Projeniz için yeni bir JavaScript dosyası ekleyin ve adını *gulpfile.js*, aşağıdaki kodu kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="b34e7-163">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

2.  <span data-ttu-id="b34e7-164">Açık *package.json* dosyası (ekleyebilir değilse vardır) ve aşağıdakileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b34e7-164">Open the *package.json* file (add if not there) and add the following.</span></span>

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

3.  <span data-ttu-id="b34e7-165">İçinde **Çözüm Gezgini**, sağ *gulpfile.js*seçip **görev Çalıştırıcı Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="b34e7-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Görev Çalıştırıcı Gezgini Çözüm Gezgini'nden açın](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="b34e7-167">**Görev Çalıştırıcı Gezgini** Gulp görevleri listesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="b34e7-168">(Tıklatın gerekebilir **yenileme** proje adı solunda görüntülenen düğmesini.)</span><span class="sxs-lookup"><span data-stu-id="b34e7-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Görev Çalıştırıcı Gezgini](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="b34e7-170">**Görev Çalıştırıcı Gezgini** bağlam menüsü öğesini görünüp görünmeyeceğini yalnızca *gulpfile.js* kök proje dizininde değil.</span><span class="sxs-lookup"><span data-stu-id="b34e7-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4.  <span data-ttu-id="b34e7-171">Altındaki **görevleri** içinde **görev Çalıştırıcı Gezgini**, sağ **temiz**seçip **çalıştırmak** açılır menüden.</span><span class="sxs-lookup"><span data-stu-id="b34e7-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Görev Çalıştırıcı Gezgini temiz görevi](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="b34e7-173">**Görev Çalıştırıcı Gezgini** adlı yeni bir sekme oluşturacak **temiz** ve içinde tanımlanan temiz görev yürütme *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b34e7-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="b34e7-174">Sağ **temiz** görev ve ardından **bağlamaları** > **önce yapı**.</span><span class="sxs-lookup"><span data-stu-id="b34e7-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Görev Çalıştırıcı Gezgini BeforeBuild bağlama](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="b34e7-176">**Önce yapı** bağlama temiz görevi her proje derleme önce otomatik olarak çalışacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="b34e7-177">Bağlamaları ile ayarladığınız **görev Çalıştırıcı Gezgini** en üstündeki bir yorum biçiminde depolanır, *gulpfile.js* ve yalnızca Visual Studio'da etkili olur.</span><span class="sxs-lookup"><span data-stu-id="b34e7-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="b34e7-178">Visual Studio gerektirmeyen bir alternatif gulp görevleri otomatik olarak yürütülmesini yapılandırmaktır, *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="b34e7-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="b34e7-179">Örneğin, bu içine, *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b34e7-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="b34e7-180">Visual Studio'da veya bir komut istemini kullanarak proje çalıştırıldığında temiz görev yürütülen artık [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) komutu (çalıştırmak `npm install` ilk).</span><span class="sxs-lookup"><span data-stu-id="b34e7-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="b34e7-181">Tanımlama ve yeni bir görevi çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b34e7-181">Defining and running a new task</span></span>

<span data-ttu-id="b34e7-182">Yeni bir Gulp görev tanımlamak için değiştirme *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b34e7-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="b34e7-183">Şu JavaScript sonuna ekleme *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="b34e7-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="b34e7-184">Bu görev adlı `first`, ve yalnızca bir dize görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b34e7-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="b34e7-185">Kaydet *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b34e7-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="b34e7-186">İçinde **Çözüm Gezgini**, sağ *gulpfile.js*seçip *görev Çalıştırıcı Gezgini*.</span><span class="sxs-lookup"><span data-stu-id="b34e7-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="b34e7-187">İçinde **görev Çalıştırıcı Gezgini**, sağ **ilk**seçip **çalıştırmak**.</span><span class="sxs-lookup"><span data-stu-id="b34e7-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Görev Çalıştırıcı Gezgini ilk görevi çalıştırma](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="b34e7-189">Çıktı metin görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-189">The output text is displayed.</span></span> <span data-ttu-id="b34e7-190">Ortak senaryolarını temel alarak örneklerini görmek için bkz: [Gulp tarif](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="b34e7-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="b34e7-191">Tanımlama ve sıralı görevleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b34e7-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="b34e7-192">Birden çok görev çalıştırdığınızda görevler eşzamanlı olarak varsayılan olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="b34e7-193">Belirli bir sırada görevleri çalıştırmanız gerekiyorsa, ancak her görev de, tamamlandığında belirtmeniz gerekir hangi görevleri olarak başka bir görev tamamlandığında bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="b34e7-194">Bir dizi sırada çalıştırılacak görevleri tanımlamak için Değiştir `first` yukarıda eklediğiniz görev *gulpfile.js* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="b34e7-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="b34e7-195">Artık üç görev vardır: `series:first`, `series:second`, ve `series`.</span><span class="sxs-lookup"><span data-stu-id="b34e7-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="b34e7-196">`series:second` Görevi çalıştırın ve önce tamamlandı için görevleri bir dizi belirten ikinci bir parametresi içerir `series:second` görev çalışır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="b34e7-197">Kodda yalnızca yukarıda belirtildiği gibi `series:first` görev tamamlandı, önce `series:second` görev çalışır.</span><span class="sxs-lookup"><span data-stu-id="b34e7-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="b34e7-198">Kaydet *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b34e7-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="b34e7-199">İçinde **Çözüm Gezgini**, sağ *gulpfile.js* seçip **görev Çalıştırıcı Gezgini** zaten açık değilse.</span><span class="sxs-lookup"><span data-stu-id="b34e7-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="b34e7-200">İçinde **görev Çalıştırıcı Gezgini**, sağ **serisi** seçip **çalıştırmak**.</span><span class="sxs-lookup"><span data-stu-id="b34e7-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Görev Çalıştırıcı Gezgini serisi görevi çalıştırma](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="b34e7-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="b34e7-202">IntelliSense</span></span>

<span data-ttu-id="b34e7-203">IntelliSense kod tamamlama, parametre açıklamaları ve üretkenliğini ve hatalarını azaltmak için diğer özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b34e7-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="b34e7-204">Gulp görevleri JavaScript'te yazılmış; Bu nedenle, IntelliSense geliştirme sırasında Yardım sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="b34e7-205">JavaScript ile çalışırken, IntelliSense listeler nesneleri, İşlevler, özellikler ve kullanılabilir parametreleri, geçerli bağlamına dayalı.</span><span class="sxs-lookup"><span data-stu-id="b34e7-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="b34e7-206">Kod tamamlamak için IntelliSense tarafından sağlanan açılır listeden bir kodlama seçeneği seçin.</span><span class="sxs-lookup"><span data-stu-id="b34e7-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![IntelliSense gulp](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="b34e7-208">IntelliSense hakkında daha fazla bilgi için bkz: [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="b34e7-208">For more information about IntelliSense, see [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="b34e7-209">Geliştirme, hazırlama ve üretim ortamları</span><span class="sxs-lookup"><span data-stu-id="b34e7-209">Development, staging, and production environments</span></span>

<span data-ttu-id="b34e7-210">Hazırlama ve üretim için istemci tarafı dosyaları en iyi duruma getirme Gulp kullanıldığında, işlenen dosyalar yerel bir hazırlama ve üretim konuma kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="b34e7-211">*_Layout.cshtml* dosya kullanır **ortam** CSS dosyaları iki farklı sürümlerini sağlamak için yardımcı etiketi.</span><span class="sxs-lookup"><span data-stu-id="b34e7-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="b34e7-212">CSS dosyaları bir sürüm için geliştirme ve başka bir sürüm hazırlama ve üretim için optimize edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="b34e7-213">Visual Studio, değiştirdiğinizde 2017 içinde **ASPNETCORE_ENVIRONMENT** ortam değişkenine `Production`, Visual Studio simge durumuna küçültülmüş CSS dosyalarına bağlantı ve Web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b34e7-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="b34e7-214">Aşağıdaki biçimlendirme gösterildiği **ortam** etiket Yardımcıları için bağlantı etiketlerini içeren `Development` CSS dosyaları ve küçültülmüş `Staging, Production` CSS dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b34e7-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="b34e7-215">Ortamlar arasında geçiş yapma</span><span class="sxs-lookup"><span data-stu-id="b34e7-215">Switching between environments</span></span>

<span data-ttu-id="b34e7-216">Farklı ortamlar için derleme arasında geçiş yapmak için değiştirme **ASPNETCORE_ENVIRONMENT** ortam değişkeninin değeri.</span><span class="sxs-lookup"><span data-stu-id="b34e7-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="b34e7-217">İçinde **görev Çalıştırıcı Gezgini**, doğrulayın **min** görevi çalıştırmak için ayarlanmış **önce yapı**.</span><span class="sxs-lookup"><span data-stu-id="b34e7-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="b34e7-218">İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="b34e7-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="b34e7-219">Web uygulaması için özellik sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="b34e7-220">Tıklatın **hata ayıklama** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="b34e7-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="b34e7-221">Değerini **barındırma: ortamı** ortam değişkenine `Production`.</span><span class="sxs-lookup"><span data-stu-id="b34e7-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="b34e7-222">Tuşuna **F5** bir tarayıcıda uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b34e7-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="b34e7-223">Tarayıcı penceresinde sayfanın sağ tıklatıp **kaynağı görüntüle** sayfasının HTML görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="b34e7-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="b34e7-224">Stil sayfası bağlantıları küçültülmüş CSS dosyaları noktası dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b34e7-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="b34e7-225">Web uygulaması durdurmak için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="b34e7-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="b34e7-226">Visual Studio'da Web uygulaması için özellik sayfasına dönmek ve değiştirmek **barındırma: ortamı** ortam değişkeni başa `Development`.</span><span class="sxs-lookup"><span data-stu-id="b34e7-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="b34e7-227">Tuşuna **F5** uygulamayı tarayıcıda yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b34e7-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="b34e7-228">Tarayıcı penceresinde sayfanın sağ tıklatıp **kaynağı görüntüle** sayfasının HTML görmek için.</span><span class="sxs-lookup"><span data-stu-id="b34e7-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="b34e7-229">CSS dosyaları unminified sürümleri için stil bağlantı noktası dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b34e7-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="b34e7-230">ASP.NET Core ortamlarda ilgili daha fazla bilgi için bkz: [çalışma ile birden çok ortamları](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="b34e7-230">For more information related to environments in ASP.NET Core, see [Work with multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="b34e7-231">Görev ve modül ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="b34e7-231">Task and module details</span></span>

<span data-ttu-id="b34e7-232">Gulp görev işlevi adıyla kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="b34e7-233">Diğer görevlerden önce geçerli görevin çalıştırmanız gerekiyorsa, bağımlılıkları belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b34e7-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="b34e7-234">Ek işlevler izin çalıştırın ve Gulp görevleri izleyin, yanı sıra kaynağı ayarlayın (*src*) ve hedef (*taşınmaya*) değiştirilen dosyaların.</span><span class="sxs-lookup"><span data-stu-id="b34e7-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="b34e7-235">Birincil Gulp API işlevleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b34e7-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="b34e7-236">Gulp işlevi</span><span class="sxs-lookup"><span data-stu-id="b34e7-236">Gulp Function</span></span>|<span data-ttu-id="b34e7-237">Sözdizimi</span><span class="sxs-lookup"><span data-stu-id="b34e7-237">Syntax</span></span>|<span data-ttu-id="b34e7-238">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b34e7-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="b34e7-239">görev</span><span class="sxs-lookup"><span data-stu-id="b34e7-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="b34e7-240">`task` İşlevi bir görev oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b34e7-240">The `task` function creates a task.</span></span> <span data-ttu-id="b34e7-241">`name` Parametre görevin adını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="b34e7-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="b34e7-242">`deps` Parametresi bu görevi çalışmadan önce tamamlanması gereken görevler dizisi içerir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="b34e7-243">`fn` Parametresi görev işlemlerini gerçekleştiren bir geri çağırma işlevini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="b34e7-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="b34e7-244">İzleme</span><span class="sxs-lookup"><span data-stu-id="b34e7-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="b34e7-245">`watch` İşlevi bir dosya değişikliği oluştuğunda dosyaları ve çalıştırmalarını görevleri izler.</span><span class="sxs-lookup"><span data-stu-id="b34e7-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="b34e7-246">`glob` Parametresi bir `string` veya `array` izlemek için hangi dosyaların belirler.</span><span class="sxs-lookup"><span data-stu-id="b34e7-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="b34e7-247">`opts` Parametresi seçenekleri izlemeye ek dosya sağlar.</span><span class="sxs-lookup"><span data-stu-id="b34e7-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="b34e7-248">src</span><span class="sxs-lookup"><span data-stu-id="b34e7-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="b34e7-249">`src` İşlevi glob değerler ile eşleşen dosyaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="b34e7-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="b34e7-250">`glob` Parametresi bir `string` veya `array` okumak için hangi dosyaların belirler.</span><span class="sxs-lookup"><span data-stu-id="b34e7-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="b34e7-251">`options` Parametresi ek dosya seçenekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b34e7-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="b34e7-252">Hedef</span><span class="sxs-lookup"><span data-stu-id="b34e7-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="b34e7-253">`dest` İşlevi için dosyaları yazılabilir bir konuma tanımlar.</span><span class="sxs-lookup"><span data-stu-id="b34e7-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="b34e7-254">`path` Parametredir bir dize veya hedef klasör belirleyen işlev.</span><span class="sxs-lookup"><span data-stu-id="b34e7-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="b34e7-255">`options` Parametredir çıkış klasörü seçeneklerini belirtir. bir nesne.</span><span class="sxs-lookup"><span data-stu-id="b34e7-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="b34e7-256">Ek Gulp API başvuru bilgileri için bkz: [Gulp belgeleri API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="b34e7-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="b34e7-257">Tarif gulp</span><span class="sxs-lookup"><span data-stu-id="b34e7-257">Gulp recipes</span></span>

<span data-ttu-id="b34e7-258">Gulp Gulp topluluk sağlar [tarif](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="b34e7-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="b34e7-259">Bu tarif ortak senaryolar için Gulp görevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b34e7-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b34e7-260">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b34e7-260">Additional resources</span></span>

* [<span data-ttu-id="b34e7-261">Gulp belgeleri</span><span class="sxs-lookup"><span data-stu-id="b34e7-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="b34e7-262">Paketleme ve ASP.NET Core küçültme</span><span class="sxs-lookup"><span data-stu-id="b34e7-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="b34e7-263">ASP.NET çekirdek Grunt kullanın</span><span class="sxs-lookup"><span data-stu-id="b34e7-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
