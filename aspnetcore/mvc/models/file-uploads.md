---
title: ASP.NET Core dosyaları karşıya yükleme
author: guardrex
description: ASP.NET Core MVC 'de dosyaları karşıya yüklemek için model bağlama ve akışı kullanma.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: b57ad4fe62de38085c11d7026d278cc6e0c565ce
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963151"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="db737-103">ASP.NET Core dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="db737-104">[Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)ve [Rutger fırtınası](https://github.com/rutix) tarafından</span><span class="sxs-lookup"><span data-stu-id="db737-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="db737-105">ASP.NET Core, daha küçük dosyalar için arabellekli model bağlama ve daha büyük dosyalar için arabelleğe alınmamış akış kullanarak bir veya daha fazla dosyanın yüklenmesini destekler</span><span class="sxs-lookup"><span data-stu-id="db737-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="db737-106">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="db737-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="db737-107">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="db737-107">Security considerations</span></span>

<span data-ttu-id="db737-108">Kullanıcılara bir sunucuya dosya yükleme yeteneği sağlarken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="db737-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="db737-109">Saldırganlar şunları deneyebilir:</span><span class="sxs-lookup"><span data-stu-id="db737-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="db737-110">[Hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) saldırıları yürütün.</span><span class="sxs-lookup"><span data-stu-id="db737-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="db737-111">Virüsleri veya kötü amaçlı yazılımları karşıya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="db737-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="db737-112">Ağları ve sunucuları diğer yollarla tehlikeye atabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="db737-113">Başarılı bir saldırının olasılığını azaltan güvenlik adımları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="db737-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="db737-114">Dosyaları, tercihen sistem dışı bir sürücüye, özel bir dosya yükleme alanına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="db737-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="db737-115">Ayrılmış bir konum, karşıya yüklenen dosyalar üzerinde güvenlik kısıtlamaları yapmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="db737-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="db737-116">Dosya yükleme konumunda yürütme izinlerini devre dışı bırakın.&dagger;</span><span class="sxs-lookup"><span data-stu-id="db737-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="db737-117">Karşıya yüklenen dosyaları uygulamayla aynı dizin **ağacında kalıcı yapma** .&dagger;</span><span class="sxs-lookup"><span data-stu-id="db737-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="db737-118">Uygulama tarafından belirlenen bir güvenli dosya adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="db737-119">Kullanıcı tarafından belirtilen bir dosya adı veya karşıya yüklenen dosyanın güvenilmeyen dosya adı kullanmayın.&dagger; HTML 'yi görüntülerken güvenilmeyen dosya adını kodlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="db737-120">Örneğin, dosya adını günlüğe kaydetme veya Kullanıcı arabiriminde görüntüleme (Razor otomatik olarak HTML kodlama çıkışı).</span><span class="sxs-lookup"><span data-stu-id="db737-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="db737-121">Uygulamanın tasarım belirtimi için yalnızca onaylanan dosya uzantılarına izin verin.&dagger;</span><span class="sxs-lookup"><span data-stu-id="db737-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="db737-122">Sunucuda istemci tarafı denetimlerinin gerçekleştirildiğinden emin olun.&dagger; Istemci tarafı denetimleri kolayca atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db737-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="db737-123">Karşıya yüklenen dosyanın boyutunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="db737-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="db737-124">Büyük karşıya yüklemeleri engellemek için en büyük boyut sınırını ayarlayın.&dagger;</span><span class="sxs-lookup"><span data-stu-id="db737-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="db737-125">Aynı ada sahip karşıya yüklenen bir dosya tarafından dosyaların üzerine yazılmaması gerektiğinde, dosyayı karşıya yüklemeden önce dosya adını veritabanına veya fiziksel depolamaya göre denetleyin.</span><span class="sxs-lookup"><span data-stu-id="db737-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="db737-126">**Dosya depolanmadan önce karşıya yüklenen içerik üzerinde bir virüs/kötü amaçlı yazılım tarayıcısı çalıştırın.**</span><span class="sxs-lookup"><span data-stu-id="db737-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="db737-127">örnek uygulama &dagger;ölçütleri karşılayan bir yaklaşımı gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="db737-128">Kötü amaçlı kodun bir sisteme yüklenmesi genellikle şu şekilde kod yürütmenin ilk adımıdır:</span><span class="sxs-lookup"><span data-stu-id="db737-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="db737-129">Sistemin denetimini tamamen elde edin.</span><span class="sxs-lookup"><span data-stu-id="db737-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="db737-130">Sistemin kilitlenme sonucuyla bir sistemi aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="db737-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="db737-131">Kullanıcı veya Sistem verilerinin güvenliğini tehlikeye atabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="db737-132">Genel Kullanıcı arabirimine Graffiti uygulayın.</span><span class="sxs-lookup"><span data-stu-id="db737-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="db737-133">Kullanıcılardan dosya kabul edilirken saldırı yüzeyi alanını azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="db737-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="db737-134">Kısıtlanmamış dosya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-134">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="db737-135">Azure güvenliği: kullanıcılardan dosya kabul edilirken uygun denetimlerin yerinde olduğundan emin olun</span><span class="sxs-lookup"><span data-stu-id="db737-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="db737-136">Örnek uygulamadaki örnekler de dahil olmak üzere güvenlik önlemlerini uygulama hakkında daha fazla bilgi için [doğrulama](#validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="db737-137">Depolama senaryoları</span><span class="sxs-lookup"><span data-stu-id="db737-137">Storage scenarios</span></span>

<span data-ttu-id="db737-138">Dosyalar için ortak depolama seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="db737-138">Common storage options for files include:</span></span>

* <span data-ttu-id="db737-139">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="db737-139">Database</span></span>

  * <span data-ttu-id="db737-140">Küçük dosya yüklemeleri için, bir veritabanı genellikle fiziksel depolama (dosya sistemi veya ağ paylaşımından) seçeneklerinden daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="db737-141">Kullanıcı verileri için bir veritabanı kaydının alınması eşzamanlı olarak dosya içeriğini (örneğin, avatar görüntüsü) sağlayabildiğinden, veritabanı genellikle fiziksel depolama seçeneklerine göre daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="db737-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="db737-142">Bir veritabanı, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="db737-143">Fiziksel depolama (dosya sistemi veya ağ paylaşma)</span><span class="sxs-lookup"><span data-stu-id="db737-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="db737-144">Büyük dosya yüklemeleri için:</span><span class="sxs-lookup"><span data-stu-id="db737-144">For large file uploads:</span></span>
    * <span data-ttu-id="db737-145">Veritabanı limitleri karşıya yükleme boyutunu kısıtlayabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="db737-146">Fiziksel depolama genellikle bir veritabanındaki depolamadan daha az ekonomik olur.</span><span class="sxs-lookup"><span data-stu-id="db737-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="db737-147">Fiziksel depolama, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="db737-148">Uygulamanın işlemi, depolama konumu için okuma ve yazma izinlerine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="db737-149">**Hiçbir zaman yürütme izni vermeyin.**</span><span class="sxs-lookup"><span data-stu-id="db737-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="db737-150">Veri depolama hizmeti (örneğin, [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="db737-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="db737-151">Hizmetler genellikle tek hata noktalarına tabi olan şirket içi çözümler üzerinde geliştirilmiş ölçeklenebilirlik ve esneklik sunar.</span><span class="sxs-lookup"><span data-stu-id="db737-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="db737-152">Hizmetler büyük depolama altyapısı senaryolarında düşük maliyetlidir.</span><span class="sxs-lookup"><span data-stu-id="db737-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="db737-153">Daha fazla bilgi için bkz. [hızlı başlangıç: nesne depolamada blob oluşturmak için .NET kullanma](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="db737-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="db737-154">Konu <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>gösterir, ancak <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> bir <xref:System.IO.Stream>çalışırken blob depolamaya <xref:System.IO.FileStream> kaydetmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="db737-155">Karşıya dosya yükleme senaryoları</span><span class="sxs-lookup"><span data-stu-id="db737-155">File upload scenarios</span></span>

<span data-ttu-id="db737-156">Dosyaları karşıya yüklemek için iki genel yaklaşım arabelleğe alınır ve akışlardır.</span><span class="sxs-lookup"><span data-stu-id="db737-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="db737-157">**Ara**</span><span class="sxs-lookup"><span data-stu-id="db737-157">**Buffering**</span></span>

<span data-ttu-id="db737-158">Dosyanın tamamı bir <xref:Microsoft.AspNetCore.Http.IFormFile>okur, bu dosya dosyayı işlemek veya kaydetmek C# için kullanılan bir gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="db737-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="db737-159">Dosya karşıya yüklemeleri tarafından kullanılan kaynaklar (disk, bellek), eşzamanlı dosya karşıya yüklemelerinin sayısına ve boyutuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="db737-160">Bir uygulama çok fazla karşıya yükleme arabelleğini denerse, bellek veya disk alanı tükenirse site çöker.</span><span class="sxs-lookup"><span data-stu-id="db737-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="db737-161">Karşıya dosya yükleme boyutu veya sıklığı uygulama kaynaklarını tüketme ise, akış ' ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="db737-162">64 KB geçen tek bir arabelleğe alınmış dosya, bellekten diskte geçici bir dosyaya taşınır.</span><span class="sxs-lookup"><span data-stu-id="db737-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="db737-163">Dosya arabelleğe alma, bu konunun aşağıdaki bölümlerinde ele alınmıştır:</span><span class="sxs-lookup"><span data-stu-id="db737-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="db737-164">Fiziksel depolama alanı</span><span class="sxs-lookup"><span data-stu-id="db737-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="db737-165">Veritabanınızı</span><span class="sxs-lookup"><span data-stu-id="db737-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="db737-166">**Akış**</span><span class="sxs-lookup"><span data-stu-id="db737-166">**Streaming**</span></span>

<span data-ttu-id="db737-167">Dosya çok parçalı bir istekten alınır ve doğrudan uygulama tarafından işlenir veya kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="db737-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="db737-168">Akış performansı önemli ölçüde iyileştirmez.</span><span class="sxs-lookup"><span data-stu-id="db737-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="db737-169">Akış, dosya karşıya yüklenirken bellek veya disk alanı taleplerini azaltır.</span><span class="sxs-lookup"><span data-stu-id="db737-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="db737-170">Akış büyük dosyaları [akış ile büyük dosyaları karşıya yükle](#upload-large-files-with-streaming) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="db737-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="db737-171">Fiziksel depolamaya arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="db737-172">Küçük dosyaları karşıya yüklemek için, çok parçalı bir form kullanın veya JavaScript kullanarak bir POST isteği oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db737-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="db737-173">Aşağıdaki örnek, tek bir dosyayı karşıya yüklemek için bir Razor Pages formunun kullanımını gösterir (örnek uygulamada*Pages/Bufferedsinglefileuploadfiziksel. cshtml* ):</span><span class="sxs-lookup"><span data-stu-id="db737-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="db737-174">Aşağıdaki örnek, önceki örneğe benzerdir, ancak şunları hariç:</span><span class="sxs-lookup"><span data-stu-id="db737-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="db737-175">Form verilerini göndermek için JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="db737-176">Doğrulama yok.</span><span class="sxs-lookup"><span data-stu-id="db737-176">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="db737-177">[Fetch API 'sini desteklemeyen](https://caniuse.com/#feat=fetch)istemcilerde form gönderisini JavaScript 'te gerçekleştirmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="db737-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="db737-178">Fetch Polyfill kullanın (örneğin, [Window. Fetch (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="db737-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="db737-179">`XMLHttpRequest`kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="db737-180">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="db737-180">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="db737-181">Dosya yüklemelerini desteklemek için, HTML formları `multipart/form-data`bir kodlama türü (`enctype`) belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="db737-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="db737-182">Birden çok dosyayı karşıya yüklemeyi desteklemek için `files` giriş öğesi için, `<input>` öğesinde `multiple` özniteliğini sağlayın:</span><span class="sxs-lookup"><span data-stu-id="db737-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="db737-183">Sunucuya yüklenen tek dosyalara <xref:Microsoft.AspNetCore.Http.IFormFile>kullanılarak [model bağlama](xref:mvc/models/model-binding) yoluyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="db737-184">Örnek uygulama, veritabanı ve fiziksel depolama senaryoları için birden çok arabellekli dosya yüklemeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="db737-185">Görüntüleme ve günlüğe kaydetme için dışında <xref:Microsoft.AspNetCore.Http.IFormFile> `FileName` **özelliğini kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="db737-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="db737-186">Görüntüleme veya günlüğe kaydetme sırasında, HTML dosya adını kodlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="db737-187">Saldırgan, tam yollar veya göreli yollar dahil olmak üzere kötü amaçlı bir dosya adı sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="db737-188">Uygulamalar:</span><span class="sxs-lookup"><span data-stu-id="db737-188">Applications should:</span></span>
>
> * <span data-ttu-id="db737-189">Kullanıcı tarafından sağlanan dosya adının yolunu kaldırın.</span><span class="sxs-lookup"><span data-stu-id="db737-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="db737-190">Kullanıcı arabirimi veya günlüğe kaydetme için HTML kodlu, yol tarafından kaldırılan dosya adını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="db737-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="db737-191">Depolama için yeni bir rastgele dosya adı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db737-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="db737-192">Aşağıdaki kod, dosya adından yolu kaldırır:</span><span class="sxs-lookup"><span data-stu-id="db737-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="db737-193">Bu nedenle, şu ana kadar dikkate alınması gereken örnekler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="db737-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="db737-194">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="db737-195">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="db737-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="db737-196">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="db737-196">Validation</span></span>](#validation)

<span data-ttu-id="db737-197">Model bağlama ve <xref:Microsoft.AspNetCore.Http.IFormFile>kullanarak dosya karşıya yüklerken, eylem yöntemi şunları kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="db737-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="db737-198">Tek bir <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="db737-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="db737-199">Birkaç dosyayı temsil eden aşağıdaki koleksiyonlardan herhangi biri:</span><span class="sxs-lookup"><span data-stu-id="db737-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="db737-200">[Liste](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="db737-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="db737-201">Bağlama, form dosyaları adına göre eşleşir.</span><span class="sxs-lookup"><span data-stu-id="db737-201">Binding matches form files by name.</span></span> <span data-ttu-id="db737-202">Örneğin, `<input type="file" name="formFile">` HTML `name` değeri C# parametre/özellik ile (`FormFile`) eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="db737-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="db737-203">Daha fazla bilgi için, [ad öznitelik DEĞERINI Post yönteminin parametre adına eşleştirin](#match-name-attribute-value-to-parameter-name-of-post-method) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="db737-204">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="db737-204">The following example:</span></span>

* <span data-ttu-id="db737-205">Karşıya yüklenen bir veya daha fazla dosya üzerinden döngü.</span><span class="sxs-lookup"><span data-stu-id="db737-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="db737-206">Dosya adı da dahil olmak üzere bir dosyanın tam yolunu döndürmek için [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanır.</span><span class="sxs-lookup"><span data-stu-id="db737-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="db737-207">Dosyalar, uygulama tarafından oluşturulan bir dosya adı kullanılarak yerel dosya sistemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="db737-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="db737-208">Karşıya yüklenen dosyaların toplam sayısını ve boyutunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="db737-208">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="db737-209">Yol olmadan bir dosya adı oluşturmak için `Path.GetRandomFileName` kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="db737-210">Aşağıdaki örnekte, yol yapılandırmadan alınır:</span><span class="sxs-lookup"><span data-stu-id="db737-210">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="db737-211"><xref:System.IO.FileStream> geçirilen yol, dosya adını *içermelidir.*</span><span class="sxs-lookup"><span data-stu-id="db737-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="db737-212">Dosya adı sağlanmazsa, çalışma zamanında bir <xref:System.UnauthorizedAccessException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="db737-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="db737-213"><xref:Microsoft.AspNetCore.Http.IFormFile> tekniği kullanılarak yüklenen dosyalar, işlemeden önce sunucuda veya diskte bellek halinde arabelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="db737-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="db737-214">Eylem yönteminde, <xref:Microsoft.AspNetCore.Http.IFormFile> içeriğe <xref:System.IO.Stream>olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="db737-215">Yerel dosya sistemine ek olarak, dosyalar bir ağ paylaşımında veya [Azure Blob depolama](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)gibi bir dosya depolama hizmetine kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="db737-216">Karşıya yüklemek için birden çok dosya üzerinde döngü yapan ve güvenli dosya adları kullanan başka bir örnek için, örnek uygulamadaki *Pages/Bufferedmultiplefileuploadfiziksel. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="db737-217">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , önceki geçici dosyaları silmeden 65.535 'den fazla dosya oluşturulduysa bir <xref:System.IO.IOException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db737-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="db737-218">65.535 dosya sınırının sunucu başına sınırı vardır.</span><span class="sxs-lookup"><span data-stu-id="db737-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="db737-219">Windows işletim sistemi için bu sınır hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="db737-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="db737-220">GetTempFileNameA işlevi</span><span class="sxs-lookup"><span data-stu-id="db737-220">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="db737-221">Bir veritabanına arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="db737-222">İkili dosya verilerini [Entity Framework](/ef/core/index)kullanarak bir veritabanında depolamak için, varlıkta bir <xref:System.Byte> Array özelliği tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="db737-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="db737-223">Bir <xref:Microsoft.AspNetCore.Http.IFormFile>içeren sınıf için bir sayfa modeli özelliği belirtin:</span><span class="sxs-lookup"><span data-stu-id="db737-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="db737-224"><xref:Microsoft.AspNetCore.Http.IFormFile>, doğrudan bir eylem yöntemi parametresi veya bir bağlantılı model özelliği olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="db737-225">Önceki örnekte, bir bağlantılı model özelliği kullanılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="db737-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="db737-226">`FileUpload` Razor Pages formunda kullanılır:</span><span class="sxs-lookup"><span data-stu-id="db737-226">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="db737-227">Form sunucuya gönderildiğinde, <xref:Microsoft.AspNetCore.Http.IFormFile> bir akışa kopyalayın ve veritabanına bir bayt dizisi olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="db737-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="db737-228">Aşağıdaki örnekte, uygulamanın veritabanı bağlamını `_dbContext` depolar:</span><span class="sxs-lookup"><span data-stu-id="db737-228">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="db737-229">Yukarıdaki örnek, örnek uygulamada gösterilen senaryoya benzerdir:</span><span class="sxs-lookup"><span data-stu-id="db737-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="db737-230">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="db737-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="db737-231">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="db737-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="db737-232">İkili verileri ilişkisel veritabanlarında depolarken dikkatli olun, çünkü performansı olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="db737-233">Doğrulaması olmadan <xref:Microsoft.AspNetCore.Http.IFormFile> `FileName` özelliğine güvenmeyin veya güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="db737-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="db737-234">`FileName` özelliği yalnızca görüntüleme amacıyla ve yalnızca HTML kodlaması sonrasında kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="db737-235">Belirtilen örneklerde dikkate alınması gereken önemli noktalar.</span><span class="sxs-lookup"><span data-stu-id="db737-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="db737-236">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="db737-237">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="db737-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="db737-238">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="db737-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="db737-239">Akışa sahip büyük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-239">Upload large files with streaming</span></span>

<span data-ttu-id="db737-240">Aşağıdaki örnek, bir denetleyiciyi bir denetleyici eyleminde akışa almak için JavaScript 'in nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="db737-241">Dosyanın antiforgery belirteci özel bir filtre özniteliği kullanılarak oluşturulur ve istek gövdesi yerine istemci HTTP üst bilgilerine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="db737-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="db737-242">Eylem yöntemi karşıya yüklenen verileri doğrudan işlediğinden, form modeli bağlama başka bir özel filtre tarafından devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="db737-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="db737-243">Eylem içinde formun içerikleri, her bir `MultipartSection`okuyan, dosyayı işleyen veya içeriği uygun şekilde depolayan bir `MultipartReader`kullanılarak okunur.</span><span class="sxs-lookup"><span data-stu-id="db737-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="db737-244">Çok parçalı bölümler okunduktan sonra eylem kendi model bağlamasını gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="db737-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="db737-245">İlk sayfa yanıtı formu yükler ve bir tanımlama bilgisine (`GenerateAntiforgeryTokenCookieAttribute` özniteliği aracılığıyla) bir antiforgery belirteci kaydeder.</span><span class="sxs-lookup"><span data-stu-id="db737-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="db737-246">Öznitelik, bir istek belirtecine sahip bir tanımlama bilgisi ayarlamak için ASP.NET Core yerleşik [antiforgery desteğini](xref:security/anti-request-forgery) kullanır:</span><span class="sxs-lookup"><span data-stu-id="db737-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="db737-247">Model bağlamayı devre dışı bırakmak için `DisableFormValueModelBindingAttribute` kullanılır:</span><span class="sxs-lookup"><span data-stu-id="db737-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="db737-248">Örnek uygulamada `GenerateAntiforgeryTokenCookieAttribute` ve `DisableFormValueModelBindingAttribute`, [`Startup.ConfigureServices` kuralları](xref:razor-pages/razor-pages-conventions)kullanılarak Razor Pages `/StreamedSingleFileUploadDb` ve `/StreamedSingleFileUploadPhysical` sayfa uygulama modellerine filtre olarak uygulanır:</span><span class="sxs-lookup"><span data-stu-id="db737-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="db737-249">Model bağlama formu okumadığından formdan bağlanan parametreler bağlanamaz (sorgu, yol ve başlık çalışmaya devam eder).</span><span class="sxs-lookup"><span data-stu-id="db737-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="db737-250">Action yöntemi doğrudan `Request` özelliği ile birlikte çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="db737-251">Her bölümü okumak için bir `MultipartReader` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="db737-252">Anahtar/değer verileri bir `KeyValueAccumulator`depolanır.</span><span class="sxs-lookup"><span data-stu-id="db737-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="db737-253">Çok parçalı bölümler okunduktan sonra, `KeyValueAccumulator` içeriği form verilerini bir model türüne bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="db737-254">EF Core ile bir veritabanına akışa yönelik tüm `StreamingController.UploadDatabase` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db737-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="db737-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="db737-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="db737-256">Fiziksel bir konuma akışa yönelik tüm `StreamingController.UploadPhysical` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db737-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="db737-257">Örnek uygulamada, doğrulama denetimleri `FileHelpers.ProcessStreamedFile`tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="db737-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="db737-258">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="db737-258">Validation</span></span>

<span data-ttu-id="db737-259">Örnek uygulamanın `FileHelpers` sınıfı, arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> ve akış dosya yüklemeleri için birkaç denetim gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="db737-260">Örnek uygulamada arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> dosya yüklemelerini işlemek için, *Utilities/Fileyardımcılar. cs* dosyasındaki `ProcessFormFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="db737-261">Akış dosyalarını işlemek için aynı dosyadaki `ProcessStreamedFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="db737-262">Örnek uygulamada gösterilen doğrulama işleme yöntemleri karşıya yüklenen dosyaların içeriğini taramaz.</span><span class="sxs-lookup"><span data-stu-id="db737-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="db737-263">Çoğu üretim senaryosunda, dosyanın kullanıcılara veya diğer sistemlere kullanılabilir hale getirilmesi için dosya üzerinde bir virüs/kötü amaçlı yazılım tarayıcı API 'SI kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="db737-264">Konu örneği, doğrulama tekniklerine yönelik çalışan bir örnek sağlasa da şunları gerçekleştirmediğiniz takdirde bir üretim uygulamasında `FileHelpers` sınıfını uygulamayın:</span><span class="sxs-lookup"><span data-stu-id="db737-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="db737-265">Uygulamayı tam olarak anlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="db737-266">Uygulamayı uygulamanın ortamı ve belirtimleri için uygun şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="db737-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="db737-267">**Bu gereksinimleri bilmeden bir uygulamada güvenlik kodunu hiçbir şekilde sayısının fark gözetmeden uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="db737-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="db737-268">İçerik doğrulama</span><span class="sxs-lookup"><span data-stu-id="db737-268">Content validation</span></span>

<span data-ttu-id="db737-269">**Karşıya yüklenen içerikte üçüncü taraf bir virüs/kötü amaçlı yazılım tarama API 'SI kullanın.**</span><span class="sxs-lookup"><span data-stu-id="db737-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="db737-270">Dosyaları tarama, yüksek hacimli senaryolarda sunucu kaynaklarında yoğun bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="db737-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="db737-271">Dosya tarama nedeniyle istek işleme performansı azaldığında, tarama işini, muhtemelen uygulamanın sunucusundan farklı bir sunucuda çalışan bir [arka plan hizmetine](xref:fundamentals/host/hosted-services)devredere göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="db737-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="db737-272">Genellikle, arka plan virüs tarayıcısı tarafından denetlene kadar karşıya yüklenen dosyalar karantinaya alınmış bir alanda tutulur.</span><span class="sxs-lookup"><span data-stu-id="db737-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="db737-273">Bir dosya geçtiğinde dosya normal dosya depolama konumuna taşınır.</span><span class="sxs-lookup"><span data-stu-id="db737-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="db737-274">Bu adımlar genellikle bir dosyanın tarama durumunu gösteren bir veritabanı kaydıyla birlikte gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="db737-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="db737-275">Böyle bir yaklaşım kullanarak, uygulama ve uygulama sunucusu isteklere yanıt vermeye odaklanmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="db737-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="db737-276">Dosya Uzantısı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="db737-276">File extension validation</span></span>

<span data-ttu-id="db737-277">Karşıya yüklenen dosyanın uzantısı izin verilen uzantılar listesine göre denetlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="db737-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="db737-278">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="db737-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="db737-279">Dosya imzası doğrulaması</span><span class="sxs-lookup"><span data-stu-id="db737-279">File signature validation</span></span>

<span data-ttu-id="db737-280">Bir dosyanın imzası, bir dosyanın başlangıcında ilk birkaç bayta göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="db737-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="db737-281">Bu baytlar, uzantının dosyanın içeriğiyle eşleşip eşleşmediğini göstermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="db737-282">Örnek uygulama, birkaç ortak dosya türü için dosya imzalarını denetler.</span><span class="sxs-lookup"><span data-stu-id="db737-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="db737-283">Aşağıdaki örnekte, bir JPEG görüntüsünün dosya imzası, dosyaya karşı denetlenir:</span><span class="sxs-lookup"><span data-stu-id="db737-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="db737-284">Ek dosya imzaları almak için [Dosya Imzaları veritabanı](https://www.filesignatures.net/) ve resmi dosya belirtimleri bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="db737-285">Dosya adı güvenliği</span><span class="sxs-lookup"><span data-stu-id="db737-285">File name security</span></span>

<span data-ttu-id="db737-286">Fiziksel depolamaya bir dosyayı kaydetmek için hiçbir şekilde istemci tarafından sağlanan dosya adı kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="db737-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="db737-287">Geçici depolama için tam yol (dosya adı da dahil olmak üzere) oluşturmak için [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) veya [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanarak dosya için güvenli bir dosya adı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db737-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="db737-288">Razor otomatik olarak HTML, görüntüleme için özellik değerlerini kodlar.</span><span class="sxs-lookup"><span data-stu-id="db737-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="db737-289">Aşağıdaki kodun kullanımı güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="db737-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="db737-290">Razor dışında, her zaman bir kullanıcının isteğinden dosya adı içeriği <xref:System.Net.WebUtility.HtmlEncode*>.</span><span class="sxs-lookup"><span data-stu-id="db737-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="db737-291">Birçok uygulama, dosyanın var olduğunu bir denetim içermelidir; Aksi takdirde, dosyanın üzerine aynı ada sahip bir dosya yazılır.</span><span class="sxs-lookup"><span data-stu-id="db737-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="db737-292">Uygulamanızın belirtimlerini karşılamak için ek mantık sağlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="db737-293">Boyut doğrulaması</span><span class="sxs-lookup"><span data-stu-id="db737-293">Size validation</span></span>

<span data-ttu-id="db737-294">Karşıya yüklenen dosyaların boyutunu sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="db737-295">Örnek uygulamada, dosyanın boyutu 2 MB ile sınırlıdır (bayt cinsinden gösterilir).</span><span class="sxs-lookup"><span data-stu-id="db737-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="db737-296">Sınır, *appSettings. JSON* dosyasındaki [yapılandırma](xref:fundamentals/configuration/index) yoluyla sağlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="db737-297">`FileSizeLimit` `PageModel` sınıflarına eklenmiş:</span><span class="sxs-lookup"><span data-stu-id="db737-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="db737-298">Dosya boyutu sınırı aştığında, dosya reddedilir:</span><span class="sxs-lookup"><span data-stu-id="db737-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="db737-299">Name öznitelik değerini POST yönteminin Parameter adı ile Eşleştir</span><span class="sxs-lookup"><span data-stu-id="db737-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="db737-300">Form verileri oluşturan veya JavaScript 'in `FormData` doğrudan kullanan Razor olmayan formlarda, formun öğesinde veya `FormData` belirtilen adın, denetleyicinin eyleminde parametrenin adıyla eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="db737-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="db737-301">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="db737-301">In the following example:</span></span>

* <span data-ttu-id="db737-302">Bir `<input>` öğesi kullanılırken, `name` özniteliği `battlePlans`değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="db737-303">JavaScript içinde `FormData` kullanırken ad, `battlePlans`değer olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="db737-304">C# Yöntemin parametresi için eşleşen bir ad kullanın (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="db737-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="db737-305">`Upload`adlı Razor Pages sayfa işleyicisi yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="db737-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="db737-306">MVC POST denetleyicisi eylem yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="db737-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="db737-307">Sunucu ve uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="db737-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="db737-308">Çok parçalı gövde uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="db737-308">Multipart body length limit</span></span>

<span data-ttu-id="db737-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> her bir çok parçalı gövdenin uzunluğu için sınır ayarlar.</span><span class="sxs-lookup"><span data-stu-id="db737-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="db737-310">Bu sınırı aşan form bölümleri ayrıştırıldığında bir <xref:System.IO.InvalidDataException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db737-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="db737-311">Varsayılan değer 134.217.728 ' dir (128 MB).</span><span class="sxs-lookup"><span data-stu-id="db737-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="db737-312">`Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarını kullanarak sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="db737-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="db737-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>, tek sayfa veya eylem için <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="db737-314">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices`bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="db737-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    });
```

<span data-ttu-id="db737-315">Razor Pages bir uygulamada veya bir MVC uygulamasında, filtreyi sayfa modeline veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="db737-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="db737-316">Kestrel maksimum istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="db737-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="db737-317">Kestrel tarafından barındırılan uygulamalar için, varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu, yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="db737-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="db737-318">[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel Server seçeneğini kullanarak sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="db737-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="db737-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>, tek sayfa veya eylem için [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) ayarlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="db737-320">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices`bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="db737-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    });
```

<span data-ttu-id="db737-321">Bir Razor sayfaları uygulamasında veya bir MVC uygulamasında, filtreyi sayfa işleyici sınıfına veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="db737-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="db737-322">`RequestSizeLimitAttribute`, [@attribute](xref:mvc/views/razor#attribute) Razor yönergesi kullanılarak da uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="db737-322">The `RequestSizeLimitAttribute` can also be applied using the [@attribute](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="db737-323">Diğer Kestrel limitleri</span><span class="sxs-lookup"><span data-stu-id="db737-323">Other Kestrel limits</span></span>

<span data-ttu-id="db737-324">Kestrel tarafından barındırılan uygulamalar için diğer Kestrel limitleri de uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="db737-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="db737-325">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="db737-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="db737-326">İstek ve yanıt veri ücretleri</span><span class="sxs-lookup"><span data-stu-id="db737-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="db737-327">IIS içerik uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="db737-327">IIS content length limit</span></span>

<span data-ttu-id="db737-328">Varsayılan istek sınırı (`maxAllowedContentLength`), yaklaşık 28.6 MB olan 30.000.000 bayttır.</span><span class="sxs-lookup"><span data-stu-id="db737-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="db737-329">*Web. config* dosyasında sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="db737-329">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="db737-330">Bu ayar yalnızca IIS için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="db737-330">This setting only applies to IIS.</span></span> <span data-ttu-id="db737-331">Kestrel üzerinde barındırırken davranış varsayılan olarak gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="db737-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="db737-332">Daha fazla bilgi için bkz. [Istek limitleri \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="db737-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="db737-333">ASP.NET Core modülündeki sınırlamalar veya IIS Istek filtreleme modülünün varlığı, karşıya yüklemeleri 2 veya 4 GB ile sınırlandırabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="db737-334">Daha fazla bilgi için bkz. [2 GB 'tan büyük dosya karşıya yüklenemiyor (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="db737-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="db737-335">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="db737-335">Troubleshoot</span></span>

<span data-ttu-id="db737-336">Dosyaları karşıya yükleme ve olası çözümleri ile çalışırken karşılaşılan bazı yaygın sorunlar aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="db737-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="db737-337">Bir IIS sunucusuna dağıtılırken bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="db737-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="db737-338">Aşağıdaki hata karşıya yüklenen dosyanın, sunucunun yapılandırılmış içerik uzunluğunu aştığını gösterir:</span><span class="sxs-lookup"><span data-stu-id="db737-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="db737-339">Limiti artırma hakkında daha fazla bilgi için bkz. [IIS içerik uzunluğu sınırı](#iis-content-length-limit) bölümü.</span><span class="sxs-lookup"><span data-stu-id="db737-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="db737-340">Bağlantı hatası</span><span class="sxs-lookup"><span data-stu-id="db737-340">Connection failure</span></span>

<span data-ttu-id="db737-341">Bir bağlantı hatası ve bir sıfırlama sunucu bağlantısı büyük olasılıkla karşıya yüklenen dosyanın Kestrel 'in en büyük istek gövdesi boyutunu aştığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="db737-342">Daha fazla bilgi için, [Kestrel maksimum istek gövdesi boyutu](#kestrel-maximum-request-body-size) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="db737-343">Kestrel istemci bağlantı limitleri de ayarlama gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="db737-344">Iformfile ile null başvuru özel durumu</span><span class="sxs-lookup"><span data-stu-id="db737-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="db737-345">Denetleyici <xref:Microsoft.AspNetCore.Http.IFormFile> kullanarak karşıya yüklenen dosyaları kabul ediyorsanız, ancak değer `null`, HTML formunun `multipart/form-data``enctype` değerini belirtdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="db737-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="db737-346">Bu öznitelik `<form>` öğesinde ayarlanmamışsa, dosya karşıya yükleme gerçekleşmez ve herhangi bir bağlı <xref:Microsoft.AspNetCore.Http.IFormFile> bağımsız değişken `null`.</span><span class="sxs-lookup"><span data-stu-id="db737-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="db737-347">Ayrıca, [form verilerinde karşıya yükleme adlandırmasının uygulamanın adlandırmayla eşleştiğinden](#match-name-attribute-value-to-parameter-name-of-post-method)emin olun.</span><span class="sxs-lookup"><span data-stu-id="db737-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="db737-348">Akış çok uzun</span><span class="sxs-lookup"><span data-stu-id="db737-348">Stream was too long</span></span>

<span data-ttu-id="db737-349">Bu konudaki örneklerde karşıya yüklenen dosyanın içeriğini tutmak için <xref:System.IO.MemoryStream> bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-349">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="db737-350">Bir `MemoryStream` boyut limiti `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="db737-350">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="db737-351">Uygulamanın dosya yükleme senaryosu, dosya içeriğinin 50 MB 'tan büyük olmasını gerektiriyorsa, karşıya yüklenen dosyanın içeriğini tutmak için tek bir `MemoryStream` kullanmayan alternatif bir yaklaşım kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-351">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="db737-352">ASP.NET Core, daha küçük dosyalar için arabellekli model bağlama ve daha büyük dosyalar için arabelleğe alınmamış akış kullanarak bir veya daha fazla dosyanın yüklenmesini destekler</span><span class="sxs-lookup"><span data-stu-id="db737-352">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="db737-353">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="db737-353">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="db737-354">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="db737-354">Security considerations</span></span>

<span data-ttu-id="db737-355">Kullanıcılara bir sunucuya dosya yükleme yeteneği sağlarken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="db737-355">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="db737-356">Saldırganlar şunları deneyebilir:</span><span class="sxs-lookup"><span data-stu-id="db737-356">Attackers may attempt to:</span></span>

* <span data-ttu-id="db737-357">[Hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) saldırıları yürütün.</span><span class="sxs-lookup"><span data-stu-id="db737-357">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="db737-358">Virüsleri veya kötü amaçlı yazılımları karşıya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="db737-358">Upload viruses or malware.</span></span>
* <span data-ttu-id="db737-359">Ağları ve sunucuları diğer yollarla tehlikeye atabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-359">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="db737-360">Başarılı bir saldırının olasılığını azaltan güvenlik adımları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="db737-360">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="db737-361">Dosyaları, tercihen sistem dışı bir sürücüye, özel bir dosya yükleme alanına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="db737-361">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="db737-362">Ayrılmış bir konum, karşıya yüklenen dosyalar üzerinde güvenlik kısıtlamaları yapmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="db737-362">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="db737-363">Dosya yükleme konumunda yürütme izinlerini devre dışı bırakın.&dagger;</span><span class="sxs-lookup"><span data-stu-id="db737-363">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="db737-364">Karşıya yüklenen dosyaları uygulamayla aynı dizin **ağacında kalıcı yapma** .&dagger;</span><span class="sxs-lookup"><span data-stu-id="db737-364">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="db737-365">Uygulama tarafından belirlenen bir güvenli dosya adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-365">Use a safe file name determined by the app.</span></span> <span data-ttu-id="db737-366">Kullanıcı tarafından belirtilen bir dosya adı veya karşıya yüklenen dosyanın güvenilmeyen dosya adı kullanmayın.&dagger; HTML 'yi görüntülerken güvenilmeyen dosya adını kodlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-366">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="db737-367">Örneğin, dosya adını günlüğe kaydetme veya Kullanıcı arabiriminde görüntüleme (Razor otomatik olarak HTML kodlama çıkışı).</span><span class="sxs-lookup"><span data-stu-id="db737-367">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="db737-368">Uygulamanın tasarım belirtimi için yalnızca onaylanan dosya uzantılarına izin verin.&dagger;</span><span class="sxs-lookup"><span data-stu-id="db737-368">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="db737-369">Sunucuda istemci tarafı denetimlerinin gerçekleştirildiğinden emin olun.&dagger; Istemci tarafı denetimleri kolayca atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db737-369">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="db737-370">Karşıya yüklenen dosyanın boyutunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="db737-370">Check the size of an uploaded file.</span></span> <span data-ttu-id="db737-371">Büyük karşıya yüklemeleri engellemek için en büyük boyut sınırını ayarlayın.&dagger;</span><span class="sxs-lookup"><span data-stu-id="db737-371">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="db737-372">Aynı ada sahip karşıya yüklenen bir dosya tarafından dosyaların üzerine yazılmaması gerektiğinde, dosyayı karşıya yüklemeden önce dosya adını veritabanına veya fiziksel depolamaya göre denetleyin.</span><span class="sxs-lookup"><span data-stu-id="db737-372">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="db737-373">**Dosya depolanmadan önce karşıya yüklenen içerik üzerinde bir virüs/kötü amaçlı yazılım tarayıcısı çalıştırın.**</span><span class="sxs-lookup"><span data-stu-id="db737-373">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="db737-374">örnek uygulama &dagger;ölçütleri karşılayan bir yaklaşımı gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-374">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="db737-375">Kötü amaçlı kodun bir sisteme yüklenmesi genellikle şu şekilde kod yürütmenin ilk adımıdır:</span><span class="sxs-lookup"><span data-stu-id="db737-375">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="db737-376">Sistemin denetimini tamamen elde edin.</span><span class="sxs-lookup"><span data-stu-id="db737-376">Completely gain control of a system.</span></span>
> * <span data-ttu-id="db737-377">Sistemin kilitlenme sonucuyla bir sistemi aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="db737-377">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="db737-378">Kullanıcı veya Sistem verilerinin güvenliğini tehlikeye atabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-378">Compromise user or system data.</span></span>
> * <span data-ttu-id="db737-379">Genel Kullanıcı arabirimine Graffiti uygulayın.</span><span class="sxs-lookup"><span data-stu-id="db737-379">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="db737-380">Kullanıcılardan dosya kabul edilirken saldırı yüzeyi alanını azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="db737-380">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="db737-381">Kısıtlanmamış dosya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-381">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="db737-382">Azure güvenliği: kullanıcılardan dosya kabul edilirken uygun denetimlerin yerinde olduğundan emin olun</span><span class="sxs-lookup"><span data-stu-id="db737-382">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="db737-383">Örnek uygulamadaki örnekler de dahil olmak üzere güvenlik önlemlerini uygulama hakkında daha fazla bilgi için [doğrulama](#validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-383">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="db737-384">Depolama senaryoları</span><span class="sxs-lookup"><span data-stu-id="db737-384">Storage scenarios</span></span>

<span data-ttu-id="db737-385">Dosyalar için ortak depolama seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="db737-385">Common storage options for files include:</span></span>

* <span data-ttu-id="db737-386">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="db737-386">Database</span></span>

  * <span data-ttu-id="db737-387">Küçük dosya yüklemeleri için, bir veritabanı genellikle fiziksel depolama (dosya sistemi veya ağ paylaşımından) seçeneklerinden daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-387">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="db737-388">Kullanıcı verileri için bir veritabanı kaydının alınması eşzamanlı olarak dosya içeriğini (örneğin, avatar görüntüsü) sağlayabildiğinden, veritabanı genellikle fiziksel depolama seçeneklerine göre daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="db737-388">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="db737-389">Bir veritabanı, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-389">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="db737-390">Fiziksel depolama (dosya sistemi veya ağ paylaşma)</span><span class="sxs-lookup"><span data-stu-id="db737-390">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="db737-391">Büyük dosya yüklemeleri için:</span><span class="sxs-lookup"><span data-stu-id="db737-391">For large file uploads:</span></span>
    * <span data-ttu-id="db737-392">Veritabanı limitleri karşıya yükleme boyutunu kısıtlayabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-392">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="db737-393">Fiziksel depolama genellikle bir veritabanındaki depolamadan daha az ekonomik olur.</span><span class="sxs-lookup"><span data-stu-id="db737-393">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="db737-394">Fiziksel depolama, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-394">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="db737-395">Uygulamanın işlemi, depolama konumu için okuma ve yazma izinlerine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-395">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="db737-396">**Hiçbir zaman yürütme izni vermeyin.**</span><span class="sxs-lookup"><span data-stu-id="db737-396">**Never grant execute permission.**</span></span>

* <span data-ttu-id="db737-397">Veri depolama hizmeti (örneğin, [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="db737-397">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="db737-398">Hizmetler genellikle tek hata noktalarına tabi olan şirket içi çözümler üzerinde geliştirilmiş ölçeklenebilirlik ve esneklik sunar.</span><span class="sxs-lookup"><span data-stu-id="db737-398">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="db737-399">Hizmetler büyük depolama altyapısı senaryolarında düşük maliyetlidir.</span><span class="sxs-lookup"><span data-stu-id="db737-399">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="db737-400">Daha fazla bilgi için bkz. [hızlı başlangıç: nesne depolamada blob oluşturmak için .NET kullanma](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="db737-400">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="db737-401">Konu <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>gösterir, ancak <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> bir <xref:System.IO.Stream>çalışırken blob depolamaya <xref:System.IO.FileStream> kaydetmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-401">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="db737-402">Karşıya dosya yükleme senaryoları</span><span class="sxs-lookup"><span data-stu-id="db737-402">File upload scenarios</span></span>

<span data-ttu-id="db737-403">Dosyaları karşıya yüklemek için iki genel yaklaşım arabelleğe alınır ve akışlardır.</span><span class="sxs-lookup"><span data-stu-id="db737-403">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="db737-404">**Ara**</span><span class="sxs-lookup"><span data-stu-id="db737-404">**Buffering**</span></span>

<span data-ttu-id="db737-405">Dosyanın tamamı bir <xref:Microsoft.AspNetCore.Http.IFormFile>okur, bu dosya dosyayı işlemek veya kaydetmek C# için kullanılan bir gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="db737-405">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="db737-406">Dosya karşıya yüklemeleri tarafından kullanılan kaynaklar (disk, bellek), eşzamanlı dosya karşıya yüklemelerinin sayısına ve boyutuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-406">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="db737-407">Bir uygulama çok fazla karşıya yükleme arabelleğini denerse, bellek veya disk alanı tükenirse site çöker.</span><span class="sxs-lookup"><span data-stu-id="db737-407">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="db737-408">Karşıya dosya yükleme boyutu veya sıklığı uygulama kaynaklarını tüketme ise, akış ' ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-408">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="db737-409">64 KB geçen tek bir arabelleğe alınmış dosya, bellekten diskte geçici bir dosyaya taşınır.</span><span class="sxs-lookup"><span data-stu-id="db737-409">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="db737-410">Dosya arabelleğe alma, bu konunun aşağıdaki bölümlerinde ele alınmıştır:</span><span class="sxs-lookup"><span data-stu-id="db737-410">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="db737-411">Fiziksel depolama alanı</span><span class="sxs-lookup"><span data-stu-id="db737-411">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="db737-412">Veritabanınızı</span><span class="sxs-lookup"><span data-stu-id="db737-412">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="db737-413">**Akış**</span><span class="sxs-lookup"><span data-stu-id="db737-413">**Streaming**</span></span>

<span data-ttu-id="db737-414">Dosya çok parçalı bir istekten alınır ve doğrudan uygulama tarafından işlenir veya kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="db737-414">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="db737-415">Akış performansı önemli ölçüde iyileştirmez.</span><span class="sxs-lookup"><span data-stu-id="db737-415">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="db737-416">Akış, dosya karşıya yüklenirken bellek veya disk alanı taleplerini azaltır.</span><span class="sxs-lookup"><span data-stu-id="db737-416">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="db737-417">Akış büyük dosyaları [akış ile büyük dosyaları karşıya yükle](#upload-large-files-with-streaming) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="db737-417">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="db737-418">Fiziksel depolamaya arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-418">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="db737-419">Küçük dosyaları karşıya yüklemek için, çok parçalı bir form kullanın veya JavaScript kullanarak bir POST isteği oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db737-419">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="db737-420">Aşağıdaki örnek, tek bir dosyayı karşıya yüklemek için bir Razor Pages formunun kullanımını gösterir (örnek uygulamada*Pages/Bufferedsinglefileuploadfiziksel. cshtml* ):</span><span class="sxs-lookup"><span data-stu-id="db737-420">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="db737-421">Aşağıdaki örnek, önceki örneğe benzerdir, ancak şunları hariç:</span><span class="sxs-lookup"><span data-stu-id="db737-421">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="db737-422">Form verilerini göndermek için JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-422">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="db737-423">Doğrulama yok.</span><span class="sxs-lookup"><span data-stu-id="db737-423">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="db737-424">[Fetch API 'sini desteklemeyen](https://caniuse.com/#feat=fetch)istemcilerde form gönderisini JavaScript 'te gerçekleştirmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="db737-424">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="db737-425">Fetch Polyfill kullanın (örneğin, [Window. Fetch (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="db737-425">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="db737-426">`XMLHttpRequest`kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-426">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="db737-427">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="db737-427">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="db737-428">Dosya yüklemelerini desteklemek için, HTML formları `multipart/form-data`bir kodlama türü (`enctype`) belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="db737-428">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="db737-429">Birden çok dosyayı karşıya yüklemeyi desteklemek için `files` giriş öğesi için, `<input>` öğesinde `multiple` özniteliğini sağlayın:</span><span class="sxs-lookup"><span data-stu-id="db737-429">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="db737-430">Sunucuya yüklenen tek dosyalara <xref:Microsoft.AspNetCore.Http.IFormFile>kullanılarak [model bağlama](xref:mvc/models/model-binding) yoluyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-430">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="db737-431">Örnek uygulama, veritabanı ve fiziksel depolama senaryoları için birden çok arabellekli dosya yüklemeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-431">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="db737-432">Görüntüleme ve günlüğe kaydetme için dışında <xref:Microsoft.AspNetCore.Http.IFormFile> `FileName` **özelliğini kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="db737-432">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="db737-433">Görüntüleme veya günlüğe kaydetme sırasında, HTML dosya adını kodlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-433">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="db737-434">Saldırgan, tam yollar veya göreli yollar dahil olmak üzere kötü amaçlı bir dosya adı sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-434">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="db737-435">Uygulamalar:</span><span class="sxs-lookup"><span data-stu-id="db737-435">Applications should:</span></span>
>
> * <span data-ttu-id="db737-436">Kullanıcı tarafından sağlanan dosya adının yolunu kaldırın.</span><span class="sxs-lookup"><span data-stu-id="db737-436">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="db737-437">Kullanıcı arabirimi veya günlüğe kaydetme için HTML kodlu, yol tarafından kaldırılan dosya adını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="db737-437">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="db737-438">Depolama için yeni bir rastgele dosya adı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db737-438">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="db737-439">Aşağıdaki kod, dosya adından yolu kaldırır:</span><span class="sxs-lookup"><span data-stu-id="db737-439">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="db737-440">Bu nedenle, şu ana kadar dikkate alınması gereken örnekler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="db737-440">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="db737-441">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-441">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="db737-442">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="db737-442">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="db737-443">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="db737-443">Validation</span></span>](#validation)

<span data-ttu-id="db737-444">Model bağlama ve <xref:Microsoft.AspNetCore.Http.IFormFile>kullanarak dosya karşıya yüklerken, eylem yöntemi şunları kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="db737-444">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="db737-445">Tek bir <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="db737-445">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="db737-446">Birkaç dosyayı temsil eden aşağıdaki koleksiyonlardan herhangi biri:</span><span class="sxs-lookup"><span data-stu-id="db737-446">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="db737-447">[Liste](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="db737-447">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="db737-448">Bağlama, form dosyaları adına göre eşleşir.</span><span class="sxs-lookup"><span data-stu-id="db737-448">Binding matches form files by name.</span></span> <span data-ttu-id="db737-449">Örneğin, `<input type="file" name="formFile">` HTML `name` değeri C# parametre/özellik ile (`FormFile`) eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="db737-449">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="db737-450">Daha fazla bilgi için, [ad öznitelik DEĞERINI Post yönteminin parametre adına eşleştirin](#match-name-attribute-value-to-parameter-name-of-post-method) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-450">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="db737-451">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="db737-451">The following example:</span></span>

* <span data-ttu-id="db737-452">Karşıya yüklenen bir veya daha fazla dosya üzerinden döngü.</span><span class="sxs-lookup"><span data-stu-id="db737-452">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="db737-453">Dosya adı da dahil olmak üzere bir dosyanın tam yolunu döndürmek için [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanır.</span><span class="sxs-lookup"><span data-stu-id="db737-453">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="db737-454">Dosyalar, uygulama tarafından oluşturulan bir dosya adı kullanılarak yerel dosya sistemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="db737-454">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="db737-455">Karşıya yüklenen dosyaların toplam sayısını ve boyutunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="db737-455">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="db737-456">Yol olmadan bir dosya adı oluşturmak için `Path.GetRandomFileName` kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-456">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="db737-457">Aşağıdaki örnekte, yol yapılandırmadan alınır:</span><span class="sxs-lookup"><span data-stu-id="db737-457">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="db737-458"><xref:System.IO.FileStream> geçirilen yol, dosya adını *içermelidir.*</span><span class="sxs-lookup"><span data-stu-id="db737-458">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="db737-459">Dosya adı sağlanmazsa, çalışma zamanında bir <xref:System.UnauthorizedAccessException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="db737-459">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="db737-460"><xref:Microsoft.AspNetCore.Http.IFormFile> tekniği kullanılarak yüklenen dosyalar, işlemeden önce sunucuda veya diskte bellek halinde arabelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="db737-460">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="db737-461">Eylem yönteminde, <xref:Microsoft.AspNetCore.Http.IFormFile> içeriğe <xref:System.IO.Stream>olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-461">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="db737-462">Yerel dosya sistemine ek olarak, dosyalar bir ağ paylaşımında veya [Azure Blob depolama](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)gibi bir dosya depolama hizmetine kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-462">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="db737-463">Karşıya yüklemek için birden çok dosya üzerinde döngü yapan ve güvenli dosya adları kullanan başka bir örnek için, örnek uygulamadaki *Pages/Bufferedmultiplefileuploadfiziksel. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-463">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="db737-464">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , önceki geçici dosyaları silmeden 65.535 'den fazla dosya oluşturulduysa bir <xref:System.IO.IOException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db737-464">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="db737-465">65.535 dosya sınırının sunucu başına sınırı vardır.</span><span class="sxs-lookup"><span data-stu-id="db737-465">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="db737-466">Windows işletim sistemi için bu sınır hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="db737-466">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="db737-467">GetTempFileNameA işlevi</span><span class="sxs-lookup"><span data-stu-id="db737-467">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="db737-468">Bir veritabanına arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-468">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="db737-469">İkili dosya verilerini [Entity Framework](/ef/core/index)kullanarak bir veritabanında depolamak için, varlıkta bir <xref:System.Byte> Array özelliği tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="db737-469">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="db737-470">Bir <xref:Microsoft.AspNetCore.Http.IFormFile>içeren sınıf için bir sayfa modeli özelliği belirtin:</span><span class="sxs-lookup"><span data-stu-id="db737-470">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="db737-471"><xref:Microsoft.AspNetCore.Http.IFormFile>, doğrudan bir eylem yöntemi parametresi veya bir bağlantılı model özelliği olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-471"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="db737-472">Önceki örnekte, bir bağlantılı model özelliği kullanılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="db737-472">The prior example uses a bound model property.</span></span>

<span data-ttu-id="db737-473">`FileUpload` Razor Pages formunda kullanılır:</span><span class="sxs-lookup"><span data-stu-id="db737-473">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="db737-474">Form sunucuya gönderildiğinde, <xref:Microsoft.AspNetCore.Http.IFormFile> bir akışa kopyalayın ve veritabanına bir bayt dizisi olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="db737-474">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="db737-475">Aşağıdaki örnekte, uygulamanın veritabanı bağlamını `_dbContext` depolar:</span><span class="sxs-lookup"><span data-stu-id="db737-475">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="db737-476">Yukarıdaki örnek, örnek uygulamada gösterilen senaryoya benzerdir:</span><span class="sxs-lookup"><span data-stu-id="db737-476">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="db737-477">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="db737-477">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="db737-478">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="db737-478">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="db737-479">İkili verileri ilişkisel veritabanlarında depolarken dikkatli olun, çünkü performansı olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-479">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="db737-480">Doğrulaması olmadan <xref:Microsoft.AspNetCore.Http.IFormFile> `FileName` özelliğine güvenmeyin veya güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="db737-480">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="db737-481">`FileName` özelliği yalnızca görüntüleme amacıyla ve yalnızca HTML kodlaması sonrasında kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-481">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="db737-482">Belirtilen örneklerde dikkate alınması gereken önemli noktalar.</span><span class="sxs-lookup"><span data-stu-id="db737-482">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="db737-483">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-483">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="db737-484">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="db737-484">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="db737-485">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="db737-485">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="db737-486">Akışa sahip büyük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-486">Upload large files with streaming</span></span>

<span data-ttu-id="db737-487">Aşağıdaki örnek, bir denetleyiciyi bir denetleyici eyleminde akışa almak için JavaScript 'in nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-487">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="db737-488">Dosyanın antiforgery belirteci özel bir filtre özniteliği kullanılarak oluşturulur ve istek gövdesi yerine istemci HTTP üst bilgilerine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="db737-488">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="db737-489">Eylem yöntemi karşıya yüklenen verileri doğrudan işlediğinden, form modeli bağlama başka bir özel filtre tarafından devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="db737-489">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="db737-490">Eylem içinde formun içerikleri, her bir `MultipartSection`okuyan, dosyayı işleyen veya içeriği uygun şekilde depolayan bir `MultipartReader`kullanılarak okunur.</span><span class="sxs-lookup"><span data-stu-id="db737-490">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="db737-491">Çok parçalı bölümler okunduktan sonra eylem kendi model bağlamasını gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="db737-491">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="db737-492">İlk sayfa yanıtı formu yükler ve bir tanımlama bilgisine (`GenerateAntiforgeryTokenCookieAttribute` özniteliği aracılığıyla) bir antiforgery belirteci kaydeder.</span><span class="sxs-lookup"><span data-stu-id="db737-492">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="db737-493">Öznitelik, bir istek belirtecine sahip bir tanımlama bilgisi ayarlamak için ASP.NET Core yerleşik [antiforgery desteğini](xref:security/anti-request-forgery) kullanır:</span><span class="sxs-lookup"><span data-stu-id="db737-493">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="db737-494">Model bağlamayı devre dışı bırakmak için `DisableFormValueModelBindingAttribute` kullanılır:</span><span class="sxs-lookup"><span data-stu-id="db737-494">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="db737-495">Örnek uygulamada `GenerateAntiforgeryTokenCookieAttribute` ve `DisableFormValueModelBindingAttribute`, [`Startup.ConfigureServices` kuralları](xref:razor-pages/razor-pages-conventions)kullanılarak Razor Pages `/StreamedSingleFileUploadDb` ve `/StreamedSingleFileUploadPhysical` sayfa uygulama modellerine filtre olarak uygulanır:</span><span class="sxs-lookup"><span data-stu-id="db737-495">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="db737-496">Model bağlama formu okumadığından formdan bağlanan parametreler bağlanamaz (sorgu, yol ve başlık çalışmaya devam eder).</span><span class="sxs-lookup"><span data-stu-id="db737-496">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="db737-497">Action yöntemi doğrudan `Request` özelliği ile birlikte çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-497">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="db737-498">Her bölümü okumak için bir `MultipartReader` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-498">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="db737-499">Anahtar/değer verileri bir `KeyValueAccumulator`depolanır.</span><span class="sxs-lookup"><span data-stu-id="db737-499">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="db737-500">Çok parçalı bölümler okunduktan sonra, `KeyValueAccumulator` içeriği form verilerini bir model türüne bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-500">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="db737-501">EF Core ile bir veritabanına akışa yönelik tüm `StreamingController.UploadDatabase` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db737-501">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="db737-502">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="db737-502">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="db737-503">Fiziksel bir konuma akışa yönelik tüm `StreamingController.UploadPhysical` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db737-503">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="db737-504">Örnek uygulamada, doğrulama denetimleri `FileHelpers.ProcessStreamedFile`tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="db737-504">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="db737-505">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="db737-505">Validation</span></span>

<span data-ttu-id="db737-506">Örnek uygulamanın `FileHelpers` sınıfı, arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> ve akış dosya yüklemeleri için birkaç denetim gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-506">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="db737-507">Örnek uygulamada arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> dosya yüklemelerini işlemek için, *Utilities/Fileyardımcılar. cs* dosyasındaki `ProcessFormFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-507">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="db737-508">Akış dosyalarını işlemek için aynı dosyadaki `ProcessStreamedFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-508">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="db737-509">Örnek uygulamada gösterilen doğrulama işleme yöntemleri karşıya yüklenen dosyaların içeriğini taramaz.</span><span class="sxs-lookup"><span data-stu-id="db737-509">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="db737-510">Çoğu üretim senaryosunda, dosyanın kullanıcılara veya diğer sistemlere kullanılabilir hale getirilmesi için dosya üzerinde bir virüs/kötü amaçlı yazılım tarayıcı API 'SI kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-510">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="db737-511">Konu örneği, doğrulama tekniklerine yönelik çalışan bir örnek sağlasa da şunları gerçekleştirmediğiniz takdirde bir üretim uygulamasında `FileHelpers` sınıfını uygulamayın:</span><span class="sxs-lookup"><span data-stu-id="db737-511">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="db737-512">Uygulamayı tam olarak anlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-512">Fully understand the implementation.</span></span>
> * <span data-ttu-id="db737-513">Uygulamayı uygulamanın ortamı ve belirtimleri için uygun şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="db737-513">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="db737-514">**Bu gereksinimleri bilmeden bir uygulamada güvenlik kodunu hiçbir şekilde sayısının fark gözetmeden uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="db737-514">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="db737-515">İçerik doğrulama</span><span class="sxs-lookup"><span data-stu-id="db737-515">Content validation</span></span>

<span data-ttu-id="db737-516">**Karşıya yüklenen içerikte üçüncü taraf bir virüs/kötü amaçlı yazılım tarama API 'SI kullanın.**</span><span class="sxs-lookup"><span data-stu-id="db737-516">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="db737-517">Dosyaları tarama, yüksek hacimli senaryolarda sunucu kaynaklarında yoğun bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="db737-517">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="db737-518">Dosya tarama nedeniyle istek işleme performansı azaldığında, tarama işini, muhtemelen uygulamanın sunucusundan farklı bir sunucuda çalışan bir [arka plan hizmetine](xref:fundamentals/host/hosted-services)devredere göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="db737-518">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="db737-519">Genellikle, arka plan virüs tarayıcısı tarafından denetlene kadar karşıya yüklenen dosyalar karantinaya alınmış bir alanda tutulur.</span><span class="sxs-lookup"><span data-stu-id="db737-519">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="db737-520">Bir dosya geçtiğinde dosya normal dosya depolama konumuna taşınır.</span><span class="sxs-lookup"><span data-stu-id="db737-520">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="db737-521">Bu adımlar genellikle bir dosyanın tarama durumunu gösteren bir veritabanı kaydıyla birlikte gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="db737-521">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="db737-522">Böyle bir yaklaşım kullanarak, uygulama ve uygulama sunucusu isteklere yanıt vermeye odaklanmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="db737-522">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="db737-523">Dosya Uzantısı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="db737-523">File extension validation</span></span>

<span data-ttu-id="db737-524">Karşıya yüklenen dosyanın uzantısı izin verilen uzantılar listesine göre denetlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="db737-524">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="db737-525">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="db737-525">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="db737-526">Dosya imzası doğrulaması</span><span class="sxs-lookup"><span data-stu-id="db737-526">File signature validation</span></span>

<span data-ttu-id="db737-527">Bir dosyanın imzası, bir dosyanın başlangıcında ilk birkaç bayta göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="db737-527">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="db737-528">Bu baytlar, uzantının dosyanın içeriğiyle eşleşip eşleşmediğini göstermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-528">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="db737-529">Örnek uygulama, birkaç ortak dosya türü için dosya imzalarını denetler.</span><span class="sxs-lookup"><span data-stu-id="db737-529">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="db737-530">Aşağıdaki örnekte, bir JPEG görüntüsünün dosya imzası, dosyaya karşı denetlenir:</span><span class="sxs-lookup"><span data-stu-id="db737-530">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="db737-531">Ek dosya imzaları almak için [Dosya Imzaları veritabanı](https://www.filesignatures.net/) ve resmi dosya belirtimleri bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-531">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="db737-532">Dosya adı güvenliği</span><span class="sxs-lookup"><span data-stu-id="db737-532">File name security</span></span>

<span data-ttu-id="db737-533">Fiziksel depolamaya bir dosyayı kaydetmek için hiçbir şekilde istemci tarafından sağlanan dosya adı kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="db737-533">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="db737-534">Geçici depolama için tam yol (dosya adı da dahil olmak üzere) oluşturmak için [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) veya [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanarak dosya için güvenli bir dosya adı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db737-534">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="db737-535">Razor otomatik olarak HTML, görüntüleme için özellik değerlerini kodlar.</span><span class="sxs-lookup"><span data-stu-id="db737-535">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="db737-536">Aşağıdaki kodun kullanımı güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="db737-536">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="db737-537">Razor dışında, her zaman bir kullanıcının isteğinden dosya adı içeriği <xref:System.Net.WebUtility.HtmlEncode*>.</span><span class="sxs-lookup"><span data-stu-id="db737-537">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="db737-538">Birçok uygulama, dosyanın var olduğunu bir denetim içermelidir; Aksi takdirde, dosyanın üzerine aynı ada sahip bir dosya yazılır.</span><span class="sxs-lookup"><span data-stu-id="db737-538">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="db737-539">Uygulamanızın belirtimlerini karşılamak için ek mantık sağlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-539">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="db737-540">Boyut doğrulaması</span><span class="sxs-lookup"><span data-stu-id="db737-540">Size validation</span></span>

<span data-ttu-id="db737-541">Karşıya yüklenen dosyaların boyutunu sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="db737-541">Limit the size of uploaded files.</span></span>

<span data-ttu-id="db737-542">Örnek uygulamada, dosyanın boyutu 2 MB ile sınırlıdır (bayt cinsinden gösterilir).</span><span class="sxs-lookup"><span data-stu-id="db737-542">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="db737-543">Sınır, *appSettings. JSON* dosyasındaki [yapılandırma](xref:fundamentals/configuration/index) yoluyla sağlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-543">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="db737-544">`FileSizeLimit` `PageModel` sınıflarına eklenmiş:</span><span class="sxs-lookup"><span data-stu-id="db737-544">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="db737-545">Dosya boyutu sınırı aştığında, dosya reddedilir:</span><span class="sxs-lookup"><span data-stu-id="db737-545">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="db737-546">Name öznitelik değerini POST yönteminin Parameter adı ile Eşleştir</span><span class="sxs-lookup"><span data-stu-id="db737-546">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="db737-547">Form verileri oluşturan veya JavaScript 'in `FormData` doğrudan kullanan Razor olmayan formlarda, formun öğesinde veya `FormData` belirtilen adın, denetleyicinin eyleminde parametrenin adıyla eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="db737-547">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="db737-548">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="db737-548">In the following example:</span></span>

* <span data-ttu-id="db737-549">Bir `<input>` öğesi kullanılırken, `name` özniteliği `battlePlans`değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-549">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="db737-550">JavaScript içinde `FormData` kullanırken ad, `battlePlans`değer olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="db737-550">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="db737-551">C# Yöntemin parametresi için eşleşen bir ad kullanın (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="db737-551">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="db737-552">`Upload`adlı Razor Pages sayfa işleyicisi yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="db737-552">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="db737-553">MVC POST denetleyicisi eylem yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="db737-553">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="db737-554">Sunucu ve uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="db737-554">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="db737-555">Çok parçalı gövde uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="db737-555">Multipart body length limit</span></span>

<span data-ttu-id="db737-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> her bir çok parçalı gövdenin uzunluğu için sınır ayarlar.</span><span class="sxs-lookup"><span data-stu-id="db737-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="db737-557">Bu sınırı aşan form bölümleri ayrıştırıldığında bir <xref:System.IO.InvalidDataException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db737-557">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="db737-558">Varsayılan değer 134.217.728 ' dir (128 MB).</span><span class="sxs-lookup"><span data-stu-id="db737-558">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="db737-559">`Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarını kullanarak sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="db737-559">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="db737-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>, tek sayfa veya eylem için <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="db737-561">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices`bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="db737-561">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="db737-562">Razor Pages bir uygulamada veya bir MVC uygulamasında, filtreyi sayfa modeline veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="db737-562">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="db737-563">Kestrel maksimum istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="db737-563">Kestrel maximum request body size</span></span>

<span data-ttu-id="db737-564">Kestrel tarafından barındırılan uygulamalar için, varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu, yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="db737-564">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="db737-565">[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel Server seçeneğini kullanarak sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="db737-565">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

<span data-ttu-id="db737-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>, tek sayfa veya eylem için [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) ayarlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db737-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="db737-567">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices`bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="db737-567">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="db737-568">Bir Razor sayfaları uygulamasında veya bir MVC uygulamasında, filtreyi sayfa işleyici sınıfına veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="db737-568">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="db737-569">Diğer Kestrel limitleri</span><span class="sxs-lookup"><span data-stu-id="db737-569">Other Kestrel limits</span></span>

<span data-ttu-id="db737-570">Kestrel tarafından barındırılan uygulamalar için diğer Kestrel limitleri de uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="db737-570">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="db737-571">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="db737-571">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="db737-572">İstek ve yanıt veri ücretleri</span><span class="sxs-lookup"><span data-stu-id="db737-572">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="db737-573">IIS içerik uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="db737-573">IIS content length limit</span></span>

<span data-ttu-id="db737-574">Varsayılan istek sınırı (`maxAllowedContentLength`), yaklaşık 28.6 MB olan 30.000.000 bayttır.</span><span class="sxs-lookup"><span data-stu-id="db737-574">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="db737-575">*Web. config* dosyasında sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="db737-575">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="db737-576">Bu ayar yalnızca IIS için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="db737-576">This setting only applies to IIS.</span></span> <span data-ttu-id="db737-577">Kestrel üzerinde barındırırken davranış varsayılan olarak gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="db737-577">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="db737-578">Daha fazla bilgi için bkz. [Istek limitleri \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="db737-578">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="db737-579">ASP.NET Core modülündeki sınırlamalar veya IIS Istek filtreleme modülünün varlığı, karşıya yüklemeleri 2 veya 4 GB ile sınırlandırabilir.</span><span class="sxs-lookup"><span data-stu-id="db737-579">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="db737-580">Daha fazla bilgi için bkz. [2 GB 'tan büyük dosya karşıya yüklenemiyor (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="db737-580">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="db737-581">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="db737-581">Troubleshoot</span></span>

<span data-ttu-id="db737-582">Dosyaları karşıya yükleme ve olası çözümleri ile çalışırken karşılaşılan bazı yaygın sorunlar aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="db737-582">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="db737-583">Bir IIS sunucusuna dağıtılırken bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="db737-583">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="db737-584">Aşağıdaki hata karşıya yüklenen dosyanın, sunucunun yapılandırılmış içerik uzunluğunu aştığını gösterir:</span><span class="sxs-lookup"><span data-stu-id="db737-584">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="db737-585">Limiti artırma hakkında daha fazla bilgi için bkz. [IIS içerik uzunluğu sınırı](#iis-content-length-limit) bölümü.</span><span class="sxs-lookup"><span data-stu-id="db737-585">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="db737-586">Bağlantı hatası</span><span class="sxs-lookup"><span data-stu-id="db737-586">Connection failure</span></span>

<span data-ttu-id="db737-587">Bir bağlantı hatası ve bir sıfırlama sunucu bağlantısı büyük olasılıkla karşıya yüklenen dosyanın Kestrel 'in en büyük istek gövdesi boyutunu aştığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="db737-587">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="db737-588">Daha fazla bilgi için, [Kestrel maksimum istek gövdesi boyutu](#kestrel-maximum-request-body-size) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="db737-588">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="db737-589">Kestrel istemci bağlantı limitleri de ayarlama gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="db737-589">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="db737-590">Iformfile ile null başvuru özel durumu</span><span class="sxs-lookup"><span data-stu-id="db737-590">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="db737-591">Denetleyici <xref:Microsoft.AspNetCore.Http.IFormFile> kullanarak karşıya yüklenen dosyaları kabul ediyorsanız, ancak değer `null`, HTML formunun `multipart/form-data``enctype` değerini belirtdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="db737-591">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="db737-592">Bu öznitelik `<form>` öğesinde ayarlanmamışsa, dosya karşıya yükleme gerçekleşmez ve herhangi bir bağlı <xref:Microsoft.AspNetCore.Http.IFormFile> bağımsız değişken `null`.</span><span class="sxs-lookup"><span data-stu-id="db737-592">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="db737-593">Ayrıca, [form verilerinde karşıya yükleme adlandırmasının uygulamanın adlandırmayla eşleştiğinden](#match-name-attribute-value-to-parameter-name-of-post-method)emin olun.</span><span class="sxs-lookup"><span data-stu-id="db737-593">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="db737-594">Akış çok uzun</span><span class="sxs-lookup"><span data-stu-id="db737-594">Stream was too long</span></span>

<span data-ttu-id="db737-595">Bu konudaki örneklerde karşıya yüklenen dosyanın içeriğini tutmak için <xref:System.IO.MemoryStream> bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="db737-595">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="db737-596">Bir `MemoryStream` boyut limiti `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="db737-596">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="db737-597">Uygulamanın dosya yükleme senaryosu, dosya içeriğinin 50 MB 'tan büyük olmasını gerektiriyorsa, karşıya yüklenen dosyanın içeriğini tutmak için tek bir `MemoryStream` kullanmayan alternatif bir yaklaşım kullanın.</span><span class="sxs-lookup"><span data-stu-id="db737-597">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="db737-598">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="db737-598">Additional resources</span></span>

* [<span data-ttu-id="db737-599">Kısıtlanmamış dosya yükleme</span><span class="sxs-lookup"><span data-stu-id="db737-599">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [<span data-ttu-id="db737-600">Azure güvenliği: güvenlik çerçevesi: giriş doğrulaması | Karşı</span><span class="sxs-lookup"><span data-stu-id="db737-600">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="db737-601">Azure bulut tasarım desenleri: Valet anahtar düzeni</span><span class="sxs-lookup"><span data-stu-id="db737-601">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
