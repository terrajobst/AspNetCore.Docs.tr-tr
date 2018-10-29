---
title: Visual Studio'da ASP.NET Core ile LibMan kullanın
author: scottaddie
description: Visual Studio ile ASP.NET Core projesinde LibMan kullanmayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: 727bd80b7f37f6ebd9d37b7aab1aa6c33b85678c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206733"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="047ed-103">Visual Studio'da ASP.NET Core ile LibMan kullanın</span><span class="sxs-lookup"><span data-stu-id="047ed-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="047ed-104">Tarafından [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="047ed-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="047ed-105">Visual Studio için yerleşik desteği vardır [LibMan](xref:client-side/libman/index) dahil olmak üzere, ASP.NET Core projelerinde:</span><span class="sxs-lookup"><span data-stu-id="047ed-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="047ed-106">Yapılandırma ve yapı LibMan geri yükleme işlemleri çalıştırma desteği.</span><span class="sxs-lookup"><span data-stu-id="047ed-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="047ed-107">LibMan geri yükleme ve temizleme işlemlerini tetikleme menü öğeleri.</span><span class="sxs-lookup"><span data-stu-id="047ed-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="047ed-108">Kitaplıkları bulma ve dosyalar projeye ekleme için arama iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="047ed-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="047ed-109">Düzenleme için desteği *libman.json*&mdash;LibMan bildirim dosyası.</span><span class="sxs-lookup"><span data-stu-id="047ed-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="047ed-110">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(karşıdan yükleme)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="047ed-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="047ed-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="047ed-111">Prerequisites</span></span>

* <span data-ttu-id="047ed-112">Visual Studio 2017 sürüm 15,8 veya üstünü **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="047ed-112">Visual Studio 2017 version 15.8 or later with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="047ed-113">Kitaplık dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="047ed-113">Add library files</span></span>

<span data-ttu-id="047ed-114">Kitaplık dosyalarını bir ASP.NET Core projesi için iki farklı şekilde eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="047ed-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. [<span data-ttu-id="047ed-115">İstemci tarafı kitaplık Ekle iletişim kutusunu kullanın</span><span class="sxs-lookup"><span data-stu-id="047ed-115">Use the Add Client-Side Library dialog</span></span>](#use-the-add-client-side-library-dialog)
1. [<span data-ttu-id="047ed-116">LibMan bildirim dosyası girişlerini el ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="047ed-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="047ed-117">İstemci tarafı kitaplık Ekle iletişim kutusunu kullanın</span><span class="sxs-lookup"><span data-stu-id="047ed-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="047ed-118">Bir istemci-tarafı kitaplığını yüklemek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="047ed-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="047ed-119">İçinde **Çözüm Gezgini**, hangi dosyaların eklenmelidir proje klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="047ed-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="047ed-120">Seçin **ekleme** > **istemci tarafı kitaplık**.</span><span class="sxs-lookup"><span data-stu-id="047ed-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="047ed-121">**İstemci tarafı kitaplık Ekle** iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="047ed-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![İstemci tarafı kitaplık iletişim kutusu Ekle](_static/add-library-dialog.png)

* <span data-ttu-id="047ed-123">Kitaplık sağlayıcısından seçin **sağlayıcısı** açılır.</span><span class="sxs-lookup"><span data-stu-id="047ed-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="047ed-124">CDNJS varsayılan sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="047ed-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="047ed-125">İçinde getirilecek kitaplığı adı **Kitaplığı** metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="047ed-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="047ed-126">IntelliSense belirtilen metinle başlayan kitaplıkların bir listesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="047ed-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="047ed-127">IntelliSense listeden kitaplığı seçin.</span><span class="sxs-lookup"><span data-stu-id="047ed-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="047ed-128">Bildirimi kitaplık adı soneki ile `@` sembol ve seçili sağlayıcıya bilinen en son kararlı sürümü.</span><span class="sxs-lookup"><span data-stu-id="047ed-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="047ed-129">Dahil etmek için hangi dosyaların karar verin:</span><span class="sxs-lookup"><span data-stu-id="047ed-129">Decide which files to include:</span></span>
  * <span data-ttu-id="047ed-130">Seçin **tüm kitaplık dosyalarını dahil et** tüm kitaplık dosyalarını içerecek şekilde radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="047ed-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="047ed-131">Seçin **belirli dosyaları seçin** kitaplığın dosyaların bir alt kümesine eklenecek radyo düğmesi.</span><span class="sxs-lookup"><span data-stu-id="047ed-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="047ed-132">Dosya Seçici ağaç radyo düğmesi seçili olduğunda etkindir.</span><span class="sxs-lookup"><span data-stu-id="047ed-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="047ed-133">İndirmek için dosya adlarını sol tarafındaki kutuları işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="047ed-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="047ed-134">Dosyaları depolamak için proje klasörü belirtin **hedef konum** metin kutusu.</span><span class="sxs-lookup"><span data-stu-id="047ed-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="047ed-135">Bir öneri olarak, her kitaplık ayrı bir klasörde depolar.</span><span class="sxs-lookup"><span data-stu-id="047ed-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="047ed-136">Önerilen **hedef konum** klasör konumu iletişim kutusu, başlatılan dayanır:</span><span class="sxs-lookup"><span data-stu-id="047ed-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="047ed-137">Başlatılan proje kök ise:</span><span class="sxs-lookup"><span data-stu-id="047ed-137">If launched from the project root:</span></span>
    * <span data-ttu-id="047ed-138">*wwwroot/lib* kullanılır *wwwroot* bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="047ed-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="047ed-139">*LIB* kullanılır *wwwroot* yok.</span><span class="sxs-lookup"><span data-stu-id="047ed-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="047ed-140">Başlatılan proje klasöründen, karşılık gelen klasörü adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="047ed-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="047ed-141">Klasör öneri kitaplık adı ile soneki.</span><span class="sxs-lookup"><span data-stu-id="047ed-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="047ed-142">Aşağıdaki tabloda, jQuery Razor sayfaları projesinde yüklerken klasör öneriler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="047ed-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="047ed-143">Başlatma konumu</span><span class="sxs-lookup"><span data-stu-id="047ed-143">Launch location</span></span>                           |<span data-ttu-id="047ed-144">Önerilen klasörü</span><span class="sxs-lookup"><span data-stu-id="047ed-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="047ed-145">Proje kök dizinini (varsa *wwwroot* var)</span><span class="sxs-lookup"><span data-stu-id="047ed-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="047ed-146">*wwwroot/lib/jquery /*</span><span class="sxs-lookup"><span data-stu-id="047ed-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="047ed-147">Proje kök dizinini (varsa *wwwroot* yok)</span><span class="sxs-lookup"><span data-stu-id="047ed-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="047ed-148">*lib/jquery /*</span><span class="sxs-lookup"><span data-stu-id="047ed-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="047ed-149">*Sayfaları* proje klasöründe</span><span class="sxs-lookup"><span data-stu-id="047ed-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="047ed-150">*Sayfa/jquery /*</span><span class="sxs-lookup"><span data-stu-id="047ed-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="047ed-151">Tıklayın **yükleme** başına yapılandırma dosyalarını indirmek için düğmeye *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="047ed-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="047ed-152">Gözden geçirme **Kitaplık Yöneticisi** akışı **çıkış** yükleme Ayrıntıları penceresi.</span><span class="sxs-lookup"><span data-stu-id="047ed-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="047ed-153">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="047ed-153">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="047ed-154">LibMan bildirim dosyası girişlerini el ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="047ed-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="047ed-155">Proje kök 's LibMan bildiriminin içeriği üzerinde Visual Studio'da tüm LibMan işlemleri temel alır (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="047ed-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="047ed-156">El ile düzenleyebilirsiniz *libman.json* proje için kitaplık dosyalarını yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="047ed-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="047ed-157">Visual Studio tüm kitaplık dosyalarını bir kez yükler *libman.json* kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="047ed-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="047ed-158">Açmak için *libman.json* düzenlemek için aşağıdaki seçenekler mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="047ed-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="047ed-159">Çift *libman.json* dosyası **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="047ed-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="047ed-160">Projeye sağ **Çözüm Gezgini** seçip **istemci tarafı kitaplıkları yönetmek**.</span><span class="sxs-lookup"><span data-stu-id="047ed-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="047ed-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="047ed-161">**&#8224;**</span></span>
* <span data-ttu-id="047ed-162">Seçin **istemci tarafı kitaplıkları yönetmek** Visual Studio'dan **proje** menüsü.</span><span class="sxs-lookup"><span data-stu-id="047ed-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="047ed-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="047ed-163">**&#8224;**</span></span>

<span data-ttu-id="047ed-164">**&#8224;** Varsa *libman.json* dosya proje kök dizininde zaten yoksa, varsayılan öğe şablonu içeriği ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="047ed-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="047ed-165">Visual Studio düzenleme desteği renklendirme gibi biçimlendirme, IntelliSense ve şema doğrulama zengin JSON sunar.</span><span class="sxs-lookup"><span data-stu-id="047ed-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="047ed-166">LibMan bildirim JSON şeması bulunduğu [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).</span><span class="sxs-lookup"><span data-stu-id="047ed-166">The LibMan manifest's JSON schema is found at [http://json.schemastore.org/libman](http://json.schemastore.org/libman).</span></span>

<span data-ttu-id="047ed-167">Aşağıdaki bildirim dosyası ile tanımlanan yapılandırma başına LibMan alır `libraries` özelliği.</span><span class="sxs-lookup"><span data-stu-id="047ed-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="047ed-168">Bir açıklama içinde tanımlanan nesne değişmez değerlerinin `libraries` izler:</span><span class="sxs-lookup"><span data-stu-id="047ed-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="047ed-169">Bir alt kümesini [jQuery](https://jquery.com/) sürüm 3.3.1 CDNJS sağlayıcısından alınır.</span><span class="sxs-lookup"><span data-stu-id="047ed-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="047ed-170">Alt tanımlanan `files` özelliği&mdash;*jquery.min.js*, *jquery.js*, ve *jquery.min.map*.</span><span class="sxs-lookup"><span data-stu-id="047ed-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="047ed-171">Dosyalar projesinin yerleştirilir *wwwroot/lib/jquery* klasör.</span><span class="sxs-lookup"><span data-stu-id="047ed-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="047ed-172">Tamamen [önyükleme](https://getbootstrap.com/) sürüm 4.1.3 alınır ve yerleştirilmiş bir *wwwroot/lib/önyükleme* klasör.</span><span class="sxs-lookup"><span data-stu-id="047ed-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="047ed-173">Nesne sabit değeri `provider` özelliklerine `defaultProvider` özellik değeri.</span><span class="sxs-lookup"><span data-stu-id="047ed-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="047ed-174">LibMan unpkg sağlayıcısından önyükleme dosyaları alır.</span><span class="sxs-lookup"><span data-stu-id="047ed-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="047ed-175">Bir alt kümesini [Lodash](https://lodash.com/) unsurun kuruluştaki tarafından onaylandı.</span><span class="sxs-lookup"><span data-stu-id="047ed-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="047ed-176">*Lodash.js* ve *lodash.min.js* dosyaları yerel dosya sisteminden alınır *C:\\temp\\lodash\\*.</span><span class="sxs-lookup"><span data-stu-id="047ed-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\temp\\lodash\\*.</span></span> <span data-ttu-id="047ed-177">Dosyalar projenin kopyalanır *wwwroot/lib/lodash* klasör.</span><span class="sxs-lookup"><span data-stu-id="047ed-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="047ed-178">LibMan yalnızca her kitaplığının her sağlayıcısından bir sürümünü destekler.</span><span class="sxs-lookup"><span data-stu-id="047ed-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="047ed-179">*Libman.json* dosya aynı kitaplık adı verilen sağlayıcı için iki kitaplıklarıyla içeriyorsa, şema doğrulaması başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="047ed-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="047ed-180">Kitaplık dosyaları geri yükleme</span><span class="sxs-lookup"><span data-stu-id="047ed-180">Restore library files</span></span>

<span data-ttu-id="047ed-181">Visual Studio içinden kitaplık dosyaları geri yüklemek için olmalıdır geçerli bir *libman.json* proje kök dosyasında.</span><span class="sxs-lookup"><span data-stu-id="047ed-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="047ed-182">Geri yüklenen dosya projenin her kitaplık için belirtilen konuma yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="047ed-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="047ed-183">ASP.NET Core projesinde iki yolla kitaplık dosyalarını geri yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="047ed-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="047ed-184">Derleme sırasında dosyaları geri yükleme</span><span class="sxs-lookup"><span data-stu-id="047ed-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="047ed-185">Dosyaları el ile geri yükleme</span><span class="sxs-lookup"><span data-stu-id="047ed-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="047ed-186">Derleme sırasında dosyaları geri yükleme</span><span class="sxs-lookup"><span data-stu-id="047ed-186">Restore files during build</span></span>

<span data-ttu-id="047ed-187">LibMan yapı işleminin bir parçası olarak tanımlanan kitaplık dosyalarını geri yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="047ed-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="047ed-188">Varsayılan olarak, *derleme üzerinde geri yükleme* davranışı devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="047ed-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="047ed-189">Ve test derleme üzerinde geri yükleme davranışı etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="047ed-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="047ed-190">Sağ *libman.json* içinde **Çözüm Gezgini** seçip **etkinleştirme geri istemci-tarafı kitaplıklarını temel yapı** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="047ed-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="047ed-191">Tıklayın **Evet** NuGet paketini yüklemek isteyip istemediğiniz sorulduğunda düğmesi.</span><span class="sxs-lookup"><span data-stu-id="047ed-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="047ed-192">[Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet paketini projeye eklendi:</span><span class="sxs-lookup"><span data-stu-id="047ed-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="047ed-193">LibMan dosya geri yüklemesi gerçekleşir doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="047ed-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="047ed-194">`Microsoft.Web.LibraryManager.Build` Paket LibMan projenin derleme işlemi sırasında çalıştırılan bir MSBuild hedefi yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="047ed-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="047ed-195">Gözden geçirme **derleme** akışı **çıkış** LibMan etkinlik günlüğü penceresi:</span><span class="sxs-lookup"><span data-stu-id="047ed-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

<span data-ttu-id="047ed-196">Derleme üzerinde geri yükleme davranışı etkinleştirildiğinde *libman.json* bağlam menüsünü görüntüler bir **devre dışı bırakma geri istemci-tarafı kitaplıklarını temel yapı** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="047ed-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="047ed-197">Bu seçeneğin belirlenmesi kaldırır `Microsoft.Web.LibraryManager.Build` paket proje dosyasından başvuru.</span><span class="sxs-lookup"><span data-stu-id="047ed-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="047ed-198">Sonuç olarak, istemci tarafı kitaplıkları üzerinde her derleme artık geri yüklenir.</span><span class="sxs-lookup"><span data-stu-id="047ed-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="047ed-199">Derleme üzerinde geri yükleme ayar ne olursa olsun, el ile diledikleri zaman geri yükleyebilirsiniz *libman.json* bağlam menüsü.</span><span class="sxs-lookup"><span data-stu-id="047ed-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="047ed-200">Daha fazla bilgi için [dosyaları el ile geri](#restore-files-manually).</span><span class="sxs-lookup"><span data-stu-id="047ed-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="047ed-201">Dosyaları el ile geri yükleme</span><span class="sxs-lookup"><span data-stu-id="047ed-201">Restore files manually</span></span>

<span data-ttu-id="047ed-202">Kitaplık dosyaları el ile geri yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="047ed-202">To manually restore library files:</span></span>

* <span data-ttu-id="047ed-203">Çözümdeki tüm projeleri için:</span><span class="sxs-lookup"><span data-stu-id="047ed-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="047ed-204">İçinde çözüm adına sağ tıklayın **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="047ed-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="047ed-205">Seçin **geri istemci tarafı kitaplıkları** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="047ed-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="047ed-206">Belirli bir proje için:</span><span class="sxs-lookup"><span data-stu-id="047ed-206">For a specific project:</span></span>
  * <span data-ttu-id="047ed-207">Sağ *libman.json* dosyası **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="047ed-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="047ed-208">Seçin **geri istemci tarafı kitaplıkları** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="047ed-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="047ed-209">Geri yükleme işlemi devam ederken:</span><span class="sxs-lookup"><span data-stu-id="047ed-209">While the restore operation is running:</span></span>

* <span data-ttu-id="047ed-210">Görev durumu Merkezi (TSC) simgesi Visual Studio durum çubuğunda animasyon uygulanır ve okuyacaksa *geri yükleme işlemi başladı*.</span><span class="sxs-lookup"><span data-stu-id="047ed-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="047ed-211">Simgesine tıklayarak, bilinen arka plan görevleri listeleme bir araç ipucu açılır.</span><span class="sxs-lookup"><span data-stu-id="047ed-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="047ed-212">Durum çubuğundaki ileti gönderilecek ve **Kitaplık Yöneticisi** akışı **çıkış** penceresi.</span><span class="sxs-lookup"><span data-stu-id="047ed-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="047ed-213">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="047ed-213">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a><span data-ttu-id="047ed-214">Kitaplık dosyaları sil</span><span class="sxs-lookup"><span data-stu-id="047ed-214">Delete library files</span></span>

<span data-ttu-id="047ed-215">Gerçekleştirilecek *temiz* Visual Studio'da daha önce geri kitaplık dosyaları silen işlemi:</span><span class="sxs-lookup"><span data-stu-id="047ed-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="047ed-216">Sağ *libman.json* dosyası **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="047ed-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="047ed-217">Seçin **temiz istemci tarafı kitaplıkları** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="047ed-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="047ed-218">Kitaplık olmayan dosyaları kaldırılmamıştır önlemek için temizleme işlemi tüm dizinleri silmez.</span><span class="sxs-lookup"><span data-stu-id="047ed-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="047ed-219">Yalnızca önceki geri yükleme dahil edilen dosyalar da kaldırır.</span><span class="sxs-lookup"><span data-stu-id="047ed-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="047ed-220">Temizleme işlemi devam ederken:</span><span class="sxs-lookup"><span data-stu-id="047ed-220">While the clean operation is running:</span></span>

* <span data-ttu-id="047ed-221">Visual Studio durum çubuğunda TSC simge animasyon uygulanır ve okuyacaksa *istemci kitaplıkları işlemi başlatıldı*.</span><span class="sxs-lookup"><span data-stu-id="047ed-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="047ed-222">Simgesine tıklayarak, bilinen arka plan görevleri listeleme bir araç ipucu açılır.</span><span class="sxs-lookup"><span data-stu-id="047ed-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="047ed-223">İletileri için durum çubuğu gönderilir ve **Kitaplık Yöneticisi** akışı **çıkış** penceresi.</span><span class="sxs-lookup"><span data-stu-id="047ed-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="047ed-224">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="047ed-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="047ed-225">Temizleme işlemi, yalnızca dosyaları projeden siler.</span><span class="sxs-lookup"><span data-stu-id="047ed-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="047ed-226">Kitaplık dosyaları için daha hızlı alma gelecekteki geri yükleme işlemleri üzerindeki önbellekte kalır.</span><span class="sxs-lookup"><span data-stu-id="047ed-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="047ed-227">Yerel makinenin önbelleğinde depolanan kitaplık dosyalarını yönetmek için kullandığınız [LibMan CLI](xref:client-side/libman/libman-cli).</span><span class="sxs-lookup"><span data-stu-id="047ed-227">To manage library files stored in the local machine's cache, use the [LibMan CLI](xref:client-side/libman/libman-cli).</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="047ed-228">Kitaplık dosyaları kaldırma</span><span class="sxs-lookup"><span data-stu-id="047ed-228">Uninstall library files</span></span>

<span data-ttu-id="047ed-229">Kitaplık dosyaları kaldırmak için:</span><span class="sxs-lookup"><span data-stu-id="047ed-229">To uninstall library files:</span></span>

* <span data-ttu-id="047ed-230">Açık *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="047ed-230">Open *libman.json*.</span></span>
* <span data-ttu-id="047ed-231">Giriş işaretini içinde karşılık gelen konum `libraries` nesne sabit değeri.</span><span class="sxs-lookup"><span data-stu-id="047ed-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="047ed-232">Sol kenar boşluğunda görünür ampul simgesini tıklatın ve seçin **kaldırma \<library_name > @\<library_version >**:</span><span class="sxs-lookup"><span data-stu-id="047ed-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![Kitaplık bağlam menüsü seçeneği kaldırın](_static/uninstall-menu-option.png)

<span data-ttu-id="047ed-234">Alternatif olarak, el ile düzenleyebilir ve LibMan bildirimi kaydetmek (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="047ed-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="047ed-235">[Geri yükleme işlemi](#restore-library-files) dosyası kaydedildiğinde çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="047ed-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="047ed-236">Artık tanımlanan kitaplık dosyalarını *libman.json* projeden kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="047ed-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="047ed-237">Kitaplık sürümünü güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="047ed-237">Update library version</span></span>

<span data-ttu-id="047ed-238">Bir güncelleştirilen kitaplığı sürümünü denetlemek için:</span><span class="sxs-lookup"><span data-stu-id="047ed-238">To check for an updated library version:</span></span>

* <span data-ttu-id="047ed-239">Açık *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="047ed-239">Open *libman.json*.</span></span>
* <span data-ttu-id="047ed-240">Giriş işaretini içinde karşılık gelen konum `libraries` nesne sabit değeri.</span><span class="sxs-lookup"><span data-stu-id="047ed-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="047ed-241">Sol kenar boşluğunda görünür ampul simgesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="047ed-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="047ed-242">Üzerine **Güncelleştirmeleri denetle**.</span><span class="sxs-lookup"><span data-stu-id="047ed-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="047ed-243">LibMan yüklü olan sürümden daha yeni bir kitaplık sürümü olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="047ed-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="047ed-244">Aşağıdaki sonuçlar ortaya çıkabilir:</span><span class="sxs-lookup"><span data-stu-id="047ed-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="047ed-245">A **güncelleştirme bulunamadı** en son sürümü zaten yüklü değilse, ileti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="047ed-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="047ed-246">En son kararlı sürüm görüntülenme olmasa bile zaten yüklü.</span><span class="sxs-lookup"><span data-stu-id="047ed-246">The latest stable version is displayed if not already installed.</span></span>

  ![Bağlam menüsü seçeneği Güncelleştirmeleri denetle](_static/update-menu-option.png)

* <span data-ttu-id="047ed-248">Yüklü olan sürümden daha yeni bir ön sürüm varsa, ön sürümü görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="047ed-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="047ed-249">Eski bir kitaplığı sürümünün indirgemek için el ile düzenlemeniz *libman.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="047ed-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="047ed-250">Dosya kaydedildiğinde, LibMan [geri yükleme işlemi](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="047ed-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="047ed-251">Gereksiz dosyaları önceki sürümden kaldırır.</span><span class="sxs-lookup"><span data-stu-id="047ed-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="047ed-252">Yeni ve güncelleştirilmiş dosyaları yeni sürümden ekler.</span><span class="sxs-lookup"><span data-stu-id="047ed-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="047ed-253">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="047ed-253">Additional resources</span></span>

* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="047ed-254">LibMan GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="047ed-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
