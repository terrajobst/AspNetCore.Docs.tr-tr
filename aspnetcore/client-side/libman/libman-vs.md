---
title: Visual Studio 'da ASP.NET Core LibMan kullanma
author: scottaddie
description: Visual Studio ile ASP.NET Core projesindeki LibMan 'ı nasıl kullanacağınızı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: e92e6bc28ec58b26785dd6c79e71512368202a26
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658314"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="70d85-103">Visual Studio 'da ASP.NET Core LibMan kullanma</span><span class="sxs-lookup"><span data-stu-id="70d85-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="70d85-104">[Scott Ade](https://twitter.com/Scott_Addie) tarafından</span><span class="sxs-lookup"><span data-stu-id="70d85-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="70d85-105">Visual Studio, aşağıdakiler dahil olmak üzere ASP.NET Core projelerinde [Libman](xref:client-side/libman/index) için yerleşik desteğe sahiptir:</span><span class="sxs-lookup"><span data-stu-id="70d85-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="70d85-106">Derlemede LibMan geri yükleme işlemlerini yapılandırma ve çalıştırma desteği.</span><span class="sxs-lookup"><span data-stu-id="70d85-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="70d85-107">LibMan geri yükleme ve temizleme işlemlerini tetikleyen menü öğeleri.</span><span class="sxs-lookup"><span data-stu-id="70d85-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="70d85-108">Kitaplıkları bulmak ve dosyaları bir projeye eklemek için arama iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="70d85-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="70d85-109">Libman *. JSON* için Düzen desteğini Libman bildirim dosyası&mdash;.</span><span class="sxs-lookup"><span data-stu-id="70d85-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="70d85-110">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(nasıl indirileceği)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="70d85-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70d85-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="70d85-111">Prerequisites</span></span>

* <span data-ttu-id="70d85-112">**ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="70d85-112">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="70d85-113">Kitaplık dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="70d85-113">Add library files</span></span>

<span data-ttu-id="70d85-114">Kitaplık dosyaları, ASP.NET Core projesine iki farklı şekilde eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="70d85-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. [<span data-ttu-id="70d85-115">Istemci tarafı kitaplığı Ekle iletişim kutusunu kullanın</span><span class="sxs-lookup"><span data-stu-id="70d85-115">Use the Add Client-Side Library dialog</span></span>](#use-the-add-client-side-library-dialog)
1. [<span data-ttu-id="70d85-116">LibMan bildirim dosyası girişlerini el ile yapılandır</span><span class="sxs-lookup"><span data-stu-id="70d85-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="70d85-117">Istemci tarafı kitaplığı Ekle iletişim kutusunu kullanın</span><span class="sxs-lookup"><span data-stu-id="70d85-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="70d85-118">İstemci tarafı kitaplığı yüklemek için şu adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="70d85-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="70d85-119">**Çözüm Gezgini**, dosyaların eklenmesi gereken proje klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="70d85-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="70d85-120"> > **Istemci tarafı kitaplığı** **Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="70d85-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="70d85-121">**Istemci tarafı kitaplığı Ekle** iletişim kutusu görünür:</span><span class="sxs-lookup"><span data-stu-id="70d85-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![Istemci tarafı kitaplığı Ekle iletişim kutusu](_static/add-library-dialog.png)

* <span data-ttu-id="70d85-123">**Sağlayıcı** açılan listesinden kitaplık sağlayıcısını seçin.</span><span class="sxs-lookup"><span data-stu-id="70d85-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="70d85-124">CDNJS varsayılan sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="70d85-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="70d85-125">**Kitaplık** metin kutusuna getirilecek kitaplık adını yazın.</span><span class="sxs-lookup"><span data-stu-id="70d85-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="70d85-126">IntelliSense, sağlanan metinle başlayan kitaplıkların bir listesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="70d85-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="70d85-127">IntelliSense listesinden kitaplığı seçin.</span><span class="sxs-lookup"><span data-stu-id="70d85-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="70d85-128">Kitaplık adının `@` simgesiyle ve seçili sağlayıcının bilinen en son kararlı sürümüyle sonekli olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="70d85-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="70d85-129">Hangi dosyaları içerecek olduğuna karar verin:</span><span class="sxs-lookup"><span data-stu-id="70d85-129">Decide which files to include:</span></span>
  * <span data-ttu-id="70d85-130">Tüm kitaplık dosyalarını dahil etmek için **tüm kitaplık dosyalarını dahil et** radyo düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="70d85-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="70d85-131">Kitaplık dosyalarının bir alt kümesini dahil etmek için **belirli dosyaları seç** radyo düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="70d85-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="70d85-132">Radyo düğmesi seçildiğinde, dosya seçici ağacı etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="70d85-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="70d85-133">İndirilecek dosya adlarının solundaki kutuları işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="70d85-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="70d85-134">**Hedef konum** metin kutusunda dosyaları depolamak için proje klasörünü belirtin.</span><span class="sxs-lookup"><span data-stu-id="70d85-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="70d85-135">Öneri olarak, her kitaplığı ayrı bir klasörde saklayın.</span><span class="sxs-lookup"><span data-stu-id="70d85-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="70d85-136">Önerilen **hedef konum** klasörü, iletişim kutusunun başlatıldığı konuma göre belirlenir:</span><span class="sxs-lookup"><span data-stu-id="70d85-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="70d85-137">Proje kökünden başlatıldığında:</span><span class="sxs-lookup"><span data-stu-id="70d85-137">If launched from the project root:</span></span>
    * <span data-ttu-id="70d85-138">*Wwwroot* varsa, *Wwwroot/lib* kullanılır.</span><span class="sxs-lookup"><span data-stu-id="70d85-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="70d85-139">*Wwwroot* yoksa *lib* kullanılır.</span><span class="sxs-lookup"><span data-stu-id="70d85-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="70d85-140">Bir proje klasöründen başlatılmışsa, karşılık gelen klasör adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="70d85-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="70d85-141">Klasör önerisi, Kitaplık adı ile sonekli olarak düzeltilir.</span><span class="sxs-lookup"><span data-stu-id="70d85-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="70d85-142">Aşağıdaki tabloda, bir Razor Pages projesine jQuery yükleme sırasında klasör önerileri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="70d85-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="70d85-143">Başlatma konumu</span><span class="sxs-lookup"><span data-stu-id="70d85-143">Launch location</span></span>                           |<span data-ttu-id="70d85-144">Önerilen klasör</span><span class="sxs-lookup"><span data-stu-id="70d85-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="70d85-145">Proje kökü ( *Wwwroot* varsa)</span><span class="sxs-lookup"><span data-stu-id="70d85-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="70d85-146">*Wwwroot/LIB/jQuery/*</span><span class="sxs-lookup"><span data-stu-id="70d85-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="70d85-147">Proje kökü ( *Wwwroot* yoksa)</span><span class="sxs-lookup"><span data-stu-id="70d85-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="70d85-148">*LIB/jQuery/*</span><span class="sxs-lookup"><span data-stu-id="70d85-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="70d85-149">Projedeki *Sayfalar* klasörü</span><span class="sxs-lookup"><span data-stu-id="70d85-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="70d85-150">*Sayfa/jQuery/*</span><span class="sxs-lookup"><span data-stu-id="70d85-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="70d85-151">Dosyaları *Libman. JSON*' da yapılandırmaya göre Indirmek için **Yükle** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="70d85-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="70d85-152">Yükleme ayrıntıları için **Çıkış** penceresinin **Kitaplık Yöneticisi** akışını gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="70d85-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="70d85-153">Örnek:</span><span class="sxs-lookup"><span data-stu-id="70d85-153">For example:</span></span>

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

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="70d85-154">LibMan bildirim dosyası girişlerini el ile yapılandır</span><span class="sxs-lookup"><span data-stu-id="70d85-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="70d85-155">Visual Studio 'daki tüm LibMan işlemleri, proje kökünün LibMan bildiriminin (*Libman. JSON*) içeriğini temel alır.</span><span class="sxs-lookup"><span data-stu-id="70d85-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="70d85-156">Kitaplık dosyalarını proje için yapılandırmak üzere *Libman. JSON* dosyasını el ile düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="70d85-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="70d85-157">Visual Studio, *Libman. JSON* dosyası kaydedildikten sonra tüm kitaplık dosyalarını geri yükler.</span><span class="sxs-lookup"><span data-stu-id="70d85-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="70d85-158">Düzenlenecek *Libman. JSON* ' u açmak için aşağıdaki seçenekler mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="70d85-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="70d85-159">**Çözüm Gezgini**'daki *Libman. JSON* dosyasına çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="70d85-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="70d85-160">**Çözüm Gezgini** ' de projeye sağ tıklayın ve **Istemci tarafı kitaplıklarını yönet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="70d85-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="70d85-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="70d85-161">**&#8224;**</span></span>
* <span data-ttu-id="70d85-162">Visual Studio **Proje** menüsünden **Istemci tarafı kitaplıklarını yönet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="70d85-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="70d85-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="70d85-163">**&#8224;**</span></span>

<span data-ttu-id="70d85-164">**&#8224;** *Libman. JSON* dosyası proje kökünde zaten mevcut değilse, varsayılan öğe şablonu içeriğiyle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="70d85-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="70d85-165">Visual Studio, renklendirme, biçimlendirme, IntelliSense ve şema doğrulaması gibi zengin JSON düzenlemesi desteği sunar.</span><span class="sxs-lookup"><span data-stu-id="70d85-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="70d85-166">LibMan bildiriminin JSON şeması [https://json.schemastore.org/libman](https://json.schemastore.org/libman)konumunda bulunur.</span><span class="sxs-lookup"><span data-stu-id="70d85-166">The LibMan manifest's JSON schema is found at [https://json.schemastore.org/libman](https://json.schemastore.org/libman).</span></span>

<span data-ttu-id="70d85-167">Aşağıdaki bildirim dosyası ile LibMan, `libraries` özelliğinde tanımlanan yapılandırma başına dosya alır.</span><span class="sxs-lookup"><span data-stu-id="70d85-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="70d85-168">`libraries` içinde tanımlanan nesne sabit değerlerinin açıklaması aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="70d85-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="70d85-169">[JQuery](https://jquery.com/) sürüm 3.3.1 'nin bir alt kümesi CDNJS sağlayıcısından alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="70d85-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="70d85-170">Alt küme,&mdash;*jQuery. min. js*, *jQuery. js*ve *jquery. min. Map*`files` özelliğinde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="70d85-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="70d85-171">Dosyalar projenin *Wwwroot/lib/jQuery* klasörüne yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="70d85-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="70d85-172">[Önyükleme](https://getbootstrap.com/) sürümü 4.1.3 tamamen alınır ve bir *Wwwroot/LIB/Bootstrap* klasörüne yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="70d85-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="70d85-173">Nesne sabit değerinin `provider` özelliği `defaultProvider` özellik değerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="70d85-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="70d85-174">LibMan, önyükleme dosyalarını unpkg sağlayıcısından alır.</span><span class="sxs-lookup"><span data-stu-id="70d85-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="70d85-175">[Lodash](https://lodash.com/) alt kümesi, kuruluş içindeki bir yöneten gövde tarafından onaylandı.</span><span class="sxs-lookup"><span data-stu-id="70d85-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="70d85-176">*Lodash. js* ve *lodash. min. js* dosyaları, *C:\\Temp\\lodash\\* konumundaki yerel dosya sisteminden alınır.</span><span class="sxs-lookup"><span data-stu-id="70d85-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\temp\\lodash\\*.</span></span> <span data-ttu-id="70d85-177">Dosyalar projenin *Wwwroot/LIB/lodash* klasörüne kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="70d85-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="70d85-178">LibMan, her bir sağlayıcının her bir kitaplığının yalnızca bir sürümünü destekler.</span><span class="sxs-lookup"><span data-stu-id="70d85-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="70d85-179">*Libman. JSON* dosyası, belirli bir sağlayıcı için aynı kitaplık adına sahip iki kitaplık içeriyorsa şema doğrulamasında başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="70d85-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="70d85-180">Kitaplık dosyalarını geri yükleme</span><span class="sxs-lookup"><span data-stu-id="70d85-180">Restore library files</span></span>

<span data-ttu-id="70d85-181">Kitaplık dosyalarını Visual Studio içinden geri yüklemek için, proje kökünde geçerli bir *Libman. JSON* dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="70d85-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="70d85-182">Geri yüklenen dosyalar her kitaplık için belirtilen konumda projeye yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="70d85-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="70d85-183">Kitaplık dosyaları, bir ASP.NET Core projesine iki şekilde geri yüklenebilir:</span><span class="sxs-lookup"><span data-stu-id="70d85-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="70d85-184">Derleme sırasında dosyaları geri yükleme</span><span class="sxs-lookup"><span data-stu-id="70d85-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="70d85-185">Dosyaları el ile geri yükleme</span><span class="sxs-lookup"><span data-stu-id="70d85-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="70d85-186">Derleme sırasında dosyaları geri yükleme</span><span class="sxs-lookup"><span data-stu-id="70d85-186">Restore files during build</span></span>

<span data-ttu-id="70d85-187">LibMan, yapı sürecinin bir parçası olarak tanımlanan kitaplık dosyalarını geri yükleyebilir.</span><span class="sxs-lookup"><span data-stu-id="70d85-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="70d85-188">Varsayılan olarak, *Derleme sonrası geri yükleme* davranışı devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="70d85-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="70d85-189">Derleme sonrası geri yükleme davranışını etkinleştirmek ve test etmek için:</span><span class="sxs-lookup"><span data-stu-id="70d85-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="70d85-190">**Çözüm Gezgini** ' de *Libman. JSON* öğesine sağ tıklayın ve bağlam menüsünden **derlemede istemci tarafı kitaplıklarını geri yükle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="70d85-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="70d85-191">Bir NuGet paketi yüklemek isteyip istemediğiniz sorulduğunda **Evet** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="70d85-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="70d85-192">Projeye [Microsoft. Web. LibraryManager. Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet paketi eklenir:</span><span class="sxs-lookup"><span data-stu-id="70d85-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="70d85-193">LibMan dosyasının geri yükleme işlemini onaylamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="70d85-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="70d85-194">`Microsoft.Web.LibraryManager.Build` paketi, projenin derleme işlemi sırasında LibMan çalıştıran bir MSBuild hedefini çıkarır.</span><span class="sxs-lookup"><span data-stu-id="70d85-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="70d85-195">Bir LibMan etkinlik günlüğü için **Çıkış** penceresinin **derleme** akışını gözden geçirin:</span><span class="sxs-lookup"><span data-stu-id="70d85-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

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

<span data-ttu-id="70d85-196">Derleme sonrası geri yükleme davranışı etkin olduğunda, *Libman. JSON* bağlam menüsünde **derleme sırasında Istemci tarafı kitaplıklarını geri yükle** seçeneği görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="70d85-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="70d85-197">Bu seçeneğin belirlenmesi, proje dosyasından `Microsoft.Web.LibraryManager.Build` paketi başvurusunu kaldırır.</span><span class="sxs-lookup"><span data-stu-id="70d85-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="70d85-198">Sonuç olarak, istemci tarafı kitaplıkları her derlemede artık geri yüklenmez.</span><span class="sxs-lookup"><span data-stu-id="70d85-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="70d85-199">Derlemeyi geri yükle ayarından bağımsız olarak, *Libman. JSON* bağlam menüsünden istediğiniz zaman el ile geri yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="70d85-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="70d85-200">Daha fazla bilgi için bkz. [dosyaları el Ile geri yükleme](#restore-files-manually).</span><span class="sxs-lookup"><span data-stu-id="70d85-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="70d85-201">Dosyaları el ile geri yükleme</span><span class="sxs-lookup"><span data-stu-id="70d85-201">Restore files manually</span></span>

<span data-ttu-id="70d85-202">Kitaplık dosyalarını el ile geri yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="70d85-202">To manually restore library files:</span></span>

* <span data-ttu-id="70d85-203">Çözümdeki tüm projeler için:</span><span class="sxs-lookup"><span data-stu-id="70d85-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="70d85-204">**Çözüm Gezgini**çözüm adına sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="70d85-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="70d85-205">**Istemci tarafı kitaplıklarını geri yükle** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="70d85-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="70d85-206">Belirli bir proje için:</span><span class="sxs-lookup"><span data-stu-id="70d85-206">For a specific project:</span></span>
  * <span data-ttu-id="70d85-207">**Çözüm Gezgini**'daki *Libman. JSON* dosyasına sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="70d85-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="70d85-208">**Istemci tarafı kitaplıklarını geri yükle** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="70d85-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="70d85-209">Geri yükleme işlemi çalışırken:</span><span class="sxs-lookup"><span data-stu-id="70d85-209">While the restore operation is running:</span></span>

* <span data-ttu-id="70d85-210">Visual Studio durum çubuğundaki Görev Durumu Merkezi (TSC) simgesi canlandırılır ve *geri yükleme işlemi başlatılır*.</span><span class="sxs-lookup"><span data-stu-id="70d85-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="70d85-211">Simgeye tıkladığınızda bilinen arka plan görevlerinin listelendiği bir araç ipucu açılır.</span><span class="sxs-lookup"><span data-stu-id="70d85-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="70d85-212">İletiler durum çubuğuna ve **Çıkış** penceresinin **Kitaplık Yöneticisi** akışına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="70d85-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="70d85-213">Örnek:</span><span class="sxs-lookup"><span data-stu-id="70d85-213">For example:</span></span>

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

## <a name="delete-library-files"></a><span data-ttu-id="70d85-214">Kitaplık dosyalarını sil</span><span class="sxs-lookup"><span data-stu-id="70d85-214">Delete library files</span></span>

<span data-ttu-id="70d85-215">Daha önce Visual Studio 'da geri yüklenen kitaplık dosyalarını silen *Temizleme* işlemini gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="70d85-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="70d85-216">**Çözüm Gezgini**'daki *Libman. JSON* dosyasına sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="70d85-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="70d85-217">**Istemci tarafı kitaplıklarını temizle** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="70d85-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="70d85-218">Kitaplık olmayan dosyaların yanlışlıkla kaldırılmasını engellemek için, temizleme işlemi tüm dizinleri silmez.</span><span class="sxs-lookup"><span data-stu-id="70d85-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="70d85-219">Yalnızca önceki geri yüklemeye dahil edilen dosyaları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="70d85-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="70d85-220">Temizleme işlemi çalışırken:</span><span class="sxs-lookup"><span data-stu-id="70d85-220">While the clean operation is running:</span></span>

* <span data-ttu-id="70d85-221">Visual Studio durum çubuğundaki TSC simgesi animasyon alınacaktır ve *istemci kitaplıkları işlemini*okur.</span><span class="sxs-lookup"><span data-stu-id="70d85-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="70d85-222">Simgeye tıkladığınızda bilinen arka plan görevlerinin listelendiği bir araç ipucu açılır.</span><span class="sxs-lookup"><span data-stu-id="70d85-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="70d85-223">İletiler durum çubuğuna ve **Çıkış** penceresinin **Kitaplık Yöneticisi** akışına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="70d85-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="70d85-224">Örnek:</span><span class="sxs-lookup"><span data-stu-id="70d85-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="70d85-225">Temizleme işlemi yalnızca projeden dosyaları siler.</span><span class="sxs-lookup"><span data-stu-id="70d85-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="70d85-226">Kitaplık dosyaları gelecekteki geri yükleme işlemlerinde daha hızlı bir şekilde almak için önbellekte kalır.</span><span class="sxs-lookup"><span data-stu-id="70d85-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="70d85-227">Yerel makinenin önbelleğinde depolanan kitaplık dosyalarını yönetmek için [Libman CLI](xref:client-side/libman/libman-cli)kullanın.</span><span class="sxs-lookup"><span data-stu-id="70d85-227">To manage library files stored in the local machine's cache, use the [LibMan CLI](xref:client-side/libman/libman-cli).</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="70d85-228">Kitaplık dosyalarını kaldır</span><span class="sxs-lookup"><span data-stu-id="70d85-228">Uninstall library files</span></span>

<span data-ttu-id="70d85-229">Kitaplık dosyalarını kaldırmak için:</span><span class="sxs-lookup"><span data-stu-id="70d85-229">To uninstall library files:</span></span>

* <span data-ttu-id="70d85-230">*Libman. JSON*öğesini açın.</span><span class="sxs-lookup"><span data-stu-id="70d85-230">Open *libman.json*.</span></span>
* <span data-ttu-id="70d85-231">Giriş işaretini karşılık gelen `libraries` nesnesi sabit değerinin içine konumlandırın.</span><span class="sxs-lookup"><span data-stu-id="70d85-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="70d85-232">Sol kenar boşluğunda görüntülenen ampul simgesine tıklayın ve **kaldır \<library_name > @\<library_version >** ' i seçin:</span><span class="sxs-lookup"><span data-stu-id="70d85-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![Kitaplığı Kaldır bağlam menü seçeneği](_static/uninstall-menu-option.png)

<span data-ttu-id="70d85-234">Alternatif olarak, LibMan bildirimini (*Libman. JSON*) el ile düzenleyebilir ve kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="70d85-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="70d85-235">[Geri yükleme işlemi](#restore-library-files) dosya kaydedildiğinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="70d85-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="70d85-236">*Libman. JSON* dosyasında artık tanımlanmamış kitaplık dosyaları projeden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="70d85-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="70d85-237">Kitaplık sürümünü Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="70d85-237">Update library version</span></span>

<span data-ttu-id="70d85-238">Güncelleştirilmiş bir kitaplık sürümünü denetlemek için:</span><span class="sxs-lookup"><span data-stu-id="70d85-238">To check for an updated library version:</span></span>

* <span data-ttu-id="70d85-239">*Libman. JSON*öğesini açın.</span><span class="sxs-lookup"><span data-stu-id="70d85-239">Open *libman.json*.</span></span>
* <span data-ttu-id="70d85-240">Giriş işaretini karşılık gelen `libraries` nesnesi sabit değerinin içine konumlandırın.</span><span class="sxs-lookup"><span data-stu-id="70d85-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="70d85-241">Sol kenar boşluğunda görünen ampul simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="70d85-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="70d85-242">**Güncelleştirme denetimi**üzerine gelin.</span><span class="sxs-lookup"><span data-stu-id="70d85-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="70d85-243">LibMan, yüklü sürümden daha yeni bir kitaplık sürümünü denetler.</span><span class="sxs-lookup"><span data-stu-id="70d85-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="70d85-244">Aşağıdaki sonuçlar oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="70d85-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="70d85-245">En son sürüm zaten yüklüyse **hiçbir güncelleştirme bulunamadı** iletisi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="70d85-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="70d85-246">Henüz yüklenmemişse en son kararlı sürüm görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="70d85-246">The latest stable version is displayed if not already installed.</span></span>

  ![Güncelleştirmeler bağlam menüsü seçeneğini denetle](_static/update-menu-option.png)

* <span data-ttu-id="70d85-248">Yüklü sürümden daha yeni bir yayın öncesi sürüm varsa, yayın öncesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="70d85-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="70d85-249">Daha eski bir kitaplık sürümüne düşürme için, *Libman. JSON* dosyasını el ile düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="70d85-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="70d85-250">Dosya kaydedildiğinde, LibMan [geri yükleme işlemi](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="70d85-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="70d85-251">Eski sürümden gereksiz dosyaları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="70d85-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="70d85-252">Yeni sürümden yeni ve güncelleştirilmiş dosyalar ekler.</span><span class="sxs-lookup"><span data-stu-id="70d85-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="70d85-253">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="70d85-253">Additional resources</span></span>

* <xref:client-side/libman/libman-cli>
* [<span data-ttu-id="70d85-254">LibMan GitHub deposu</span><span class="sxs-lookup"><span data-stu-id="70d85-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
