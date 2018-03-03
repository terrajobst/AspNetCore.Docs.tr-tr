---
title: "ASP.NET Core dosya yüklemeleri"
author: ardalis
description: "Model bağlama ve akış ASP.NET Core MVC dosyaları karşıya yüklemek için nasıl kullanılacağı."
manager: wpickett
ms.author: riande
ms.date: 07/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/file-uploads
ms.openlocfilehash: bfcbddb208fedaa4de46df782336176d97ea5bdc
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="29df9-103">ASP.NET Core dosya yüklemeleri</span><span class="sxs-lookup"><span data-stu-id="29df9-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="29df9-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="29df9-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="29df9-105">ASP.NET MVC Eylemler, daha küçük dosyalar için bağlama veya daha büyük dosyalar için akış basit modeli kullanarak bir veya daha fazla dosya karşıya yüklemeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="29df9-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="29df9-106">Görüntülemek veya örnek Github'dan indirin</span><span class="sxs-lookup"><span data-stu-id="29df9-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="29df9-107">Model bağlama ile küçük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="29df9-107">Uploading small files with model binding</span></span>

<span data-ttu-id="29df9-108">Küçük dosyaları yüklemek için çok parçalı bir HTML formu kullanın veya JavaScript kullanarak bir POST isteği oluşturun.</span><span class="sxs-lookup"><span data-stu-id="29df9-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="29df9-109">Birden çok karşıya yüklenen dosyaların destekler, Razor kullanarak bir örnek form aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="29df9-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="29df9-110">Dosya yüklemeleri desteklemek için HTML formları belirtmeniz gerekir. bir `enctype` , `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="29df9-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="29df9-111">`files` Yukarıda gösterilen giriş öğesi tarafından desteklenen birden çok dosya karşıya yükleme.</span><span class="sxs-lookup"><span data-stu-id="29df9-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="29df9-112">Atlayın `multiple` özniteliği giriş bu öğede yalnızca tek bir karşıya yüklenecek dosyayı izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="29df9-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="29df9-113">Bir tarayıcıda yukarıdaki biçimlendirme oluşturur:</span><span class="sxs-lookup"><span data-stu-id="29df9-113">The above markup renders in a browser as:</span></span>

![Dosyayı karşıya yükle formunu](file-uploads/_static/upload-form.png)

<span data-ttu-id="29df9-115">Sunucuya yüklenen tek tek dosyaların üzerinden erişilen [Model bağlama](xref:mvc/models/model-binding) kullanarak [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="29df9-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="29df9-116">`IFormFile` aşağıdaki yapıya sahiptir:</span><span class="sxs-lookup"><span data-stu-id="29df9-116">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="29df9-117">Kullanır veya güven ilişkisi olmayan `FileName` özelliği doğrulama olmadan.</span><span class="sxs-lookup"><span data-stu-id="29df9-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="29df9-118">`FileName` Özelliği görüntüleme amacıyla yalnızca kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="29df9-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="29df9-119">Model bağlama kullanarak dosyaları karşıya yüklenirken ve `IFormFile` arabirimi, ya da tek bir eylem yönteminin kabul edebilir `IFormFile` veya bir `IEnumerable<IFormFile>` (veya `List<IFormFile>`) pek çok dosya temsil eden.</span><span class="sxs-lookup"><span data-stu-id="29df9-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="29df9-120">Aşağıdaki örnekte bir veya daha fazla karşıya yüklenen dosyalar ile döngüler, yerel dosya sistemine kaydeder ve toplam sayısı ve karşıya yüklenen dosyaların boyutu döndürür.</span><span class="sxs-lookup"><span data-stu-id="29df9-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="29df9-121">Kullanarak karşıya yüklenen dosyaların `IFormFile` tekniği arabelleğe bellek veya disk web sunucusu üzerinde işlenmeden önce.</span><span class="sxs-lookup"><span data-stu-id="29df9-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="29df9-122">Eylem yönteminin içine `IFormFile` içeriklerini bir akış olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="29df9-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="29df9-123">Yerel dosya sistemi ek olarak, dosyalar için gönderilebilen [Azure Blob Depolama](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) veya [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="29df9-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="29df9-124">Entity Framework kullanarak bir veritabanında ikili dosya verileri depolamak için türünde bir özellik tanımlamak `byte[]` varlık üzerinde:</span><span class="sxs-lookup"><span data-stu-id="29df9-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="29df9-125">Türünde bir viewmodel özelliği belirtin `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="29df9-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="29df9-126">`IFormFile` doğrudan eylem yöntemi parametresini veya viewmodel özelliği olarak yukarıda gösterildiği gibi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="29df9-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="29df9-127">Kopya `IFormFile` akış ve bayt dizisine kaydedin:</span><span class="sxs-lookup"><span data-stu-id="29df9-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="29df9-128">Performansı olumsuz etkileyebilir gibi ilişkisel veritabanları, ikili veri depolarken dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="29df9-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="29df9-129">Akış ile büyük dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="29df9-129">Uploading large files with streaming</span></span>

<span data-ttu-id="29df9-130">Boyutunu veya dosya yüklemeleriyle sıklığını kaynak uygulama için soruna yol açıyorsa, yukarıda gösterilen model bağlama yaklaşım yaptığı gibi tamamının, arabelleğe alma yerine dosya karşıya yükleme akış göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="29df9-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="29df9-131">Kullanırken `IFormFile` ve model bağlama çok daha basit bir çözüm, akış çeşitli düzgün bir şekilde uygulamak için adımları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="29df9-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="29df9-132">64 KB aşan herhangi tek bir arabelleğe alınan dosya RAM disk üzerinde geçici bir dosya sunucusunda taşınır.</span><span class="sxs-lookup"><span data-stu-id="29df9-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="29df9-133">Dosya yüklemeleri tarafından kullanılan kaynakları (disk, RAM) numarasını ve eşzamanlı dosya yüklemeleri boyutuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="29df9-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="29df9-134">Akış çok perf ilgili değil, Ölçek hakkında.</span><span class="sxs-lookup"><span data-stu-id="29df9-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="29df9-135">Çok fazla karşıya arabellek çalışırsanız, bellek veya disk alanı dışında çalıştığında, sitenizi kilitleniyor.</span><span class="sxs-lookup"><span data-stu-id="29df9-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="29df9-136">Aşağıdaki örnek, bir denetleyici eylemi akış için JavaScript/Angular kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="29df9-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="29df9-137">Dosyanın antiforgery belirteci özel bir filtre özniteliğini kullanarak ve istek gövdesindeki HTTP üstbilgileri yerine geçilen oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="29df9-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="29df9-138">Eylem yönteminin karşıya yüklenen veriler doğrudan işlediğinden, model bağlama başka bir filtre tarafından devre dışı.</span><span class="sxs-lookup"><span data-stu-id="29df9-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="29df9-139">Kullanarak formun içeriklerini okuma eylem içinde bir `MultipartReader`, her kişi okur `MultipartSection`, dosya işleme veya içeriği uygun şekilde saklanması.</span><span class="sxs-lookup"><span data-stu-id="29df9-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="29df9-140">Tüm bölümleri okuyun sonra kendi model bağlama eylemi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="29df9-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="29df9-141">İlk eylemin formun yükler ve bir antiforgery belirteci bir tanımlama bilgisinde kaydeder (aracılığıyla `GenerateAntiforgeryTokenCookieForAjax` özniteliği):</span><span class="sxs-lookup"><span data-stu-id="29df9-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="29df9-142">ASP.NET Core'nın yerleşik özniteliğini kullanır [Antiforgery](xref:security/anti-request-forgery) bir tanımlama bilgisi isteği belirteci ile ayarlamak için destek:</span><span class="sxs-lookup"><span data-stu-id="29df9-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="29df9-143">Açısal otomatik olarak geçirir antiforgery belirteci adlı bir istek üstbilgisinde `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="29df9-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="29df9-144">ASP.NET Core MVC uygulama yapılandırmasıyla bu başlığında başvurmak için yapılandırılmış *haline*:</span><span class="sxs-lookup"><span data-stu-id="29df9-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="29df9-145">`DisableFormValueModelBinding` Aşağıda gösterilen özniteliği, model bağlama için devre dışı bırakmak için kullanılan `Upload` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="29df9-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="29df9-146">Model bağlama devre dışı olduğundan `Upload` eylem yönteminin parametreleri kabul etmez.</span><span class="sxs-lookup"><span data-stu-id="29df9-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="29df9-147">Doğrudan birlikte çalıştığı `Request` özelliği `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="29df9-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="29df9-148">A `MultipartReader` her bölüm okumak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="29df9-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="29df9-149">Dosya bir GUID dosya adı ile kaydedilir ve anahtar/değer veriler bir `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="29df9-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="29df9-150">Tüm bölümleri okuyun sonra içeriğini `KeyValueAccumulator` form verilerinin bir model türünü bağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="29df9-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="29df9-151">Tam `Upload` yöntemi aşağıda gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="29df9-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="29df9-152">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="29df9-152">Troubleshooting</span></span>

<span data-ttu-id="29df9-153">Dosyaları ve olası çözümleri karşıya ile çalışırken, bazı sık karşılaşılan sorunlar altındadır.</span><span class="sxs-lookup"><span data-stu-id="29df9-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="29df9-154">IIS ile beklenmeyen bulunamadı hatası</span><span class="sxs-lookup"><span data-stu-id="29df9-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="29df9-155">Dosya karşıya yükleme aşıyor sunucu şu hatayı gösterir yapılandırılmış `maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="29df9-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="29df9-156">Varsayılan ayar `30000000`, yaklaşık 28.6 MB olması.</span><span class="sxs-lookup"><span data-stu-id="29df9-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="29df9-157">Değer düzenleyerek özelleştirilebilir *web.config*:</span><span class="sxs-lookup"><span data-stu-id="29df9-157">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="29df9-158">Bu ayar, yalnızca IIS için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="29df9-158">This setting only applies to IIS.</span></span> <span data-ttu-id="29df9-159">Davranışı varsayılan olarak Kestrel üzerinde barındırdığında oluşmaz.</span><span class="sxs-lookup"><span data-stu-id="29df9-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="29df9-160">Daha fazla bilgi için bkz: [istek sınırları \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="29df9-160">For more information, see [Request Limits \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="29df9-161">IFormFile ile null başvuru özel durumu</span><span class="sxs-lookup"><span data-stu-id="29df9-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="29df9-162">Denetleyicinizi kabul etmiş olması durumunda kullanarak dosyaları karşıya `IFormFile` ancak değer her zaman null ise bulmak, HTML formu belirtilmesidir onaylayın bir `enctype` değerini `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="29df9-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="29df9-163">Bu öznitelik üzerinde ayarlanmamışsa `<form>` olmaz öğesi, dosya karşıya yükleme oluşur ve tüm ilişkili `IFormFile` bağımsız değişkenleri boş olacaktır.</span><span class="sxs-lookup"><span data-stu-id="29df9-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
