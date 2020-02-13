---
title: ASP.NET Core dosyaları karşıya yükleme
author: guardrex
description: ASP.NET Core MVC 'de dosyaları karşıya yüklemek için model bağlama ve akışı kullanma.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2020
uid: mvc/models/file-uploads
ms.openlocfilehash: 56fd26c1864089558f5cd89f693dc86ea30c3331
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172466"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="f5cf9-103">ASP.NET Core dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="f5cf9-104">[Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)ve [Rutger fırtınası](https://github.com/rutix) tarafından</span><span class="sxs-lookup"><span data-stu-id="f5cf9-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f5cf9-105">ASP.NET Core, daha küçük dosyalar için arabellekli model bağlama ve daha büyük dosyalar için arabelleğe alınmamış akış kullanarak bir veya daha fazla dosyanın yüklenmesini destekler</span><span class="sxs-lookup"><span data-stu-id="f5cf9-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="f5cf9-106">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f5cf9-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="f5cf9-107">Güvenlikle ilgili dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="f5cf9-107">Security considerations</span></span>

<span data-ttu-id="f5cf9-108">Kullanıcılara bir sunucuya dosya yükleme yeteneği sağlarken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="f5cf9-109">Saldırganlar şunları deneyebilir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="f5cf9-110">[Hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) saldırıları yürütün.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="f5cf9-111">Virüsleri veya kötü amaçlı yazılımları karşıya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="f5cf9-112">Ağları ve sunucuları diğer yollarla tehlikeye atabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="f5cf9-113">Başarılı bir saldırının olasılığını azaltan güvenlik adımları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="f5cf9-114">Dosyaları, tercihen sistem dışı bir sürücüye, özel bir dosya yükleme alanına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="f5cf9-115">Ayrılmış bir konum, karşıya yüklenen dosyalar üzerinde güvenlik kısıtlamaları yapmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="f5cf9-116">Dosya yükleme konumunda yürütme izinlerini devre dışı bırakın.&dagger;</span><span class="sxs-lookup"><span data-stu-id="f5cf9-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="f5cf9-117">Karşıya yüklenen dosyaları uygulamayla aynı dizin **ağacında kalıcı yapma** .&dagger;</span><span class="sxs-lookup"><span data-stu-id="f5cf9-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="f5cf9-118">Uygulama tarafından belirlenen bir güvenli dosya adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="f5cf9-119">Kullanıcı tarafından belirtilen bir dosya adı veya karşıya yüklenen dosyanın güvenilmeyen dosya adı kullanmayın.&dagger; HTML 'yi görüntülerken güvenilmeyen dosya adını kodlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="f5cf9-120">Örneğin, dosya adını günlüğe kaydetme veya Kullanıcı arabiriminde görüntüleme (Razor otomatik olarak HTML kodlama çıkışı).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="f5cf9-121">Uygulamanın tasarım belirtimi için yalnızca onaylanan dosya uzantılarına izin verin.&dagger;</span><span class="sxs-lookup"><span data-stu-id="f5cf9-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="f5cf9-122">Sunucuda istemci tarafı denetimlerinin gerçekleştirildiğinden emin olun.&dagger; Istemci tarafı denetimleri kolayca atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="f5cf9-123">Karşıya yüklenen dosyanın boyutunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="f5cf9-124">Büyük karşıya yüklemeleri engellemek için en büyük boyut sınırını ayarlayın.&dagger;</span><span class="sxs-lookup"><span data-stu-id="f5cf9-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="f5cf9-125">Aynı ada sahip karşıya yüklenen bir dosya tarafından dosyaların üzerine yazılmaması gerektiğinde, dosyayı karşıya yüklemeden önce dosya adını veritabanına veya fiziksel depolamaya göre denetleyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="f5cf9-126">**Dosya depolanmadan önce karşıya yüklenen içerik üzerinde bir virüs/kötü amaçlı yazılım tarayıcısı çalıştırın.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="f5cf9-127">örnek uygulama &dagger;ölçütleri karşılayan bir yaklaşımı gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="f5cf9-128">Kötü amaçlı bir kodun bir sisteme karşıya yükleme için kod yürütme için ilk adımı sık şöyledir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="f5cf9-129">Sistemin denetimini tamamen elde edin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="f5cf9-130">Sistemin kilitlenme sonucuyla bir sistemi aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="f5cf9-131">Kullanıcı veya sistem verilerini tehlikeye.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="f5cf9-132">Genel Kullanıcı arabirimine Graffiti uygulayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="f5cf9-133">Kullanıcıların dosyaları kabul ederken saldırı yüzey alanı azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="f5cf9-134">Kısıtlanmamış dosya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-134">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="f5cf9-135">Azure güvenliği: kullanıcılardan dosya kabul edilirken uygun denetimlerin yerinde olduğundan emin olun</span><span class="sxs-lookup"><span data-stu-id="f5cf9-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="f5cf9-136">Örnek uygulamadaki örnekler de dahil olmak üzere güvenlik önlemlerini uygulama hakkında daha fazla bilgi için [doğrulama](#validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="f5cf9-137">Depolama senaryoları</span><span class="sxs-lookup"><span data-stu-id="f5cf9-137">Storage scenarios</span></span>

<span data-ttu-id="f5cf9-138">Dosyalar için ortak depolama seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-138">Common storage options for files include:</span></span>

* <span data-ttu-id="f5cf9-139">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-139">Database</span></span>

  * <span data-ttu-id="f5cf9-140">Küçük dosya yüklemeleri için, bir veritabanı genellikle fiziksel depolama (dosya sistemi veya ağ paylaşımından) seçeneklerinden daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="f5cf9-141">Kullanıcı verileri için bir veritabanı kaydının alınması eşzamanlı olarak dosya içeriğini (örneğin, avatar görüntüsü) sağlayabildiğinden, veritabanı genellikle fiziksel depolama seçeneklerine göre daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="f5cf9-142">Bir veritabanı, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="f5cf9-143">Fiziksel depolama (dosya sistemi veya ağ paylaşma)</span><span class="sxs-lookup"><span data-stu-id="f5cf9-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="f5cf9-144">Büyük dosya yüklemeleri için:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-144">For large file uploads:</span></span>
    * <span data-ttu-id="f5cf9-145">Veritabanı limitleri karşıya yükleme boyutunu kısıtlayabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="f5cf9-146">Fiziksel depolama genellikle bir veritabanındaki depolamadan daha az ekonomik olur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="f5cf9-147">Fiziksel depolama, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="f5cf9-148">Uygulamanın işlemi, depolama konumu için okuma ve yazma izinlerine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="f5cf9-149">**Hiçbir zaman yürütme izni vermeyin.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="f5cf9-150">Veri depolama hizmeti (örneğin, [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="f5cf9-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="f5cf9-151">Hizmetler genellikle tek hata noktalarına tabi olan şirket içi çözümler üzerinde geliştirilmiş ölçeklenebilirlik ve esneklik sunar.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="f5cf9-152">Hizmetler büyük depolama altyapısı senaryolarında düşük maliyetlidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="f5cf9-153">Daha fazla bilgi için bkz. [hızlı başlangıç: nesne depolamada blob oluşturmak için .NET kullanma](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="f5cf9-154">Karşıya dosya yükleme senaryoları</span><span class="sxs-lookup"><span data-stu-id="f5cf9-154">File upload scenarios</span></span>

<span data-ttu-id="f5cf9-155">Dosyaları karşıya yüklemek için iki genel yaklaşım arabelleğe alınır ve akışlardır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="f5cf9-156">**Ara**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-156">**Buffering**</span></span>

<span data-ttu-id="f5cf9-157">Dosyanın tamamı bir <xref:Microsoft.AspNetCore.Http.IFormFile>okur, bu dosya dosyayı işlemek veya kaydetmek C# için kullanılan bir gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="f5cf9-158">Dosya karşıya yüklemeleri tarafından kullanılan kaynaklar (disk, bellek), eşzamanlı dosya karşıya yüklemelerinin sayısına ve boyutuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="f5cf9-159">Bir uygulama çok fazla karşıya yükleme arabelleğini denerse, bellek veya disk alanı tükenirse site çöker.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="f5cf9-160">Karşıya dosya yükleme boyutu veya sıklığı uygulama kaynaklarını tüketme ise, akış ' ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="f5cf9-161">64 KB geçen tek bir arabelleğe alınmış dosya, bellekten diskte geçici bir dosyaya taşınır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="f5cf9-162">Dosya arabelleğe alma, bu konunun aşağıdaki bölümlerinde ele alınmıştır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="f5cf9-163">Fiziksel depolama alanı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="f5cf9-164">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-164">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="f5cf9-165">**Akış**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-165">**Streaming**</span></span>

<span data-ttu-id="f5cf9-166">Dosya çok parçalı bir istekten alınır ve doğrudan uygulama tarafından işlenir veya kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="f5cf9-167">Akış performansı önemli ölçüde iyileştirmez.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="f5cf9-168">Akış, dosya karşıya yüklenirken bellek veya disk alanı taleplerini azaltır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="f5cf9-169">Akış büyük dosyaları [akış ile büyük dosyaları karşıya yükle](#upload-large-files-with-streaming) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="f5cf9-170">Fiziksel depolamaya arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="f5cf9-171">Küçük dosyaları karşıya yüklemek için, çok parçalı bir form kullanın veya JavaScript kullanarak bir POST isteği oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="f5cf9-172">Aşağıdaki örnek, tek bir dosyayı karşıya yüklemek için bir Razor Pages formunun kullanımını gösterir (örnek uygulamada*Pages/Bufferedsinglefileuploadfiziksel. cshtml* ):</span><span class="sxs-lookup"><span data-stu-id="f5cf9-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="f5cf9-173">Aşağıdaki örnek, önceki örneğe benzerdir, ancak şunları hariç:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="f5cf9-174">Form verilerini göndermek için JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="f5cf9-175">Doğrulama yok.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-175">There's no validation.</span></span>

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

<span data-ttu-id="f5cf9-176">[Fetch API 'sini desteklemeyen](https://caniuse.com/#feat=fetch)istemcilerde form gönderisini JavaScript 'te gerçekleştirmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="f5cf9-177">Fetch Polyfill kullanın (örneğin, [Window. Fetch (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="f5cf9-178">`XMLHttpRequest`kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="f5cf9-179">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-179">For example:</span></span>

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

<span data-ttu-id="f5cf9-180">Dosya yüklemelerini desteklemek için, HTML formları `multipart/form-data`bir kodlama türü (`enctype`) belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="f5cf9-181">Birden çok dosyayı karşıya yüklemeyi desteklemek için `files` giriş öğesi için, `<input>` öğesinde `multiple` özniteliğini sağlayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="f5cf9-182">Sunucuya yüklenen tek dosyalara <xref:Microsoft.AspNetCore.Http.IFormFile>kullanılarak [model bağlama](xref:mvc/models/model-binding) yoluyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="f5cf9-183">Örnek uygulama, veritabanı ve fiziksel depolama senaryoları için birden çok arabellekli dosya yüklemeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="f5cf9-184">Görüntüleme ve günlüğe kaydetme için dışında <xref:Microsoft.AspNetCore.Http.IFormFile> `FileName` **özelliğini kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-184">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="f5cf9-185">Görüntüleme veya günlüğe kaydetme sırasında, HTML dosya adını kodlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-185">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="f5cf9-186">Saldırgan, tam yollar veya göreli yollar dahil olmak üzere kötü amaçlı bir dosya adı sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-186">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="f5cf9-187">Uygulamalar:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-187">Applications should:</span></span>
>
> * <span data-ttu-id="f5cf9-188">Kullanıcı tarafından sağlanan dosya adının yolunu kaldırın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-188">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="f5cf9-189">Kullanıcı arabirimi veya günlüğe kaydetme için HTML kodlu, yol tarafından kaldırılan dosya adını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-189">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="f5cf9-190">Depolama için yeni bir rastgele dosya adı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-190">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="f5cf9-191">Aşağıdaki kod, dosya adından yolu kaldırır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-191">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="f5cf9-192">Bu nedenle, şu ana kadar dikkate alınması gereken örnekler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-192">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="f5cf9-193">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-193">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="f5cf9-194">Güvenlikle ilgili dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="f5cf9-194">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="f5cf9-195">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="f5cf9-195">Validation</span></span>](#validation)

<span data-ttu-id="f5cf9-196">Model bağlama ve <xref:Microsoft.AspNetCore.Http.IFormFile>kullanarak dosya karşıya yüklerken, eylem yöntemi şunları kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-196">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="f5cf9-197">Tek bir <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-197">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="f5cf9-198">Birkaç dosyayı temsil eden aşağıdaki koleksiyonlardan herhangi biri:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-198">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="f5cf9-199">[Liste](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="f5cf9-199">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="f5cf9-200">Bağlama, form dosyaları adına göre eşleşir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-200">Binding matches form files by name.</span></span> <span data-ttu-id="f5cf9-201">Örneğin, `<input type="file" name="formFile">` HTML `name` değeri C# parametre/özellik ile (`FormFile`) eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-201">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="f5cf9-202">Daha fazla bilgi için, [ad öznitelik DEĞERINI Post yönteminin parametre adına eşleştirin](#match-name-attribute-value-to-parameter-name-of-post-method) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-202">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="f5cf9-203">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-203">The following example:</span></span>

* <span data-ttu-id="f5cf9-204">Karşıya yüklenen bir veya daha fazla dosya üzerinden döngü.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-204">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="f5cf9-205">Dosya adı da dahil olmak üzere bir dosyanın tam yolunu döndürmek için [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-205">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="f5cf9-206">Dosyalar, uygulama tarafından oluşturulan bir dosya adı kullanılarak yerel dosya sistemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-206">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="f5cf9-207">Karşıya yüklenen dosyaların toplam sayısını ve boyutunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-207">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="f5cf9-208">Yol olmadan bir dosya adı oluşturmak için `Path.GetRandomFileName` kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-208">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="f5cf9-209">Aşağıdaki örnekte, yol yapılandırmadan alınır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-209">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="f5cf9-210"><xref:System.IO.FileStream> geçirilen yol, dosya adını *içermelidir.*</span><span class="sxs-lookup"><span data-stu-id="f5cf9-210">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="f5cf9-211">Dosya adı sağlanmazsa, çalışma zamanında bir <xref:System.UnauthorizedAccessException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-211">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="f5cf9-212"><xref:Microsoft.AspNetCore.Http.IFormFile> tekniği kullanılarak yüklenen dosyalar, işlemeden önce sunucuda veya diskte bellek halinde arabelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-212">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="f5cf9-213">Eylem yönteminde, <xref:Microsoft.AspNetCore.Http.IFormFile> içeriğe <xref:System.IO.Stream>olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-213">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="f5cf9-214">Yerel dosya sistemine ek olarak, dosyalar bir ağ paylaşımında veya [Azure Blob depolama](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)gibi bir dosya depolama hizmetine kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-214">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="f5cf9-215">Karşıya yüklemek için birden çok dosya üzerinde döngü yapan ve güvenli dosya adları kullanan başka bir örnek için, örnek uygulamadaki *Pages/Bufferedmultiplefileuploadfiziksel. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-215">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="f5cf9-216">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , önceki geçici dosyaları silmeden 65.535 'den fazla dosya oluşturulduysa bir <xref:System.IO.IOException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-216">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="f5cf9-217">65.535 dosya sınırının sunucu başına sınırı vardır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-217">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="f5cf9-218">Windows işletim sistemi için bu sınır hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-218">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="f5cf9-219">GetTempFileNameA işlevi</span><span class="sxs-lookup"><span data-stu-id="f5cf9-219">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="f5cf9-220">Bir veritabanına arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-220">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="f5cf9-221">İkili dosya verilerini [Entity Framework](/ef/core/index)kullanarak bir veritabanında depolamak için, varlıkta bir <xref:System.Byte> Array özelliği tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-221">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="f5cf9-222">Bir <xref:Microsoft.AspNetCore.Http.IFormFile>içeren sınıf için bir sayfa modeli özelliği belirtin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-222">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="f5cf9-223"><xref:Microsoft.AspNetCore.Http.IFormFile>, doğrudan bir eylem yöntemi parametresi veya bir bağlantılı model özelliği olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-223"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="f5cf9-224">Önceki örnekte, bir bağlantılı model özelliği kullanılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-224">The prior example uses a bound model property.</span></span>

<span data-ttu-id="f5cf9-225">`FileUpload` Razor Pages formunda kullanılır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-225">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="f5cf9-226">Form sunucuya gönderildiğinde, <xref:Microsoft.AspNetCore.Http.IFormFile> bir akışa kopyalayın ve veritabanına bir bayt dizisi olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-226">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="f5cf9-227">Aşağıdaki örnekte, uygulamanın veritabanı bağlamını `_dbContext` depolar:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-227">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="f5cf9-228">Yukarıdaki örnek, örnek uygulamada gösterilen senaryoya benzerdir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-228">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="f5cf9-229">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5cf9-229">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="f5cf9-230">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="f5cf9-230">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="f5cf9-231">İkili verileri ilişkisel veritabanlarında depolarken dikkatli olun, çünkü performansı olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-231">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="f5cf9-232">Doğrulaması olmadan <xref:Microsoft.AspNetCore.Http.IFormFile> `FileName` özelliğine güvenmeyin veya güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-232">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="f5cf9-233">`FileName` özelliği yalnızca görüntüleme amacıyla ve yalnızca HTML kodlaması sonrasında kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-233">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="f5cf9-234">Belirtilen örneklerde dikkate alınması gereken önemli noktalar.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-234">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="f5cf9-235">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-235">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="f5cf9-236">Güvenlikle ilgili dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="f5cf9-236">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="f5cf9-237">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="f5cf9-237">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="f5cf9-238">Akışa sahip büyük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-238">Upload large files with streaming</span></span>

<span data-ttu-id="f5cf9-239">Aşağıdaki örnek, bir denetleyiciyi bir denetleyici eyleminde akışa almak için JavaScript 'in nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-239">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="f5cf9-240">Dosyanın antiforgery belirteci özel bir filtre özniteliği kullanılarak oluşturulur ve istek gövdesi yerine istemci HTTP üst bilgilerine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-240">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="f5cf9-241">Eylem yöntemi karşıya yüklenen verileri doğrudan işlediğinden, form modeli bağlama başka bir özel filtre tarafından devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-241">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="f5cf9-242">Eylem içinde formun içerikleri, her bir `MultipartSection`okuyan, dosyayı işleyen veya içeriği uygun şekilde depolayan bir `MultipartReader`kullanılarak okunur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-242">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="f5cf9-243">Çok parçalı bölümler okunduktan sonra eylem kendi model bağlamasını gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-243">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="f5cf9-244">İlk sayfa yanıtı formu yükler ve bir tanımlama bilgisine (`GenerateAntiforgeryTokenCookieAttribute` özniteliği aracılığıyla) bir antiforgery belirteci kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-244">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="f5cf9-245">Öznitelik, bir istek belirtecine sahip bir tanımlama bilgisi ayarlamak için ASP.NET Core yerleşik [antiforgery desteğini](xref:security/anti-request-forgery) kullanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-245">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="f5cf9-246">Model bağlamayı devre dışı bırakmak için `DisableFormValueModelBindingAttribute` kullanılır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-246">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="f5cf9-247">Örnek uygulamada `GenerateAntiforgeryTokenCookieAttribute` ve `DisableFormValueModelBindingAttribute`, [`Startup.ConfigureServices` kuralları](xref:razor-pages/razor-pages-conventions)kullanılarak Razor Pages `/StreamedSingleFileUploadDb` ve `/StreamedSingleFileUploadPhysical` sayfa uygulama modellerine filtre olarak uygulanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-247">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="f5cf9-248">Model bağlama formu okumadığından formdan bağlanan parametreler bağlanamaz (sorgu, yol ve başlık çalışmaya devam eder).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-248">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="f5cf9-249">Action yöntemi doğrudan `Request` özelliği ile birlikte çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-249">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="f5cf9-250">Her bölümü okumak için bir `MultipartReader` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-250">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="f5cf9-251">Anahtar/değer verileri bir `KeyValueAccumulator`depolanır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-251">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="f5cf9-252">Çok parçalı bölümler okunduktan sonra, `KeyValueAccumulator` içeriği form verilerini bir model türüne bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-252">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="f5cf9-253">EF Core ile bir veritabanına akışa yönelik tüm `StreamingController.UploadDatabase` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-253">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="f5cf9-254">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="f5cf9-254">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="f5cf9-255">Fiziksel bir konuma akışa yönelik tüm `StreamingController.UploadPhysical` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-255">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="f5cf9-256">Örnek uygulamada, doğrulama denetimleri `FileHelpers.ProcessStreamedFile`tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-256">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="f5cf9-257">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="f5cf9-257">Validation</span></span>

<span data-ttu-id="f5cf9-258">Örnek uygulamanın `FileHelpers` sınıfı, arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> ve akış dosya yüklemeleri için birkaç denetim gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-258">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="f5cf9-259">Örnek uygulamada arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> dosya yüklemelerini işlemek için, *Utilities/Fileyardımcılar. cs* dosyasındaki `ProcessFormFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-259">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="f5cf9-260">Akış dosyalarını işlemek için aynı dosyadaki `ProcessStreamedFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-260">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="f5cf9-261">Örnek uygulamada gösterilen doğrulama işleme yöntemleri karşıya yüklenen dosyaların içeriğini taramaz.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-261">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="f5cf9-262">Çoğu üretim senaryosunda, dosyanın kullanıcılara veya diğer sistemlere kullanılabilir hale getirilmesi için dosya üzerinde bir virüs/kötü amaçlı yazılım tarayıcı API 'SI kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-262">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="f5cf9-263">Konu örneği, doğrulama tekniklerine yönelik çalışan bir örnek sağlasa da şunları gerçekleştirmediğiniz takdirde bir üretim uygulamasında `FileHelpers` sınıfını uygulamayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-263">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="f5cf9-264">Uygulamayı tam olarak anlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-264">Fully understand the implementation.</span></span>
> * <span data-ttu-id="f5cf9-265">Uygulamayı uygulamanın ortamı ve belirtimleri için uygun şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-265">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="f5cf9-266">**Bu gereksinimleri bilmeden bir uygulamada güvenlik kodunu hiçbir şekilde sayısının fark gözetmeden uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-266">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="f5cf9-267">İçerik doğrulama</span><span class="sxs-lookup"><span data-stu-id="f5cf9-267">Content validation</span></span>

<span data-ttu-id="f5cf9-268">**Karşıya yüklenen içerikte üçüncü taraf bir virüs/kötü amaçlı yazılım tarama API 'SI kullanın.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-268">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="f5cf9-269">Dosyaları tarama, yüksek hacimli senaryolarda sunucu kaynaklarında yoğun bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-269">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="f5cf9-270">Dosya tarama nedeniyle istek işleme performansı azaldığında, tarama işini, muhtemelen uygulamanın sunucusundan farklı bir sunucuda çalışan bir [arka plan hizmetine](xref:fundamentals/host/hosted-services)devredere göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-270">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="f5cf9-271">Genellikle, arka plan virüs tarayıcısı tarafından denetlene kadar karşıya yüklenen dosyalar karantinaya alınmış bir alanda tutulur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-271">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="f5cf9-272">Bir dosya geçtiğinde dosya normal dosya depolama konumuna taşınır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-272">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="f5cf9-273">Bu adımlar genellikle bir dosyanın tarama durumunu gösteren bir veritabanı kaydıyla birlikte gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-273">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="f5cf9-274">Böyle bir yaklaşım kullanarak, uygulama ve uygulama sunucusu isteklere yanıt vermeye odaklanmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-274">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="f5cf9-275">Dosya Uzantısı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f5cf9-275">File extension validation</span></span>

<span data-ttu-id="f5cf9-276">Karşıya yüklenen dosyanın uzantısı izin verilen uzantılar listesine göre denetlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-276">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="f5cf9-277">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-277">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="f5cf9-278">Dosya imzası doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f5cf9-278">File signature validation</span></span>

<span data-ttu-id="f5cf9-279">Bir dosyanın imzası, bir dosyanın başlangıcında ilk birkaç bayta göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-279">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="f5cf9-280">Bu baytlar, uzantının dosyanın içeriğiyle eşleşip eşleşmediğini göstermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-280">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="f5cf9-281">Örnek uygulama, birkaç ortak dosya türü için dosya imzalarını denetler.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-281">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="f5cf9-282">Aşağıdaki örnekte, bir JPEG görüntüsünün dosya imzası, dosyaya karşı denetlenir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-282">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="f5cf9-283">Ek dosya imzaları almak için [Dosya Imzaları veritabanı](https://www.filesignatures.net/) ve resmi dosya belirtimleri bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-283">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="f5cf9-284">Dosya adı güvenliği</span><span class="sxs-lookup"><span data-stu-id="f5cf9-284">File name security</span></span>

<span data-ttu-id="f5cf9-285">Fiziksel depolamaya bir dosyayı kaydetmek için hiçbir şekilde istemci tarafından sağlanan dosya adı kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-285">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="f5cf9-286">Geçici depolama için tam yol (dosya adı da dahil olmak üzere) oluşturmak için [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) veya [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanarak dosya için güvenli bir dosya adı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-286">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="f5cf9-287">Razor otomatik olarak HTML, görüntüleme için özellik değerlerini kodlar.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-287">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="f5cf9-288">Aşağıdaki kodun kullanımı güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-288">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="f5cf9-289">Razor dışında, her zaman bir kullanıcının isteğinden dosya adı içeriği <xref:System.Net.WebUtility.HtmlEncode*>.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-289">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="f5cf9-290">Birçok uygulama, dosyanın var olduğunu bir denetim içermelidir; Aksi takdirde, dosyanın üzerine aynı ada sahip bir dosya yazılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-290">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="f5cf9-291">Uygulamanızın belirtimlerini karşılamak için ek mantık sağlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-291">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="f5cf9-292">Boyut doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f5cf9-292">Size validation</span></span>

<span data-ttu-id="f5cf9-293">Karşıya yüklenen dosyaların boyutunu sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-293">Limit the size of uploaded files.</span></span>

<span data-ttu-id="f5cf9-294">Örnek uygulamada, dosyanın boyutu 2 MB ile sınırlıdır (bayt cinsinden gösterilir).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-294">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="f5cf9-295">Sınır, *appSettings. JSON* dosyasındaki [yapılandırma](xref:fundamentals/configuration/index) yoluyla sağlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-295">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="f5cf9-296">`FileSizeLimit` `PageModel` sınıflarına eklenmiş:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-296">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="f5cf9-297">Dosya boyutu sınırı aştığında, dosya reddedilir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-297">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="f5cf9-298">Name öznitelik değerini POST yönteminin Parameter adı ile Eşleştir</span><span class="sxs-lookup"><span data-stu-id="f5cf9-298">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="f5cf9-299">Form verileri oluşturan veya JavaScript 'in `FormData` doğrudan kullanan Razor olmayan formlarda, formun öğesinde veya `FormData` belirtilen adın, denetleyicinin eyleminde parametrenin adıyla eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-299">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="f5cf9-300">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-300">In the following example:</span></span>

* <span data-ttu-id="f5cf9-301">Bir `<input>` öğesi kullanılırken, `name` özniteliği `battlePlans`değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-301">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="f5cf9-302">JavaScript içinde `FormData` kullanırken ad, `battlePlans`değer olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-302">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="f5cf9-303">C# Yöntemin parametresi için eşleşen bir ad kullanın (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="f5cf9-303">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="f5cf9-304">`Upload`adlı Razor Pages sayfa işleyicisi yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-304">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="f5cf9-305">MVC POST denetleyicisi eylem yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-305">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="f5cf9-306">Sunucu ve uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="f5cf9-306">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="f5cf9-307">Çok parçalı gövde uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-307">Multipart body length limit</span></span>

<span data-ttu-id="f5cf9-308"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> her bir çok parçalı gövdenin uzunluğu için sınır ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-308"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="f5cf9-309">Bu sınırı aşan form bölümleri ayrıştırıldığında bir <xref:System.IO.InvalidDataException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-309">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="f5cf9-310">Varsayılan değer 134.217.728 ' dir (128 MB).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-310">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="f5cf9-311">`Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarını kullanarak sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-311">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="f5cf9-312"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>, tek sayfa veya eylem için <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-312"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="f5cf9-313">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices`bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-313">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="f5cf9-314">Razor Pages bir uygulamada veya bir MVC uygulamasında, filtreyi sayfa modeline veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-314">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="f5cf9-315">Kestrel maksimum istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="f5cf9-315">Kestrel maximum request body size</span></span>

<span data-ttu-id="f5cf9-316">Kestrel tarafından barındırılan uygulamalar için, varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu, yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-316">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="f5cf9-317">[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel Server seçeneğini kullanarak sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-317">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="f5cf9-318"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>, tek sayfa veya eylem için [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) ayarlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-318"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="f5cf9-319">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices`bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-319">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="f5cf9-320">Bir Razor sayfaları uygulamasında veya bir MVC uygulamasında, filtreyi sayfa işleyici sınıfına veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-320">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="f5cf9-321">`RequestSizeLimitAttribute`, [`@attribute`](xref:mvc/views/razor#attribute) Razor yönergesi kullanılarak da uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-321">The `RequestSizeLimitAttribute` can also be applied using the [`@attribute`](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="f5cf9-322">Diğer Kestrel limitleri</span><span class="sxs-lookup"><span data-stu-id="f5cf9-322">Other Kestrel limits</span></span>

<span data-ttu-id="f5cf9-323">Kestrel tarafından barındırılan uygulamalar için diğer Kestrel limitleri de uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-323">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="f5cf9-324">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-324">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="f5cf9-325">İstek ve yanıt veri ücretleri</span><span class="sxs-lookup"><span data-stu-id="f5cf9-325">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="f5cf9-326">IIS içerik uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-326">IIS content length limit</span></span>

<span data-ttu-id="f5cf9-327">Varsayılan istek sınırı (`maxAllowedContentLength`), yaklaşık 28.6 MB olan 30.000.000 bayttır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-327">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="f5cf9-328">*Web. config* dosyasında sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-328">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="f5cf9-329">Bu ayar yalnızca IIS için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-329">This setting only applies to IIS.</span></span> <span data-ttu-id="f5cf9-330">Kestrel üzerinde barındırırken davranış varsayılan olarak gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-330">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="f5cf9-331">Daha fazla bilgi için bkz. [Istek limitleri \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-331">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="f5cf9-332">ASP.NET Core modülündeki sınırlamalar veya IIS Istek filtreleme modülünün varlığı, karşıya yüklemeleri 2 veya 4 GB ile sınırlandırabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-332">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="f5cf9-333">Daha fazla bilgi için bkz. [2 GB 'tan büyük dosya karşıya yüklenemiyor (DotNet/AspNetCore #2711)](https://github.com/dotnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-333">For more information, see [Unable to upload file greater than 2GB in size (dotnet/AspNetCore #2711)](https://github.com/dotnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="f5cf9-334">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-334">Troubleshoot</span></span>

<span data-ttu-id="f5cf9-335">Dosyaları karşıya yükleme ve olası çözümleri ile çalışırken karşılaşılan bazı yaygın sorunlar aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-335">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="f5cf9-336">Bir IIS sunucusuna dağıtılırken bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="f5cf9-336">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="f5cf9-337">Aşağıdaki hata karşıya yüklenen dosyanın, sunucunun yapılandırılmış içerik uzunluğunu aştığını gösterir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-337">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="f5cf9-338">Limiti artırma hakkında daha fazla bilgi için bkz. [IIS içerik uzunluğu sınırı](#iis-content-length-limit) bölümü.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-338">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="f5cf9-339">Bağlantı hatası</span><span class="sxs-lookup"><span data-stu-id="f5cf9-339">Connection failure</span></span>

<span data-ttu-id="f5cf9-340">Bir bağlantı hatası ve bir sıfırlama sunucu bağlantısı büyük olasılıkla karşıya yüklenen dosyanın Kestrel 'in en büyük istek gövdesi boyutunu aştığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-340">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="f5cf9-341">Daha fazla bilgi için, [Kestrel maksimum istek gövdesi boyutu](#kestrel-maximum-request-body-size) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-341">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="f5cf9-342">Kestrel istemci bağlantı limitleri de ayarlama gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-342">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="f5cf9-343">Iformfile ile null başvuru özel durumu</span><span class="sxs-lookup"><span data-stu-id="f5cf9-343">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="f5cf9-344">Denetleyici <xref:Microsoft.AspNetCore.Http.IFormFile> kullanarak karşıya yüklenen dosyaları kabul ediyorsanız, ancak değer `null`, HTML formunun `multipart/form-data``enctype` değerini belirtdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-344">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="f5cf9-345">Bu öznitelik `<form>` öğesinde ayarlanmamışsa, dosya karşıya yükleme gerçekleşmez ve herhangi bir bağlı <xref:Microsoft.AspNetCore.Http.IFormFile> bağımsız değişken `null`.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-345">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="f5cf9-346">Ayrıca, [form verilerinde karşıya yükleme adlandırmasının uygulamanın adlandırmayla eşleştiğinden](#match-name-attribute-value-to-parameter-name-of-post-method)emin olun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-346">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="f5cf9-347">Akış çok uzun</span><span class="sxs-lookup"><span data-stu-id="f5cf9-347">Stream was too long</span></span>

<span data-ttu-id="f5cf9-348">Bu konudaki örneklerde karşıya yüklenen dosyanın içeriğini tutmak için <xref:System.IO.MemoryStream> bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-348">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="f5cf9-349">Bir `MemoryStream` boyut limiti `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-349">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="f5cf9-350">Uygulamanın dosya yükleme senaryosu, dosya içeriğinin 50 MB 'tan büyük olmasını gerektiriyorsa, karşıya yüklenen dosyanın içeriğini tutmak için tek bir `MemoryStream` kullanmayan alternatif bir yaklaşım kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-350">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f5cf9-351">ASP.NET Core, daha küçük dosyalar için arabellekli model bağlama ve daha büyük dosyalar için arabelleğe alınmamış akış kullanarak bir veya daha fazla dosyanın yüklenmesini destekler</span><span class="sxs-lookup"><span data-stu-id="f5cf9-351">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="f5cf9-352">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f5cf9-352">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="f5cf9-353">Güvenlikle ilgili dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="f5cf9-353">Security considerations</span></span>

<span data-ttu-id="f5cf9-354">Kullanıcılara bir sunucuya dosya yükleme yeteneği sağlarken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-354">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="f5cf9-355">Saldırganlar şunları deneyebilir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-355">Attackers may attempt to:</span></span>

* <span data-ttu-id="f5cf9-356">[Hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) saldırıları yürütün.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-356">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="f5cf9-357">Virüsleri veya kötü amaçlı yazılımları karşıya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-357">Upload viruses or malware.</span></span>
* <span data-ttu-id="f5cf9-358">Ağları ve sunucuları diğer yollarla tehlikeye atabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-358">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="f5cf9-359">Başarılı bir saldırının olasılığını azaltan güvenlik adımları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-359">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="f5cf9-360">Dosyaları, tercihen sistem dışı bir sürücüye, özel bir dosya yükleme alanına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-360">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="f5cf9-361">Ayrılmış bir konum, karşıya yüklenen dosyalar üzerinde güvenlik kısıtlamaları yapmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-361">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="f5cf9-362">Dosya yükleme konumunda yürütme izinlerini devre dışı bırakın.&dagger;</span><span class="sxs-lookup"><span data-stu-id="f5cf9-362">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="f5cf9-363">Karşıya yüklenen dosyaları uygulamayla aynı dizin **ağacında kalıcı yapma** .&dagger;</span><span class="sxs-lookup"><span data-stu-id="f5cf9-363">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="f5cf9-364">Uygulama tarafından belirlenen bir güvenli dosya adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-364">Use a safe file name determined by the app.</span></span> <span data-ttu-id="f5cf9-365">Kullanıcı tarafından belirtilen bir dosya adı veya karşıya yüklenen dosyanın güvenilmeyen dosya adı kullanmayın.&dagger; HTML 'yi görüntülerken güvenilmeyen dosya adını kodlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-365">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="f5cf9-366">Örneğin, dosya adını günlüğe kaydetme veya Kullanıcı arabiriminde görüntüleme (Razor otomatik olarak HTML kodlama çıkışı).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-366">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="f5cf9-367">Uygulamanın tasarım belirtimi için yalnızca onaylanan dosya uzantılarına izin verin.&dagger;</span><span class="sxs-lookup"><span data-stu-id="f5cf9-367">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="f5cf9-368">Sunucuda istemci tarafı denetimlerinin gerçekleştirildiğinden emin olun.&dagger; Istemci tarafı denetimleri kolayca atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-368">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="f5cf9-369">Karşıya yüklenen dosyanın boyutunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-369">Check the size of an uploaded file.</span></span> <span data-ttu-id="f5cf9-370">Büyük karşıya yüklemeleri engellemek için en büyük boyut sınırını ayarlayın.&dagger;</span><span class="sxs-lookup"><span data-stu-id="f5cf9-370">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="f5cf9-371">Aynı ada sahip karşıya yüklenen bir dosya tarafından dosyaların üzerine yazılmaması gerektiğinde, dosyayı karşıya yüklemeden önce dosya adını veritabanına veya fiziksel depolamaya göre denetleyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-371">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="f5cf9-372">**Dosya depolanmadan önce karşıya yüklenen içerik üzerinde bir virüs/kötü amaçlı yazılım tarayıcısı çalıştırın.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-372">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="f5cf9-373">örnek uygulama &dagger;ölçütleri karşılayan bir yaklaşımı gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-373">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="f5cf9-374">Kötü amaçlı bir kodun bir sisteme karşıya yükleme için kod yürütme için ilk adımı sık şöyledir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-374">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="f5cf9-375">Sistemin denetimini tamamen elde edin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-375">Completely gain control of a system.</span></span>
> * <span data-ttu-id="f5cf9-376">Sistemin kilitlenme sonucuyla bir sistemi aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-376">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="f5cf9-377">Kullanıcı veya sistem verilerini tehlikeye.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-377">Compromise user or system data.</span></span>
> * <span data-ttu-id="f5cf9-378">Genel Kullanıcı arabirimine Graffiti uygulayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-378">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="f5cf9-379">Kullanıcıların dosyaları kabul ederken saldırı yüzey alanı azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-379">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="f5cf9-380">Kısıtlanmamış dosya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-380">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="f5cf9-381">Azure güvenliği: kullanıcılardan dosya kabul edilirken uygun denetimlerin yerinde olduğundan emin olun</span><span class="sxs-lookup"><span data-stu-id="f5cf9-381">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="f5cf9-382">Örnek uygulamadaki örnekler de dahil olmak üzere güvenlik önlemlerini uygulama hakkında daha fazla bilgi için [doğrulama](#validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-382">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="f5cf9-383">Depolama senaryoları</span><span class="sxs-lookup"><span data-stu-id="f5cf9-383">Storage scenarios</span></span>

<span data-ttu-id="f5cf9-384">Dosyalar için ortak depolama seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-384">Common storage options for files include:</span></span>

* <span data-ttu-id="f5cf9-385">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-385">Database</span></span>

  * <span data-ttu-id="f5cf9-386">Küçük dosya yüklemeleri için, bir veritabanı genellikle fiziksel depolama (dosya sistemi veya ağ paylaşımından) seçeneklerinden daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-386">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="f5cf9-387">Kullanıcı verileri için bir veritabanı kaydının alınması eşzamanlı olarak dosya içeriğini (örneğin, avatar görüntüsü) sağlayabildiğinden, veritabanı genellikle fiziksel depolama seçeneklerine göre daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-387">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="f5cf9-388">Bir veritabanı, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-388">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="f5cf9-389">Fiziksel depolama (dosya sistemi veya ağ paylaşma)</span><span class="sxs-lookup"><span data-stu-id="f5cf9-389">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="f5cf9-390">Büyük dosya yüklemeleri için:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-390">For large file uploads:</span></span>
    * <span data-ttu-id="f5cf9-391">Veritabanı limitleri karşıya yükleme boyutunu kısıtlayabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-391">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="f5cf9-392">Fiziksel depolama genellikle bir veritabanındaki depolamadan daha az ekonomik olur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-392">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="f5cf9-393">Fiziksel depolama, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-393">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="f5cf9-394">Uygulamanın işlemi, depolama konumu için okuma ve yazma izinlerine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-394">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="f5cf9-395">**Hiçbir zaman yürütme izni vermeyin.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-395">**Never grant execute permission.**</span></span>

* <span data-ttu-id="f5cf9-396">Veri depolama hizmeti (örneğin, [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="f5cf9-396">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="f5cf9-397">Hizmetler genellikle tek hata noktalarına tabi olan şirket içi çözümler üzerinde geliştirilmiş ölçeklenebilirlik ve esneklik sunar.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-397">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="f5cf9-398">Hizmetler büyük depolama altyapısı senaryolarında düşük maliyetlidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-398">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="f5cf9-399">Daha fazla bilgi için bkz. [hızlı başlangıç: nesne depolamada blob oluşturmak için .NET kullanma](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-399">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="f5cf9-400">Konu <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>gösterir, ancak <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> bir <xref:System.IO.Stream>çalışırken blob depolamaya <xref:System.IO.FileStream> kaydetmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-400">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="f5cf9-401">Karşıya dosya yükleme senaryoları</span><span class="sxs-lookup"><span data-stu-id="f5cf9-401">File upload scenarios</span></span>

<span data-ttu-id="f5cf9-402">Dosyaları karşıya yüklemek için iki genel yaklaşım arabelleğe alınır ve akışlardır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-402">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="f5cf9-403">**Ara**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-403">**Buffering**</span></span>

<span data-ttu-id="f5cf9-404">Dosyanın tamamı bir <xref:Microsoft.AspNetCore.Http.IFormFile>okur, bu dosya dosyayı işlemek veya kaydetmek C# için kullanılan bir gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-404">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="f5cf9-405">Dosya karşıya yüklemeleri tarafından kullanılan kaynaklar (disk, bellek), eşzamanlı dosya karşıya yüklemelerinin sayısına ve boyutuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-405">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="f5cf9-406">Bir uygulama çok fazla karşıya yükleme arabelleğini denerse, bellek veya disk alanı tükenirse site çöker.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-406">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="f5cf9-407">Karşıya dosya yükleme boyutu veya sıklığı uygulama kaynaklarını tüketme ise, akış ' ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-407">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="f5cf9-408">64 KB geçen tek bir arabelleğe alınmış dosya, bellekten diskte geçici bir dosyaya taşınır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-408">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="f5cf9-409">Dosya arabelleğe alma, bu konunun aşağıdaki bölümlerinde ele alınmıştır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-409">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="f5cf9-410">Fiziksel depolama alanı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-410">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="f5cf9-411">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-411">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="f5cf9-412">**Akış**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-412">**Streaming**</span></span>

<span data-ttu-id="f5cf9-413">Dosya çok parçalı bir istekten alınır ve doğrudan uygulama tarafından işlenir veya kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-413">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="f5cf9-414">Akış performansı önemli ölçüde iyileştirmez.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-414">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="f5cf9-415">Akış, dosya karşıya yüklenirken bellek veya disk alanı taleplerini azaltır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-415">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="f5cf9-416">Akış büyük dosyaları [akış ile büyük dosyaları karşıya yükle](#upload-large-files-with-streaming) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-416">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="f5cf9-417">Fiziksel depolamaya arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-417">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="f5cf9-418">Küçük dosyaları karşıya yüklemek için, çok parçalı bir form kullanın veya JavaScript kullanarak bir POST isteği oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-418">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="f5cf9-419">Aşağıdaki örnek, tek bir dosyayı karşıya yüklemek için bir Razor Pages formunun kullanımını gösterir (örnek uygulamada*Pages/Bufferedsinglefileuploadfiziksel. cshtml* ):</span><span class="sxs-lookup"><span data-stu-id="f5cf9-419">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="f5cf9-420">Aşağıdaki örnek, önceki örneğe benzerdir, ancak şunları hariç:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-420">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="f5cf9-421">Form verilerini göndermek için JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-421">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="f5cf9-422">Doğrulama yok.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-422">There's no validation.</span></span>

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

<span data-ttu-id="f5cf9-423">[Fetch API 'sini desteklemeyen](https://caniuse.com/#feat=fetch)istemcilerde form gönderisini JavaScript 'te gerçekleştirmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-423">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="f5cf9-424">Fetch Polyfill kullanın (örneğin, [Window. Fetch (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-424">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="f5cf9-425">`XMLHttpRequest`kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-425">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="f5cf9-426">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-426">For example:</span></span>

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

<span data-ttu-id="f5cf9-427">Dosya yüklemelerini desteklemek için, HTML formları `multipart/form-data`bir kodlama türü (`enctype`) belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-427">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="f5cf9-428">Birden çok dosyayı karşıya yüklemeyi desteklemek için `files` giriş öğesi için, `<input>` öğesinde `multiple` özniteliğini sağlayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-428">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="f5cf9-429">Sunucuya yüklenen tek dosyalara <xref:Microsoft.AspNetCore.Http.IFormFile>kullanılarak [model bağlama](xref:mvc/models/model-binding) yoluyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-429">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="f5cf9-430">Örnek uygulama, veritabanı ve fiziksel depolama senaryoları için birden çok arabellekli dosya yüklemeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-430">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="f5cf9-431">Görüntüleme ve günlüğe kaydetme için dışında <xref:Microsoft.AspNetCore.Http.IFormFile> `FileName` **özelliğini kullanmayın.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-431">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="f5cf9-432">Görüntüleme veya günlüğe kaydetme sırasında, HTML dosya adını kodlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-432">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="f5cf9-433">Saldırgan, tam yollar veya göreli yollar dahil olmak üzere kötü amaçlı bir dosya adı sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-433">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="f5cf9-434">Uygulamalar:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-434">Applications should:</span></span>
>
> * <span data-ttu-id="f5cf9-435">Kullanıcı tarafından sağlanan dosya adının yolunu kaldırın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-435">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="f5cf9-436">Kullanıcı arabirimi veya günlüğe kaydetme için HTML kodlu, yol tarafından kaldırılan dosya adını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-436">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="f5cf9-437">Depolama için yeni bir rastgele dosya adı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-437">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="f5cf9-438">Aşağıdaki kod, dosya adından yolu kaldırır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-438">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="f5cf9-439">Bu nedenle, şu ana kadar dikkate alınması gereken örnekler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-439">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="f5cf9-440">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-440">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="f5cf9-441">Güvenlikle ilgili dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="f5cf9-441">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="f5cf9-442">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="f5cf9-442">Validation</span></span>](#validation)

<span data-ttu-id="f5cf9-443">Model bağlama ve <xref:Microsoft.AspNetCore.Http.IFormFile>kullanarak dosya karşıya yüklerken, eylem yöntemi şunları kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-443">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="f5cf9-444">Tek bir <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-444">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="f5cf9-445">Birkaç dosyayı temsil eden aşağıdaki koleksiyonlardan herhangi biri:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-445">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="f5cf9-446">[Liste](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span><span class="sxs-lookup"><span data-stu-id="f5cf9-446">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="f5cf9-447">Bağlama, form dosyaları adına göre eşleşir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-447">Binding matches form files by name.</span></span> <span data-ttu-id="f5cf9-448">Örneğin, `<input type="file" name="formFile">` HTML `name` değeri C# parametre/özellik ile (`FormFile`) eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-448">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="f5cf9-449">Daha fazla bilgi için, [ad öznitelik DEĞERINI Post yönteminin parametre adına eşleştirin](#match-name-attribute-value-to-parameter-name-of-post-method) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-449">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="f5cf9-450">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-450">The following example:</span></span>

* <span data-ttu-id="f5cf9-451">Karşıya yüklenen bir veya daha fazla dosya üzerinden döngü.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-451">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="f5cf9-452">Dosya adı da dahil olmak üzere bir dosyanın tam yolunu döndürmek için [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-452">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="f5cf9-453">Dosyalar, uygulama tarafından oluşturulan bir dosya adı kullanılarak yerel dosya sistemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-453">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="f5cf9-454">Karşıya yüklenen dosyaların toplam sayısını ve boyutunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-454">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="f5cf9-455">Yol olmadan bir dosya adı oluşturmak için `Path.GetRandomFileName` kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-455">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="f5cf9-456">Aşağıdaki örnekte, yol yapılandırmadan alınır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-456">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="f5cf9-457"><xref:System.IO.FileStream> geçirilen yol, dosya adını *içermelidir.*</span><span class="sxs-lookup"><span data-stu-id="f5cf9-457">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="f5cf9-458">Dosya adı sağlanmazsa, çalışma zamanında bir <xref:System.UnauthorizedAccessException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-458">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="f5cf9-459"><xref:Microsoft.AspNetCore.Http.IFormFile> tekniği kullanılarak yüklenen dosyalar, işlemeden önce sunucuda veya diskte bellek halinde arabelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-459">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="f5cf9-460">Eylem yönteminde, <xref:Microsoft.AspNetCore.Http.IFormFile> içeriğe <xref:System.IO.Stream>olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-460">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="f5cf9-461">Yerel dosya sistemine ek olarak, dosyalar bir ağ paylaşımında veya [Azure Blob depolama](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)gibi bir dosya depolama hizmetine kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-461">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="f5cf9-462">Karşıya yüklemek için birden çok dosya üzerinde döngü yapan ve güvenli dosya adları kullanan başka bir örnek için, örnek uygulamadaki *Pages/Bufferedmultiplefileuploadfiziksel. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-462">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="f5cf9-463">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , önceki geçici dosyaları silmeden 65.535 'den fazla dosya oluşturulduysa bir <xref:System.IO.IOException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-463">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="f5cf9-464">65.535 dosya sınırının sunucu başına sınırı vardır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-464">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="f5cf9-465">Windows işletim sistemi için bu sınır hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-465">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="f5cf9-466">GetTempFileNameA işlevi</span><span class="sxs-lookup"><span data-stu-id="f5cf9-466">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="f5cf9-467">Bir veritabanına arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-467">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="f5cf9-468">İkili dosya verilerini [Entity Framework](/ef/core/index)kullanarak bir veritabanında depolamak için, varlıkta bir <xref:System.Byte> Array özelliği tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-468">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="f5cf9-469">Bir <xref:Microsoft.AspNetCore.Http.IFormFile>içeren sınıf için bir sayfa modeli özelliği belirtin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-469">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="f5cf9-470"><xref:Microsoft.AspNetCore.Http.IFormFile>, doğrudan bir eylem yöntemi parametresi veya bir bağlantılı model özelliği olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-470"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="f5cf9-471">Önceki örnekte, bir bağlantılı model özelliği kullanılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-471">The prior example uses a bound model property.</span></span>

<span data-ttu-id="f5cf9-472">`FileUpload` Razor Pages formunda kullanılır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-472">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="f5cf9-473">Form sunucuya gönderildiğinde, <xref:Microsoft.AspNetCore.Http.IFormFile> bir akışa kopyalayın ve veritabanına bir bayt dizisi olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-473">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="f5cf9-474">Aşağıdaki örnekte, uygulamanın veritabanı bağlamını `_dbContext` depolar:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-474">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="f5cf9-475">Yukarıdaki örnek, örnek uygulamada gösterilen senaryoya benzerdir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-475">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="f5cf9-476">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="f5cf9-476">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="f5cf9-477">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="f5cf9-477">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="f5cf9-478">İkili verileri ilişkisel veritabanlarında depolarken dikkatli olun, çünkü performansı olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-478">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="f5cf9-479">Doğrulaması olmadan <xref:Microsoft.AspNetCore.Http.IFormFile> `FileName` özelliğine güvenmeyin veya güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-479">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="f5cf9-480">`FileName` özelliği yalnızca görüntüleme amacıyla ve yalnızca HTML kodlaması sonrasında kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-480">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="f5cf9-481">Belirtilen örneklerde dikkate alınması gereken önemli noktalar.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-481">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="f5cf9-482">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-482">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="f5cf9-483">Güvenlikle ilgili dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="f5cf9-483">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="f5cf9-484">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="f5cf9-484">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="f5cf9-485">Akışa sahip büyük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-485">Upload large files with streaming</span></span>

<span data-ttu-id="f5cf9-486">Aşağıdaki örnek, bir denetleyiciyi bir denetleyici eyleminde akışa almak için JavaScript 'in nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-486">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="f5cf9-487">Dosyanın antiforgery belirteci özel bir filtre özniteliği kullanılarak oluşturulur ve istek gövdesi yerine istemci HTTP üst bilgilerine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-487">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="f5cf9-488">Eylem yöntemi karşıya yüklenen verileri doğrudan işlediğinden, form modeli bağlama başka bir özel filtre tarafından devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-488">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="f5cf9-489">Eylem içinde formun içerikleri, her bir `MultipartSection`okuyan, dosyayı işleyen veya içeriği uygun şekilde depolayan bir `MultipartReader`kullanılarak okunur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-489">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="f5cf9-490">Çok parçalı bölümler okunduktan sonra eylem kendi model bağlamasını gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-490">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="f5cf9-491">İlk sayfa yanıtı formu yükler ve bir tanımlama bilgisine (`GenerateAntiforgeryTokenCookieAttribute` özniteliği aracılığıyla) bir antiforgery belirteci kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-491">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="f5cf9-492">Öznitelik, bir istek belirtecine sahip bir tanımlama bilgisi ayarlamak için ASP.NET Core yerleşik [antiforgery desteğini](xref:security/anti-request-forgery) kullanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-492">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="f5cf9-493">Model bağlamayı devre dışı bırakmak için `DisableFormValueModelBindingAttribute` kullanılır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-493">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="f5cf9-494">Örnek uygulamada `GenerateAntiforgeryTokenCookieAttribute` ve `DisableFormValueModelBindingAttribute`, [`Startup.ConfigureServices` kuralları](xref:razor-pages/razor-pages-conventions)kullanılarak Razor Pages `/StreamedSingleFileUploadDb` ve `/StreamedSingleFileUploadPhysical` sayfa uygulama modellerine filtre olarak uygulanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-494">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="f5cf9-495">Model bağlama formu okumadığından formdan bağlanan parametreler bağlanamaz (sorgu, yol ve başlık çalışmaya devam eder).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-495">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="f5cf9-496">Action yöntemi doğrudan `Request` özelliği ile birlikte çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-496">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="f5cf9-497">Her bölümü okumak için bir `MultipartReader` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-497">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="f5cf9-498">Anahtar/değer verileri bir `KeyValueAccumulator`depolanır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-498">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="f5cf9-499">Çok parçalı bölümler okunduktan sonra, `KeyValueAccumulator` içeriği form verilerini bir model türüne bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-499">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="f5cf9-500">EF Core ile bir veritabanına akışa yönelik tüm `StreamingController.UploadDatabase` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-500">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="f5cf9-501">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="f5cf9-501">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="f5cf9-502">Fiziksel bir konuma akışa yönelik tüm `StreamingController.UploadPhysical` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-502">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="f5cf9-503">Örnek uygulamada, doğrulama denetimleri `FileHelpers.ProcessStreamedFile`tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-503">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="f5cf9-504">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="f5cf9-504">Validation</span></span>

<span data-ttu-id="f5cf9-505">Örnek uygulamanın `FileHelpers` sınıfı, arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> ve akış dosya yüklemeleri için birkaç denetim gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-505">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="f5cf9-506">Örnek uygulamada arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> dosya yüklemelerini işlemek için, *Utilities/Fileyardımcılar. cs* dosyasındaki `ProcessFormFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-506">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="f5cf9-507">Akış dosyalarını işlemek için aynı dosyadaki `ProcessStreamedFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-507">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="f5cf9-508">Örnek uygulamada gösterilen doğrulama işleme yöntemleri karşıya yüklenen dosyaların içeriğini taramaz.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-508">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="f5cf9-509">Çoğu üretim senaryosunda, dosyanın kullanıcılara veya diğer sistemlere kullanılabilir hale getirilmesi için dosya üzerinde bir virüs/kötü amaçlı yazılım tarayıcı API 'SI kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-509">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="f5cf9-510">Konu örneği, doğrulama tekniklerine yönelik çalışan bir örnek sağlasa da şunları gerçekleştirmediğiniz takdirde bir üretim uygulamasında `FileHelpers` sınıfını uygulamayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-510">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="f5cf9-511">Uygulamayı tam olarak anlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-511">Fully understand the implementation.</span></span>
> * <span data-ttu-id="f5cf9-512">Uygulamayı uygulamanın ortamı ve belirtimleri için uygun şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-512">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="f5cf9-513">**Bu gereksinimleri bilmeden bir uygulamada güvenlik kodunu hiçbir şekilde sayısının fark gözetmeden uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-513">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="f5cf9-514">İçerik doğrulama</span><span class="sxs-lookup"><span data-stu-id="f5cf9-514">Content validation</span></span>

<span data-ttu-id="f5cf9-515">**Karşıya yüklenen içerikte üçüncü taraf bir virüs/kötü amaçlı yazılım tarama API 'SI kullanın.**</span><span class="sxs-lookup"><span data-stu-id="f5cf9-515">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="f5cf9-516">Dosyaları tarama, yüksek hacimli senaryolarda sunucu kaynaklarında yoğun bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-516">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="f5cf9-517">Dosya tarama nedeniyle istek işleme performansı azaldığında, tarama işini, muhtemelen uygulamanın sunucusundan farklı bir sunucuda çalışan bir [arka plan hizmetine](xref:fundamentals/host/hosted-services)devredere göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-517">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="f5cf9-518">Genellikle, arka plan virüs tarayıcısı tarafından denetlene kadar karşıya yüklenen dosyalar karantinaya alınmış bir alanda tutulur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-518">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="f5cf9-519">Bir dosya geçtiğinde dosya normal dosya depolama konumuna taşınır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-519">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="f5cf9-520">Bu adımlar genellikle bir dosyanın tarama durumunu gösteren bir veritabanı kaydıyla birlikte gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-520">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="f5cf9-521">Böyle bir yaklaşım kullanarak, uygulama ve uygulama sunucusu isteklere yanıt vermeye odaklanmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-521">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="f5cf9-522">Dosya Uzantısı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f5cf9-522">File extension validation</span></span>

<span data-ttu-id="f5cf9-523">Karşıya yüklenen dosyanın uzantısı izin verilen uzantılar listesine göre denetlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-523">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="f5cf9-524">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-524">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="f5cf9-525">Dosya imzası doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f5cf9-525">File signature validation</span></span>

<span data-ttu-id="f5cf9-526">Bir dosyanın imzası, bir dosyanın başlangıcında ilk birkaç bayta göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-526">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="f5cf9-527">Bu baytlar, uzantının dosyanın içeriğiyle eşleşip eşleşmediğini göstermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-527">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="f5cf9-528">Örnek uygulama, birkaç ortak dosya türü için dosya imzalarını denetler.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-528">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="f5cf9-529">Aşağıdaki örnekte, bir JPEG görüntüsünün dosya imzası, dosyaya karşı denetlenir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-529">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="f5cf9-530">Ek dosya imzaları almak için [Dosya Imzaları veritabanı](https://www.filesignatures.net/) ve resmi dosya belirtimleri bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-530">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="f5cf9-531">Dosya adı güvenliği</span><span class="sxs-lookup"><span data-stu-id="f5cf9-531">File name security</span></span>

<span data-ttu-id="f5cf9-532">Fiziksel depolamaya bir dosyayı kaydetmek için hiçbir şekilde istemci tarafından sağlanan dosya adı kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-532">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="f5cf9-533">Geçici depolama için tam yol (dosya adı da dahil olmak üzere) oluşturmak için [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) veya [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanarak dosya için güvenli bir dosya adı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-533">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="f5cf9-534">Razor otomatik olarak HTML, görüntüleme için özellik değerlerini kodlar.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-534">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="f5cf9-535">Aşağıdaki kodun kullanımı güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-535">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="f5cf9-536">Razor dışında, her zaman bir kullanıcının isteğinden dosya adı içeriği <xref:System.Net.WebUtility.HtmlEncode*>.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-536">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="f5cf9-537">Birçok uygulama, dosyanın var olduğunu bir denetim içermelidir; Aksi takdirde, dosyanın üzerine aynı ada sahip bir dosya yazılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-537">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="f5cf9-538">Uygulamanızın belirtimlerini karşılamak için ek mantık sağlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-538">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="f5cf9-539">Boyut doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f5cf9-539">Size validation</span></span>

<span data-ttu-id="f5cf9-540">Karşıya yüklenen dosyaların boyutunu sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-540">Limit the size of uploaded files.</span></span>

<span data-ttu-id="f5cf9-541">Örnek uygulamada, dosyanın boyutu 2 MB ile sınırlıdır (bayt cinsinden gösterilir).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-541">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="f5cf9-542">Sınır, *appSettings. JSON* dosyasındaki [yapılandırma](xref:fundamentals/configuration/index) yoluyla sağlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-542">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="f5cf9-543">`FileSizeLimit` `PageModel` sınıflarına eklenmiş:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-543">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="f5cf9-544">Dosya boyutu sınırı aştığında, dosya reddedilir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-544">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="f5cf9-545">Name öznitelik değerini POST yönteminin Parameter adı ile Eşleştir</span><span class="sxs-lookup"><span data-stu-id="f5cf9-545">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="f5cf9-546">Form verileri oluşturan veya JavaScript 'in `FormData` doğrudan kullanan Razor olmayan formlarda, formun öğesinde veya `FormData` belirtilen adın, denetleyicinin eyleminde parametrenin adıyla eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-546">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="f5cf9-547">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-547">In the following example:</span></span>

* <span data-ttu-id="f5cf9-548">Bir `<input>` öğesi kullanılırken, `name` özniteliği `battlePlans`değere ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-548">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="f5cf9-549">JavaScript içinde `FormData` kullanırken ad, `battlePlans`değer olarak ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-549">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="f5cf9-550">C# Yöntemin parametresi için eşleşen bir ad kullanın (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="f5cf9-550">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="f5cf9-551">`Upload`adlı Razor Pages sayfa işleyicisi yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-551">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="f5cf9-552">MVC POST denetleyicisi eylem yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-552">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="f5cf9-553">Sunucu ve uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="f5cf9-553">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="f5cf9-554">Çok parçalı gövde uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-554">Multipart body length limit</span></span>

<span data-ttu-id="f5cf9-555"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> her bir çok parçalı gövdenin uzunluğu için sınır ayarlar.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-555"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="f5cf9-556">Bu sınırı aşan form bölümleri ayrıştırıldığında bir <xref:System.IO.InvalidDataException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-556">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="f5cf9-557">Varsayılan değer 134.217.728 ' dir (128 MB).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-557">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="f5cf9-558">`Startup.ConfigureServices`<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarını kullanarak sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-558">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="f5cf9-559"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>, tek sayfa veya eylem için <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-559"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="f5cf9-560">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices`bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-560">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="f5cf9-561">Razor Pages bir uygulamada veya bir MVC uygulamasında, filtreyi sayfa modeline veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-561">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="f5cf9-562">Kestrel maksimum istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="f5cf9-562">Kestrel maximum request body size</span></span>

<span data-ttu-id="f5cf9-563">Kestrel tarafından barındırılan uygulamalar için, varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu, yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-563">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="f5cf9-564">[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel Server seçeneğini kullanarak sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-564">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

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

<span data-ttu-id="f5cf9-565"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>, tek sayfa veya eylem için [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) ayarlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-565"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="f5cf9-566">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices`bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-566">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="f5cf9-567">Bir Razor sayfaları uygulamasında veya bir MVC uygulamasında, filtreyi sayfa işleyici sınıfına veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-567">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="f5cf9-568">Diğer Kestrel limitleri</span><span class="sxs-lookup"><span data-stu-id="f5cf9-568">Other Kestrel limits</span></span>

<span data-ttu-id="f5cf9-569">Kestrel tarafından barındırılan uygulamalar için diğer Kestrel limitleri de uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-569">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="f5cf9-570">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-570">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="f5cf9-571">İstek ve yanıt veri ücretleri</span><span class="sxs-lookup"><span data-stu-id="f5cf9-571">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="f5cf9-572">IIS içerik uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-572">IIS content length limit</span></span>

<span data-ttu-id="f5cf9-573">Varsayılan istek sınırı (`maxAllowedContentLength`), yaklaşık 28.6 MB olan 30.000.000 bayttır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-573">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="f5cf9-574">*Web. config* dosyasında sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-574">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="f5cf9-575">Bu ayar yalnızca IIS için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-575">This setting only applies to IIS.</span></span> <span data-ttu-id="f5cf9-576">Kestrel üzerinde barındırırken davranış varsayılan olarak gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-576">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="f5cf9-577">Daha fazla bilgi için bkz. [Istek limitleri \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-577">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="f5cf9-578">ASP.NET Core modülündeki sınırlamalar veya IIS Istek filtreleme modülünün varlığı, karşıya yüklemeleri 2 veya 4 GB ile sınırlandırabilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-578">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="f5cf9-579">Daha fazla bilgi için bkz. [2 GB 'tan büyük dosya karşıya yüklenemiyor (DotNet/AspNetCore #2711)](https://github.com/dotnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="f5cf9-579">For more information, see [Unable to upload file greater than 2GB in size (dotnet/AspNetCore #2711)](https://github.com/dotnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="f5cf9-580">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-580">Troubleshoot</span></span>

<span data-ttu-id="f5cf9-581">Dosyaları karşıya yükleme ve olası çözümleri ile çalışırken karşılaşılan bazı yaygın sorunlar aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-581">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="f5cf9-582">Bir IIS sunucusuna dağıtılırken bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="f5cf9-582">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="f5cf9-583">Aşağıdaki hata karşıya yüklenen dosyanın, sunucunun yapılandırılmış içerik uzunluğunu aştığını gösterir:</span><span class="sxs-lookup"><span data-stu-id="f5cf9-583">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="f5cf9-584">Limiti artırma hakkında daha fazla bilgi için bkz. [IIS içerik uzunluğu sınırı](#iis-content-length-limit) bölümü.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-584">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="f5cf9-585">Bağlantı hatası</span><span class="sxs-lookup"><span data-stu-id="f5cf9-585">Connection failure</span></span>

<span data-ttu-id="f5cf9-586">Bir bağlantı hatası ve bir sıfırlama sunucu bağlantısı büyük olasılıkla karşıya yüklenen dosyanın Kestrel 'in en büyük istek gövdesi boyutunu aştığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-586">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="f5cf9-587">Daha fazla bilgi için, [Kestrel maksimum istek gövdesi boyutu](#kestrel-maximum-request-body-size) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-587">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="f5cf9-588">Kestrel istemci bağlantı limitleri de ayarlama gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-588">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="f5cf9-589">Iformfile ile null başvuru özel durumu</span><span class="sxs-lookup"><span data-stu-id="f5cf9-589">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="f5cf9-590">Denetleyici <xref:Microsoft.AspNetCore.Http.IFormFile> kullanarak karşıya yüklenen dosyaları kabul ediyorsanız, ancak değer `null`, HTML formunun `multipart/form-data``enctype` değerini belirtdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-590">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="f5cf9-591">Bu öznitelik `<form>` öğesinde ayarlanmamışsa, dosya karşıya yükleme gerçekleşmez ve herhangi bir bağlı <xref:Microsoft.AspNetCore.Http.IFormFile> bağımsız değişken `null`.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-591">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="f5cf9-592">Ayrıca, [form verilerinde karşıya yükleme adlandırmasının uygulamanın adlandırmayla eşleştiğinden](#match-name-attribute-value-to-parameter-name-of-post-method)emin olun.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-592">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="f5cf9-593">Akış çok uzun</span><span class="sxs-lookup"><span data-stu-id="f5cf9-593">Stream was too long</span></span>

<span data-ttu-id="f5cf9-594">Bu konudaki örneklerde karşıya yüklenen dosyanın içeriğini tutmak için <xref:System.IO.MemoryStream> bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-594">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="f5cf9-595">Bir `MemoryStream` boyut limiti `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-595">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="f5cf9-596">Uygulamanın dosya yükleme senaryosu, dosya içeriğinin 50 MB 'tan büyük olmasını gerektiriyorsa, karşıya yüklenen dosyanın içeriğini tutmak için tek bir `MemoryStream` kullanmayan alternatif bir yaklaşım kullanın.</span><span class="sxs-lookup"><span data-stu-id="f5cf9-596">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="f5cf9-597">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="f5cf9-597">Additional resources</span></span>

* [<span data-ttu-id="f5cf9-598">Kısıtlanmamış dosya yükleme</span><span class="sxs-lookup"><span data-stu-id="f5cf9-598">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [<span data-ttu-id="f5cf9-599">Azure güvenliği: güvenlik çerçevesi: giriş doğrulaması | Karşı</span><span class="sxs-lookup"><span data-stu-id="f5cf9-599">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="f5cf9-600">Azure bulut tasarım desenleri: Valet anahtar düzeni</span><span class="sxs-lookup"><span data-stu-id="f5cf9-600">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
