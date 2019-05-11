---
title: ASP.NET Core Grunt kullanma
author: rick-anderson
description: ''
ms.author: riande
ms.date: 05/10/2019
uid: client-side/using-grunt
ms.openlocfilehash: 73b7704726db9d93588dddd3f3b05a23fb3425b3
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65535948"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="4b8d4-102">ASP.NET Core Grunt kullanma</span><span class="sxs-lookup"><span data-stu-id="4b8d4-102">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="4b8d4-103">Tarafından [Noel pirinç](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="4b8d4-103">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="4b8d4-104">Grunt betik küçültme, TypeScript derleme, kod kalitesini "lint" araçları, CSS ön işlemci ve neredeyse tüm istemci geliştirme desteği için yapmanız gereken yinelenen işi otomatikleştiren bir JavaScript görev Çalıştırıcı ' dir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-104">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="4b8d4-105">Grunt, Visual Studio'da tam olarak desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-105">Grunt is fully supported in Visual Studio.</span></span>

<span data-ttu-id="4b8d4-106">Bu örnek, boş bir ASP.NET Core projesi sıfırdan istemci derleme işlemini otomatik hale getirmek nasıl göstermek için başlangıç noktası olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-106">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="4b8d4-107">Tamamlanan örnek hedef dağıtım dizinine temizler, JavaScript dosyaları birleştirir, kod kalitesini denetler, JavaScript dosya içeriği toplar ve web uygulamanızın kök dizinine dağıtır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-107">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="4b8d4-108">Aşağıdaki paketler kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="4b8d4-108">We will use the following packages:</span></span>

* <span data-ttu-id="4b8d4-109">**grunt**: Grunt görev Çalıştırıcı paket.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-109">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="4b8d4-110">**grunt contrib temiz**: Dosyaları veya dizinleri kaldıran bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-110">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="4b8d4-111">**grunt-contrib-jshint**: JavaScript kod kalitesini gözden geçirmeleri bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-111">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="4b8d4-112">**grunt contrib concat**: Bir eklenti dosyalarını tek bir dosya halinde birleştirir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-112">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="4b8d4-113">**grunt contrib uglify**: Bir eklenti boyutunu azaltmak için JavaScript küçültür.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-113">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="4b8d4-114">**grunt contrib watch**: Dosya etkinliği izleyen bir eklenti.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-114">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="4b8d4-115">Uygulama hazırlama</span><span class="sxs-lookup"><span data-stu-id="4b8d4-115">Preparing the application</span></span>

<span data-ttu-id="4b8d4-116">Başlamak için yeni bir boş web uygulaması oluşturma ve TypeScript örnek dosyaları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-116">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="4b8d4-117">TypeScript dosyalarını otomatik olarak varsayılan Visual Studio ayarları kullanarak JavaScript ile derlenen ve Grunt kullanarak işlemek için sunduğumuz ham madde olacaktır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-117">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1. <span data-ttu-id="4b8d4-118">Visual Studio'da yeni bir oluşturma `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-118">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2. <span data-ttu-id="4b8d4-119">İçinde **yeni ASP.NET projesi** iletişim kutusunda, ASP.NET Core seçin **boş** şablon ve Tamam düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-119">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3. <span data-ttu-id="4b8d4-120">Çözüm Gezgini'nde proje yapısını inceleyin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-120">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="4b8d4-121">`\src` Klasörünü de içeren boş `wwwroot` ve `Dependencies` düğümleri.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-121">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![boş web çözümü](using-grunt/_static/grunt-solution-explorer.png)

4. <span data-ttu-id="4b8d4-123">Adlı yeni bir klasör eklemek `TypeScript` proje dizininiz için.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-123">Add a new folder named `TypeScript` to your project directory.</span></span>

5. <span data-ttu-id="4b8d4-124">Tüm dosyalar eklemeden önce Visual Studio seçeneği bulunduğundan emin ' derleme kaydederken ' işaretli TypeScript dosyaları için.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-124">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="4b8d4-125">Gidin **Araçları** > **seçenekleri** > **metin düzenleyici** > **Typescript**  >  **Proje**:</span><span class="sxs-lookup"><span data-stu-id="4b8d4-125">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![TypeScript dosyalarının otomatik derleme ayarlama seçenekleri](using-grunt/_static/typescript-options.png)

6. <span data-ttu-id="4b8d4-127">Sağ `TypeScript` dizin ve select **Ekle > Yeni öğe** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-127">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="4b8d4-128">Seçin **JavaScript dosyası** öğe ve dosya adı *Tastes.ts* (Not \*.ts uzantısı).</span><span class="sxs-lookup"><span data-stu-id="4b8d4-128">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="4b8d4-129">TypeScript kod satırının dosyaya kopyalayın (kaydettiğinizde, yeni bir *Tastes.js* dosya ile JavaScript kaynak görünür).</span><span class="sxs-lookup"><span data-stu-id="4b8d4-129">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. <span data-ttu-id="4b8d4-130">İkinci bir dosya ekleyin **TypeScript** dizin ve adlandırın `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-130">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="4b8d4-131">Dosyaya aşağıdaki kodu kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-131">Copy the code below into the file.</span></span>

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

## <a name="configuring-npm"></a><span data-ttu-id="4b8d4-132">NPM yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4b8d4-132">Configuring NPM</span></span>

<span data-ttu-id="4b8d4-133">Ardından, NPM, grunt ve grunt görevlerini indirmek için yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-133">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="4b8d4-134">Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Ekle > Yeni öğe** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-134">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="4b8d4-135">Seçin **NPM yapılandırma dosyası** öğesi, varsayılan adı bırakın *package.json*, tıklatıp **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-135">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="4b8d4-136">İçinde *package.json* içinde dosya `devDependencies` nesne küme ayraçları, "grunt" girin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-136">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="4b8d4-137">Seçin `grunt` IntelliSense'de listesinde ve Enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-137">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="4b8d4-138">Visual Studio teklifi grunt paket adı ve bir iki nokta üst üste ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-138">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="4b8d4-139">İki nokta üst üste sağında, IntelliSense listesini üst kısmından paketin en son kararlı sürümünü seçin (basın `Ctrl-Space` IntelliSense görünmüyorsa).</span><span class="sxs-lookup"><span data-stu-id="4b8d4-139">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![IntelliSense grunt](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > <span data-ttu-id="4b8d4-141">NPM kullanan [semantic versioning](http://semver.org/) bağımlılıkları düzenlemek için.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-141">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="4b8d4-142">SemVer olarak da bilinen semantik sürüm oluşturma, numaralandırma düzeni ile paketleri tanımlayan \<ana >.\< küçük >. \<düzeltme eki >.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-142">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="4b8d4-143">IntelliSense, yalnızca birkaç genel seçenek göstererek semantic versioning basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-143">Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="4b8d4-144">Üst öğeyi IntelliSense listesinde (yukarıdaki örnekte 0.4.5) paketin en son kararlı sürüm olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-144">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="4b8d4-145">Şapka (^) simgesi en son sürümle eşleşen ve son ikincil sürümle tilde (~) ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-145">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="4b8d4-146">Bkz: [NPM semver sürümü çözümleyici başvurusu](https://www.npmjs.com/package/semver) SemVer sağlayan tam expressivity kılavuzu olarak.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-146">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="4b8d4-147">Yüklemek için daha fazla bağımlılıkları grunt Ekle-contrib -\* için paketler *temiz*, *jshint*, *concat*, *uglify*ve *watch* aşağıdaki örnekte gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-147">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="4b8d4-148">Sürümleri örnekle eşleşmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-148">The versions don't need to match the example.</span></span>

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

4. <span data-ttu-id="4b8d4-149">Kaydet *package.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-149">Save the *package.json* file.</span></span>

<span data-ttu-id="4b8d4-150">Her paket için gerekli tüm dosyaları ile birlikte her devDependencies öğesi paketleri indirir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-150">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="4b8d4-151">Paket dosyaları bulabilirsiniz `node_modules` etkinleştirerek dizin **tüm dosyaları göster** Çözüm Gezgini'nde düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-151">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="4b8d4-153">Gerek duyarsanız, el ile bağımlılıkları Çözüm Gezgini'nde sağ tıklayarak geri yükleyebilirsiniz `Dependencies\NPM` seçerek **paketleri geri** menü seçeneği.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-153">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![Paketleri geri yükle](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="4b8d4-155">Grunt yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4b8d4-155">Configuring Grunt</span></span>

<span data-ttu-id="4b8d4-156">Grunt adlı bir bildiri kullanarak yapılandırılmış *Gruntfile.js* tanımlar, yükler ve el ile çalıştırmak veya Visual Studio'da olaylara göre otomatik olarak çalıştırılmak için yapılandırılan görevleri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-156">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="4b8d4-157">Projeye sağ tıklayıp **Ekle > Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-157">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="4b8d4-158">Seçin **Grunt yapılandırma dosyası** seçeneği, varsayılan adı bırakın *Gruntfile.js*, tıklatıp **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-158">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

   <span data-ttu-id="4b8d4-159">Başlangıç kodunu bir modül tanımı içerir ve `grunt.initConfig()` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-159">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="4b8d4-160">`initConfig()` Her paket için seçenekleri ayarlamak için kullanılır ve modül geri kalanında yüklemek ve görevleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-160">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. <span data-ttu-id="4b8d4-161">İçinde `initConfig()` yöntemi eklemek için Seçenekler `clean` örnekte gösterildiği gibi görev *Gruntfile.js* aşağıda.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-161">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="4b8d4-162">Temizleme görevini, bir dizi dizin dizeleri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-162">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="4b8d4-163">Bu görev dosyaları wwwroot/lib ve tamamını/temp dizini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-163">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="4b8d4-164">İnitConfig() yöntemi bir çağrı ekleyin `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-164">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="4b8d4-165">Bu görev çalıştırılabilir Visual Studio'dan yapar.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-165">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="4b8d4-166">Kaydet *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-166">Save *Gruntfile.js*.</span></span> <span data-ttu-id="4b8d4-167">Dosya aşağıdaki ekran gibi görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-167">The file should look something like the screenshot below.</span></span>

    ![ilk gruntfile](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="4b8d4-169">Sağ *Gruntfile.js* seçip **görev Çalıştırıcı Gezgini** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-169">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="4b8d4-170">Görev Çalıştırıcı Gezgini penceresi açılır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-170">The Task Runner Explorer window will open.</span></span>

    ![Görev Çalıştırıcı Gezgini menüsü](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="4b8d4-172">Doğrulayın `clean` altında gösterir **görevleri** görev Çalıştırıcı Gezgini içinde.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-172">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![Görev Çalıştırıcı Gezgini görev listesi](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="4b8d4-174">Temizleme görevini sağ tıklayıp **çalıştırma** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-174">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="4b8d4-175">Bir komut penceresi, görevin ilerleme durumunu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-175">A command window displays progress of the task.</span></span>

    ![Görev Çalıştırıcı Gezgini çalıştırma temizleme görevini](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > <span data-ttu-id="4b8d4-177">Dosyaları veya dizinleri henüz temizlemek için yoktur.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-177">There are no files or directories to clean yet.</span></span> <span data-ttu-id="4b8d4-178">İsterseniz, bunları Çözüm Gezgini'nde el ile oluşturmanız ve ardından bir test olarak temizleme görevini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-178">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>

8. <span data-ttu-id="4b8d4-179">İnitConfig() yöntemi eklemek için bir giriş `concat` aşağıdaki kodu kullanarak.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-179">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="4b8d4-180">`src` Özellik dizisi, bunlar birleştirilmelidir, sırayla birleştirmek için dosyaları listeler.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-180">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="4b8d4-181">`dest` Özelliği üretilen birleşik bir dosyaya yol atar.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-181">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="4b8d4-182">`all` Yukarıdaki kod özelliğinde bir hedef adıdır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-182">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="4b8d4-183">Hedefleri, bazı Grunt görevleri birden çok yapı ortamları izin vermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-183">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="4b8d4-184">IntelliSense kullanarak yerleşik hedefleri görüntüle veya kendi atayın.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-184">You can view the built-in targets using Intellisense or assign your own.</span></span>

9. <span data-ttu-id="4b8d4-185">Ekleme `jshint` aşağıdaki kodu kullanarak görev.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-185">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="4b8d4-186">Geçici dizinde bulunan her bir JavaScript dosyası karşı jshint kod kalitesini yardımcı programını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-186">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="4b8d4-187">Seçenek "-W069" JavaScript kullanır, yani noktalı gösterim yerine bir özellik atamak için söz dizimi köşeli ayraç olduğunda bir hata jshint tarafından üretilen `Tastes["Sweet"]` yerine `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-187">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="4b8d4-188">Seçeneği, rest işleminin devam etmesine izin vermek için uyarı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-188">The option turns off the warning to allow the rest of the process to continue.</span></span>

10. <span data-ttu-id="4b8d4-189">Ekleme `uglify` aşağıdaki kodu kullanarak görev.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-189">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="4b8d4-190">Görev küçültür *combined.js* dosya geçici dizinde bulunan ve wwwroot / standart bir adlandırma kuralının LIB sonuç dosyası oluşturur  *\<dosya adı\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-190">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. <span data-ttu-id="4b8d4-191">Grunt contrib temiz yükleyen çağrı grunt.loadNpmTasks() altında aşağıdaki kodu kullanarak uglify ve jshint, concat aynı çağrısı içerir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-191">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="4b8d4-192">Kaydet *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-192">Save *Gruntfile.js*.</span></span> <span data-ttu-id="4b8d4-193">Dosya aşağıdaki örnek gibi görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-193">The file should look something like the example below.</span></span>

    ![tam grunt dosyası örneği](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="4b8d4-195">Görev Çalıştırıcı Gezgini Görevler listesini içeren bir bildirim `clean`, `concat`, `jshint` ve `uglify` görevleri.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-195">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="4b8d4-196">Her görev sıraya göre çalıştırın ve Çözüm Gezgini'nde sonuçlarını gözlemleyin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-196">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="4b8d4-197">Her görev hatasız çalışmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-197">Each task should run without errors.</span></span>

    ![Görev Çalıştırıcı Gezgini her görevi çalıştır](using-grunt/_static/task-runner-explorer-run-each-task.png)

    <span data-ttu-id="4b8d4-199">Concat görev yeni bir oluşturur *combined.js* dosya ve geçici dizine yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-199">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="4b8d4-200">Jshint görevi yalnızca çalışır ve çıktı oluşturmuyor.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-200">The jshint task simply runs and doesn't produce output.</span></span> <span data-ttu-id="4b8d4-201">Yeni bir uglify görev oluşturur *combined.min.js* dosya ve wwwroot/lib yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-201">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="4b8d4-202">Tamamlandığında, çözüm aşağıdaki ekran görüntüsüne benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="4b8d4-202">On completion, the solution should look something like the screenshot below:</span></span>

    ![Çözüm Gezgini tüm görevler](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > <span data-ttu-id="4b8d4-204">Her bir paket için seçenekleri hakkında daha fazla bilgi için ziyaret [ https://www.npmjs.com/ ](https://www.npmjs.com/) ve arama ana sayfada arama kutusuna paket adı.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-204">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="4b8d4-205">Örneğin, tüm parametrelerinin açıklayan bir belge bağlantısını almak için grunt contrib temiz paketi bakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-205">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="4b8d4-206">Özet</span><span class="sxs-lookup"><span data-stu-id="4b8d4-206">All together now</span></span>

<span data-ttu-id="4b8d4-207">Grunt kullanma `registerTask()` belirli bir sırayla bir dizi görevi çalıştırmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-207">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="4b8d4-208">Örneğin, yukarıdaki adımları sırayla temiz -> örneği çalıştırmak için concat -> jshint -> uglify, modüle aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-208">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="4b8d4-209">Kod initConfig dışında loadNpmTasks() çağrıları ile aynı düzeyde eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-209">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="4b8d4-210">Yeni görevin görev Çalıştırıcı Gezgini diğer ad görevleri altında görünür.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-210">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="4b8d4-211">Sağ tıklayın ve diğer görevler gibi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-211">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="4b8d4-212">`all` Görevin çalışacağını `clean`, `concat`, `jshint` ve `uglify`, sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-212">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![diğer ad grunt görevleri](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="4b8d4-214">Değişiklikleri izleniyor</span><span class="sxs-lookup"><span data-stu-id="4b8d4-214">Watching for changes</span></span>

<span data-ttu-id="4b8d4-215">A `watch` dosyaları ve dizinleri görev tutar.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-215">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="4b8d4-216">Değişiklik algılarsa izleme görevleri otomatik olarak tetikler.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-216">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="4b8d4-217">İnitConfig değişiklikleri izlemek için aşağıdaki kodu ekleyin \*TypeScript dizinde .js dosyası.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-217">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="4b8d4-218">Bir JavaScript dosyası değiştirilirse, `watch` çalışacak `all` görev.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-218">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="4b8d4-219">Bir çağrı ekleyin `loadNpmTasks()` gösterilecek `watch` görev Çalıştırıcı Gezgini görevi.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-219">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="4b8d4-220">Görev Çalıştırıcı Gezgini izleme görevi sağ tıklayın ve bağlam menüsünden Çalıştır'ı seçin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-220">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="4b8d4-221">İzleme görev çalıştırma gösteren komut penceresinde bir "bekleniyor..." görüntülenir İleti.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-221">The command window that shows the watch task running will display a "Waiting…" message.</span></span> <span data-ttu-id="4b8d4-222">TypeScript dosyaları birini açın, bir alan ekleyin ve ardından dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-222">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="4b8d4-223">Bu izleme tetikleyici ve sırayla çalışması diğer görevlerini tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-223">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="4b8d4-224">Aşağıdaki ekran görüntüsünde, bir örnek çalıştırmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-224">The screenshot below shows a sample run.</span></span>

![çalışan görevleri çıkış](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="4b8d4-226">Visual Studio olaylara bağlama</span><span class="sxs-lookup"><span data-stu-id="4b8d4-226">Binding to Visual Studio events</span></span>

<span data-ttu-id="4b8d4-227">Visual Studio'daki çalışmalar her zaman el ile görevlerinizi başlatmak istediğiniz sürece, görevler bağlayabilirsiniz **önce yapı**, **sonra yapı**, **temiz**, ve  **Proje Aç** olayları.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-227">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="4b8d4-228">Şimdi bağlama `watch` böylece Visual Studio her açtığında çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-228">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="4b8d4-229">Görev Çalıştırıcı Gezgini, izleme görev sağ tıklatın ve seçin **bağlamaları > Proje Aç** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-229">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![bir görev için proje açma bağlama](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="4b8d4-231">Kaldırın ve projeyi yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-231">Unload and reload the project.</span></span> <span data-ttu-id="4b8d4-232">Projeyi yeniden yüklediğinde, izleme görev otomatik olarak çalıştırarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-232">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="4b8d4-233">Özet</span><span class="sxs-lookup"><span data-stu-id="4b8d4-233">Summary</span></span>

<span data-ttu-id="4b8d4-234">Grunt, çoğu istemci derleme görevleri otomatik hale getirmek için kullanılan bir güçlü görev Çalıştırıcı ' dir.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-234">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="4b8d4-235">Grunt, NPM, paketleri ve araçları Visual Studio ile tümleştirme özelliklerini sunmak için yararlanır.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-235">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="4b8d4-236">Görev Çalıştırıcı Gezgini Visual Studio'nun, yapılandırma dosyalarındaki değişiklikleri algılar ve görevleri çalıştırmak, çalışmakta olan görevlerin görüntüleyin ve görevler için Visual Studio olaylar bağlamak için kullanışlı bir arabirim sağlar.</span><span class="sxs-lookup"><span data-stu-id="4b8d4-236">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>
