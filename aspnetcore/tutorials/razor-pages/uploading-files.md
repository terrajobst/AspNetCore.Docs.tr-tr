---
title: ASP.NET Core bir Razor sayfasına dosyaları karşıya yükleme
author: guardrex
description: Bir Razor sayfasına dosyaları karşıya yükleme hakkında bilgi edinin.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 5f86164b3d227e55e11244da7600394809b6a4a7
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
ms.locfileid: "32006134"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="96ee0-103">ASP.NET Core bir Razor sayfasına dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="96ee0-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="96ee0-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="96ee0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="96ee0-105">Bu bölümde, Razor sayfasını içeren dosyaları karşıya yükleme gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="96ee0-106">[Razor sayfalarının film örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) dosyaları karşıya yükleme bağlama Bu öğretici kullanan basit modelde çalıştığı iyi küçük dosyaları yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="96ee0-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="96ee0-107">Büyük dosyaları akış hakkında daha fazla bilgi için bkz: [akış ile büyük dosyalar](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="96ee0-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="96ee0-108">Aşağıdaki adımlarda, bir filmi zamanlama dosya karşıya yükleme özelliği örnek uygulaması'na eklenir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="96ee0-109">Bir filmi zamanlama tarafından temsil edilen bir `Schedule` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="96ee0-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="96ee0-110">Sınıfı, zamanlama iki sürümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="96ee0-111">Bir sürüm müşterilere sağlanan `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="96ee0-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="96ee0-112">Başka bir sürüm şirket çalışanlarının kullanılan `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="96ee0-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="96ee0-113">Her bir sürümü ayrı bir dosya olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="96ee0-114">Öğretici iki dosya yüklemeleriyle tek bir POST ile bir sayfadan sunucuya nasıl gerçekleştirileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="96ee0-115">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="96ee0-115">Security considerations</span></span>

<span data-ttu-id="96ee0-116">Kullanıcılar bir sunucuya dosyaları karşıya yükleme olanağı sağlarken dikkat alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="96ee0-117">Saldırganlar yürütme [hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) ve bir sistem diğer saldırılar.</span><span class="sxs-lookup"><span data-stu-id="96ee0-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="96ee0-118">Başarılı bir saldırı olasılığını azaltmak bazı güvenlik adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="96ee0-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="96ee0-119">Dosyaları karşıya yükleme adanmış dosya karşıya yükleme alanına sisteminde, karşıya yüklenen içerik üzerinde güvenlik önlemleri zorunlu tuttukları kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="96ee0-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="96ee0-120">Dosya yüklemeleri sorgulamasına olduğunda, yürütme izinleri emin olun karşıya yükleme konumuna devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="96ee0-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="96ee0-121">Uygulamadan değil kullanıcı girişi tarafından belirlenen bir güvenli dosya adı veya karşıya yüklenen dosyanın dosya adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="96ee0-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="96ee0-122">Yalnızca onaylanan dosya uzantılarını belirli bir dizi izin verir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="96ee0-123">İstemci-tarafı denetimleri sunucu üzerinde gerçekleştirilen doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="96ee0-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="96ee0-124">İstemci-tarafı denetimleri aşmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="96ee0-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="96ee0-125">Karşıya yükleme boyutunu denetlemek ve beklenenden daha büyük yüklemeler engelleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="96ee0-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="96ee0-126">Virüs/kötü amaçlı yazılım tarayıcı karşıya yüklenen içerikte çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="96ee0-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="96ee0-127">Kötü amaçlı kod bir sisteme karşıya sık yapabilirsiniz kod yürütmek için ilk adımdır:</span><span class="sxs-lookup"><span data-stu-id="96ee0-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="96ee0-128">Tamamen devralma sistemin.</span><span class="sxs-lookup"><span data-stu-id="96ee0-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="96ee0-129">Sistem tamamen başarısız sonucu sistemiyle aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="96ee0-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="96ee0-130">Kullanıcı veya sistem veri tehlikeye.</span><span class="sxs-lookup"><span data-stu-id="96ee0-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="96ee0-131">Graffiti ortak bir arabirim için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="96ee0-132">Dosya yükleme sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="96ee0-132">Add a FileUpload class</span></span>

<span data-ttu-id="96ee0-133">Dosya yüklemeleri çifti işlemek için bir Razor sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="96ee0-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="96ee0-134">Ekleme bir `FileUpload` zamanlama verileri elde etmek için sayfaya bağlı sınıfı.</span><span class="sxs-lookup"><span data-stu-id="96ee0-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="96ee0-135">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="96ee0-135">Right click the *Models* folder.</span></span> <span data-ttu-id="96ee0-136">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="96ee0-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="96ee0-137">Sınıf adını **dosya yükleme** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="96ee0-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="96ee0-138">Sınıfı, zamanlamanın başlık özelliğini ve her iki sürümü zamanlama için bir özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="96ee0-139">Tüm üç özellikleri gereklidir ve başlığı 3-60 karakter uzunluğunda olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="96ee0-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="96ee0-140">Dosyaları karşıya yüklemek için bir yardımcı yöntemi ekleyin</span><span class="sxs-lookup"><span data-stu-id="96ee0-140">Add a helper method to upload files</span></span>

<span data-ttu-id="96ee0-141">Karşıya yüklenen zamanlama dosyalarını işlemek için kod yinelemesinden kaçınmak için önce bir statik yardımcı yöntemi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="96ee0-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="96ee0-142">Oluşturma bir *yardımcı programları* uygulama klasöründe ve ekleme bir *FileHelpers.cs* dosya aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="96ee0-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="96ee0-143">Yardımcı yöntemi `ProcessFormFile`, alan bir [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) ve dosyanın boyutu ve içeriğini içeren bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="96ee0-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="96ee0-144">İçerik türü ve uzunluğu denetlenir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-144">The content type and length are checked.</span></span> <span data-ttu-id="96ee0-145">Dosya bir doğrulama denetimi geçmiyor, bir hata eklenen `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="96ee0-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="96ee0-146">Dosyayı diske kaydedin</span><span class="sxs-lookup"><span data-stu-id="96ee0-146">Save the file to disk</span></span>

<span data-ttu-id="96ee0-147">Örnek uygulamayı karşıya yüklenen dosyaların veritabanı alanlarına kaydeder.</span><span class="sxs-lookup"><span data-stu-id="96ee0-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="96ee0-148">Bir dosyayı diske kaydetmek için kullanın bir [FILESTREAM](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="96ee0-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="96ee0-149">Aşağıdaki örnek tutulan bir dosya kopyalar `FileUpload.UploadPublicSchedule` için bir `FileStream` içinde bir `OnPostAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="96ee0-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="96ee0-150">`FileStream` Disk dosya Yazar `<PATH-AND-FILE-NAME>` sağlanan:</span><span class="sxs-lookup"><span data-stu-id="96ee0-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="96ee0-151">Çalışan işlemi tarafından belirtilen konuma yazma izinlerine sahip olmalıdır `filePath`.</span><span class="sxs-lookup"><span data-stu-id="96ee0-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="96ee0-152">`filePath` *Gerekir* dosya adını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="96ee0-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="96ee0-153">Dosya adı sağlanmadı, bir [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) çalışma zamanında atılır.</span><span class="sxs-lookup"><span data-stu-id="96ee0-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="96ee0-154">Hiçbir zaman uygulama aynı dizin ağacında karşıya yüklenen dosyaların kalıcı olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="96ee0-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="96ee0-155">Kod örneği, kötü amaçlı dosya yüklemeleriyle karşı hiçbir sunucu tarafı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="96ee0-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="96ee0-156">Kullanıcıların dosyaları kabul ederken saldırı alanını azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="96ee0-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="96ee0-157">Sınırsız dosya karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="96ee0-157">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="96ee0-158">Azure güvenlik: uygun denetimleri dosyaların kullanıcılardan kabul ederken karşılandığından emin</span><span class="sxs-lookup"><span data-stu-id="96ee0-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="96ee0-159">Dosyayı Azure Blob depolama alanına kaydedin</span><span class="sxs-lookup"><span data-stu-id="96ee0-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="96ee0-160">Azure Blob Depolama birimine dosya içerik yüklemek için bkz: [.NET kullanarak Azure Blob Storage ile çalışmaya başlama](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="96ee0-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="96ee0-161">Nasıl kullanılacağı konusunda ortaya [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) kaydetmek için bir [FILESTREAM](/dotnet/api/system.io.filestream) blob depolama.</span><span class="sxs-lookup"><span data-stu-id="96ee0-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="96ee0-162">Zamanlama sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="96ee0-162">Add the Schedule class</span></span>

<span data-ttu-id="96ee0-163">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="96ee0-163">Right click the *Models* folder.</span></span> <span data-ttu-id="96ee0-164">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="96ee0-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="96ee0-165">Sınıf adını **zamanlama** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="96ee0-165">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="96ee0-166">Sınıf kullandığı `Display` ve `DisplayFormat` kolay başlıkları ve zamanlama veri işlendiğinde biçimlendirme oluşturur özniteliklerini.</span><span class="sxs-lookup"><span data-stu-id="96ee0-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="96ee0-167">Güncelleştirme MovieContext</span><span class="sxs-lookup"><span data-stu-id="96ee0-167">Update the MovieContext</span></span>

<span data-ttu-id="96ee0-168">Belirtin bir `DbSet` içinde `MovieContext` (*Models/MovieContext.cs*) tabloları için:</span><span class="sxs-lookup"><span data-stu-id="96ee0-168">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="96ee0-169">Zamanlama tablo veritabanına ekleyin</span><span class="sxs-lookup"><span data-stu-id="96ee0-169">Add the Schedule table to the database</span></span>

<span data-ttu-id="96ee0-170">(PMC) Paket Yöneticisi konsolunu açın: **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="96ee0-170">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="96ee0-172">PMC aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="96ee0-172">In the PMC, execute the following commands.</span></span> <span data-ttu-id="96ee0-173">Bu komutlar ekleme bir `Schedule` veritabanı tablosuna:</span><span class="sxs-lookup"><span data-stu-id="96ee0-173">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="96ee0-174">Bir dosyayı karşıya yükleme Razor sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="96ee0-174">Add a file upload Razor Page</span></span>

<span data-ttu-id="96ee0-175">İçinde *sayfaları* klasörü oluşturmak bir *zamanlamaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="96ee0-175">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="96ee0-176">İçinde *zamanlamaları* klasörünü adlı bir sayfa oluşturma *Index.cshtml* aşağıdaki içeriğe sahip bir zamanlama karşıya yükleme için:</span><span class="sxs-lookup"><span data-stu-id="96ee0-176">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="96ee0-177">Her form grubu içeren bir  **\<etiketi >** her sınıf özelliğinin adını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="96ee0-177">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="96ee0-178">`Display` Öznitelikleri `FileUpload` modeli etiketlerini görüntüleme değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="96ee0-178">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="96ee0-179">Örneğin, `UploadPublicSchedule` özelliğin görünen adı ayarlandığında `[Display(Name="Public Schedule")]` ve formu işleyen böylece "Ortak zamanlama" etiketi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-179">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="96ee0-180">Her form grubu bir doğrulama içeren  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="96ee0-180">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="96ee0-181">Kullanıcı karşılamak üzere başarısız giriş olması durumunda özellik öznitelikleri kümesinde `FileUpload` sınıfı veya varsa `ProcessFormFile` yöntemi dosya doğrulama başarısız denetler, model doğrulamak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="96ee0-181">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="96ee0-182">Model doğrulama başarısız olduğunda, bir yardımcı doğrulama ileti kullanıcıya işlenir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-182">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="96ee0-183">Örneğin, `Title` özellik açıklama ile `[Required]` ve `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="96ee0-183">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="96ee0-184">Kullanıcı bir başlık sağlamanız başarısız olursa, bir değer gerekli olduğunu belirten bir ileti alırlar.</span><span class="sxs-lookup"><span data-stu-id="96ee0-184">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="96ee0-185">Kullanıcı değerinden üç veya daha fazla altmış karakter değeri girerse, bunlar değeri yanlış bir uzunluğa sahip olduğunu belirten bir ileti alırsınız.</span><span class="sxs-lookup"><span data-stu-id="96ee0-185">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="96ee0-186">İçeriği yok sağlanan bir dosya varsa, dosyayı boş olduğunu belirten bir ileti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-186">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="96ee0-187">Sayfa modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="96ee0-187">Add the page model</span></span>

<span data-ttu-id="96ee0-188">Sayfa modeli ekleme (*Index.cshtml.cs*) için *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="96ee0-188">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="96ee0-189">Sayfa modeli (`IndexModel` içinde *Index.cshtml.cs*) bağlar `FileUpload` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="96ee0-189">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="96ee0-190">Model zamanlamalar listesini de kullanır (`IList<Schedule>`) sayfasında veritabanında depolanan zamanlamaları görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="96ee0-190">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="96ee0-191">Sayfa yüklediğinde ile `OnGetAsync`, `Schedules` veritabanından doldurulur ve yüklenen zamanlamalar HTML tablosu oluşturmak için kullanılan:</span><span class="sxs-lookup"><span data-stu-id="96ee0-191">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="96ee0-192">Sunucuya form gönderildiğinde `ModelState` denetlenir.</span><span class="sxs-lookup"><span data-stu-id="96ee0-192">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="96ee0-193">Geçersiz, `Schedule` yeniden oluşturulur ve sayfa doğrulamayı neden geçemediğini belirten bir veya daha fazla doğrulama iletileri ile sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="96ee0-193">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="96ee0-194">Geçerliyse, `FileUpload` özellikleri kullanıldığı *OnPostAsync* zamanlama iki sürümleri için dosya karşıya yükleme işlemini tamamlamak için ve yeni bir oluşturmak için `Schedule` verileri depolamak için nesne.</span><span class="sxs-lookup"><span data-stu-id="96ee0-194">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="96ee0-195">Zamanlama sonra veritabanına kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="96ee0-195">The schedule is then saved to the database:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="96ee0-196">Dosya karşıya yükleme Razor sayfasını bağlantı</span><span class="sxs-lookup"><span data-stu-id="96ee0-196">Link the file upload Razor Page</span></span>

<span data-ttu-id="96ee0-197">Açık *_Layout.cshtml* ve bir bağlantı dosya karşıya yükleme sayfasına ulaşmak için gezinti çubuğu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="96ee0-197">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="96ee0-198">Zamanlama silmeyi onaylamak için bir sayfa ekleyin</span><span class="sxs-lookup"><span data-stu-id="96ee0-198">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="96ee0-199">Kullanıcı bir zamanlama silinecek tıklattığında işlemi iptal etmek için bir fırsat sağlanır.</span><span class="sxs-lookup"><span data-stu-id="96ee0-199">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="96ee0-200">Silme onayı sayfası ekleme (*Delete.cshtml*) için *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="96ee0-200">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="96ee0-201">Sayfa modeli (*Delete.cshtml.cs*) tarafından tanımlanan tek bir zamanlama yükler `id` isteğin rota verilerindeki.</span><span class="sxs-lookup"><span data-stu-id="96ee0-201">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="96ee0-202">Ekleme *Delete.cshtml.cs* dosya *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="96ee0-202">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="96ee0-203">`OnPostAsync` Yöntemi işler tarafından zamanlama silinirken kendi `id`:</span><span class="sxs-lookup"><span data-stu-id="96ee0-203">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="96ee0-204">Zamanlama başarıyla sildikten sonra `RedirectToPage` zamanlamaları için kullanıcı gönderir *Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="96ee0-204">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="96ee0-205">Zamanlamalar Razor sayfasını çalışma</span><span class="sxs-lookup"><span data-stu-id="96ee0-205">The working Schedules Razor Page</span></span>

<span data-ttu-id="96ee0-206">Etiketleri ve girişleri zamanlama başlık, sayfa yüklendiğinde ortak zamanlama ve özel zamanlama bir gönderme düğmesi ile işlenir:</span><span class="sxs-lookup"><span data-stu-id="96ee0-206">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Hiçbir doğrulama hataları ve boş alanları ile ilk yükü görülen Razor sayfasını zamanlar](uploading-files/_static/browser1.png)

<span data-ttu-id="96ee0-208">Seçme **karşıya** tüm alanları doldurmak ihlal olmadan düğmesini `[Required]` model üzerinde öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="96ee0-208">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="96ee0-209">`ModelState` Geçersiz.</span><span class="sxs-lookup"><span data-stu-id="96ee0-209">The `ModelState` is invalid.</span></span> <span data-ttu-id="96ee0-210">Kullanıcı için bir doğrulama hata iletisi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="96ee0-210">The validation error messages are displayed to the user:</span></span>

![Her giriş denetiminin yanındaki bir doğrulama hata iletisi görüntülenir](uploading-files/_static/browser2.png)

<span data-ttu-id="96ee0-212">İki harf içine yazın **başlık** alan.</span><span class="sxs-lookup"><span data-stu-id="96ee0-212">Type two letters into the **Title** field.</span></span> <span data-ttu-id="96ee0-213">Başlık 3-60 karakter arasında olması gerektiğini belirtmek için doğrulama ileti değişiklikler:</span><span class="sxs-lookup"><span data-stu-id="96ee0-213">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Başlık doğrulama ileti değiştirildi](uploading-files/_static/browser3.png)

<span data-ttu-id="96ee0-215">Bir veya daha fazla Zamanlama yüklenirken **yüklenen zamanlamaları** bölüm yüklenen zamanlamaları işler:</span><span class="sxs-lookup"><span data-stu-id="96ee0-215">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Yüklenen zamanlamaları, her zamanlamanın başlık gösteren tablosunun tarihi UTC, genel bir sürümü dosya boyutu ve özel sürüm dosya boyutu karşıya](uploading-files/_static/browser4.png)

<span data-ttu-id="96ee0-217">Kullanıcı tıklatabilirsiniz **silmek** buradan onaylayın ya da silme işlemini iptal etmek için bir fırsat sahip oldukları silme onayı görünüm ulaşmak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="96ee0-217">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="96ee0-218">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="96ee0-218">Troubleshooting</span></span>

<span data-ttu-id="96ee0-219">Sorun giderme bilgileri ile `IFormFile` yüklemek bkz [dosya yüklemeleri ASP.NET Core: sorun giderme](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="96ee0-219">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="96ee0-220">Bu giriş Razor sayfalarının tamamlamak için teşekkür ederiz.</span><span class="sxs-lookup"><span data-stu-id="96ee0-220">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="96ee0-221">Geri bildirim veriyoruz.</span><span class="sxs-lookup"><span data-stu-id="96ee0-221">We appreciate feedback.</span></span> <span data-ttu-id="96ee0-222">[MVC ve EF çekirdek kullanmaya başlama](xref:data/ef-mvc/intro) mükemmel bir izleme Bu öğretici kadar olan.</span><span class="sxs-lookup"><span data-stu-id="96ee0-222">[Get started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="96ee0-223">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="96ee0-223">Additional resources</span></span>

* [<span data-ttu-id="96ee0-224">ASP.NET Core dosya yüklemeleri</span><span class="sxs-lookup"><span data-stu-id="96ee0-224">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="96ee0-225">IFormFile</span><span class="sxs-lookup"><span data-stu-id="96ee0-225">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

> [!div class="step-by-step"]
> [<span data-ttu-id="96ee0-226">Önceki: doğrulama</span><span class="sxs-lookup"><span data-stu-id="96ee0-226">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
