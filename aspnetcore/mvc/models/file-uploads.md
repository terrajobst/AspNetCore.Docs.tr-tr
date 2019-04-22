---
title: ASP.NET core'da dosya yüklemeleri
author: ardalis
description: ASP.NET Core MVC dosyaları karşıya yüklemek için model bağlama ve akış'ı kullanma
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 831f0c84f0ff062e9e24ccbf4ca81b7143c66e66
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59468559"
---
# <a name="file-uploads-in-aspnet-core"></a>ASP.NET core'da dosya yüklemeleri

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET MVC Eylemler, daha küçük dosyalar için bağlama veya daha büyük dosyalar için akış basit modelini kullanarak bir veya daha fazla dosya karşıya yüklemeyi destekler.

[Github'dan örnek görüntüleme veya indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Model bağlama ile küçük dosyaları karşıya yükleme

Küçük dosyaları karşıya yüklemek için çok bölümlü HTML form kullanın veya JavaScript kullanarak bir POST isteği oluşturun. Birden çok yüklenen dosyalar destekleyen Razor kullanarak bir örnek form aşağıda gösterilmiştir:

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple>
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload">
        </div>
    </div>
</form>
```

Karşıya dosya yükleme işlemleri desteklemek için HTML formları belirtmelisiniz bir `enctype` , `multipart/form-data`. `files` Yukarıda gösterilen giriş öğesinin desteklediği birden çok dosya karşıya yükleniyor. Atlamak `multiple` tek bir karşıya yüklenecek dosyayı izin vermek için bu giriş öğesindeki özniteliği. Bir tarayıcıda yukarıdaki biçimlendirme oluşturur:

![Dosya karşıya yükle formunu](file-uploads/_static/upload-form.png)

Sunucuya yüklenen tek tek dosyaları erişilebilir [Model bağlama](xref:mvc/models/model-binding) kullanarak [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) arabirimi. `IFormFile` aşağıdaki yapıya sahiptir:

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
> Dayanan veya güven ilişkisi olmayan `FileName` doğrulama olmadan özelliği. `FileName` Özelliği görüntüleme amacıyla yalnızca kullanılmalıdır.

Model bağlama kullanarak dosyaları karşıya yüklenirken ve `IFormFile` arabirimi, tek bir eylem yönteminin kabul edebilir `IFormFile` veya `IEnumerable<IFormFile>` (veya `List<IFormFile>`) çeşitli dosyaları gösteren. Aşağıdaki örnek bir veya daha fazla karşıya yüklenen dosyaları döngü, bunları yerel dosya sistemine kaydeder ve toplam sayısı ve karşıya yüklenen dosyaların boyutunu döndürür.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Kullanarak karşıya yüklenen dosyaların `IFormFile` tekniği arabelleğe bellekte veya diskte web sunucusunda işlenmeden önce. Eylem yöntemi içinde `IFormFile` içeriklerini bir akış olarak erişilebilir. Yerel dosya sistemi ek olarak, dosyalar için yapılabilen [Azure Blob Depolama](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs) veya [Entity Framework](/ef/core/index).

Entity Framework kullanarak bir veritabanında ikili dosya verilerini depolamak için bir özellik türü tanımlamak `byte[]` varlık üzerinde:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Viewmodel vlastnost typu belirtin `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile` yukarıda gösterildiği gibi bir eylem yöntemi parametresini doğrudan veya bir viewmodel özelliği olarak kullanılabilir.

Kopyalama `IFormFile` bir akışa ve bayt dizisine kaydedin:

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
        var user = new ApplicationUser 
        {
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
> Performansı olumsuz etkileyebilir gibi ilişkisel veritabanları, ikili veri depolarken dikkatli olun.

## <a name="uploading-large-files-with-streaming"></a>Akış ile büyük dosyaları karşıya yükleme

Dosya yüklemeleri sıklığını ve boyutunu kaynak uygulama için soruna yol açıyorsa, yukarıda gösterilen model bağlama yaklaşımı gibi dosya karşıya yükleme akış yerine sunabilen, arabelleğe göz önünde bulundurun. Kullanırken `IFormFile` ve model bağlama çok daha basit bir çözüm, akış, bir dizi adımı düzgün bir şekilde uygulamak için gerektirir.

> [!NOTE]
> 64 KB'yi aşan herhangi tek arabelleğe alınan dosya RAM disk üzerinde geçici bir dosya için sunucu üzerinde taşınır. Dosya yüklemeleri tarafından kullanılan kaynakları (disk, RAM), sayı ve eş zamanlı dosya yüklemeleri boyutuna bağlıdır. Akış çok perf hakkında değil, Ölçek hakkında. Çok fazla karşıya yüklemeler arabellek denerseniz, bellek veya disk alanı yetersiz çalıştığında, site kilitlenir.

Aşağıdaki örnek, bir denetleyici eylemi için akış için JavaScript/Angular kullanmayı gösterir. Özel bir filtre özniteliğini kullanarak ve istek gövdesindeki HTTP üst bilgilerini yerine geçirilen dosyanın antiforgery belirteç oluşturulur. Eylem yöntemi karşıya yüklenen verileri doğrudan işler, model bağlama başka bir filtre tarafından devre dışı bırakıldı. Kullanarak form denetiminin içeriğini okumak eylem içinde bir `MultipartReader`, her okuyan `MultipartSection`, dosya işleme veya içeriği uygun şekilde depolanması. Tüm bölümleri okuduktan sonra kendi model bağlama eylemi gerçekleştirir.

İlk eylemin formu yükler ve bir tanımlama bilgisinde antiforgery bir belirteci kaydeder (aracılığıyla `GenerateAntiforgeryTokenCookieForAjax` öznitelik):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

ASP.NET Core'nın yerleşik özniteliğini kullanır [Antiforgery](xref:security/anti-request-forgery) bir tanımlama bilgisi ile istek belirtecini ayarlamak için destek:

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angular otomatik olarak adlandırılmış bir istek üst bilgisinde antiforgery bir belirteç geçirir `X-XSRF-TOKEN`. ASP.NET Core MVC uygulaması yapılandırmasında bu başlığında başvurmak için yapılandırılmış *Startup.cs*:

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

`DisableFormValueModelBinding` Model bağlama için devre dışı bırakmak için kullanılan öznitelik, aşağıda gösterilen `Upload` eylem yöntemi.

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Model bağlama devre dışı bırakıldığından `Upload` eylem yönteminin parametreleri kabul etmez. Doğrudan çalıştığını `Request` özelliği `ControllerBase`. A `MultipartReader` her bölümde okumak için kullanılır. Dosya bir GUID dosya adı ile kaydedilir ve anahtar/değer veri depolanan bir `KeyValueAccumulator`. Tüm bölümleri okuduktan sonra içeriği `KeyValueAccumulator` form verilerinin bir model türünü bağlamak için kullanılır.

Tam `Upload` yöntemi aşağıda gösterilmektedir:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Sorun giderme

Karşıya yükleme, dosyaları ve olası çözümleri ile çalışırken bazı sık karşılaşılan sorunlar aşağıdadır.

### <a name="unexpected-not-found-error-with-iis"></a>IIS ile beklenmeyen bulunamadı hatası

Dosya karşıya yükleme işleminiz aşıyor sunucu şu hatayı gösterir yapılandırılmış `maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

Varsayılan ayar `30000000`, yaklaşık 28.6 MB olan. Değer düzenleyerek özelleştirilebilir *web.config*:

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

Bu ayar, yalnızca IIS için geçerlidir. Varsayılan davranışı üzerinde Kestrel barındırırken gerçekleşmez. Daha fazla bilgi için [istek sınırları \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>IFormFile ile null başvurusu özel durumu

Denetleyicinizi kabul etmiş olması durumunda kullanarak dosyaları karşıya `IFormFile` ancak değeri her zaman null olduğunu, HTML formunuza belirtilmesidir onaylayın bir `enctype` değerini `multipart/form-data`. Bu öznitelik üzerinde ayarlanmadıysa `<form>` olmaz öğesi, dosyayı karşıya yükleme oluşur ve herhangi ilişkili `IFormFile` bağımsız değişkenleri null olacaktır.
