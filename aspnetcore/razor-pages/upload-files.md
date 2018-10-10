---
title: Bir ASP.NET Core Razor sayfa dosya yükleme
author: guardrex
description: Bir Razor sayfası için dosyaları karşıya yüklemeyi öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/11/2018
uid: razor-pages/upload-files
ms.openlocfilehash: 92e72869967b6e3202c97b92e341ea22adc69651
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912507"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="ee4c0-103">Bir ASP.NET Core Razor sayfa dosya yükleme</span><span class="sxs-lookup"><span data-stu-id="ee4c0-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="ee4c0-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ee4c0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ee4c0-105">Bu konuda bağlı derlemeler [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) içinde <xref:tutorials/razor-pages/razor-pages-start>.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-105">This topic builds upon the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span></span>

<span data-ttu-id="ee4c0-106">Bu konuda, küçük dosyalar da karşıya yükleme için çalışan basit bir model bağlama dosyaları, karşıya yüklemek için nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-106">This topic shows how to use simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="ee4c0-107">Büyük dosyaları akış hakkında daha fazla bilgi için bkz: [akış ile büyük dosyaları karşıya yükleme](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="ee4c0-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="ee4c0-108">Aşağıdaki adımlarda, örnek uygulamaya bir film zamanlaması dosyası karşıya yükleme özelliğini eklenir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="ee4c0-109">Bir film zamanlama tarafından temsil edilen bir `Schedule` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="ee4c0-110">Sınıfı, zamanlama iki sürümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="ee4c0-111">Bir sürüm müşterilere sağlanan `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="ee4c0-112">Başka bir sürüm şirket çalışanlarının kullanılan `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="ee4c0-113">Her sürümü ayrı bir dosya olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="ee4c0-114">Bu öğreticide, tek bir GÖNDERİ ile sayfasından sunucuya iki dosya yüklemelerini gerçekleştirmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="ee4c0-115">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="ee4c0-115">Security considerations</span></span>

<span data-ttu-id="ee4c0-116">Kullanıcılar bir sunucuya dosya yüklemek olanağı sağlarken dikkat alınması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="ee4c0-117">Saldırganlar yürütme [hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) sistemindeki diğer saldırıları belirleyin.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="ee4c0-118">Başarılı bir saldırı olasılığını azaltmak bazı güvenlik adımlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="ee4c0-119">Daha kolay hale getirir sistem üzerindeki bir adanmış dosya karşıya yükleme alanına karşıya yükleme dosyalarını karşıya yüklenen içerik güvenlik önlemlerinin büyük oranda yansıtmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="ee4c0-120">Dosya yüklemeleri sorgulamasına, yürütme izinleri emin olun karşıya yükleme konumuna devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="ee4c0-121">Uygulamadan değil kullanıcı girişi tarafından belirlenen güvenli dosya adı veya karşıya yüklenen dosya dosya adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="ee4c0-122">Yalnızca belirli bir onaylı dosya uzantıları kümesini izin verir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="ee4c0-123">Sunucu üzerinde istemci tarafı denetimleri yapılır doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="ee4c0-124">İstemci tarafı denetimleri sağlamasına kolaydır.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="ee4c0-125">Karşıya yükleme boyutu denetleyin ve beklenenden daha büyük karşıya engelleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="ee4c0-126">Virüsten/kötü amaçlı yazılım tarayıcı karşıya yüklenen içerik üzerinde çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="ee4c0-127">Kötü amaçlı bir kodun bir sisteme karşıya yükleme için kod yürütme için ilk adımı sık şöyledir:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="ee4c0-128">Tamamen devralma sistemin.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="ee4c0-129">Bir sistemi tamamen başarısız sonucu sistemiyle aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="ee4c0-130">Kullanıcı veya sistem verilerini tehlikeye.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="ee4c0-131">Graffiti ortak bir arabirim için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="ee4c0-132">FileUpload sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="ee4c0-132">Add a FileUpload class</span></span>

<span data-ttu-id="ee4c0-133">Bir çift dosya yüklemeleri işlemek için bir Razor sayfası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="ee4c0-134">Ekleme bir `FileUpload` sayfasına zamanlama verileri almak için bağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="ee4c0-135">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-135">Right click the *Models* folder.</span></span> <span data-ttu-id="ee4c0-136">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="ee4c0-137">Sınıf adı **FileUpload** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-137">Name the class **FileUpload** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

<span data-ttu-id="ee4c0-138">Sınıfı, zamanlamanın başlık için bir özelliği ve her iki sürümü zamanlama için bir özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="ee4c0-139">Tüm üç özellik gereklidir ve başlığı 3-60 karakter uzunluğunda olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="ee4c0-140">Dosyaları karşıya yüklemek için bir yardımcı yöntemi ekleyin</span><span class="sxs-lookup"><span data-stu-id="ee4c0-140">Add a helper method to upload files</span></span>

<span data-ttu-id="ee4c0-141">Karşıya yüklenen zamanlama dosyalarını işlemek için kod yinelemesinden kaçınmak için bir statik yardımcı yöntemi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="ee4c0-142">Oluşturma bir *yardımcı programları* uygulama klasöründe ve ekleme bir *FileHelpers.cs* dosya aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="ee4c0-143">Yardımcı yöntemi `ProcessFormFile`, alan bir [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) ve dosyanın boyutu ve içeriğini içeren bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="ee4c0-144">İçerik türünü ve uzunluğu denetlenir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-144">The content type and length are checked.</span></span> <span data-ttu-id="ee4c0-145">Dosya doğrulama denetimi başarısız olursa hata eklenen `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a><span data-ttu-id="ee4c0-146">Dosyayı diske kaydedin.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-146">Save the file to disk</span></span>

<span data-ttu-id="ee4c0-147">Örnek uygulama, veritabanı alanlarına karşıya yüklenen dosyaları kaydeder.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="ee4c0-148">Bir dosyayı diske kaydetmek için kullanan bir [FILESTREAM](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="ee4c0-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="ee4c0-149">Aşağıdaki örnek tarafından tutulan bir dosyayı kopyalar `FileUpload.UploadPublicSchedule` için bir `FileStream` içinde bir `OnPostAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="ee4c0-150">`FileStream` Dosyayı diske yazar `<PATH-AND-FILE-NAME>` sağlanan:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

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

<span data-ttu-id="ee4c0-151">Çalışan işlemi tarafından belirtilen konuma yazma izinlerine sahip olmalıdır `filePath`.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="ee4c0-152">`filePath` *Gerekir* dosya adını ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="ee4c0-153">Dosya adı belirtilmezse, bir [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) çalışma zamanında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="ee4c0-154">Hiçbir zaman uygulama olarak aynı dizin ağacında karşıya yüklenen dosyalar kalıcı hale getirin.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="ee4c0-155">Kod örneği, kötü amaçlı dosya yüklemeleri karşı sunucu tarafı koruma sağlar.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="ee4c0-156">Kullanıcıların dosyaları kabul ederken saldırı yüzey alanı azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="ee4c0-157">Sınırsız dosya karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="ee4c0-157">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="ee4c0-158">Azure güvenlik: uygun denetimleri kullanıcıların dosyaları kabul ederken karşılandığından emin</span><span class="sxs-lookup"><span data-stu-id="ee4c0-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="ee4c0-159">Dosyayı Azure Blob depolama alanına kaydedin</span><span class="sxs-lookup"><span data-stu-id="ee4c0-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="ee4c0-160">Dosya içeriği, Azure Blob depolama alanına yüklemek için bkz: [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="ee4c0-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="ee4c0-161">Konu nasıl kullanılacağını gösteren [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) kaydetmek için bir [FILESTREAM](/dotnet/api/system.io.filestream) blob depolama.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="ee4c0-162">Zamanlama sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="ee4c0-162">Add the Schedule class</span></span>

<span data-ttu-id="ee4c0-163">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-163">Right click the *Models* folder.</span></span> <span data-ttu-id="ee4c0-164">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="ee4c0-165">Sınıf adı **zamanlama** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-165">Name the class **Schedule** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

<span data-ttu-id="ee4c0-166">Sınıfın kullandığı `Display` ve `DisplayFormat` öznitelikleri, kolay başlıklar ve zamanlama verilerini işlendiğinde biçimlendirme oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a><span data-ttu-id="ee4c0-167">Güncelleştirme RazorPagesMovieContext</span><span class="sxs-lookup"><span data-stu-id="ee4c0-167">Update the RazorPagesMovieContext</span></span>

<span data-ttu-id="ee4c0-168">Belirtin bir `DbSet` içinde `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) zamanlamalar:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-168">Specify a `DbSet` in the `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a><span data-ttu-id="ee4c0-169">Güncelleştirme MovieContext</span><span class="sxs-lookup"><span data-stu-id="ee4c0-169">Update the MovieContext</span></span>

<span data-ttu-id="ee4c0-170">Belirtin bir `DbSet` içinde `MovieContext` (*Models/MovieContext.cs*) zamanlamalar:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-170">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="ee4c0-171">Zamanlama tablo veritabanına ekleme</span><span class="sxs-lookup"><span data-stu-id="ee4c0-171">Add the Schedule table to the database</span></span>

<span data-ttu-id="ee4c0-172">Paket Yöneticisi Konsolu (PMC): **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-172">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC menüsü](upload-files/_static/pmc.png)

<span data-ttu-id="ee4c0-174">PMC'de aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-174">In the PMC, execute the following commands.</span></span> <span data-ttu-id="ee4c0-175">Bu komutlar ekleme bir `Schedule` veritabanı tablosu:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-175">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="ee4c0-176">Dosyayı karşıya yükleme Razor sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="ee4c0-176">Add a file upload Razor Page</span></span>

<span data-ttu-id="ee4c0-177">İçinde *sayfaları* klasör oluşturma bir *zamanlamaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-177">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="ee4c0-178">İçinde *zamanlamaları* klasöründe adlı bir sayfa oluşturun *Index.cshtml* aşağıdaki içeriğe sahip bir zamanlama karşıya yükleme:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-178">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

<span data-ttu-id="ee4c0-179">Her form grubu içeren bir  **\<etiket >** , her sınıf özelliği adı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-179">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="ee4c0-180">`Display` Öznitelikleri `FileUpload` modeli için etiketleri görüntüleme değerleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-180">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="ee4c0-181">Örneğin, `UploadPublicSchedule` özellik görünen adı ile ayarlanır `[Display(Name="Public Schedule")]` ve böylece form oluşturulduğunda etikette "Genel zamanlama" görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-181">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="ee4c0-182">Her form grubu içeren bir doğrulama  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-182">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="ee4c0-183">Kullanıcının girişinin karşılamak için başarısız olursa özellik öznitelikleri kümesi'nde `FileUpload` sınıfı veya varsa `ProcessFormFile` yöntemi dosya doğrulama denetimleri başarısız, model doğrulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-183">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="ee4c0-184">Model doğrulama başarısız olduğunda, kullanıcıya yardımcı doğrulama iletisi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-184">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="ee4c0-185">Örneğin, `Title` özelliği ile ek açıklamalı `[Required]` ve `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-185">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="ee4c0-186">Bir başlık sağlamak kullanıcı başarısız olursa, bunlar bir değer gerekli olduğunu belirten bir ileti alırsınız.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-186">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="ee4c0-187">Kullanıcı Üçten az veya fazla altmış karakter değeri girerse, bunlar değeri yanlış bir uzunluk olduğunu belirten bir ileti alırsınız.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-187">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="ee4c0-188">Sağlanan içerik olan bir dosya ise, dosya boş olduğunu belirten bir ileti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-188">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="ee4c0-189">Sayfa modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="ee4c0-189">Add the page model</span></span>

<span data-ttu-id="ee4c0-190">Sayfa modeli ekleyin (*Index.cshtml.cs*) için *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-190">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

<span data-ttu-id="ee4c0-191">Sayfa modeli (`IndexModel` içinde *Index.cshtml.cs*) bağlar `FileUpload` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-191">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ee4c0-192">Model zamanlamalar listesini de kullanır (`IList<Schedule>`) sayfasında veritabanında depolanan zamanlamaları görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-192">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="ee4c0-193">Sayfa yüklediğinde ile `OnGetAsync`, `Schedules` veritabanından doldurulur ve yüklenen zamanlaması ile HTML tablosu oluşturmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-193">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="ee4c0-194">Sunucuya form gönderildiğinde `ModelState` denetlenir.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-194">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="ee4c0-195">Geçersiz olursa `Schedule` yeniden oluşturulur ve sayfa doğrulama başarısız olmasının belirten bir veya daha fazla doğrulama iletilerinin ile sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-195">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="ee4c0-196">Geçerliyse, `FileUpload` özellikleri kullanılır *OnPostAsync* dosya karşıya yükleme iki sürümün zamanlamasını tamamlanması ve yeni bir `Schedule` verileri depolamak için nesne.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-196">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="ee4c0-197">Zamanlama, ardından veritabanına kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-197">The schedule is then saved to the database:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="ee4c0-198">Dosya karşıya yükleme Razor sayfası bağlantı</span><span class="sxs-lookup"><span data-stu-id="ee4c0-198">Link the file upload Razor Page</span></span>

<span data-ttu-id="ee4c0-199">Açık *Pages/Shared/_Layout.cshtml* ve zamanlamaları sayfaya gitmek için gezinti çubuğunda bir bağlantı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-199">Open *Pages/Shared/_Layout.cshtml* and add a link to the navigation bar to reach the Schedules page:</span></span>

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

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="ee4c0-200">Zamanlama silme işlemini onaylamak için bir sayfa ekleyin</span><span class="sxs-lookup"><span data-stu-id="ee4c0-200">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="ee4c0-201">Bir zamanlama Silinecek kullanıcı tıkladığında işlemi iptal etmek için bir fırsat sağlanır.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-201">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="ee4c0-202">Bir silme onayı sayfası ekleyin (*Delete.cshtml*) için *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-202">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

<span data-ttu-id="ee4c0-203">Sayfa modeli (*Delete.cshtml.cs*) tarafından tanımlanmış tek bir zamanlamanın yükler `id` isteğin rota verilerindeki.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-203">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="ee4c0-204">Ekleme *Delete.cshtml.cs* dosyasını *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-204">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

<span data-ttu-id="ee4c0-205">`OnPostAsync` Yöntemi işler zamanlamayı silme kendi `id`:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-205">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

<span data-ttu-id="ee4c0-206">Zamanlama başarıyla sildikten sonra `RedirectToPage` kullanıcı tablolarına geri gönderir *Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-206">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="ee4c0-207">Çalışan zamanlamaları Razor sayfası</span><span class="sxs-lookup"><span data-stu-id="ee4c0-207">The working Schedules Razor Page</span></span>

<span data-ttu-id="ee4c0-208">Etiketleri ve zamanlaması başlığı için girişler sayfa yüklendiğinde bir gönderme düğmesi ile genel zamanlama ve özel bir zamanlama oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-208">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Razor sayfası ilk yükü hiçbir doğrulama hatalarını ve boş alanları görüldüğü şekilde zamanlar.](upload-files/_static/browser1.png)

<span data-ttu-id="ee4c0-210">Seçme **karşıya** alanlar doldurma ihlal olmadan düğmesini `[Required]` modelini öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-210">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="ee4c0-211">`ModelState` Geçersiz.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-211">The `ModelState` is invalid.</span></span> <span data-ttu-id="ee4c0-212">Doğrulama hatası iletilerini kullanıcıya görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-212">The validation error messages are displayed to the user:</span></span>

![Her giriş denetim bitişiğinde doğrulama hata iletisi görüntülenir](upload-files/_static/browser2.png)

<span data-ttu-id="ee4c0-214">İçinde iki harf yazın **başlık** alan.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-214">Type two letters into the **Title** field.</span></span> <span data-ttu-id="ee4c0-215">Doğrulama iletisi başlığı 3-60 karakter arasında olması gerektiğini belirtmek için değişiklikler:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-215">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Başlık doğrulama iletisi değiştirildi](upload-files/_static/browser3.png)

<span data-ttu-id="ee4c0-217">Bir veya daha fazla zamanlama karşıya yüklendiğinde **yüklenen zamanlamaları** bölümünde yüklenen zamanlamaları işler:</span><span class="sxs-lookup"><span data-stu-id="ee4c0-217">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Yüklenen zamanlaması, her tablosunun başlık gösteren tablo karşıya tarihi UTC, genel sürüm dosya boyutu ve özel sürüm dosya boyutu](upload-files/_static/browser4.png)

<span data-ttu-id="ee4c0-219">Kullanıcının tıklayabileceği **Sil** buradan onaylayın ya da silme işlemini iptal etmek için bir fırsat olduğu bunlar silme onayı görünümü erişmek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="ee4c0-219">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ee4c0-220">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="ee4c0-220">Troubleshooting</span></span>

<span data-ttu-id="ee4c0-221">Sorun giderme bilgileri ile `IFormFile` yüklemek bkz [dosyasını karşıya yükler, ASP.NET Core: sorun giderme](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="ee4c0-221">For troubleshooting information with `IFormFile` uploading, see [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>
