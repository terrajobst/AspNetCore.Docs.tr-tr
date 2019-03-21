---
title: ASP.NET Core Gulp kullanma
author: rick-anderson
description: ASP.NET Core Gulp kullanmayı öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 9f6d03a1e8a81bceca15cb1e1aa664c22c31e1d3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209878"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="b8146-103">ASP.NET Core Gulp kullanma</span><span class="sxs-lookup"><span data-stu-id="b8146-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="b8146-104">Tarafından [Scott Addie](https://scottaddie.com), [Shayne boyer'ın](https://twitter.com/spboyer), ve [David Çam](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="b8146-104">By [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="b8146-105">Tipik bir modern web uygulamasında, yapı işlemi olabilir:</span><span class="sxs-lookup"><span data-stu-id="b8146-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="b8146-106">Ve JavaScript ve CSS dosyalarına küçültme.</span><span class="sxs-lookup"><span data-stu-id="b8146-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="b8146-107">Paketleme ve küçültme görevleri her yapıdan önce çağrılacak araçları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b8146-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="b8146-108">CSS daha az derleme veya SASS dosyalarına.</span><span class="sxs-lookup"><span data-stu-id="b8146-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="b8146-109">CoffeeScript veya TypeScript dosyaları JavaScript'e derleyin.</span><span class="sxs-lookup"><span data-stu-id="b8146-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="b8146-110">A *görev Çalıştırıcı* , bu yordamı geliştirme görevlerini ve daha fazlasını otomatikleştiren bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="b8146-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="b8146-111">Visual Studio iki popüler JavaScript tabanlı görev çalıştırıcıların için yerleşik destek sağlar: [Gulp](https://gulpjs.com/) ve [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="b8146-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="b8146-112">Gulp</span><span class="sxs-lookup"><span data-stu-id="b8146-112">Gulp</span></span>

<span data-ttu-id="b8146-113">Gulp bir JavaScript tabanlı akış derleme araç için istemci tarafı kod setidir.</span><span class="sxs-lookup"><span data-stu-id="b8146-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="b8146-114">Ayrıca, bir derleme ortamında belirli bir olayı tetiklendiğinde süreçlerini bir dizi istemci-tarafı dosyaları akışla aktarma için yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b8146-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="b8146-115">Örneğin, Gulp otomatikleştirmek için kullanılabilir [paketleme ve küçültme](bundling-and-minification.md) veya yeni bir yapıdan önce bir geliştirme ortamı temizleniyor.</span><span class="sxs-lookup"><span data-stu-id="b8146-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleaning of a development environment before a new build.</span></span>

<span data-ttu-id="b8146-116">Gulp görev kümesini tanımlanan *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b8146-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="b8146-117">Aşağıdaki JavaScript Gulp modüllerini içerir ve gelecek görevlerde başvurulabilmesi için dosya yollarını belirtir:</span><span class="sxs-lookup"><span data-stu-id="b8146-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="b8146-118">Yukarıdaki kod, düğüm modüllerine gerekli olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="b8146-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="b8146-119">`require` İşlevi özelliklerine bağımlı görevler kullanabilir, böylece her bir modülü içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="b8146-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="b8146-120">Her bir içeri aktarılan modül bir değişkene atanır.</span><span class="sxs-lookup"><span data-stu-id="b8146-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="b8146-121">Modül adı veya yolu yer alabilir.</span><span class="sxs-lookup"><span data-stu-id="b8146-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="b8146-122">Bu örnekte, modülleri adlı `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, ve `gulp-uglify` adına göre alınır.</span><span class="sxs-lookup"><span data-stu-id="b8146-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="b8146-123">Ayrıca, yolları bir dizi oluşturulur, CSS ve JavaScript dosyalarının konumları yeniden ve görevlerde başvurulan.</span><span class="sxs-lookup"><span data-stu-id="b8146-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="b8146-124">Aşağıdaki tabloda bulunan modüllerinin açıklamaları verilmiştir *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b8146-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="b8146-125">Modül adı</span><span class="sxs-lookup"><span data-stu-id="b8146-125">Module Name</span></span> | <span data-ttu-id="b8146-126">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b8146-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="b8146-127">Gulp</span><span class="sxs-lookup"><span data-stu-id="b8146-127">gulp</span></span>        | <span data-ttu-id="b8146-128">Akış Gulp derleme sistemi.</span><span class="sxs-lookup"><span data-stu-id="b8146-128">The Gulp streaming build system.</span></span> <span data-ttu-id="b8146-129">Daha fazla bilgi için [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="b8146-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="b8146-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="b8146-130">rimraf</span></span>      | <span data-ttu-id="b8146-131">Bir düğüm silme modülü.</span><span class="sxs-lookup"><span data-stu-id="b8146-131">A Node deletion module.</span></span> <span data-ttu-id="b8146-132">Daha fazla bilgi için [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="b8146-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="b8146-133">gulp Birleştir</span><span class="sxs-lookup"><span data-stu-id="b8146-133">gulp-concat</span></span> | <span data-ttu-id="b8146-134">İşletim sisteminin yeni satır karakterine bağlı dosyaları birleştiren bir modül.</span><span class="sxs-lookup"><span data-stu-id="b8146-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="b8146-135">Daha fazla bilgi için [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="b8146-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="b8146-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="b8146-136">gulp-cssmin</span></span> | <span data-ttu-id="b8146-137">CSS dosyaları küçültür modülü.</span><span class="sxs-lookup"><span data-stu-id="b8146-137">A module that minifies CSS files.</span></span> <span data-ttu-id="b8146-138">Daha fazla bilgi için [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="b8146-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="b8146-139">gulp uglify</span><span class="sxs-lookup"><span data-stu-id="b8146-139">gulp-uglify</span></span> | <span data-ttu-id="b8146-140">Küçültür bir modül *.js* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b8146-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="b8146-141">Daha fazla bilgi için [gulp uglify](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="b8146-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="b8146-142">Gerekli modülleri alındıktan sonra görevleri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="b8146-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="b8146-143">Burada altı görevler vardır kayıtlı, aşağıdaki kodun gösterdiği:</span><span class="sxs-lookup"><span data-stu-id="b8146-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="b8146-144">Aşağıdaki tabloda, yukarıdaki kodda belirtilen görevlerin bir açıklama sağlar:</span><span class="sxs-lookup"><span data-stu-id="b8146-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="b8146-145">Görev adı</span><span class="sxs-lookup"><span data-stu-id="b8146-145">Task Name</span></span>|<span data-ttu-id="b8146-146">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b8146-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="b8146-147">temiz: js</span><span class="sxs-lookup"><span data-stu-id="b8146-147">clean:js</span></span>|<span data-ttu-id="b8146-148">Site.js dosyasını küçültülmüş sürümünü kaldırmak için rimraf düğüm silme modülü kullanan bir görev.</span><span class="sxs-lookup"><span data-stu-id="b8146-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="b8146-149">temiz: css</span><span class="sxs-lookup"><span data-stu-id="b8146-149">clean:css</span></span>|<span data-ttu-id="b8146-150">Site.css dosya küçültülmüş sürümünü kaldırmak için rimraf düğüm silme modülü kullanan bir görev.</span><span class="sxs-lookup"><span data-stu-id="b8146-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="b8146-151">Temizleme</span><span class="sxs-lookup"><span data-stu-id="b8146-151">clean</span></span>|<span data-ttu-id="b8146-152">Çağıran göreve `clean:js` görev, ardından `clean:css` görev.</span><span class="sxs-lookup"><span data-stu-id="b8146-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="b8146-153">Min:js</span><span class="sxs-lookup"><span data-stu-id="b8146-153">min:js</span></span>|<span data-ttu-id="b8146-154">Küçültür ve js klasördeki tüm .js dosyaları birleştiren bir görev.</span><span class="sxs-lookup"><span data-stu-id="b8146-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="b8146-155">. Min.js dosyaları hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="b8146-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="b8146-156">min:css</span><span class="sxs-lookup"><span data-stu-id="b8146-156">min:css</span></span>|<span data-ttu-id="b8146-157">Bir görev, küçültür ve css klasördeki tüm .css dosyaları art arda ekler.</span><span class="sxs-lookup"><span data-stu-id="b8146-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="b8146-158">. Min.css dosyaları hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="b8146-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="b8146-159">min</span><span class="sxs-lookup"><span data-stu-id="b8146-159">min</span></span>|<span data-ttu-id="b8146-160">Çağıran göreve `min:js` görev, ardından `min:css` görev.</span><span class="sxs-lookup"><span data-stu-id="b8146-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="b8146-161">Varsayılan görevleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b8146-161">Running default tasks</span></span>

<span data-ttu-id="b8146-162">Yeni bir Web uygulaması oluşturmadıysanız, Visual Studio'da yeni bir ASP.NET Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b8146-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1. <span data-ttu-id="b8146-163">Açık *package.json* dosyası (ekleyin Aksi halde var) ve aşağıdakileri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b8146-163">Open the *package.json* file (add if not there) and add the following.</span></span>

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

2. <span data-ttu-id="b8146-164">Projenize yeni bir JavaScript dosyası ekleyin ve adlandırın *gulpfile.js*, ardından aşağıdaki kodu kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="b8146-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

3. <span data-ttu-id="b8146-165">İçinde **Çözüm Gezgini**, sağ *gulpfile.js*seçip **görev Çalıştırıcı Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="b8146-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>

    ![Görev Çalıştırıcı Gezgini Çözüm Gezgini'nden açın](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)

    <span data-ttu-id="b8146-167">**Görev Çalıştırıcı Gezgini** Gulp görev listesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="b8146-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="b8146-168">('ye tıklamanız gerekebilir **Yenile** proje adının solunda görünen düğme.)</span><span class="sxs-lookup"><span data-stu-id="b8146-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>

    ![Görev Çalıştırıcı Gezgini](using-gulp/_static/03-TaskRunnerExplorer.png)

    > [!IMPORTANT]
    > <span data-ttu-id="b8146-170">**Görev Çalıştırıcı Gezgini** bağlam menüsü öğesi, yalnızca görünür *gulpfile.js* kök proje dizininde olduğu.</span><span class="sxs-lookup"><span data-stu-id="b8146-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4. <span data-ttu-id="b8146-171">Altındaki **görevleri** içinde **görev Çalıştırıcı Gezgini**, sağ **temiz**seçip **çalıştırma** açılır menüden.</span><span class="sxs-lookup"><span data-stu-id="b8146-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Görev Çalıştırıcı Gezgini temizleme görevini](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="b8146-173">**Görev Çalıştırıcı Gezgini** adlı yeni bir sekme oluşturacak **temiz** ve içinde tanımlanan temizleme görevini yürütün *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b8146-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5. <span data-ttu-id="b8146-174">Sağ **temiz** görev ve ardından **bağlamaları** > **önce yapı**.</span><span class="sxs-lookup"><span data-stu-id="b8146-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Görev Çalıştırıcı Gezgini BeforeBuild bağlama](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="b8146-176">**Önce yapı** bağlama temizleme görevini her yapı projenin önce otomatik olarak çalışacak şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="b8146-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="b8146-177">Bağlamaları ile ayarladığınız **görev Çalıştırıcı Gezgini** en üstündeki açıklama biçiminde depolanır, *gulpfile.js* ve yalnızca Visual Studio'da etkili olur.</span><span class="sxs-lookup"><span data-stu-id="b8146-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="b8146-178">Gulp görev otomatik yürütme yapılandırmak için Visual Studio gerektirmeyen bir alternatif olan, *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="b8146-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="b8146-179">Örneğin, bu yerleştirecek, *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b8146-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="b8146-180">Artık Visual Studio'da veya bir komut istemi kullanarak proje çalıştırıldığında temizleme görevini yürütülür [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) komut (çalıştırma `npm install` ilk).</span><span class="sxs-lookup"><span data-stu-id="b8146-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="b8146-181">Tanımlama ve yeni bir görevi çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b8146-181">Defining and running a new task</span></span>

<span data-ttu-id="b8146-182">Yeni Gulp görev tanımlamak için değiştirme *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b8146-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1. <span data-ttu-id="b8146-183">Aşağıdaki JavaScript sonuna ekleyin *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="b8146-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    <span data-ttu-id="b8146-184">Bu görev adlı `first`, ve yalnızca bir dize görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b8146-184">This task is named `first`, and it simply displays a string.</span></span>

2. <span data-ttu-id="b8146-185">Kaydet *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b8146-185">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="b8146-186">İçinde **Çözüm Gezgini**, sağ *gulpfile.js*seçip *görev Çalıştırıcı Gezgini*.</span><span class="sxs-lookup"><span data-stu-id="b8146-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4. <span data-ttu-id="b8146-187">İçinde **görev Çalıştırıcı Gezgini**, sağ **ilk**seçip **çalıştırma**.</span><span class="sxs-lookup"><span data-stu-id="b8146-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Görev Çalıştırıcı Gezgini ilk görevi çalıştır](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="b8146-189">Çıkış metni görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b8146-189">The output text is displayed.</span></span> <span data-ttu-id="b8146-190">Yaygın senaryoları tabanlı örnekler görmek için bkz: [Gulp tarifler](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="b8146-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="b8146-191">Tanımlama ve sıralı görevleri çalıştırma</span><span class="sxs-lookup"><span data-stu-id="b8146-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="b8146-192">Görevler, eşzamanlı olarak birden çok görev çalıştırdığınızda, varsayılan olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="b8146-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="b8146-193">Belirli bir sırada görevleri çalıştırmak ihtiyacınız varsa, ancak her görevi de, tamamlandığında belirtmelisiniz hangi görevleri olarak başka bir görev öğesinin tamamlanmasına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="b8146-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1. <span data-ttu-id="b8146-194">Bir sırada çalıştırılacak görev dizisini tanımlamak için değiştirin `first` , yukarıda eklediğiniz görev *gulpfile.js* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="b8146-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

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

    <span data-ttu-id="b8146-195">Artık üç görev vardır: `series:first`, `series:second`, ve `series`.</span><span class="sxs-lookup"><span data-stu-id="b8146-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="b8146-196">`series:second` Görevi çalıştırın ve önce tamamlanan görevleri bir dizi belirtir, ikinci bir parametre içeren `series:second` görev çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b8146-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="b8146-197">Kodu yalnızca yukarıda belirtildiği gibi `series:first` görevi tamamlandı, önce `series:second` görev çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b8146-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2. <span data-ttu-id="b8146-198">Kaydet *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="b8146-198">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="b8146-199">İçinde **Çözüm Gezgini**, sağ *gulpfile.js* seçip **görev Çalıştırıcı Gezgini** zaten açık değilse.</span><span class="sxs-lookup"><span data-stu-id="b8146-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4. <span data-ttu-id="b8146-200">İçinde **görev Çalıştırıcı Gezgini**, sağ **serisi** seçip **çalıştırma**.</span><span class="sxs-lookup"><span data-stu-id="b8146-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Görev Çalıştırıcı Gezgini serisi görevini Çalıştır](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="b8146-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="b8146-202">IntelliSense</span></span>

<span data-ttu-id="b8146-203">IntelliSense kod tamamlama, parametre açıklamaları ve verimliliğini artırın ve hatalarını azaltmak için diğer özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="b8146-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="b8146-204">Gulp görev JavaScript'te yazılmış; Bu nedenle, IntelliSense, geliştirme sırasında Yardım sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="b8146-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="b8146-205">JavaScript ile çalışırken, IntelliSense, nesneleri, işlevleri, özellikleri listeler ve kullanılabilir parametreleri geçerli Bağlamınızı temel alarak.</span><span class="sxs-lookup"><span data-stu-id="b8146-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="b8146-206">Kodunuzu tamamlayabilirsiniz IntelliSense'in sağladığı açılan listeden bir kodlama seçeneğini seçin.</span><span class="sxs-lookup"><span data-stu-id="b8146-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![IntelliSense gulp](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="b8146-208">IntelliSense hakkında daha fazla bilgi için bkz: [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="b8146-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="b8146-209">Geliştirme, hazırlama ve üretim ortamları</span><span class="sxs-lookup"><span data-stu-id="b8146-209">Development, staging, and production environments</span></span>

<span data-ttu-id="b8146-210">Gulp, istemci tarafı dosyaları hazırlama ve üretim için en iyi duruma getirmek için kullanıldığında, işlenen dosyalar yerel bir hazırlama ve üretim konuma kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b8146-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="b8146-211">*_Layout.cshtml* dosya kullandığı **ortam** etiket Yardımcısı CSS dosyaları iki farklı sürümleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b8146-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="b8146-212">Bir CSS dosyaları için geliştirme sürümüdür ve başka bir sürüm, hazırlama ve üretim için optimize edilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b8146-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="b8146-213">Visual Studio değiştirdiğinizde 2017, **ASPNETCORE_ENVIRONMENT** ortam değişkenine `Production`, Visual Studio bağlantı azaltılmış CSS dosyaları ve Web uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b8146-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="b8146-214">Aşağıdaki biçimlendirme gösterildiği **ortam** etiket Yardımcıları için bağlantı etiketlerini içeren `Development` CSS dosyaları ve küçültülmüş `Staging, Production` CSS dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b8146-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="b8146-215">Ortamlar arasında geçiş yapma</span><span class="sxs-lookup"><span data-stu-id="b8146-215">Switching between environments</span></span>

<span data-ttu-id="b8146-216">Derleme için farklı ortamlar arasında geçiş yapmak için değişiklik **ASPNETCORE_ENVIRONMENT** ortam değişkenin değeri.</span><span class="sxs-lookup"><span data-stu-id="b8146-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1. <span data-ttu-id="b8146-217">İçinde **görev Çalıştırıcı Gezgini**, doğrulayın **min** görev olarak çalıştırmak için ayarlandı **önce yapı**.</span><span class="sxs-lookup"><span data-stu-id="b8146-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2. <span data-ttu-id="b8146-218">İçinde **Çözüm Gezgini**, proje adını sağ tıklatın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="b8146-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="b8146-219">Web uygulaması için özellik sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b8146-219">The property sheet for the Web app is displayed.</span></span>

3. <span data-ttu-id="b8146-220">Tıklayın **hata ayıklama** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="b8146-220">Click the **Debug** tab.</span></span>

4. <span data-ttu-id="b8146-221">Değerini **barındırma: ortam** ortam değişkenine `Production`.</span><span class="sxs-lookup"><span data-stu-id="b8146-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5. <span data-ttu-id="b8146-222">Tuşuna **F5** uygulamayı tarayıcıda çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b8146-222">Press **F5** to run the application in a browser.</span></span>

6. <span data-ttu-id="b8146-223">Tarayıcı penceresinde, sayfanın sağ tıklayıp **kaynağı görüntüle** sayfanın HTML görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="b8146-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="b8146-224">Stil sayfası bağlantıları küçültülmüş CSS dosyalarının olduğu noktaya dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b8146-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7. <span data-ttu-id="b8146-225">Web uygulamasını Durdur için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="b8146-225">Close the browser to stop the Web app.</span></span>

8. <span data-ttu-id="b8146-226">Visual Studio'da Web uygulaması için özellik sayfasına dönmek ve değiştirmek **barındırma: ortam** ortam değişkeni başa `Development`.</span><span class="sxs-lookup"><span data-stu-id="b8146-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9. <span data-ttu-id="b8146-227">Tuşuna **F5** uygulamayı bir tarayıcıda yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b8146-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="b8146-228">Tarayıcı penceresinde, sayfanın sağ tıklayıp **kaynağı görüntüle** sayfanın HTML görmek için.</span><span class="sxs-lookup"><span data-stu-id="b8146-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="b8146-229">CSS dosyaları unminified sürümleri için stil sayfası bağlantı noktası dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="b8146-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="b8146-230">ASP.NET Core ortamlarda ilgili daha fazla bilgi için bkz. [birden fazla ortam kullanayım](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="b8146-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="b8146-231">Görev ve modül ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="b8146-231">Task and module details</span></span>

<span data-ttu-id="b8146-232">Gulp görev, bir işlev adı ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b8146-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="b8146-233">Diğer görevlerden önce geçerli görevin çalıştırmanız gerekiyorsa, bağımlılıkları belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b8146-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="b8146-234">Ek işlevler izin çalıştırın ve Gulp görevleri izleyin, hem de kaynak ayarlayın (*src*) ve hedef (*dest*) değiştirilen dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b8146-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="b8146-235">Birincil Gulp API işlevleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b8146-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="b8146-236">Gulp işlevi</span><span class="sxs-lookup"><span data-stu-id="b8146-236">Gulp Function</span></span>|<span data-ttu-id="b8146-237">Sözdizimi</span><span class="sxs-lookup"><span data-stu-id="b8146-237">Syntax</span></span>|<span data-ttu-id="b8146-238">Açıklama</span><span class="sxs-lookup"><span data-stu-id="b8146-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="b8146-239">görev</span><span class="sxs-lookup"><span data-stu-id="b8146-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="b8146-240">`task` İşlevi, bir görev oluşturur.</span><span class="sxs-lookup"><span data-stu-id="b8146-240">The `task` function creates a task.</span></span> <span data-ttu-id="b8146-241">`name` Parametre görev adını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="b8146-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="b8146-242">`deps` Parametresi, bu görevi çalışmadan önce tamamlanması için görev dizisi içerir.</span><span class="sxs-lookup"><span data-stu-id="b8146-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="b8146-243">`fn` Parametre görevin işlemleri gerçekleştiren bir geri çağırma işlevini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="b8146-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="b8146-244">İzleme</span><span class="sxs-lookup"><span data-stu-id="b8146-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="b8146-245">`watch` İşlevi, bir dosya değişikliği oluştuğunda dosyaları ve çalışan görevleri izler.</span><span class="sxs-lookup"><span data-stu-id="b8146-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="b8146-246">`glob` Parametresi bir `string` veya `array` izlemek için hangi dosyaların belirler.</span><span class="sxs-lookup"><span data-stu-id="b8146-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="b8146-247">`opts` Parametresi ek dosya izleme seçenekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b8146-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="b8146-248">src</span><span class="sxs-lookup"><span data-stu-id="b8146-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="b8146-249">`src` İşlevi glob değerleri eşleşen dosyaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="b8146-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="b8146-250">`glob` Parametresi bir `string` veya `array` okumak için hangi dosyaların belirler.</span><span class="sxs-lookup"><span data-stu-id="b8146-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="b8146-251">`options` Parametresi ek dosya seçenekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="b8146-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="b8146-252">Hedef</span><span class="sxs-lookup"><span data-stu-id="b8146-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="b8146-253">`dest` İşlevi olduğu dosyaları yazılabilir bir konuma tanımlar.</span><span class="sxs-lookup"><span data-stu-id="b8146-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="b8146-254">`path` Parametresi, bir dize veya hedef klasör belirleyen işlev.</span><span class="sxs-lookup"><span data-stu-id="b8146-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="b8146-255">`options` Parametredir çıkış klasör seçenekleri belirten bir nesne.</span><span class="sxs-lookup"><span data-stu-id="b8146-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="b8146-256">Ek Gulp API başvuru bilgileri için bkz: [Gulp Docs API](https://gulpjs.org/API.html).</span><span class="sxs-lookup"><span data-stu-id="b8146-256">For additional Gulp API reference information, see [Gulp Docs API](https://gulpjs.org/API.html).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="b8146-257">Gulp tarifleri</span><span class="sxs-lookup"><span data-stu-id="b8146-257">Gulp recipes</span></span>

<span data-ttu-id="b8146-258">Gulp Gulp topluluk sağlar [tarifler](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="b8146-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="b8146-259">Bu tarif içeren yaygın senaryolara Gulp görevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="b8146-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b8146-260">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b8146-260">Additional resources</span></span>

* [<span data-ttu-id="b8146-261">Gulp belgeleri</span><span class="sxs-lookup"><span data-stu-id="b8146-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="b8146-262">Paketleme ve küçültme ASP.NET core'da</span><span class="sxs-lookup"><span data-stu-id="b8146-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="b8146-263">ASP.NET Core Grunt kullanma</span><span class="sxs-lookup"><span data-stu-id="b8146-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
