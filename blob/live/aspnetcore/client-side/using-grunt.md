---
title: Grunt ASP.NET Core kullanarak
author: rick-anderson
description: 
keywords: "ASP.NET Çekirdeği"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 471112e9-2c33-454b-96fc-32916102ce73
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-grunt
ms.openlocfilehash: 8ae50514ce24c7f9e3bb1e347d5d860e1de43c5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="using-grunt-in-aspnet-core"></a><span data-ttu-id="253af-103">Grunt ASP.NET Core kullanarak</span><span class="sxs-lookup"><span data-stu-id="253af-103">Using Grunt in ASP.NET Core</span></span> 

<span data-ttu-id="253af-104">Tarafından [Noel pirinç](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="253af-104">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="253af-105">Grunt betik küçültme, TypeScript derlemesini, kod kalitesi "tüy" araçları, CSS ön işlemci ve istemci geliştirme desteklemek için yapılması gereken neredeyse tüm yinelenen işi otomatikleştiren bir JavaScript görev Çalıştırıcı ' dir.</span><span class="sxs-lookup"><span data-stu-id="253af-105">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="253af-106">ASP.NET proje şablonları varsayılan olarak Gulp kullanır ancak grunt Visual Studio'da tam olarak desteklenmektedir (bkz [kullanarak Gulp](using-gulp.md)).</span><span class="sxs-lookup"><span data-stu-id="253af-106">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Using Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="253af-107">Bu örnekte boş bir ASP.NET Core projeyi sıfırdan istemci derleme işlemini otomatikleştirmek nasıl göstermek için başlangıç noktası olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="253af-107">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="253af-108">Tamamlanan örnek hedef dağıtım dizini temizler, JavaScript dosyaları birleştirir, kod kalitesini denetler, JavaScript dosyası içeriği toplar ve web uygulamanızın kök dizinine dağıtır.</span><span class="sxs-lookup"><span data-stu-id="253af-108">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="253af-109">Aşağıdaki paketler kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="253af-109">We will use the following packages:</span></span>

* <span data-ttu-id="253af-110">**grunt**: Grunt görev Çalıştırıcı paket.</span><span class="sxs-lookup"><span data-stu-id="253af-110">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="253af-111">**grunt contrib temiz**: dosya veya dizinlerin kaldıran bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="253af-111">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="253af-112">**grunt contrib jshint**: JavaScript kod kalitesini incelemeleri bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="253af-112">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="253af-113">**grunt contrib concat**: tek bir dosyaya dosyaları birleştiren bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="253af-113">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="253af-114">**grunt contrib uglify**: boyutunu azaltmak için JavaScript küçültür bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="253af-114">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="253af-115">**grunt contrib izleme**: dosya etkinliğini izleyen bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="253af-115">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="253af-116">Uygulama hazırlama</span><span class="sxs-lookup"><span data-stu-id="253af-116">Preparing the application</span></span>

<span data-ttu-id="253af-117">Başlamak için yeni bir boş web uygulamasını ayarlama ve TypeScript örnek dosyalarını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="253af-117">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="253af-118">TypeScript dosyaları otomatik olarak varsayılan Visual Studio ayarları kullanarak JavaScript ile derlenen ve Grunt kullanarak işlemek için bizim ham madde olacaktır.</span><span class="sxs-lookup"><span data-stu-id="253af-118">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1.  <span data-ttu-id="253af-119">Visual Studio'da yeni bir oluşturma `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="253af-119">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2.  <span data-ttu-id="253af-120">İçinde **yeni ASP.NET projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablonu ve Tamam düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="253af-120">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3.  <span data-ttu-id="253af-121">Çözüm Gezgini'nde proje yapısını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="253af-121">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="253af-122">`\src` Klasörü boş içerir `wwwroot` ve `Dependencies` düğümleri.</span><span class="sxs-lookup"><span data-stu-id="253af-122">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![boş web çözümü](using-grunt/_static/grunt-solution-explorer.png)

4.  <span data-ttu-id="253af-124">Adlı yeni bir klasör ekleyin `TypeScript` proje dizininiz için.</span><span class="sxs-lookup"><span data-stu-id="253af-124">Add a new folder named `TypeScript` to your project directory.</span></span>

5.  <span data-ttu-id="253af-125">Herhangi bir dosya eklemeden önce Visual Studio seçeneğine sahip olduğundan emin olalım ' derleme kaydederken ' işaretli TypeScript dosyaları için.</span><span class="sxs-lookup"><span data-stu-id="253af-125">Before adding any files, let’s make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="253af-126">*Araçlar > Seçenekler > Metin Düzenleyicisi > Typescript > Proje*</span><span class="sxs-lookup"><span data-stu-id="253af-126">*Tools > Options > Text Editor > Typescript > Project*</span></span>

    ![Otomatik compliation TypeScript dosyaları ayarlama seçenekleri](using-grunt/_static/typescript-options.png)

6.  <span data-ttu-id="253af-128">Sağ `TypeScript` seçin ve dizin **Ekle > Yeni öğe** ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="253af-128">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="253af-129">Seçin **JavaScript dosyası** öğe ve dosya adı *Tastes.ts* (Not \*.ts uzantısı).</span><span class="sxs-lookup"><span data-stu-id="253af-129">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="253af-130">TypeScript kod satırı dosyaya kopyalayın (kaydettiğinizde, yeni bir *Tastes.js* dosya JavaScript kaynağıyla görünür).</span><span class="sxs-lookup"><span data-stu-id="253af-130">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  <span data-ttu-id="253af-131">İkinci bir dosya eklemek **TypeScript** dizin ve adlandırın `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="253af-131">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="253af-132">Aşağıdaki kod dosyasına kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="253af-132">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }
    
      private _name: string;
      get Name() {
        return this._name;
      }
    
      private _calories: number;
      get Calories() {
        return this._calories;
      }
    
      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="253af-133">NPM yapılandırma</span><span class="sxs-lookup"><span data-stu-id="253af-133">Configuring NPM</span></span>

<span data-ttu-id="253af-134">Ardından, grunt ve grunt görevleri indirmek için NPM yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="253af-134">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="253af-135">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle > Yeni öğe** ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="253af-135">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="253af-136">Seçin **NPM yapılandırma dosyası** öğesi, varsayılan adı bırakın *package.json*, tıklatıp **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="253af-136">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="253af-137">İçinde *package.json* içinde dosya `devDependencies` nesne ayraçlar, "grunt" girin.</span><span class="sxs-lookup"><span data-stu-id="253af-137">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="253af-138">Seçin `grunt` IntelliSense listesinde ve Enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="253af-138">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="253af-139">Visual Studio grunt paket adı teklif ve iki nokta ekleyin.</span><span class="sxs-lookup"><span data-stu-id="253af-139">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="253af-140">İki nokta üst üste sağında, paketin en son kararlı sürümü IntelliSense listenin başından seçin (basın `Ctrl-Space` IntelliSense görünmüyorsa).</span><span class="sxs-lookup"><span data-stu-id="253af-140">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense does not appear).</span></span>

    ![grun IntelliSense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > <span data-ttu-id="253af-142">NPM kullanan [anlamsal sürüm oluşturma](http://semver.org/) bağımlılıkları düzenlemek için.</span><span class="sxs-lookup"><span data-stu-id="253af-142">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="253af-143">Anlamsal sürüm oluşturma olarak da bilinen SemVer tanımlayan paketleri numaralandırma düzeniyle <major>.<minor>. <patch>. IntelliSense, yalnızca birkaç genel seçenek göstererek anlamsal sürüm oluşturma basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="253af-143">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme <major>.<minor>.<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="253af-144">Üst öğeyi IntelliSense listesinde (yukarıdaki örnekte 0.4.5), paketin en son kararlı sürümü olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="253af-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="253af-145">Şapka (^) karakteri son ana sürümle eşleşen ve en son ikincil sürümle tilde (~) eşleşir.</span><span class="sxs-lookup"><span data-stu-id="253af-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="253af-146">Bkz: [NPM semver sürüm ayrıştırıcı başvurusu](https://www.npmjs.com/package/semver) SemVer sağlar tam expressivity bir kılavuz olarak.</span><span class="sxs-lookup"><span data-stu-id="253af-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="253af-147">Yüklemek için daha fazla bağımlılıkları grunt add-contrib -\* için paketler *temiz*, *jshint*, *concat*, *uglify*ve *izleme* aşağıdaki örnekte gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="253af-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="253af-148">Sürümleri örnekle eşleşmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="253af-148">The versions do not need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="253af-149">Kaydet *package.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="253af-149">Save the *package.json* file.</span></span>

<span data-ttu-id="253af-150">Her paket için gerekli dosyalarla birlikte her devDependencies öğesi paketleri indirir.</span><span class="sxs-lookup"><span data-stu-id="253af-150">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="253af-151">Paket dosyaları bulabilirsiniz `node_modules` etkinleştirerek dizin **tüm dosyaları göster** Çözüm Gezgini'nde düğmesi.</span><span class="sxs-lookup"><span data-stu-id="253af-151">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="253af-153">Gerekiyorsa, el ile Çözüm Gezgini'nde bağımlılıkları üzerinde sağ tıklayarak geri yükleyebilirsiniz `Dependencies\NPM` ve seçerek **geri yükleme paketleri** menü seçeneği.</span><span class="sxs-lookup"><span data-stu-id="253af-153">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![geri yükleme paketleri](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="253af-155">Grunt yapılandırma</span><span class="sxs-lookup"><span data-stu-id="253af-155">Configuring Grunt</span></span>

<span data-ttu-id="253af-156">Grunt adlı bir bildirimi kullanarak yapılandırılmış *Gruntfile.js* tanımlar, yükler ve el ile çalıştırın ya da Visual Studio olaylarına göre otomatik olarak çalışacak şekilde yapılandırılmış görevlerini kaydeder.</span><span class="sxs-lookup"><span data-stu-id="253af-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1.  <span data-ttu-id="253af-157">Projeye sağ tıklayın ve seçin **Ekle > Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="253af-157">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="253af-158">Seçin **Grunt yapılandırma dosyası** seçeneği, varsayılan adı bırakın *Gruntfile.js*, tıklatıp **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="253af-158">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

    <span data-ttu-id="253af-159">Modül tanımı ilk kod içerir ve `grunt.initConfig()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="253af-159">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="253af-160">`initConfig()` Her bir paket için seçenekleri ayarlamak için kullanılır ve modülü kalanı yüklemek ve görevleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="253af-160">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. <span data-ttu-id="253af-161">İçinde `initConfig()` yöntemi seçeneklerini eklemek `clean` görev örnekte gösterildiği gibi *Gruntfile.js* aşağıda.</span><span class="sxs-lookup"><span data-stu-id="253af-161">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="253af-162">Temiz görev dizini bir dizeler dizisi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="253af-162">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="253af-163">Bu görev dosyaları wwwroot/lib ve tüm/temp dizini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="253af-163">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="253af-164">İnitConfig() yöntemi bir çağrı ekleyin `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="253af-164">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="253af-165">Bu görev runnable Visual Studio'dan hale getirir.</span><span class="sxs-lookup"><span data-stu-id="253af-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="253af-166">Kaydet *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="253af-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="253af-167">Dosya aşağıdaki ekran görüntüsüne benzer görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="253af-167">The file should look something like the screenshot below.</span></span>

    ![ilk gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="253af-169">Sağ *Gruntfile.js* seçip **görev Çalıştırıcı Gezgini** ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="253af-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="253af-170">Görev Çalıştırıcı Gezgini penceresi açılır.</span><span class="sxs-lookup"><span data-stu-id="253af-170">The Task Runner Explorer window will open.</span></span>

    ![Görev Çalıştırıcı Gezgini menüsü](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="253af-172">Doğrulayın `clean` altında gösterir **görevleri** görev Çalıştırıcı Gezgini içinde.</span><span class="sxs-lookup"><span data-stu-id="253af-172">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![Görev Çalıştırıcı Gezgini görev listesi](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="253af-174">Temiz görevi sağ tıklatın ve seçin **çalıştırmak** ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="253af-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="253af-175">Bir komut penceresi görevin ilerlemesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="253af-175">A command window displays progress of the task.</span></span>

    ![Görev Çalıştırıcı Gezgini çalıştırmak temiz görevi](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > <span data-ttu-id="253af-177">Hiçbir dosya veya dizinlerin henüz temizlemek için vardır.</span><span class="sxs-lookup"><span data-stu-id="253af-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="253af-178">İsterseniz, el ile bunları Çözüm Gezgini'nde oluşturabilir ve ardından temiz görev bir test olarak çalıştırmak.</span><span class="sxs-lookup"><span data-stu-id="253af-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>
    
8. <span data-ttu-id="253af-179">İnitConfig() yönteminde için bir giriş ekleyin `concat` aşağıdaki kodu kullanarak.</span><span class="sxs-lookup"><span data-stu-id="253af-179">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="253af-180">`src` Özellik dizisi, bunlar birleştirilmelidir, sırayla birleştirmek için dosyaları listelenmektedir.</span><span class="sxs-lookup"><span data-stu-id="253af-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="253af-181">`dest` Özelliği üretilen birleşik bir dosyaya yolu atar.</span><span class="sxs-lookup"><span data-stu-id="253af-181">The `dest` property assigns the path to the combined file that is produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > <span data-ttu-id="253af-182">`all` Yukarıdaki kod özelliğinde bir hedef adıdır.</span><span class="sxs-lookup"><span data-stu-id="253af-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="253af-183">Hedefleri bazı Grunt görevleri birden çok derleme ortamları izin vermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="253af-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="253af-184">IntelliSense kullanarak yerleşik hedeflerini görüntüleyebilir veya kendi atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="253af-184">You can view the built-in targets using Intellisense or assign your own.</span></span>
    
9. <span data-ttu-id="253af-185">Ekleme `jshint` aşağıdaki kodu kullanarak görev.</span><span class="sxs-lookup"><span data-stu-id="253af-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="253af-186">Geçici dizinde bulunan tüm JavaScript dosyası karşı jshint kod kalitesini yardımcı programını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="253af-186">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="253af-187">Seçenek "-W069" noktalı gösterim yerine bir özellik yani atamak için sözdizimi JavaScript kullanan ayraç olduğunda hata jshint tarafından üretilen `Tastes["Sweet"]` yerine `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="253af-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="253af-188">Seçeneği, devam etmek için işleminin geri kalanında izin vermek için uyarı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="253af-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

10.  <span data-ttu-id="253af-189">Ekleme `uglify` aşağıdaki kodu kullanarak görev.</span><span class="sxs-lookup"><span data-stu-id="253af-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="253af-190">Görev küçültür *combined.js* dosya geçici dizinde bulunan ve wwwroot/standart adlandırma kuralı aşağıdaki lib içinde sonuç dosyası oluşturur  *\<dosya adı\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="253af-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. <span data-ttu-id="253af-191">Grunt contrib temiz yükleyen çağrısı grunt.loadNpmTasks() altında aşağıdaki kodu kullanarak uglify ve jshint, concat aynı çağrısı içerir.</span><span class="sxs-lookup"><span data-stu-id="253af-191">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="253af-192">Kaydet *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="253af-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="253af-193">Dosya aşağıdaki örnek gibi görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="253af-193">The file should look something like the example below.</span></span>

    ![tam grunt dosyası örneği](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="253af-195">Görev Çalıştırıcı Gezgini Görevler listesini içeren bildirim `clean`, `concat`, `jshint` ve `uglify` görevler.</span><span class="sxs-lookup"><span data-stu-id="253af-195">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="253af-196">Çözüm Gezgini'nde sonuçları gözlemlemek ve her görev sırasına çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="253af-196">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="253af-197">Her görev, hatasız çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="253af-197">Each task should run without errors.</span></span>
    
    ![Görev Çalıştırıcı Gezgini her görevi çalıştırma](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    <span data-ttu-id="253af-199">Concat görev yeni bir oluşturur *combined.js* dosya ve temp dizinine yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="253af-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="253af-200">Jshint görevi yalnızca çalışır ve çıktı oluşturmuyor.</span><span class="sxs-lookup"><span data-stu-id="253af-200">The jshint task simply runs and doesn’t produce output.</span></span> <span data-ttu-id="253af-201">Yeni bir uglify görev oluşturur *combined.min.js* dosya ve wwwroot/lib yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="253af-201">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="253af-202">Tamamlanma, çözümü aşağıdaki ekran görüntüsüne benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="253af-202">On completion, the solution should look something like the screenshot below:</span></span>
    
    ![Çözüm Gezgini tüm görevler](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > <span data-ttu-id="253af-204">Her paket için seçenekleri hakkında daha fazla bilgi için ziyaret [https://www.npmjs.com/](https://www.npmjs.com/) ve arama ana sayfada arama kutusuna paket adı.</span><span class="sxs-lookup"><span data-stu-id="253af-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="253af-205">Örneğin, tüm parametreleri açıklayan bir belge bağlantı almak için grunt contrib temiz paketi bakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="253af-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="253af-206">Artık tümü bir arada</span><span class="sxs-lookup"><span data-stu-id="253af-206">All together now</span></span>

<span data-ttu-id="253af-207">Grunt kullanmak `registerTask()` belirli bir sırada bir dizi görev çalıştırılacak yöntemi.</span><span class="sxs-lookup"><span data-stu-id="253af-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="253af-208">Örneğin, yukarıdaki adımları sırayla temiz -> örneği çalıştırmak için concat -> jshint -> uglify, modüle aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="253af-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="253af-209">Kod initConfig dışında loadNpmTasks() çağrıları aynı düzeyde eklenmelidir.</span><span class="sxs-lookup"><span data-stu-id="253af-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="253af-210">Yeni görev görev Çalıştırıcı Gezgini diğer görevler'in altında görünür.</span><span class="sxs-lookup"><span data-stu-id="253af-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="253af-211">Sağ tıklayın ve diğer görevler gibi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="253af-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="253af-212">`all` Görevin çalışacağını `clean`, `concat`, `jshint` ve `uglify`, sırayla.</span><span class="sxs-lookup"><span data-stu-id="253af-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![diğer ad grunt görevleri](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="253af-214">Değişiklikleri izleme</span><span class="sxs-lookup"><span data-stu-id="253af-214">Watching for changes</span></span>

<span data-ttu-id="253af-215">A `watch` görev dosya ve dizinlerin bir göz tutar.</span><span class="sxs-lookup"><span data-stu-id="253af-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="253af-216">Değişiklikler algılarsa izleme görevleri otomatik olarak tetikler.</span><span class="sxs-lookup"><span data-stu-id="253af-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="253af-217">İnitConfig yapılan değişiklikleri izlemek için aşağıdaki kodu ekleyin \*TypeScript dizindeki .js dosya.</span><span class="sxs-lookup"><span data-stu-id="253af-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="253af-218">Bir JavaScript dosyası değiştirdiyseniz, `watch` çalışacak `all` görev.</span><span class="sxs-lookup"><span data-stu-id="253af-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="253af-219">Bir çağrı ekleyin `loadNpmTasks()` göstermek için `watch` görev Çalıştırıcı Gezgini görevi.</span><span class="sxs-lookup"><span data-stu-id="253af-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="253af-220">Görev Çalıştırıcı Gezgini izleme görevi sağ tıklatın ve bağlam menüsünden Çalıştır'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="253af-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="253af-221">Çalışan izleme görev gösterir komut penceresinde bir "bekleniyor..." görüntülenir</span><span class="sxs-lookup"><span data-stu-id="253af-221">The command window that shows the watch task running will display a "Waiting…"</span></span> <span data-ttu-id="253af-222">İleti.</span><span class="sxs-lookup"><span data-stu-id="253af-222">message.</span></span> <span data-ttu-id="253af-223">TypeScript dosyalarından birini açın, bir alan ekleyin ve ardından dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="253af-223">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="253af-224">Bu izleme tetikleyici ve sırayla çalıştırmak için diğer görevlerini tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="253af-224">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="253af-225">Aşağıdaki ekran görüntüsünde bir örnek çalışma gösterir.</span><span class="sxs-lookup"><span data-stu-id="253af-225">The screenshot below shows a sample run.</span></span>

![Görev çıktı çalıştırma](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="253af-227">Visual Studio olayları bağlama</span><span class="sxs-lookup"><span data-stu-id="253af-227">Binding to Visual Studio events</span></span>

<span data-ttu-id="253af-228">Visual Studio içindeki çalışma her zaman görevlerinizi el ile başlatmak istediğiniz sürece, görevler bağlayabilirsiniz **önce yapı**, **sonra yapı**, **temiz**, ve  **Proje Aç** olaylar.</span><span class="sxs-lookup"><span data-stu-id="253af-228">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="253af-229">Şimdi bağlamak `watch` Visual Studio her açtığında çalışır.</span><span class="sxs-lookup"><span data-stu-id="253af-229">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="253af-230">Görev Çalıştırıcı Gezgini, izleme görevi sağ tıklatın ve seçin **bağlamaları > Proje Aç** ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="253af-230">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![bir görev için proje açma bağlama](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="253af-232">Unload ve projeyi yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="253af-232">Unload and reload the project.</span></span> <span data-ttu-id="253af-233">Projeyi yeniden yüklediğinde, izleme görev otomatik olarak çalıştırarak başlar.</span><span class="sxs-lookup"><span data-stu-id="253af-233">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="253af-234">Özet</span><span class="sxs-lookup"><span data-stu-id="253af-234">Summary</span></span>

<span data-ttu-id="253af-235">Grunt çoğu istemci derleme görevleri otomatik hale getirmek için kullanılan bir güçlü görev Çalıştırıcı ' dir.</span><span class="sxs-lookup"><span data-stu-id="253af-235">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="253af-236">Grunt, paketleri ve Visual Studio ile tümleştirme tooling özelliklerini sunmak için NPM yararlanır.</span><span class="sxs-lookup"><span data-stu-id="253af-236">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="253af-237">Visual Studio'nun görev Çalıştırıcı Gezgini yapılandırma dosyalarını değişiklikleri algılar ve görevleri çalıştırmak, çalışan görevleri görüntülemek ve görevleri Visual Studio olaylarına Bağla için kullanışlı bir arabirim sağlar.</span><span class="sxs-lookup"><span data-stu-id="253af-237">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="253af-238">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="253af-238">Additional resources</span></span>

   * [<span data-ttu-id="253af-239">Gulp kullanma</span><span class="sxs-lookup"><span data-stu-id="253af-239">Using Gulp</span></span>](using-gulp.md)
