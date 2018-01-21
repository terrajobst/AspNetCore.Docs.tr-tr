---
title: "ASP.NET Core dosya yüklemeleri"
author: ardalis
description: "Model bağlama ve akış ASP.NET Core MVC dosyaları karşıya yüklemek için nasıl kullanılacağı."
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: 3c5abe84a5c7cc399e0586e680a414fab7a26c1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="file-uploads-in-aspnet-core"></a>ASP.NET Core dosya yüklemeleri

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET MVC Eylemler, daha küçük dosyalar için bağlama veya daha büyük dosyalar için akış basit modeli kullanarak bir veya daha fazla dosya karşıya yüklemeyi destekler.

[Görüntülemek veya örnek Github'dan indirin](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Model bağlama ile küçük dosyaları karşıya yükleme

Küçük dosyaları yüklemek için çok parçalı bir HTML formu kullanın veya JavaScript kullanarak bir POST isteği oluşturun. Birden çok karşıya yüklenen dosyaların destekler, Razor kullanarak bir örnek form aşağıda gösterilmiştir:

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

Dosya yüklemeleri desteklemek için HTML formları belirtmeniz gerekir. bir `enctype` , `multipart/form-data`. `files` Yukarıda gösterilen giriş öğesi tarafından desteklenen birden çok dosya karşıya yükleme. Atlayın `multiple` özniteliği giriş bu öğede yalnızca tek bir karşıya yüklenecek dosyayı izin vermek için. Bir tarayıcıda yukarıdaki biçimlendirme oluşturur:

![Dosyayı karşıya yükle formunu](file-uploads/_static/upload-form.png)

Sunucuya yüklenen tek tek dosyaların üzerinden erişilen [Model bağlama](xref:mvc/models/model-binding) kullanarak [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) arabirimi. `IFormFile`aşağıdaki yapıya sahiptir:

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
> Kullanır veya güven ilişkisi olmayan `FileName` özelliği doğrulama olmadan. `FileName` Özelliği görüntüleme amacıyla yalnızca kullanılmalıdır.

Model bağlama kullanarak dosyaları karşıya yüklenirken ve `IFormFile` arabirimi, ya da tek bir eylem yönteminin kabul edebilir `IFormFile` veya bir `IEnumerable<IFormFile>` (veya `List<IFormFile>`) pek çok dosya temsil eden. Aşağıdaki örnekte bir veya daha fazla karşıya yüklenen dosyalar ile döngüler, yerel dosya sistemine kaydeder ve toplam sayısı ve karşıya yüklenen dosyaların boyutu döndürür.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Kullanarak karşıya yüklenen dosyaların `IFormFile` tekniği arabelleğe bellek veya disk web sunucusu üzerinde işlenmeden önce. Eylem yönteminin içine `IFormFile` içeriklerini bir akış olarak erişilebilir. Yerel dosya sistemi ek olarak, dosyalar için gönderilebilen [Azure Blob Depolama](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) veya [Entity Framework](https://docs.microsoft.com/ef/core/index).

Entity Framework kullanarak bir veritabanında ikili dosya verileri depolamak için türünde bir özellik tanımlamak `byte[]` varlık üzerinde:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Türünde bir viewmodel özelliği belirtin `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile`doğrudan eylem yöntemi parametresini veya viewmodel özelliği olarak yukarıda gösterildiği gibi kullanılabilir.

Kopya `IFormFile` akış ve bayt dizisine kaydedin:

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
> Performansı olumsuz etkileyebilir gibi ilişkisel veritabanları, ikili veri depolarken dikkatli olun.

## <a name="uploading-large-files-with-streaming"></a>Akış ile büyük dosyaları karşıya yükleme

Boyutunu veya dosya yüklemeleriyle sıklığını kaynak uygulama için soruna yol açıyorsa, yukarıda gösterilen model bağlama yaklaşım yaptığı gibi tamamının, arabelleğe alma yerine dosya karşıya yükleme akış göz önünde bulundurun. Kullanırken `IFormFile` ve model bağlama çok daha basit bir çözüm, akış çeşitli düzgün bir şekilde uygulamak için adımları gerektirir.

> [!NOTE]
> 64 KB aşan herhangi tek bir arabelleğe alınan dosya RAM disk üzerinde geçici bir dosya sunucusunda taşınır. Dosya yüklemeleri tarafından kullanılan kaynakları (disk, RAM) numarasını ve eşzamanlı dosya yüklemeleri boyutuna bağlıdır. Akış çok perf ilgili değil, Ölçek hakkında. Çok fazla karşıya arabellek çalışırsanız, bellek veya disk alanı dışında çalıştığında, sitenizi kilitleniyor.

Aşağıdaki örnek, bir denetleyici eylemi akış için JavaScript/Angular kullanmayı gösterir. Dosyanın antiforgery belirteci özel bir filtre özniteliğini kullanarak ve istek gövdesindeki HTTP üstbilgileri yerine geçilen oluşturulur. Eylem yönteminin karşıya yüklenen veriler doğrudan işlediğinden, model bağlama başka bir filtre tarafından devre dışı. Kullanarak formun içeriklerini okuma eylem içinde bir `MultipartReader`, her kişi okur `MultipartSection`, dosya işleme veya içeriği uygun şekilde saklanması. Tüm bölümleri okuyun sonra kendi model bağlama eylemi gerçekleştirir.

İlk eylemin formun yükler ve bir antiforgery belirteci bir tanımlama bilgisinde kaydeder (aracılığıyla `GenerateAntiforgeryTokenCookieForAjax` özniteliği):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

ASP.NET Core'nın yerleşik özniteliğini kullanır [Antiforgery](xref:security/anti-request-forgery) bir tanımlama bilgisi isteği belirteci ile ayarlamak için destek:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Açısal otomatik olarak geçirir antiforgery belirteci adlı bir istek üstbilgisinde `X-XSRF-TOKEN`. ASP.NET Core MVC uygulama yapılandırmasıyla bu başlığında başvurmak için yapılandırılmış *haline*:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

`DisableFormValueModelBinding` Aşağıda gösterilen özniteliği, model bağlama için devre dışı bırakmak için kullanılan `Upload` eylem yöntemi.

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Model bağlama devre dışı olduğundan `Upload` eylem yönteminin parametreleri kabul etmez. Doğrudan birlikte çalıştığı `Request` özelliği `ControllerBase`. A `MultipartReader` her bölüm okumak için kullanılır. Dosya bir GUID dosya adı ile kaydedilir ve anahtar/değer veriler bir `KeyValueAccumulator`. Tüm bölümleri okuyun sonra içeriğini `KeyValueAccumulator` form verilerinin bir model türünü bağlamak için kullanılır.

Tam `Upload` yöntemi aşağıda gösterilmektedir:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Sorun giderme

Dosyaları ve olası çözümleri karşıya ile çalışırken, bazı sık karşılaşılan sorunlar altındadır.

### <a name="unexpected-not-found-error-with-iis"></a>IIS ile beklenmeyen bulunamadı hatası

Dosya karşıya yükleme aşıyor sunucu şu hatayı gösterir yapılandırılmış `maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

Varsayılan ayar `30000000`, yaklaşık 28.6 MB olması. Değer düzenleyerek özelleştirilebilir *web.config*:

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

Bu ayar, yalnızca IIS için geçerlidir. Davranışı varsayılan olarak Kestrel üzerinde barındırdığında oluşmaz. Daha fazla bilgi için bkz: [istek sınırları \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>IFormFile ile null başvuru özel durumu

Denetleyicinizi kabul etmiş olması durumunda kullanarak dosyaları karşıya `IFormFile` ancak değer her zaman null ise bulmak, HTML formu belirtilmesidir onaylayın bir `enctype` değerini `multipart/form-data`. Bu özniteliği ayarlanmamış ise `<form>` öğesi, dosya karşıya yükleme gerçekleşmez ve tüm ilişkili `IFormFile` bağımsız değişkenleri boş olacaktır.
