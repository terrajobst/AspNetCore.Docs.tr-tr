---
title: ASP.NET core'da Bower ile istemci tarafı paketleri yönetme
author: rick-anderson
description: Bower ile istemci tarafı paketleri yönetme.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 08e6daa537c6c6f92a1cf80d70745e8ef606f580
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665619"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a><span data-ttu-id="bc445-103">ASP.NET core'da Bower ile istemci tarafı paketleri yönetme</span><span class="sxs-lookup"><span data-stu-id="bc445-103">Manage client-side packages with Bower in ASP.NET Core</span></span>

<span data-ttu-id="bc445-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel pirinç](https://twitter.com/noelrice1), ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="bc445-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel Rice](https://twitter.com/noelrice1), and [Scott Addie](https://scottaddie.com)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bc445-105">Bower korunur, ancak farklı bir çözüm kullanarak kendi maintainers önerilir.</span><span class="sxs-lookup"><span data-stu-id="bc445-105">While Bower is maintained, its maintainers recommend using a different solution.</span></span> <span data-ttu-id="bc445-106">[Kitaplık Yöneticisi](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (kısaca LibMan), Visual Studio'nun yeni istemci tarafı kitaplık edinme Aracı (Visual Studio 15,8 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="bc445-106">[Library Manager](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan for short) is Visual Studio's new client-side library acquisition tool (Visual Studio 15.8 or later).</span></span> <span data-ttu-id="bc445-107">Daha fazla bilgi için bkz. <xref:client-side/libman/index>.</span><span class="sxs-lookup"><span data-stu-id="bc445-107">For more information, see <xref:client-side/libman/index>.</span></span> <span data-ttu-id="bc445-108">Bower Visual Studio'da sürüm 15.5 desteklenir.</span><span class="sxs-lookup"><span data-stu-id="bc445-108">Bower is supported in Visual Studio through version 15.5.</span></span>
>
> <span data-ttu-id="bc445-109">Yarn Web ile olan popüler bir alternatif, için [geçiş yönergeleri](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc445-109">Yarn with Webpack is one popular alternative for which [migration instructions](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) are available.</span></span>

<span data-ttu-id="bc445-110">[Bower](https://bower.io/) kendisini "Web için bir paket Yöneticisi" çağırır.</span><span class="sxs-lookup"><span data-stu-id="bc445-110">[Bower](https://bower.io/) calls itself "A package manager for the web".</span></span> <span data-ttu-id="bc445-111">.NET ekosistemlerinde statik içerik dosyalarını NuGet verilmemesine göre sola void doldurur.</span><span class="sxs-lookup"><span data-stu-id="bc445-111">Within the .NET ecosystem, it fills the void left by NuGet's inability to deliver static content files.</span></span> <span data-ttu-id="bc445-112">ASP.NET Core projeleri için bu statik dosyalar gibi istemci tarafı kitaplıkları için devralınmış [jQuery](http://jquery.com/) ve [önyükleme](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="bc445-112">For ASP.NET Core projects, these static files are inherent to client-side libraries like [jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/).</span></span> <span data-ttu-id="bc445-113">.NET kitaplıkları için kullanmaya devam [NuGet](https://www.nuget.org/) Paket Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="bc445-113">For .NET libraries, you still use [NuGet](https://www.nuget.org/) package manager.</span></span>

<span data-ttu-id="bc445-114">İşlem istemci tarafında ayarlamak ASP.NET Core proje şablonları ile oluşturulan yeni projeler oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bc445-114">New projects created with the ASP.NET Core project templates set up the client-side build process.</span></span> <span data-ttu-id="bc445-115">[jQuery](http://jquery.com/) ve [önyükleme](http://getbootstrap.com/) yüklü olan ve Bower desteklenir.</span><span class="sxs-lookup"><span data-stu-id="bc445-115">[jQuery](http://jquery.com/) and [Bootstrap](http://getbootstrap.com/) are installed, and Bower is supported.</span></span>

<span data-ttu-id="bc445-116">İstemci tarafı paketleri listelenir *bower.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="bc445-116">Client-side packages are listed in the *bower.json* file.</span></span> <span data-ttu-id="bc445-117">ASP.NET Core proje şablonları yapılandırır *bower.json* jQuery, jQuery doğrulama ve önyükleme.</span><span class="sxs-lookup"><span data-stu-id="bc445-117">The ASP.NET Core project templates configures *bower.json* with jQuery, jQuery validation, and Bootstrap.</span></span>

<span data-ttu-id="bc445-118">Bu öğreticide, için destek ekleyeceğiz [Font Awesome](http://fontawesome.io).</span><span class="sxs-lookup"><span data-stu-id="bc445-118">In this tutorial, we'll add support for [Font Awesome](http://fontawesome.io).</span></span> <span data-ttu-id="bc445-119">Bower paketlerini ile yüklenebilir **Bower paketlerini Yönet** kullanıcı Arabirimi veya el ile *bower.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="bc445-119">Bower packages can be installed with the **Manage Bower Packages** UI or manually in the *bower.json* file.</span></span>

### <a name="installation-via-manage-bower-packages-ui"></a><span data-ttu-id="bc445-120">Yönet Bower paketlerini kullanıcı Arabirimi aracılığıyla yükleme</span><span class="sxs-lookup"><span data-stu-id="bc445-120">Installation via Manage Bower Packages UI</span></span>

* <span data-ttu-id="bc445-121">İle yeni bir ASP.NET Core Web uygulaması oluşturma **ASP.NET Core Web uygulaması (.NET Core)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="bc445-121">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="bc445-122">Seçin **Web uygulaması** ve **kimlik doğrulamasız**.</span><span class="sxs-lookup"><span data-stu-id="bc445-122">Select **Web Application** and **No Authentication**.</span></span>

* <span data-ttu-id="bc445-123">Çözüm Gezgini'nde projeye sağ tıklayıp **Bower paketlerini Yönet** (Alternatif olarak ana menüden **proje** > **Bower paketlerini Yönet**).</span><span class="sxs-lookup"><span data-stu-id="bc445-123">Right-click the project in Solution Explorer and select **Manage Bower Packages** (alternatively from the main menu, **Project** > **Manage Bower Packages**).</span></span>

* <span data-ttu-id="bc445-124">İçinde **Bower: \<proje adı\>**  penceresinde "Gözatma türü" sekmesine tıklayın ve ardından girerek paketleri listeyi filtreleyin `font-awesome` arama kutusuna:</span><span class="sxs-lookup"><span data-stu-id="bc445-124">In the **Bower: \<project name\>** window, click the "Browse" tab, and then filter the packages list by entering `font-awesome` in the search box:</span></span>

  ![bower paketlerini Yönet](bower/_static/manage-bower-packages.png)

* <span data-ttu-id="bc445-126">Onaylayın "değişiklikleri kaydedilsin *bower.json*" onay kutusunun işaretli.</span><span class="sxs-lookup"><span data-stu-id="bc445-126">Confirm that the "Save changes to *bower.json*" check box is checked.</span></span> <span data-ttu-id="bc445-127">Aşağı açılan listeden bir sürüm seçin ve tıklayın **yükleme** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="bc445-127">Select a version from the drop-down list and click the **Install** button.</span></span> <span data-ttu-id="bc445-128">**Çıkış** penceresi, yükleme ayrıntılarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc445-128">The **Output** window shows the installation details.</span></span>

### <a name="manual-installation-in-bowerjson"></a><span data-ttu-id="bc445-129">Bower.json el ile yükleme</span><span class="sxs-lookup"><span data-stu-id="bc445-129">Manual installation in bower.json</span></span>

<span data-ttu-id="bc445-130">Açık *bower.json* dosya ve "font awesome" bağımlılıklarına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bc445-130">Open the *bower.json* file and add "font-awesome" to the dependencies.</span></span> <span data-ttu-id="bc445-131">IntelliSense, kullanılabilir paketler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bc445-131">IntelliSense shows the available packages.</span></span> <span data-ttu-id="bc445-132">Kullanılabilir sürümler, bir paket seçtiğinizde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="bc445-132">When a package is selected, the available versions are displayed.</span></span> <span data-ttu-id="bc445-133">Aşağıdaki görüntülerin, eski ve gördüğünüz eşleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="bc445-133">The images below are older and won't match what you see.</span></span>

![IntelliSense bower paket Gezgini](bower/_static/add-package.png)

![bower version IntelliSense](bower/_static/version-intelliSense.png)

<span data-ttu-id="bc445-136">Bower kullanan [semantic versioning](http://semver.org/) bağımlılıkları düzenlemek için.</span><span class="sxs-lookup"><span data-stu-id="bc445-136">Bower uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="bc445-137">SemVer olarak da bilinen semantik sürüm oluşturma, numaralandırma düzeni ile paketleri tanımlayan \<ana >.\< küçük >. \<düzeltme eki >.</span><span class="sxs-lookup"><span data-stu-id="bc445-137">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="bc445-138">IntelliSense, yalnızca birkaç genel seçenek göstererek semantic versioning basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="bc445-138">IntelliSense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="bc445-139">Üst öğeyi IntelliSense listesinde (yukarıdaki örnekte 4.6.3) paketin en son kararlı sürüm olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="bc445-139">The top item in the IntelliSense list (4.6.3 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="bc445-140">Şapka (^) simgesi en son sürümle eşleşen ve son ikincil sürümle tilde (~) ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="bc445-140">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span>

<span data-ttu-id="bc445-141">Kaydet *bower.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="bc445-141">Save the *bower.json* file.</span></span> <span data-ttu-id="bc445-142">Visual Studio izleyen *bower.json* dosyasındaki değişiklikleri.</span><span class="sxs-lookup"><span data-stu-id="bc445-142">Visual Studio watches the *bower.json* file for changes.</span></span> <span data-ttu-id="bc445-143">Kaydedilmesi, *bower yükleme* komutu yürütülür.</span><span class="sxs-lookup"><span data-stu-id="bc445-143">Upon saving, the *bower install* command is executed.</span></span> <span data-ttu-id="bc445-144">Çıkış pencere bkz **Bower/npm** görünümü için tam komut yürütüldü.</span><span class="sxs-lookup"><span data-stu-id="bc445-144">See the Output window's **Bower/npm** view for the exact command executed.</span></span>

<span data-ttu-id="bc445-145">Açık *.bowerrc* altında dosya *bower.json*.</span><span class="sxs-lookup"><span data-stu-id="bc445-145">Open the *.bowerrc* file under *bower.json*.</span></span> <span data-ttu-id="bc445-146">`directory` Özelliği *wwwroot/lib* Bower konumu belirten paket varlıkları yükler.</span><span class="sxs-lookup"><span data-stu-id="bc445-146">The `directory` property is set to *wwwroot/lib* which indicates the location Bower will install the package assets.</span></span>

```json
{
 "directory": "wwwroot/lib"
}
```

<span data-ttu-id="bc445-147">Çözüm Gezgini arama kutusuna bulmak ve font awesome paket görüntülemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc445-147">You can use the search box in Solution Explorer to find and display the font-awesome package.</span></span>

<span data-ttu-id="bc445-148">Açık *görünümler/paylaşılan\_Layout.cshtml* dosya ve font awesome CSS dosyası ortama ekleyin [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro) için `Development`.</span><span class="sxs-lookup"><span data-stu-id="bc445-148">Open the *Views\Shared\_Layout.cshtml* file and add the font-awesome CSS file to the environment [Tag Helper](xref:mvc/views/tag-helpers/intro) for `Development`.</span></span> <span data-ttu-id="bc445-149">Çözüm Gezgini'nden sürükle ve bırak *yazı tipi awesome.css* içinde `<environment names="Development">` öğesi.</span><span class="sxs-lookup"><span data-stu-id="bc445-149">From Solution Explorer, drag and drop *font-awesome.css* inside the `<environment names="Development">` element.</span></span>

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

<span data-ttu-id="bc445-150">Bir üretim uygulamasında, eklediğiniz *yazı tipi awesome.min.css* için ortam etiketi Yardımcısı için `Staging,Production`.</span><span class="sxs-lookup"><span data-stu-id="bc445-150">In a production app you would add *font-awesome.min.css* to the environment tag helper for `Staging,Production`.</span></span>

<span data-ttu-id="bc445-151">Öğesinin içeriğini değiştirin *Views\Home\About.cshtml* aşağıdaki işaretlemeyle Razor dosyası:</span><span class="sxs-lookup"><span data-stu-id="bc445-151">Replace the contents of the *Views\Home\About.cshtml* Razor file with the following markup:</span></span>

[!code-html[](bower/sample/About.cshtml)]

<span data-ttu-id="bc445-152">Uygulamayı çalıştırın ve font awesome paket çalıştığını doğrulamak için hakkında görünümüne gidin.</span><span class="sxs-lookup"><span data-stu-id="bc445-152">Run the app and navigate to the About view to verify the font-awesome package works.</span></span>

## <a name="exploring-the-client-side-build-process"></a><span data-ttu-id="bc445-153">İstemci tarafı derleme işlemi keşfetme</span><span class="sxs-lookup"><span data-stu-id="bc445-153">Exploring the client-side build process</span></span>

<span data-ttu-id="bc445-154">Çoğu ASP.NET Core proje şablonları, Bower kullanmak için önceden yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="bc445-154">Most ASP.NET Core project templates are already configured to use Bower.</span></span> <span data-ttu-id="bc445-155">Bu sonraki izlenecek yolda boş bir ASP.NET Core projesi ile başlar ve Bower projede nasıl kullanıldığını için bir genel görünüm alabilmeniz için her parçayı el ile ekler.</span><span class="sxs-lookup"><span data-stu-id="bc445-155">This next walkthrough starts with an empty ASP.NET Core project and adds each piece manually, so you can get a feel for how Bower is used in a project.</span></span> <span data-ttu-id="bc445-156">Proje yapısını ve her bir yapılandırma değişikliği yapılmış gibi çıktı çalışma zamanı için ne olacağını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bc445-156">You can see what happens to the project structure and the runtime output as each configuration change is made.</span></span>

<span data-ttu-id="bc445-157">Bower ile istemci tarafı derleme işlemi kullanılacak genel adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="bc445-157">The general steps to use the client-side build process with Bower are:</span></span>

* <span data-ttu-id="bc445-158">Projenizde kullanılan paketler tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="bc445-158">Define packages used in your project.</span></span> <!-- once defined, you don't need to download them, VS does -->
* <span data-ttu-id="bc445-159">Web sayfalarınıza paketlerden başvuru.</span><span class="sxs-lookup"><span data-stu-id="bc445-159">Reference packages from your web pages.</span></span>

### <a name="define-packages"></a><span data-ttu-id="bc445-160">Paket tanımlama</span><span class="sxs-lookup"><span data-stu-id="bc445-160">Define packages</span></span>

<span data-ttu-id="bc445-161">Paketleri listesi sonra *bower.json* dosya, Visual Studio bunları indirir.</span><span class="sxs-lookup"><span data-stu-id="bc445-161">Once you list packages in the *bower.json* file, Visual Studio will download them.</span></span> <span data-ttu-id="bc445-162">Aşağıdaki örnek, jQuery ve Bootstrap için yüklenecek Bower kullanır. *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="bc445-162">The following example uses Bower to load jQuery and Bootstrap to the *wwwroot* folder.</span></span>

* <span data-ttu-id="bc445-163">İle yeni bir ASP.NET Core Web uygulaması oluşturma **ASP.NET Core Web uygulaması (.NET Core)** şablonu.</span><span class="sxs-lookup"><span data-stu-id="bc445-163">Create a new ASP.NET Core Web app with the **ASP.NET Core Web Application (.NET Core)** template.</span></span> <span data-ttu-id="bc445-164">Seçin **boş** proje şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="bc445-164">Select the **Empty** project template and click **OK**.</span></span>

* <span data-ttu-id="bc445-165">Çözüm Gezgini'nde projeye sağ tıklayın > **Yeni Öğe Ekle** seçip **Bower yapılandırma dosyası**.</span><span class="sxs-lookup"><span data-stu-id="bc445-165">In Solution Explorer, right-click the project > **Add New Item** and select **Bower Configuration File**.</span></span> <span data-ttu-id="bc445-166">Not: A *.bowerrc* dosya da eklenir.</span><span class="sxs-lookup"><span data-stu-id="bc445-166">Note: A *.bowerrc* file is also added.</span></span>

* <span data-ttu-id="bc445-167">Açık *bower.json*, jquery ekleyin ve için önyükleme `dependencies` bölümü.</span><span class="sxs-lookup"><span data-stu-id="bc445-167">Open *bower.json*, and add jquery and bootstrap to the `dependencies` section.</span></span> <span data-ttu-id="bc445-168">Ortaya çıkan *bower.json* dosya, aşağıdaki örnekteki gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="bc445-168">The resulting *bower.json* file will look like the following example.</span></span> <span data-ttu-id="bc445-169">Sürümler, zaman içinde değişir ve aşağıdaki resimde eşleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="bc445-169">The versions will change over time and may not match the image below.</span></span>

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* <span data-ttu-id="bc445-170">Kaydet *bower.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="bc445-170">Save the *bower.json* file.</span></span>

  <span data-ttu-id="bc445-171">Proje içerdiğini doğrulayın *önyükleme* ve *jQuery* dizinlerde *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="bc445-171">Verify the project includes the *bootstrap* and *jQuery* directories in *wwwroot/lib*.</span></span> <span data-ttu-id="bc445-172">Bower kullanan *.bowerrc* varlıkları yüklemek için dosya *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="bc445-172">Bower uses the *.bowerrc* file to install the assets in *wwwroot/lib*.</span></span>

  <span data-ttu-id="bc445-173">Not: El ile Dosya düzenlemeye alternatif "Bower paketlerini Yönet" kullanıcı Arabirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc445-173">Note: The "Manage Bower Packages" UI provides an alternative to manual file editing.</span></span>

### <a name="enable-static-files"></a><span data-ttu-id="bc445-174">Statik dosyaları etkinleştir</span><span class="sxs-lookup"><span data-stu-id="bc445-174">Enable static files</span></span>

* <span data-ttu-id="bc445-175">Ekleme `Microsoft.AspNetCore.StaticFiles` NuGet paketini projeye.</span><span class="sxs-lookup"><span data-stu-id="bc445-175">Add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project.</span></span>
* <span data-ttu-id="bc445-176">İle sunulan statik dosyaların [statik dosya ara yazılımlarını](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span><span class="sxs-lookup"><span data-stu-id="bc445-176">Enable static files to be served with the [Static file middleware](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions).</span></span> <span data-ttu-id="bc445-177">Bir çağrı ekleyin [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) için `Configure` yöntemi `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bc445-177">Add a call to [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) to the `Configure` method of `Startup`.</span></span>

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a><span data-ttu-id="bc445-178">Referans paketleri</span><span class="sxs-lookup"><span data-stu-id="bc445-178">Reference packages</span></span>

<span data-ttu-id="bc445-179">Bu bölümde, dağıtılan paketler erişebileceğini doğrulamak için bir HTML sayfası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bc445-179">In this section, you will create an HTML page to verify it can access the deployed packages.</span></span>

* <span data-ttu-id="bc445-180">Adlı yeni bir HTML sayfası Ekle *Index.html* için *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="bc445-180">Add a new HTML page named *Index.html* to the *wwwroot* folder.</span></span> <span data-ttu-id="bc445-181">Not: HTML dosyasına eklemelisiniz *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="bc445-181">Note: You must add the HTML file to the *wwwroot* folder.</span></span> <span data-ttu-id="bc445-182">Varsayılan olarak, statik içerik dışında sunulamayan *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="bc445-182">By default, static content cannot be served outside *wwwroot*.</span></span> <span data-ttu-id="bc445-183">Bkz: [statik dosyalar](xref:fundamentals/static-files) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="bc445-183">See [Static files](xref:fundamentals/static-files) for more information.</span></span>

  <span data-ttu-id="bc445-184">Öğesinin içeriğini değiştirin *Index.html* aşağıdaki işaretlemeyle:</span><span class="sxs-lookup"><span data-stu-id="bc445-184">Replace the contents of *Index.html* with the following markup:</span></span>

[!code-html[](bower/sample/Index.html)]

* <span data-ttu-id="bc445-185">Uygulamayı çalıştırın ve gidin `http://localhost:<port>/Index.html`.</span><span class="sxs-lookup"><span data-stu-id="bc445-185">Run the app and navigate to `http://localhost:<port>/Index.html`.</span></span> <span data-ttu-id="bc445-186">Alternatif olarak, ile *Index.html* açıldı, basın `Ctrl+Shift+W`.</span><span class="sxs-lookup"><span data-stu-id="bc445-186">Alternatively, with *Index.html* opened, press `Ctrl+Shift+W`.</span></span> <span data-ttu-id="bc445-187">Önyükleme düğmenin durumu değişir jumbotron stil uygulanır ve düğmeye tıklandığında, jQuery kodunun yanıt doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="bc445-187">Verify that the jumbotron styling is applied, the jQuery code responds when the button is clicked, and that the Bootstrap button changes state.</span></span>

  ![jumbotron stili](bower/_static/jumbotron.png)
