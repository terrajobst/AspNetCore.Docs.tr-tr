---
title: Bir ASP.NET Core Razor sayfa dosya yükleme
author: guardrex
description: Bir Razor sayfası için dosyaları karşıya yüklemeyi öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 11/10/2018
uid: razor-pages/upload-files
ms.openlocfilehash: 8d86a84bcd31cc1e1e6fbe0693c7ec179e589f3d
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570015"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="52f00-103">Bir ASP.NET Core Razor sayfa dosya yükleme</span><span class="sxs-lookup"><span data-stu-id="52f00-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="52f00-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="52f00-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="52f00-105">Bu konuda bağlı derlemeler [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) içinde <xref:tutorials/razor-pages/razor-pages-start>.</span><span class="sxs-lookup"><span data-stu-id="52f00-105">This topic builds upon the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span></span>

<span data-ttu-id="52f00-106">Bu konuda, küçük dosyalar da karşıya yükleme için çalışan basit bir model bağlama dosyaları, karşıya yüklemek için nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="52f00-106">This topic shows how to use simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="52f00-107">Büyük dosyaları akış hakkında daha fazla bilgi için bkz: [akış ile büyük dosyaları karşıya yükleme](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="52f00-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="52f00-108">Aşağıdaki adımlarda, örnek uygulamaya bir film zamanlaması dosyası karşıya yükleme özelliğini eklenir.</span><span class="sxs-lookup"><span data-stu-id="52f00-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="52f00-109">Bir film zamanlama tarafından temsil edilen bir `Schedule` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="52f00-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="52f00-110">Sınıfı, zamanlama iki sürümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="52f00-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="52f00-111">Bir sürüm müşterilere sağlanan `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="52f00-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="52f00-112">Başka bir sürüm şirket çalışanlarının kullanılan `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="52f00-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="52f00-113">Her sürümü ayrı bir dosya olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="52f00-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="52f00-114">Bu öğreticide, tek bir GÖNDERİ ile sayfasından sunucuya iki dosya yüklemelerini gerçekleştirmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="52f00-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

<span data-ttu-id="52f00-115">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="52f00-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="52f00-116">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="52f00-116">Security considerations</span></span>

<span data-ttu-id="52f00-117">Kullanıcılar bir sunucuya dosya yüklemek olanağı sağlarken dikkat alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="52f00-117">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="52f00-118">Saldırganlar yürütme [hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) sistemindeki diğer saldırıları belirleyin.</span><span class="sxs-lookup"><span data-stu-id="52f00-118">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="52f00-119">Başarılı bir saldırı olasılığını azaltmak bazı güvenlik adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="52f00-119">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="52f00-120">Daha kolay hale getirir sistem üzerindeki bir adanmış dosya karşıya yükleme alanına karşıya yükleme dosyalarını karşıya yüklenen içerik güvenlik önlemlerinin büyük oranda yansıtmaktadır.</span><span class="sxs-lookup"><span data-stu-id="52f00-120">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="52f00-121">Dosya yüklemeleri sorgulamasına, yürütme izinleri emin olun karşıya yükleme konumuna devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="52f00-121">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="52f00-122">Uygulamadan değil kullanıcı girişi tarafından belirlenen güvenli dosya adı veya karşıya yüklenen dosya dosya adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="52f00-122">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="52f00-123">Yalnızca belirli bir onaylı dosya uzantıları kümesini izin verir.</span><span class="sxs-lookup"><span data-stu-id="52f00-123">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="52f00-124">Sunucu üzerinde istemci tarafı denetimleri yapılır doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="52f00-124">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="52f00-125">İstemci tarafı denetimleri sağlamasına kolaydır.</span><span class="sxs-lookup"><span data-stu-id="52f00-125">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="52f00-126">Karşıya yükleme boyutu denetleyin ve beklenenden daha büyük karşıya engelleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="52f00-126">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="52f00-127">Virüsten/kötü amaçlı yazılım tarayıcı karşıya yüklenen içerik üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="52f00-127">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="52f00-128">Kötü amaçlı bir kodun bir sisteme karşıya yükleme için kod yürütme için ilk adımı sık şöyledir:</span><span class="sxs-lookup"><span data-stu-id="52f00-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="52f00-129">Tamamen devralma sistemin.</span><span class="sxs-lookup"><span data-stu-id="52f00-129">Completely takeover a system.</span></span>
> * <span data-ttu-id="52f00-130">Bir sistemi tamamen başarısız sonucu sistemiyle aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="52f00-130">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="52f00-131">Kullanıcı veya sistem verilerini tehlikeye.</span><span class="sxs-lookup"><span data-stu-id="52f00-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="52f00-132">Graffiti ortak bir arabirim için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="52f00-132">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="52f00-133">FileUpload sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="52f00-133">Add a FileUpload class</span></span>

<span data-ttu-id="52f00-134">Bir çift dosya yüklemeleri işlemek için bir Razor sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="52f00-134">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="52f00-135">Ekleme bir `FileUpload` sayfasına zamanlama verileri almak için bağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="52f00-135">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="52f00-136">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="52f00-136">Right click the *Models* folder.</span></span> <span data-ttu-id="52f00-137">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="52f00-137">Select **Add** > **Class**.</span></span> <span data-ttu-id="52f00-138">Sınıf adı **FileUpload** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="52f00-138">Name the class **FileUpload** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

<span data-ttu-id="52f00-139">Sınıfı, zamanlamanın başlık için bir özelliği ve her iki sürümü zamanlama için bir özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="52f00-139">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="52f00-140">Tüm üç özellik gereklidir ve başlığı 3-60 karakter uzunluğunda olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="52f00-140">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="52f00-141">Dosyaları karşıya yüklemek için bir yardımcı yöntemi ekleyin</span><span class="sxs-lookup"><span data-stu-id="52f00-141">Add a helper method to upload files</span></span>

<span data-ttu-id="52f00-142">Karşıya yüklenen zamanlama dosyalarını işlemek için kod yinelemesinden kaçınmak için bir statik yardımcı yöntemi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="52f00-142">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="52f00-143">Oluşturma bir *yardımcı programları* uygulama klasöründe ve ekleme bir *FileHelpers.cs* dosya aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="52f00-143">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="52f00-144">Yardımcı yöntemi `ProcessFormFile`, alan bir [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) ve dosyanın boyutu ve içeriğini içeren bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="52f00-144">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="52f00-145">İçerik türünü ve uzunluğu denetlenir.</span><span class="sxs-lookup"><span data-stu-id="52f00-145">The content type and length are checked.</span></span> <span data-ttu-id="52f00-146">Dosya doğrulama denetimi başarısız olursa hata eklenen `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="52f00-146">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a><span data-ttu-id="52f00-147">Dosyayı diske kaydedin.</span><span class="sxs-lookup"><span data-stu-id="52f00-147">Save the file to disk</span></span>

<span data-ttu-id="52f00-148">Örnek uygulama, veritabanı alanlarına karşıya yüklenen dosyaları kaydeder.</span><span class="sxs-lookup"><span data-stu-id="52f00-148">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="52f00-149">Bir dosyayı diske kaydetmek için kullanan bir [FILESTREAM](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="52f00-149">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="52f00-150">Aşağıdaki örnek tarafından tutulan bir dosyayı kopyalar `FileUpload.UploadPublicSchedule` için bir `FileStream` içinde bir `OnPostAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="52f00-150">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="52f00-151">`FileStream` Dosyayı diske yazar `<PATH-AND-FILE-NAME>` sağlanan:</span><span class="sxs-lookup"><span data-stu-id="52f00-151">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

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

<span data-ttu-id="52f00-152">Çalışan işlemi tarafından belirtilen konuma yazma izinlerine sahip olmalıdır `filePath`.</span><span class="sxs-lookup"><span data-stu-id="52f00-152">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="52f00-153">`filePath` *Gerekir* dosya adını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="52f00-153">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="52f00-154">Dosya adı belirtilmezse, bir [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) çalışma zamanında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="52f00-154">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="52f00-155">Hiçbir zaman uygulama olarak aynı dizin ağacında karşıya yüklenen dosyalar kalıcı hale getirin.</span><span class="sxs-lookup"><span data-stu-id="52f00-155">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="52f00-156">Kod örneği, kötü amaçlı dosya yüklemeleri karşı sunucu tarafı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="52f00-156">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="52f00-157">Kullanıcıların dosyaları kabul ederken saldırı yüzey alanı azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="52f00-157">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="52f00-158">Sınırsız dosya karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="52f00-158">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="52f00-159">Azure güvenlik: uygun denetimleri kullanıcıların dosyaları kabul ederken karşılandığından emin</span><span class="sxs-lookup"><span data-stu-id="52f00-159">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="52f00-160">Dosyayı Azure Blob depolama alanına kaydedin</span><span class="sxs-lookup"><span data-stu-id="52f00-160">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="52f00-161">Dosya içeriği, Azure Blob depolama alanına yüklemek için bkz: [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="52f00-161">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="52f00-162">Konu nasıl kullanılacağını gösteren [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) kaydetmek için bir [FILESTREAM](/dotnet/api/system.io.filestream) blob depolama.</span><span class="sxs-lookup"><span data-stu-id="52f00-162">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="52f00-163">Zamanlama sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="52f00-163">Add the Schedule class</span></span>

<span data-ttu-id="52f00-164">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="52f00-164">Right click the *Models* folder.</span></span> <span data-ttu-id="52f00-165">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="52f00-165">Select **Add** > **Class**.</span></span> <span data-ttu-id="52f00-166">Sınıf adı **zamanlama** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="52f00-166">Name the class **Schedule** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

<span data-ttu-id="52f00-167">Sınıfın kullandığı `Display` ve `DisplayFormat` öznitelikleri, kolay başlıklar ve zamanlama verilerini işlendiğinde biçimlendirme oluşturur.</span><span class="sxs-lookup"><span data-stu-id="52f00-167">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a><span data-ttu-id="52f00-168">Güncelleştirme RazorPagesMovieContext</span><span class="sxs-lookup"><span data-stu-id="52f00-168">Update the RazorPagesMovieContext</span></span>

<span data-ttu-id="52f00-169">Belirtin bir `DbSet` içinde `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) zamanlamalar:</span><span class="sxs-lookup"><span data-stu-id="52f00-169">Specify a `DbSet` in the `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a><span data-ttu-id="52f00-170">Güncelleştirme MovieContext</span><span class="sxs-lookup"><span data-stu-id="52f00-170">Update the MovieContext</span></span>

<span data-ttu-id="52f00-171">Belirtin bir `DbSet` içinde `MovieContext` (*Models/MovieContext.cs*) zamanlamalar:</span><span class="sxs-lookup"><span data-stu-id="52f00-171">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="52f00-172">Zamanlama tablo veritabanına ekleme</span><span class="sxs-lookup"><span data-stu-id="52f00-172">Add the Schedule table to the database</span></span>

<span data-ttu-id="52f00-173">Paket Yöneticisi Konsolu (PMC): **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="52f00-173">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC menüsü](upload-files/_static/pmc.png)

<span data-ttu-id="52f00-175">PMC'de aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="52f00-175">In the PMC, execute the following commands.</span></span> <span data-ttu-id="52f00-176">Bu komutlar ekleme bir `Schedule` veritabanı tablosu:</span><span class="sxs-lookup"><span data-stu-id="52f00-176">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="52f00-177">Dosyayı karşıya yükleme Razor sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="52f00-177">Add a file upload Razor Page</span></span>

<span data-ttu-id="52f00-178">İçinde *sayfaları* klasör oluşturma bir *zamanlamaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="52f00-178">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="52f00-179">İçinde *zamanlamaları* klasöründe adlı bir sayfa oluşturun *Index.cshtml* aşağıdaki içeriğe sahip bir zamanlama karşıya yükleme:</span><span class="sxs-lookup"><span data-stu-id="52f00-179">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

<span data-ttu-id="52f00-180">Her form grubu içeren bir  **\<etiket >** , her sınıf özelliği adı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="52f00-180">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="52f00-181">`Display` Öznitelikleri `FileUpload` modeli için etiketleri görüntüleme değerleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="52f00-181">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="52f00-182">Örneğin, `UploadPublicSchedule` özellik görünen adı ile ayarlanır `[Display(Name="Public Schedule")]` ve böylece form oluşturulduğunda etikette "Genel zamanlama" görüntüler.</span><span class="sxs-lookup"><span data-stu-id="52f00-182">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="52f00-183">Her form grubu içeren bir doğrulama  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="52f00-183">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="52f00-184">Kullanıcının girişinin karşılamak için başarısız olursa özellik öznitelikleri kümesi'nde `FileUpload` sınıfı veya varsa `ProcessFormFile` yöntemi dosya doğrulama denetimleri başarısız, model doğrulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="52f00-184">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="52f00-185">Model doğrulama başarısız olduğunda, kullanıcıya yardımcı doğrulama iletisi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="52f00-185">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="52f00-186">Örneğin, `Title` özelliği ile ek açıklamalı `[Required]` ve `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="52f00-186">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="52f00-187">Bir başlık sağlamak kullanıcı başarısız olursa, bunlar bir değer gerekli olduğunu belirten bir ileti alırsınız.</span><span class="sxs-lookup"><span data-stu-id="52f00-187">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="52f00-188">Kullanıcı Üçten az veya fazla altmış karakter değeri girerse, bunlar değeri yanlış bir uzunluk olduğunu belirten bir ileti alırsınız.</span><span class="sxs-lookup"><span data-stu-id="52f00-188">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="52f00-189">Sağlanan içerik olan bir dosya ise, dosya boş olduğunu belirten bir ileti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="52f00-189">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="52f00-190">Sayfa modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="52f00-190">Add the page model</span></span>

<span data-ttu-id="52f00-191">Sayfa modeli ekleyin (*Index.cshtml.cs*) için *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="52f00-191">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

<span data-ttu-id="52f00-192">Sayfa modeli (`IndexModel` içinde *Index.cshtml.cs*) bağlar `FileUpload` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="52f00-192">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="52f00-193">Model zamanlamalar listesini de kullanır (`IList<Schedule>`) sayfasında veritabanında depolanan zamanlamaları görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="52f00-193">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="52f00-194">Sayfa yüklediğinde ile `OnGetAsync`, `Schedules` veritabanından doldurulur ve yüklenen zamanlaması ile HTML tablosu oluşturmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="52f00-194">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="52f00-195">Sunucuya form gönderildiğinde `ModelState` denetlenir.</span><span class="sxs-lookup"><span data-stu-id="52f00-195">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="52f00-196">Geçersiz olursa `Schedule` yeniden oluşturulur ve sayfa doğrulama başarısız olmasının belirten bir veya daha fazla doğrulama iletilerinin ile sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="52f00-196">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="52f00-197">Geçerliyse, `FileUpload` özellikleri kullanılır *OnPostAsync* dosya karşıya yükleme iki sürümün zamanlamasını tamamlanması ve yeni bir `Schedule` verileri depolamak için nesne.</span><span class="sxs-lookup"><span data-stu-id="52f00-197">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="52f00-198">Zamanlama, ardından veritabanına kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="52f00-198">The schedule is then saved to the database:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="52f00-199">Dosya karşıya yükleme Razor sayfası bağlantı</span><span class="sxs-lookup"><span data-stu-id="52f00-199">Link the file upload Razor Page</span></span>

<span data-ttu-id="52f00-200">Açık *Pages/Shared/_Layout.cshtml* ve zamanlamaları sayfaya gitmek için gezinti çubuğunda bir bağlantı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="52f00-200">Open *Pages/Shared/_Layout.cshtml* and add a link to the navigation bar to reach the Schedules page:</span></span>

```cshtml
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-page="/Index">Home</a></li>
        <li><a asp-page="/Schedules/Index">Schedules</a></li>
        <li><a asp-page="/About">About</a></li>
        <li><a asp-page="/Contact">Contact</a></li>
    </ul>
</div>
```

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="52f00-201">Zamanlama silme işlemini onaylamak için bir sayfa ekleyin</span><span class="sxs-lookup"><span data-stu-id="52f00-201">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="52f00-202">Bir zamanlama Silinecek kullanıcı tıkladığında işlemi iptal etmek için bir fırsat sağlanır.</span><span class="sxs-lookup"><span data-stu-id="52f00-202">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="52f00-203">Bir silme onayı sayfası ekleyin (*Delete.cshtml*) için *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="52f00-203">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

<span data-ttu-id="52f00-204">Sayfa modeli (*Delete.cshtml.cs*) tarafından tanımlanmış tek bir zamanlamanın yükler `id` isteğin rota verilerindeki.</span><span class="sxs-lookup"><span data-stu-id="52f00-204">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="52f00-205">Ekleme *Delete.cshtml.cs* dosyasını *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="52f00-205">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

<span data-ttu-id="52f00-206">`OnPostAsync` Yöntemi işler zamanlamayı silme kendi `id`:</span><span class="sxs-lookup"><span data-stu-id="52f00-206">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

<span data-ttu-id="52f00-207">Zamanlama başarıyla sildikten sonra `RedirectToPage` kullanıcı tablolarına geri gönderir *Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="52f00-207">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="52f00-208">Çalışan zamanlamaları Razor sayfası</span><span class="sxs-lookup"><span data-stu-id="52f00-208">The working Schedules Razor Page</span></span>

<span data-ttu-id="52f00-209">Etiketleri ve zamanlaması başlığı için girişler sayfa yüklendiğinde bir gönderme düğmesi ile genel zamanlama ve özel bir zamanlama oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="52f00-209">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Razor sayfası ilk yükü hiçbir doğrulama hatalarını ve boş alanları görüldüğü şekilde zamanlar.](upload-files/_static/browser1.png)

<span data-ttu-id="52f00-211">Seçme **karşıya** alanlar doldurma ihlal olmadan düğmesini `[Required]` modelini öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="52f00-211">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="52f00-212">`ModelState` Geçersiz.</span><span class="sxs-lookup"><span data-stu-id="52f00-212">The `ModelState` is invalid.</span></span> <span data-ttu-id="52f00-213">Doğrulama hatası iletilerini kullanıcıya görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="52f00-213">The validation error messages are displayed to the user:</span></span>

![Her giriş denetim bitişiğinde doğrulama hata iletisi görüntülenir](upload-files/_static/browser2.png)

<span data-ttu-id="52f00-215">İçinde iki harf yazın **başlık** alan.</span><span class="sxs-lookup"><span data-stu-id="52f00-215">Type two letters into the **Title** field.</span></span> <span data-ttu-id="52f00-216">Doğrulama iletisi başlığı 3-60 karakter arasında olması gerektiğini belirtmek için değişiklikler:</span><span class="sxs-lookup"><span data-stu-id="52f00-216">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Başlık doğrulama iletisi değiştirildi](upload-files/_static/browser3.png)

<span data-ttu-id="52f00-218">Bir veya daha fazla zamanlama karşıya yüklendiğinde **yüklenen zamanlamaları** bölümünde yüklenen zamanlamaları işler:</span><span class="sxs-lookup"><span data-stu-id="52f00-218">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Yüklenen zamanlaması, her tablosunun başlık gösteren tablo karşıya tarihi UTC, genel sürüm dosya boyutu ve özel sürüm dosya boyutu](upload-files/_static/browser4.png)

<span data-ttu-id="52f00-220">Kullanıcının tıklayabileceği **Sil** buradan onaylayın ya da silme işlemini iptal etmek için bir fırsat olduğu bunlar silme onayı görünümü erişmek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="52f00-220">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="52f00-221">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="52f00-221">Troubleshooting</span></span>

<span data-ttu-id="52f00-222">Sorun giderme bilgileri ile `IFormFile` yüklemek bkz [dosyasını karşıya yükler, ASP.NET Core: sorun giderme](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="52f00-222">For troubleshooting information with `IFormFile` uploading, see [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>
