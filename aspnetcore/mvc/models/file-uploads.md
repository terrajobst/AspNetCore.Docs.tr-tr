---
title: ASP.NET Core dosyaları karşıya yükleme
author: guardrex
description: ASP.NET Core MVC 'de dosyaları karşıya yüklemek için model bağlama ve akışı kullanma.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/30/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 46cdb517742431a7f2a93155564832d586233e68
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925111"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="98ef0-103">ASP.NET Core dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="98ef0-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="98ef0-104">[Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)ve [Rutger fırtınası](https://github.com/rutix) tarafından</span><span class="sxs-lookup"><span data-stu-id="98ef0-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

<span data-ttu-id="98ef0-105">ASP.NET Core, daha küçük dosyalar için arabellekli model bağlama ve daha büyük dosyalar için arabelleğe alınmamış akış kullanarak bir veya daha fazla dosyanın yüklenmesini destekler</span><span class="sxs-lookup"><span data-stu-id="98ef0-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="98ef0-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="98ef0-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="98ef0-107">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="98ef0-107">Security considerations</span></span>

<span data-ttu-id="98ef0-108">Kullanıcılara bir sunucuya dosya yükleme yeteneği sağlarken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="98ef0-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="98ef0-109">Saldırganlar şunları deneyebilir:</span><span class="sxs-lookup"><span data-stu-id="98ef0-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="98ef0-110">[Hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) saldırıları yürütün.</span><span class="sxs-lookup"><span data-stu-id="98ef0-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="98ef0-111">Virüsleri veya kötü amaçlı yazılımları karşıya yükleyin.</span><span class="sxs-lookup"><span data-stu-id="98ef0-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="98ef0-112">Ağları ve sunucuları diğer yollarla tehlikeye atabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="98ef0-113">Başarılı bir saldırının olasılığını azaltan güvenlik adımları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="98ef0-114">Dosyaları, tercihen sistem dışı bir sürücüye sahip bir özel dosya yükleme alanına yükleyin.</span><span class="sxs-lookup"><span data-stu-id="98ef0-114">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="98ef0-115">Adanmış bir konum kullanılması, karşıya yüklenen dosyalar üzerinde güvenlik kısıtlamaları yapmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-115">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="98ef0-116">Dosya karşıya yükleme konumunda yürütme izinlerini devre dışı bırak. &dagger;</span><span class="sxs-lookup"><span data-stu-id="98ef0-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="98ef0-117">Karşıya yüklenen dosyaları uygulamayla aynı dizin ağacında hiçbir şekilde kalıcı hale getirme. &dagger;</span><span class="sxs-lookup"><span data-stu-id="98ef0-117">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="98ef0-118">Uygulama tarafından belirlenen bir güvenli dosya adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="98ef0-119">Kullanıcı tarafından belirtilen bir dosya adı veya karşıya yüklenen dosyanın güvenilmeyen dosya adı kullanmayın. &dagger; Güvenilmeyen bir dosya adını bir kullanıcı arabiriminde veya bir günlüğe kaydetme iletisinde göstermek için, HTML-değeri kodlayın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="98ef0-120">Yalnızca belirli bir onaylanan dosya uzantıları kümesine izin ver. &dagger;</span><span class="sxs-lookup"><span data-stu-id="98ef0-120">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="98ef0-121">Kullanıcının bir açılan dosyayı karşıya yüklemesini engellemek için dosya biçimi imzasını denetleyin. &dagger; Örneğin, bir kullanıcının. *txt* uzantılı bir *. exe* dosyasını karşıya yüklemesine izin verme.</span><span class="sxs-lookup"><span data-stu-id="98ef0-121">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="98ef0-122">Sunucu üzerinde istemci tarafı denetimlerinin de gerçekleştirildiğinden emin olun. &dagger; İstemci tarafı denetimleri sağlamasına kolaydır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-122">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="98ef0-123">Karşıya yüklenen bir dosyanın boyutunu denetleyin ve beklenenden daha büyük olan karşıya yüklemeleri önleyin. &dagger;</span><span class="sxs-lookup"><span data-stu-id="98ef0-123">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="98ef0-124">Aynı ada sahip karşıya yüklenen bir dosya tarafından dosyaların üzerine yazılmaması gerektiğinde, dosyayı karşıya yüklemeden önce dosya adını veritabanına veya fiziksel depolamaya göre denetleyin.</span><span class="sxs-lookup"><span data-stu-id="98ef0-124">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="98ef0-125">**Dosya depolanmadan önce karşıya yüklenen içerik üzerinde bir virüs/kötü amaçlı yazılım tarayıcısı çalıştırın.**</span><span class="sxs-lookup"><span data-stu-id="98ef0-125">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="98ef0-126">&dagger; örnek uygulama, ölçütlere uyan bir yaklaşımı gösterir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-126">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="98ef0-127">Kötü amaçlı bir kodun bir sisteme karşıya yükleme için kod yürütme için ilk adımı sık şöyledir:</span><span class="sxs-lookup"><span data-stu-id="98ef0-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="98ef0-128">Sistemin denetimini tamamen elde edin.</span><span class="sxs-lookup"><span data-stu-id="98ef0-128">Completely gain control of a system.</span></span>
> * <span data-ttu-id="98ef0-129">Sistemin kilitlenme sonucuyla bir sistemi aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="98ef0-129">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="98ef0-130">Kullanıcı veya sistem verilerini tehlikeye.</span><span class="sxs-lookup"><span data-stu-id="98ef0-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="98ef0-131">Genel Kullanıcı arabirimine Graffiti uygulayın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-131">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="98ef0-132">Kullanıcıların dosyaları kabul ederken saldırı yüzey alanı azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-132">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="98ef0-133">Sınırsız dosya karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="98ef0-133">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="98ef0-134">[Azure güvenliği: Kullanıcılardan dosyaları kabul etmek için uygun denetimlerin yerinde olduğundan emin olun @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="98ef0-134">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="98ef0-135">Örnek uygulamadaki örnekler de dahil olmak üzere güvenlik önlemlerini uygulama hakkında daha fazla bilgi için [doğrulama](#validation) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-135">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="98ef0-136">Depolama senaryoları</span><span class="sxs-lookup"><span data-stu-id="98ef0-136">Storage scenarios</span></span>

<span data-ttu-id="98ef0-137">Dosyalar için ortak depolama seçenekleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-137">Common storage options for files include:</span></span>

* <span data-ttu-id="98ef0-138">Database</span><span class="sxs-lookup"><span data-stu-id="98ef0-138">Database</span></span>

  * <span data-ttu-id="98ef0-139">Küçük dosya yüklemeleri için, bir veritabanı genellikle fiziksel depolama (dosya sistemi veya ağ paylaşımından) seçeneklerinden daha hızlıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-139">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="98ef0-140">Kullanıcı verileri için bir veritabanı kaydının alınması eşzamanlı olarak dosya içeriğini (örneğin, avatar görüntüsü) sağlayabildiğinden, veritabanı genellikle fiziksel depolama seçeneklerine göre daha uygundur.</span><span class="sxs-lookup"><span data-stu-id="98ef0-140">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="98ef0-141">Bir veritabanı, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-141">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="98ef0-142">Fiziksel depolama (dosya sistemi veya ağ paylaşma)</span><span class="sxs-lookup"><span data-stu-id="98ef0-142">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="98ef0-143">Büyük dosya yüklemeleri için:</span><span class="sxs-lookup"><span data-stu-id="98ef0-143">For large file uploads:</span></span>
    * <span data-ttu-id="98ef0-144">Veritabanı limitleri karşıya yükleme boyutunu kısıtlayabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-144">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="98ef0-145">Fiziksel depolama genellikle bir veritabanındaki depolamadan daha az ekonomik olur.</span><span class="sxs-lookup"><span data-stu-id="98ef0-145">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="98ef0-146">Fiziksel depolama, veri depolama hizmeti kullanmaktan daha ucuz olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-146">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="98ef0-147">Uygulamanın işlemi, depolama konumu için okuma ve yazma izinlerine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-147">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="98ef0-148">**Hiçbir zaman yürütme izni vermeyin.**</span><span class="sxs-lookup"><span data-stu-id="98ef0-148">**Never grant execute permission.**</span></span>

* <span data-ttu-id="98ef0-149">Veri depolama hizmeti (örneğin, [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="98ef0-149">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="98ef0-150">Hizmetler genellikle tek hata noktalarına tabi olan şirket içi çözümler üzerinde geliştirilmiş ölçeklenebilirlik ve esneklik sunar.</span><span class="sxs-lookup"><span data-stu-id="98ef0-150">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="98ef0-151">Hizmetler büyük depolama altyapısı senaryolarında düşük maliyetlidir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-151">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="98ef0-152">Daha fazla bilgi için bkz [. hızlı başlangıç: Nesne depolamada bir blob oluşturmak için .NET kullanın @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="98ef0-152">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="98ef0-153">Konu <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*> ' ı gösterir, ancak <xref:System.IO.Stream> ile çalışırken <xref:System.IO.FileStream> ' yi blob depolamaya kaydetmek için <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-153">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="98ef0-154">Karşıya dosya yükleme senaryoları</span><span class="sxs-lookup"><span data-stu-id="98ef0-154">File upload scenarios</span></span>

<span data-ttu-id="98ef0-155">Dosyaları karşıya yüklemek için iki genel yaklaşım arabelleğe alınır ve akışlardır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="98ef0-156">**Ara**</span><span class="sxs-lookup"><span data-stu-id="98ef0-156">**Buffering**</span></span>

<span data-ttu-id="98ef0-157">Dosyanın tamamı, dosyayı işlemek veya kaydetmek için kullanılan dosyanın C# temsili olan <xref:Microsoft.AspNetCore.Http.IFormFile> ' a okunurdur.</span><span class="sxs-lookup"><span data-stu-id="98ef0-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="98ef0-158">Dosya karşıya yüklemeleri tarafından kullanılan kaynaklar (disk, bellek), eşzamanlı dosya karşıya yüklemelerinin sayısına ve boyutuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="98ef0-159">Bir uygulama çok fazla karşıya yükleme arabelleğini denerse, bellek veya disk alanı tükenirse site çöker.</span><span class="sxs-lookup"><span data-stu-id="98ef0-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="98ef0-160">Karşıya dosya yükleme boyutu veya sıklığı uygulama kaynaklarını tüketme ise, akış ' ı kullanın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="98ef0-161">64 KB geçen tek bir arabelleğe alınmış dosya, bellekten diskte geçici bir dosyaya taşınır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="98ef0-162">Dosya arabelleğe alma, bu konunun aşağıdaki bölümlerinde ele alınmıştır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="98ef0-163">Fiziksel depolama alanı</span><span class="sxs-lookup"><span data-stu-id="98ef0-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="98ef0-164">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="98ef0-164">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="98ef0-165">**Akış**</span><span class="sxs-lookup"><span data-stu-id="98ef0-165">**Streaming**</span></span>

<span data-ttu-id="98ef0-166">Dosya çok parçalı bir istekten alınır ve doğrudan uygulama tarafından işlenir veya kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="98ef0-167">Akış performansı önemli ölçüde iyileştirmez.</span><span class="sxs-lookup"><span data-stu-id="98ef0-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="98ef0-168">Akış, dosya karşıya yüklenirken bellek veya disk alanı taleplerini azaltır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="98ef0-169">Akış büyük dosyaları [akış ile büyük dosyaları karşıya yükle](#upload-large-files-with-streaming) bölümünde ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="98ef0-170">Fiziksel depolamaya arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="98ef0-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="98ef0-171">Küçük dosyaları karşıya yüklemek için, çok parçalı bir form kullanın veya JavaScript kullanarak bir POST isteği oluşturun.</span><span class="sxs-lookup"><span data-stu-id="98ef0-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="98ef0-172">Aşağıdaki örnek, tek bir dosyayı karşıya yüklemek için bir Razor Pages formunun kullanımını gösterir (örnek uygulamada*Pages/Bufferedsinglefileuploadfiziksel. cshtml* ):</span><span class="sxs-lookup"><span data-stu-id="98ef0-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="98ef0-173">Aşağıdaki örnek, önceki örneğe benzerdir, ancak şunları hariç:</span><span class="sxs-lookup"><span data-stu-id="98ef0-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="98ef0-174">Form verilerini göndermek için JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="98ef0-175">Doğrulama yok.</span><span class="sxs-lookup"><span data-stu-id="98ef0-175">There's no validation.</span></span>

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

<span data-ttu-id="98ef0-176">[Fetch API 'sini desteklemeyen](https://caniuse.com/#feat=fetch)istemcilerde form gönderisini JavaScript 'te gerçekleştirmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="98ef0-177">Fetch Polyfill kullanın (örneğin, [Window. Fetch (GitHub/fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="98ef0-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="98ef0-178">Kullanın `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="98ef0-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="98ef0-179">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="98ef0-179">For example:</span></span>

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

<span data-ttu-id="98ef0-180">Dosya karşıya yüklemelerini desteklemek için HTML formları `multipart/form-data` ' in bir kodlama türü (`enctype`) belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="98ef0-181">Birden çok dosyayı karşıya yüklemeyi destekleyecek @no__t 0 giriş öğesi için `<input>` öğesinde `multiple` özniteliği sağlayın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="98ef0-182">Sunucuya yüklenen tek dosyalara <xref:Microsoft.AspNetCore.Http.IFormFile> kullanılarak [model bağlama](xref:mvc/models/model-binding) aracılığıyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="98ef0-183">Örnek uygulama, veritabanı ve fiziksel depolama senaryoları için birden çok arabellekli dosya yüklemeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="98ef0-184">Doğrulaması olmadan <xref:Microsoft.AspNetCore.Http.IFormFile> ' in `FileName` özelliğine güvenmeyin veya güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="98ef0-184">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="98ef0-185">@No__t-0 özelliği yalnızca görüntüleme amacıyla ve değeri HTML kodladıktan sonra kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-185">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="98ef0-186">Bu nedenle, şu ana kadar dikkate alınması gereken örnekler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-186">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="98ef0-187">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-187">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="98ef0-188">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="98ef0-188">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="98ef0-189">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="98ef0-189">Validation</span></span>](#validation)

<span data-ttu-id="98ef0-190">Model bağlama ve <xref:Microsoft.AspNetCore.Http.IFormFile> kullanarak dosyaları karşıya yüklerken eylem yöntemi şunları kabul edebilir:</span><span class="sxs-lookup"><span data-stu-id="98ef0-190">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="98ef0-191">Tek bir <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="98ef0-191">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="98ef0-192">Birkaç dosyayı temsil eden aşağıdaki koleksiyonlardan herhangi biri:</span><span class="sxs-lookup"><span data-stu-id="98ef0-192">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="98ef0-193">[Liste](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3</span><span class="sxs-lookup"><span data-stu-id="98ef0-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="98ef0-194">Bağlama, form dosyaları adına göre eşleşir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-194">Binding matches form files by name.</span></span> <span data-ttu-id="98ef0-195">Örneğin, `<input type="file" name="formFile">` ' deki HTML `name` değeri C# parametre/özellik ile (`FormFile`) eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-195">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="98ef0-196">Daha fazla bilgi için, [ad öznitelik DEĞERINI Post yönteminin parametre adına eşleştirin](#match-name-attribute-value-to-parameter-name-of-post-method) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-196">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="98ef0-197">Aşağıdaki örnek:</span><span class="sxs-lookup"><span data-stu-id="98ef0-197">The following example:</span></span>

* <span data-ttu-id="98ef0-198">Karşıya yüklenen bir veya daha fazla dosya üzerinden döngü.</span><span class="sxs-lookup"><span data-stu-id="98ef0-198">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="98ef0-199">Dosya adı da dahil olmak üzere bir dosyanın tam yolunu döndürmek için [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-199">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="98ef0-200">Dosyalar, uygulama tarafından oluşturulan bir dosya adı kullanılarak yerel dosya sistemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-200">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="98ef0-201">Karşıya yüklenen dosyaların toplam sayısını ve boyutunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="98ef0-201">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="98ef0-202">Yol olmadan bir dosya adı oluşturmak için `Path.GetRandomFileName` kullanın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-202">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="98ef0-203">Aşağıdaki örnekte, yol yapılandırmadan alınır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-203">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="98ef0-204">@No__t-0 ' a geçirilen yolun dosya adı içermesi *gerekir* .</span><span class="sxs-lookup"><span data-stu-id="98ef0-204">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="98ef0-205">Dosya adı sağlanmazsa, çalışma zamanında bir <xref:System.UnauthorizedAccessException> oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="98ef0-205">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="98ef0-206">@No__t-0 tekniği kullanılarak yüklenen dosyalar, işlemeden önce sunucuda veya diskte bellek halinde arabelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-206">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="98ef0-207">Eylem yönteminin içinde, <xref:Microsoft.AspNetCore.Http.IFormFile> içerikleri <xref:System.IO.Stream> olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-207">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="98ef0-208">Yerel dosya sistemine ek olarak, dosyalar bir ağ paylaşımında veya [Azure Blob depolama](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)gibi bir dosya depolama hizmetine kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-208">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="98ef0-209">Karşıya yüklemek için birden çok dosya üzerinde döngü yapan ve güvenli dosya adları kullanan başka bir örnek için, örnek uygulamadaki *Pages/Bufferedmultiplefileuploadfiziksel. cshtml. cs* dosyasına bakın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-209">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="98ef0-210">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , önceki geçici dosyaları silmeden 65.535 'den fazla dosya oluşturulduysa <xref:System.IO.IOException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98ef0-210">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="98ef0-211">65.535 dosya sınırının sunucu başına sınırı vardır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-211">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="98ef0-212">Windows işletim sistemi için bu sınır hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-212">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="98ef0-213">GetTempFileNameA işlevi</span><span class="sxs-lookup"><span data-stu-id="98ef0-213">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="98ef0-214">Bir veritabanına arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="98ef0-214">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="98ef0-215">İkili dosya verilerini [Entity Framework](/ef/core/index)kullanarak bir veritabanında depolamak için, varlıkta bir <xref:System.Byte> dizi özelliği tanımlayın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-215">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="98ef0-216">@No__t içeren sınıf için bir sayfa modeli özelliği belirtin-0:</span><span class="sxs-lookup"><span data-stu-id="98ef0-216">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="98ef0-217"><xref:Microsoft.AspNetCore.Http.IFormFile>, doğrudan bir eylem yöntemi parametresi veya bir bağlantılı model özelliği olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-217"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="98ef0-218">Önceki örnekte, bir bağlantılı model özelliği kullanılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-218">The prior example uses a bound model property.</span></span>

<span data-ttu-id="98ef0-219">@No__t-0 Razor Pages formunda kullanılır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-219">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="98ef0-220">Form sunucuya gönderildiğinde, <xref:Microsoft.AspNetCore.Http.IFormFile> ' ı bir akışa kopyalayın ve veritabanına bir bayt dizisi olarak kaydedin.</span><span class="sxs-lookup"><span data-stu-id="98ef0-220">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="98ef0-221">Aşağıdaki örnekte, `_dbContext`, uygulamanın veritabanı bağlamını depolar:</span><span class="sxs-lookup"><span data-stu-id="98ef0-221">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="98ef0-222">Yukarıdaki örnek, örnek uygulamada gösterilen senaryoya benzerdir:</span><span class="sxs-lookup"><span data-stu-id="98ef0-222">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="98ef0-223">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="98ef0-223">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="98ef0-224">*Pages/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="98ef0-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="98ef0-225">İkili verileri ilişkisel veritabanlarında depolarken dikkatli olun, çünkü performansı olumsuz etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-225">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="98ef0-226">Doğrulaması olmadan <xref:Microsoft.AspNetCore.Http.IFormFile> ' in `FileName` özelliğine güvenmeyin veya güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="98ef0-226">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="98ef0-227">@No__t-0 özelliği yalnızca görüntüleme amacıyla ve yalnızca HTML kodlaması sonrasında kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-227">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="98ef0-228">Belirtilen örneklerde dikkate alınması gereken önemli noktalar.</span><span class="sxs-lookup"><span data-stu-id="98ef0-228">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="98ef0-229">Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-229">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="98ef0-230">Güvenlik konuları</span><span class="sxs-lookup"><span data-stu-id="98ef0-230">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="98ef0-231">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="98ef0-231">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="98ef0-232">Akışa sahip büyük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="98ef0-232">Upload large files with streaming</span></span>

<span data-ttu-id="98ef0-233">Aşağıdaki örnek, bir denetleyiciyi bir denetleyici eyleminde akışa almak için JavaScript 'in nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-233">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="98ef0-234">Dosyanın antiforgery belirteci özel bir filtre özniteliği kullanılarak oluşturulur ve istek gövdesi yerine istemci HTTP üst bilgilerine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-234">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="98ef0-235">Eylem yöntemi karşıya yüklenen verileri doğrudan işlediğinden, form modeli bağlama başka bir özel filtre tarafından devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="98ef0-235">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="98ef0-236">Eylem içinde formun içerikleri `MultipartReader` kullanarak okunur, her bir `MultipartSection` ' i okur, dosyayı işliyor veya içeriği uygun şekilde depoluyor.</span><span class="sxs-lookup"><span data-stu-id="98ef0-236">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="98ef0-237">Çok parçalı bölümler okunduktan sonra eylem kendi model bağlamasını gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-237">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="98ef0-238">İlk sayfa yanıtı formu yükler ve bir tanımlama bilgisine bir antiforgery belirteci kaydeder (`GenerateAntiforgeryTokenCookieAttribute` özniteliği aracılığıyla).</span><span class="sxs-lookup"><span data-stu-id="98ef0-238">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="98ef0-239">Öznitelik, bir istek belirtecine sahip bir tanımlama bilgisi ayarlamak için ASP.NET Core yerleşik [antiforgery desteğini](xref:security/anti-request-forgery) kullanır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-239">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="98ef0-240">@No__t-0, model bağlamayı devre dışı bırakmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-240">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="98ef0-241">Örnek uygulamada `GenerateAntiforgeryTokenCookieAttribute` ve `DisableFormValueModelBindingAttribute`, [@no__t kuralları](xref:razor-pages/razor-pages-conventions)kullanılarak Razor Pages-4 ' te `/StreamedSingleFileUploadDb` ve `/StreamedSingleFileUploadPhysical` ' ün sayfa uygulama modellerine filtre olarak uygulanır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-241">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="98ef0-242">Model bağlama formu okumadığından formdan bağlanan parametreler bağlanamaz (sorgu, yol ve başlık çalışmaya devam eder).</span><span class="sxs-lookup"><span data-stu-id="98ef0-242">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="98ef0-243">Action yöntemi, `Request` özelliği ile doğrudan işe yarar.</span><span class="sxs-lookup"><span data-stu-id="98ef0-243">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="98ef0-244">@No__t-0, her bölümü okumak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-244">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="98ef0-245">Anahtar/değer verileri `KeyValueAccumulator` ' da depolanır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-245">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="98ef0-246">Çok parçalı bölümler okunduktan sonra, form verilerini bir model türüne bağlamak için `KeyValueAccumulator` içerikleri kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-246">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="98ef0-247">EF Core bir veritabanına akış için tam `StreamingController.UploadDatabase` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="98ef0-247">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="98ef0-248">Fiziksel bir konuma akış için tam `StreamingController.UploadPhysical` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="98ef0-248">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="98ef0-249">Örnek uygulamada, doğrulama denetimleri `FileHelpers.ProcessStreamedFile` tarafından işlenir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-249">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="98ef0-250">Doğrulama</span><span class="sxs-lookup"><span data-stu-id="98ef0-250">Validation</span></span>

<span data-ttu-id="98ef0-251">Örnek uygulamanın `FileHelpers` sınıfı, arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> ve akış dosya yüklemeleri için birkaç denetim gösterir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-251">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="98ef0-252">Örnek uygulamada <xref:Microsoft.AspNetCore.Http.IFormFile> arabelleğe alınmış dosya yüklemelerini işlemek için, *Utilities/Fileyardımcılar. cs* dosyasındaki `ProcessFormFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-252">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="98ef0-253">Akış dosyalarını işlemek için aynı dosyada `ProcessStreamedFile` yöntemine bakın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-253">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="98ef0-254">Örnek uygulamada gösterilen doğrulama işleme yöntemleri karşıya yüklenen dosyaların içeriğini taramaz.</span><span class="sxs-lookup"><span data-stu-id="98ef0-254">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="98ef0-255">Çoğu üretim senaryosunda, dosyanın kullanıcılara veya diğer sistemlere kullanılabilir hale getirilmesi için dosya üzerinde bir virüs/kötü amaçlı yazılım tarayıcı API 'SI kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-255">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="98ef0-256">Konu örneği, doğrulama tekniklerine yönelik çalışan bir örnek sağlasa da şunları gerçekleştirmediğiniz takdirde bir üretim uygulamasında `FileHelpers` sınıfını uygulamayın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-256">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="98ef0-257">Uygulamayı tam olarak anlayın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-257">Fully understand the implementation.</span></span>
> * <span data-ttu-id="98ef0-258">Uygulamayı uygulamanın ortamı ve belirtimleri için uygun şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="98ef0-258">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="98ef0-259">**Bu gereksinimleri bilmeden bir uygulamada güvenlik kodunu hiçbir şekilde sayısının fark gözetmeden uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="98ef0-259">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="98ef0-260">İçerik doğrulama</span><span class="sxs-lookup"><span data-stu-id="98ef0-260">Content validation</span></span>

<span data-ttu-id="98ef0-261">**Karşıya yüklenen içerikte üçüncü taraf bir virüs/kötü amaçlı yazılım tarama API 'SI kullanın.**</span><span class="sxs-lookup"><span data-stu-id="98ef0-261">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="98ef0-262">Dosyaları tarama, yüksek hacimli senaryolarda sunucu kaynaklarında yoğun bir şekilde yapılır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-262">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="98ef0-263">Dosya tarama nedeniyle istek işleme performansı azaldığında, tarama işini, muhtemelen uygulamanın sunucusundan farklı bir sunucuda çalışan bir [arka plan hizmetine](xref:fundamentals/host/hosted-services)devredere göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="98ef0-263">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="98ef0-264">Genellikle, arka plan virüs tarayıcısı tarafından denetlene kadar karşıya yüklenen dosyalar karantinaya alınmış bir alanda tutulur.</span><span class="sxs-lookup"><span data-stu-id="98ef0-264">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="98ef0-265">Bir dosya geçtiğinde dosya normal dosya depolama konumuna taşınır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-265">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="98ef0-266">Bu adımlar genellikle bir dosyanın tarama durumunu gösteren bir veritabanı kaydıyla birlikte gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-266">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="98ef0-267">Böyle bir yaklaşım kullanarak, uygulama ve uygulama sunucusu isteklere yanıt vermeye odaklanmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="98ef0-267">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="98ef0-268">Dosya Uzantısı doğrulaması</span><span class="sxs-lookup"><span data-stu-id="98ef0-268">File extension validation</span></span>

<span data-ttu-id="98ef0-269">Karşıya yüklenen dosyanın uzantısı izin verilen uzantılar listesine göre denetlenmelidir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-269">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="98ef0-270">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="98ef0-270">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="98ef0-271">Dosya imzası doğrulaması</span><span class="sxs-lookup"><span data-stu-id="98ef0-271">File signature validation</span></span>

<span data-ttu-id="98ef0-272">Bir dosyanın imzası, bir dosyanın başlangıcında ilk birkaç bayta göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-272">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="98ef0-273">Bu baytlar, uzantının dosyanın içeriğiyle eşleşip eşleşmediğini göstermek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-273">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="98ef0-274">Örnek uygulama, birkaç ortak dosya türü için dosya imzalarını denetler.</span><span class="sxs-lookup"><span data-stu-id="98ef0-274">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="98ef0-275">Aşağıdaki örnekte, bir JPEG görüntüsünün dosya imzası, dosyaya karşı denetlenir:</span><span class="sxs-lookup"><span data-stu-id="98ef0-275">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="98ef0-276">Ek dosya imzaları almak için [Dosya Imzaları veritabanı](https://www.filesignatures.net/) ve resmi dosya belirtimleri bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-276">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="98ef0-277">Dosya adı güvenliği</span><span class="sxs-lookup"><span data-stu-id="98ef0-277">File name security</span></span>

<span data-ttu-id="98ef0-278">Fiziksel depolamaya bir dosyayı kaydetmek için hiçbir şekilde istemci tarafından sağlanan dosya adı kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-278">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="98ef0-279">Geçici depolama için tam yol (dosya adı da dahil olmak üzere) oluşturmak için [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) veya [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanarak dosya için güvenli bir dosya adı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="98ef0-279">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="98ef0-280">Razor otomatik olarak HTML, görüntüleme için özellik değerlerini kodlar.</span><span class="sxs-lookup"><span data-stu-id="98ef0-280">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="98ef0-281">Aşağıdaki kodun kullanımı güvenlidir:</span><span class="sxs-lookup"><span data-stu-id="98ef0-281">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="98ef0-282">Razor dışında, her zaman bir kullanıcının isteğinden <xref:System.Net.WebUtility.HtmlEncode*> dosya adı içeriği.</span><span class="sxs-lookup"><span data-stu-id="98ef0-282">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="98ef0-283">Birçok uygulama, dosyanın var olduğunu bir denetim içermelidir; Aksi takdirde, dosyanın üzerine aynı ada sahip bir dosya yazılır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-283">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="98ef0-284">Uygulamanızın belirtimlerini karşılamak için ek mantık sağlayın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-284">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="98ef0-285">Boyut doğrulaması</span><span class="sxs-lookup"><span data-stu-id="98ef0-285">Size validation</span></span>

<span data-ttu-id="98ef0-286">Karşıya yüklenen dosyaların boyutunu sınırlayın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-286">Limit the size of uploaded files.</span></span>

<span data-ttu-id="98ef0-287">Örnek uygulamada, dosyanın boyutu 2 MB ile sınırlıdır (bayt cinsinden gösterilir).</span><span class="sxs-lookup"><span data-stu-id="98ef0-287">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="98ef0-288">Sınır, *appSettings. JSON* dosyasındaki [yapılandırma](xref:fundamentals/configuration/index) yoluyla sağlanır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-288">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="98ef0-289">@No__t-0 `PageModel` sınıflarına eklenmiş:</span><span class="sxs-lookup"><span data-stu-id="98ef0-289">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="98ef0-290">Dosya boyutu sınırı aştığında, dosya reddedilir:</span><span class="sxs-lookup"><span data-stu-id="98ef0-290">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="98ef0-291">Name öznitelik değerini POST yönteminin Parameter adı ile Eşleştir</span><span class="sxs-lookup"><span data-stu-id="98ef0-291">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="98ef0-292">Form verileri oluşturan veya JavaScript 'in `FormData` ' ı kullanan Razor olmayan formlarda, formun öğesinde belirtilen adın veya `FormData` ' in, denetleyicinin eyleminde bulunan parametre adıyla aynı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-292">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="98ef0-293">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="98ef0-293">In the following example:</span></span>

* <span data-ttu-id="98ef0-294">@No__t-0 öğesi kullanılırken, `name` özniteliği `battlePlans` değerine ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-294">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="98ef0-295">JavaScript 'te `FormData` kullanırken, ad `battlePlans` değerine ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="98ef0-295">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="98ef0-296">C# Yöntemin parametresi için eşleşen bir ad kullanın (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="98ef0-296">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="98ef0-297">@No__t adlı Razor Pages sayfa işleyicisi yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="98ef0-297">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="98ef0-298">MVC POST denetleyicisi eylem yöntemi için:</span><span class="sxs-lookup"><span data-stu-id="98ef0-298">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="98ef0-299">Sunucu ve uygulama yapılandırması</span><span class="sxs-lookup"><span data-stu-id="98ef0-299">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="98ef0-300">Çok parçalı gövde uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="98ef0-300">Multipart body length limit</span></span>

<span data-ttu-id="98ef0-301"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>, her bir çok parçalı gövdenin uzunluk sınırını ayarlar.</span><span class="sxs-lookup"><span data-stu-id="98ef0-301"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="98ef0-302">Bu sınırı aşan form bölümleri ayrıştırıldığında <xref:System.IO.InvalidDataException> oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98ef0-302">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="98ef0-303">Varsayılan değer 134.217.728 ' dir (128 MB).</span><span class="sxs-lookup"><span data-stu-id="98ef0-303">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="98ef0-304">@No__t-1 ' deki <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarını kullanarak limiti özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="98ef0-304">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="98ef0-305"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>, tek bir sayfa veya eylem için <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-305"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="98ef0-306">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices` ' deki bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-306">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="98ef0-307">Razor Pages bir uygulamada veya bir MVC uygulamasında, filtreyi sayfa modeline veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-307">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="98ef0-308">Kestrel maksimum istek gövdesi boyutu</span><span class="sxs-lookup"><span data-stu-id="98ef0-308">Kestrel maximum request body size</span></span>

<span data-ttu-id="98ef0-309">Kestrel tarafından barındırılan uygulamalar için, varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu, yaklaşık 28,6 MB 'tır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-309">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="98ef0-310">[MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel Server seçeneğini kullanarak sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="98ef0-310">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        })
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

<span data-ttu-id="98ef0-311"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>, tek sayfa veya eylem için [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) ayarlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-311"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="98ef0-312">Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices` ' deki bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-312">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="98ef0-313">Bir Razor sayfaları uygulamasında veya bir MVC uygulamasında, filtreyi sayfa işleyici sınıfına veya eylem yöntemine uygulayın:</span><span class="sxs-lookup"><span data-stu-id="98ef0-313">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="98ef0-314">Diğer Kestrel limitleri</span><span class="sxs-lookup"><span data-stu-id="98ef0-314">Other Kestrel limits</span></span>

<span data-ttu-id="98ef0-315">Kestrel tarafından barındırılan uygulamalar için diğer Kestrel limitleri de uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="98ef0-315">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="98ef0-316">İstemci bağlantıları üst sınırı</span><span class="sxs-lookup"><span data-stu-id="98ef0-316">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="98ef0-317">İstek ve yanıt veri ücretleri</span><span class="sxs-lookup"><span data-stu-id="98ef0-317">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="98ef0-318">IIS içerik uzunluğu sınırı</span><span class="sxs-lookup"><span data-stu-id="98ef0-318">IIS content length limit</span></span>

<span data-ttu-id="98ef0-319">Varsayılan istek sınırı (`maxAllowedContentLength`), yaklaşık 28.6 MB olan 30.000.000 bayttır.</span><span class="sxs-lookup"><span data-stu-id="98ef0-319">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="98ef0-320">*Web. config* dosyasında sınırı özelleştirin:</span><span class="sxs-lookup"><span data-stu-id="98ef0-320">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="98ef0-321">Bu ayar yalnızca IIS için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-321">This setting only applies to IIS.</span></span> <span data-ttu-id="98ef0-322">Kestrel üzerinde barındırırken davranış varsayılan olarak gerçekleşmez.</span><span class="sxs-lookup"><span data-stu-id="98ef0-322">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="98ef0-323">Daha fazla bilgi için bkz. [Istek limitleri \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="98ef0-323">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="98ef0-324">ASP.NET Core modülündeki sınırlamalar veya IIS Istek filtreleme modülünün varlığı, karşıya yüklemeleri 2 veya 4 GB ile sınırlandırabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-324">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="98ef0-325">Daha fazla bilgi için bkz. [2 GB 'tan büyük dosya karşıya yüklenemiyor (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="98ef0-325">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="98ef0-326">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="98ef0-326">Troubleshoot</span></span>

<span data-ttu-id="98ef0-327">Dosyaları karşıya yükleme ve olası çözümleri ile çalışırken karşılaşılan bazı yaygın sorunlar aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-327">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="98ef0-328">Bir IIS sunucusuna dağıtılırken bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="98ef0-328">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="98ef0-329">Aşağıdaki hata karşıya yüklenen dosyanın, sunucunun yapılandırılmış içerik uzunluğunu aştığını gösterir:</span><span class="sxs-lookup"><span data-stu-id="98ef0-329">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="98ef0-330">Limiti artırma hakkında daha fazla bilgi için bkz. [IIS içerik uzunluğu sınırı](#iis-content-length-limit) bölümü.</span><span class="sxs-lookup"><span data-stu-id="98ef0-330">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="98ef0-331">Bağlantı hatası</span><span class="sxs-lookup"><span data-stu-id="98ef0-331">Connection failure</span></span>

<span data-ttu-id="98ef0-332">Bir bağlantı hatası ve bir sıfırlama sunucu bağlantısı büyük olasılıkla karşıya yüklenen dosyanın Kestrel 'in en büyük istek gövdesi boyutunu aştığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-332">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="98ef0-333">Daha fazla bilgi için, [Kestrel maksimum istek gövdesi boyutu](#kestrel-maximum-request-body-size) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-333">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="98ef0-334">Kestrel istemci bağlantı limitleri de ayarlama gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-334">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="98ef0-335">Iformfile ile null başvuru özel durumu</span><span class="sxs-lookup"><span data-stu-id="98ef0-335">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="98ef0-336">Denetleyici <xref:Microsoft.AspNetCore.Http.IFormFile> kullanarak karşıya yüklenen dosyaları kabul ediyorsanız, ancak değer `null` ise HTML formunun `multipart/form-data` @no__t 2 değerini belirtdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="98ef0-336">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="98ef0-337">Bu öznitelik `<form>` öğesinde ayarlanmamışsa, dosya karşıya yükleme gerçekleşmez ve herhangi bir bağlı <xref:Microsoft.AspNetCore.Http.IFormFile> bağımsız değişkeni `null` ' dir.</span><span class="sxs-lookup"><span data-stu-id="98ef0-337">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="98ef0-338">Ayrıca, [form verilerinde karşıya yükleme adlandırmasının uygulamanın adlandırmayla eşleştiğinden](#match-name-attribute-value-to-parameter-name-of-post-method)emin olun.</span><span class="sxs-lookup"><span data-stu-id="98ef0-338">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="98ef0-339">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="98ef0-339">Additional resources</span></span>

* [<span data-ttu-id="98ef0-340">Sınırsız dosya karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="98ef0-340">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* <span data-ttu-id="98ef0-341">[Azure güvenliği: Güvenlik çerçevesi: Giriş doğrulaması | Azaltmaları @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="98ef0-341">[Azure Security: Security Frame: Input Validation | Mitigations](/azure/security/azure-security-threat-modeling-tool-input-validation)</span></span>
* <span data-ttu-id="98ef0-342">[Azure bulut tasarım desenleri: Valet anahtar deseninin @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="98ef0-342">[Azure Cloud Design Patterns: Valet Key pattern](/azure/architecture/patterns/valet-key)</span></span>
