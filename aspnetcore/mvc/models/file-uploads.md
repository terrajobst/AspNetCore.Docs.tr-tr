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
# <a name="upload-files-in-aspnet-core"></a>ASP.NET Core dosyaları karşıya yükleme

[Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)ve [Rutger fırtınası](https://github.com/rutix) tarafından

ASP.NET Core, daha küçük dosyalar için arabellekli model bağlama ve daha büyük dosyalar için arabelleğe alınmamış akış kullanarak bir veya daha fazla dosyanın yüklenmesini destekler

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>Güvenlik konuları

Kullanıcılara bir sunucuya dosya yükleme yeteneği sağlarken dikkatli olun. Saldırganlar şunları deneyebilir:

* [Hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) saldırıları yürütün.
* Virüsleri veya kötü amaçlı yazılımları karşıya yükleyin.
* Ağları ve sunucuları diğer yollarla tehlikeye atabilir.

Başarılı bir saldırının olasılığını azaltan güvenlik adımları şunlardır:

* Dosyaları, tercihen sistem dışı bir sürücüye sahip bir özel dosya yükleme alanına yükleyin. Adanmış bir konum kullanılması, karşıya yüklenen dosyalar üzerinde güvenlik kısıtlamaları yapmayı kolaylaştırır. Dosya karşıya yükleme konumunda yürütme izinlerini devre dışı bırak. &dagger;
* Karşıya yüklenen dosyaları uygulamayla aynı dizin ağacında hiçbir şekilde kalıcı hale getirme. &dagger;
* Uygulama tarafından belirlenen bir güvenli dosya adı kullanın. Kullanıcı tarafından belirtilen bir dosya adı veya karşıya yüklenen dosyanın güvenilmeyen dosya adı kullanmayın. &dagger; Güvenilmeyen bir dosya adını bir kullanıcı arabiriminde veya bir günlüğe kaydetme iletisinde göstermek için, HTML-değeri kodlayın.
* Yalnızca belirli bir onaylanan dosya uzantıları kümesine izin ver. &dagger;
* Kullanıcının bir açılan dosyayı karşıya yüklemesini engellemek için dosya biçimi imzasını denetleyin. &dagger; Örneğin, bir kullanıcının. *txt* uzantılı bir *. exe* dosyasını karşıya yüklemesine izin verme.
* Sunucu üzerinde istemci tarafı denetimlerinin de gerçekleştirildiğinden emin olun. &dagger; İstemci tarafı denetimleri sağlamasına kolaydır.
* Karşıya yüklenen bir dosyanın boyutunu denetleyin ve beklenenden daha büyük olan karşıya yüklemeleri önleyin. &dagger;
* Aynı ada sahip karşıya yüklenen bir dosya tarafından dosyaların üzerine yazılmaması gerektiğinde, dosyayı karşıya yüklemeden önce dosya adını veritabanına veya fiziksel depolamaya göre denetleyin.
* **Dosya depolanmadan önce karşıya yüklenen içerik üzerinde bir virüs/kötü amaçlı yazılım tarayıcısı çalıştırın.**

&dagger; örnek uygulama, ölçütlere uyan bir yaklaşımı gösterir.

> [!WARNING]
> Kötü amaçlı bir kodun bir sisteme karşıya yükleme için kod yürütme için ilk adımı sık şöyledir:
>
> * Sistemin denetimini tamamen elde edin.
> * Sistemin kilitlenme sonucuyla bir sistemi aşırı yükleme.
> * Kullanıcı veya sistem verilerini tehlikeye.
> * Genel Kullanıcı arabirimine Graffiti uygulayın.
>
> Kullanıcıların dosyaları kabul ederken saldırı yüzey alanı azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
>
> * [Sınırsız dosya karşıya yükleme](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Azure güvenliği: Kullanıcılardan dosyaları kabul etmek için uygun denetimlerin yerinde olduğundan emin olun @ no__t-0

Örnek uygulamadaki örnekler de dahil olmak üzere güvenlik önlemlerini uygulama hakkında daha fazla bilgi için [doğrulama](#validation) bölümüne bakın.

## <a name="storage-scenarios"></a>Depolama senaryoları

Dosyalar için ortak depolama seçenekleri şunlardır:

* Database

  * Küçük dosya yüklemeleri için, bir veritabanı genellikle fiziksel depolama (dosya sistemi veya ağ paylaşımından) seçeneklerinden daha hızlıdır.
  * Kullanıcı verileri için bir veritabanı kaydının alınması eşzamanlı olarak dosya içeriğini (örneğin, avatar görüntüsü) sağlayabildiğinden, veritabanı genellikle fiziksel depolama seçeneklerine göre daha uygundur.
  * Bir veritabanı, veri depolama hizmeti kullanmaktan daha ucuz olabilir.

* Fiziksel depolama (dosya sistemi veya ağ paylaşma)

  * Büyük dosya yüklemeleri için:
    * Veritabanı limitleri karşıya yükleme boyutunu kısıtlayabilir.
    * Fiziksel depolama genellikle bir veritabanındaki depolamadan daha az ekonomik olur.
  * Fiziksel depolama, veri depolama hizmeti kullanmaktan daha ucuz olabilir.
  * Uygulamanın işlemi, depolama konumu için okuma ve yazma izinlerine sahip olmalıdır. **Hiçbir zaman yürütme izni vermeyin.**

* Veri depolama hizmeti (örneğin, [Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/))

  * Hizmetler genellikle tek hata noktalarına tabi olan şirket içi çözümler üzerinde geliştirilmiş ölçeklenebilirlik ve esneklik sunar.
  * Hizmetler büyük depolama altyapısı senaryolarında düşük maliyetlidir.

  Daha fazla bilgi için bkz [. hızlı başlangıç: Nesne depolamada bir blob oluşturmak için .NET kullanın @ no__t-0. Konu <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*> ' ı gösterir, ancak <xref:System.IO.Stream> ile çalışırken <xref:System.IO.FileStream> ' yi blob depolamaya kaydetmek için <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> kullanılabilir.

## <a name="file-upload-scenarios"></a>Karşıya dosya yükleme senaryoları

Dosyaları karşıya yüklemek için iki genel yaklaşım arabelleğe alınır ve akışlardır.

**Ara**

Dosyanın tamamı, dosyayı işlemek veya kaydetmek için kullanılan dosyanın C# temsili olan <xref:Microsoft.AspNetCore.Http.IFormFile> ' a okunurdur.

Dosya karşıya yüklemeleri tarafından kullanılan kaynaklar (disk, bellek), eşzamanlı dosya karşıya yüklemelerinin sayısına ve boyutuna bağlıdır. Bir uygulama çok fazla karşıya yükleme arabelleğini denerse, bellek veya disk alanı tükenirse site çöker. Karşıya dosya yükleme boyutu veya sıklığı uygulama kaynaklarını tüketme ise, akış ' ı kullanın.

> [!NOTE]
> 64 KB geçen tek bir arabelleğe alınmış dosya, bellekten diskte geçici bir dosyaya taşınır.

Dosya arabelleğe alma, bu konunun aşağıdaki bölümlerinde ele alınmıştır:

* [Fiziksel depolama alanı](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [Veritabanı](#upload-small-files-with-buffered-model-binding-to-a-database)

**Akış**

Dosya çok parçalı bir istekten alınır ve doğrudan uygulama tarafından işlenir veya kaydedilir. Akış performansı önemli ölçüde iyileştirmez. Akış, dosya karşıya yüklenirken bellek veya disk alanı taleplerini azaltır.

Akış büyük dosyaları [akış ile büyük dosyaları karşıya yükle](#upload-large-files-with-streaming) bölümünde ele alınmıştır.

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a>Fiziksel depolamaya arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme

Küçük dosyaları karşıya yüklemek için, çok parçalı bir form kullanın veya JavaScript kullanarak bir POST isteği oluşturun.

Aşağıdaki örnek, tek bir dosyayı karşıya yüklemek için bir Razor Pages formunun kullanımını gösterir (örnek uygulamada*Pages/Bufferedsinglefileuploadfiziksel. cshtml* ):

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

Aşağıdaki örnek, önceki örneğe benzerdir, ancak şunları hariç:

* Form verilerini göndermek için JavaScript ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) kullanılır.
* Doğrulama yok.

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

[Fetch API 'sini desteklemeyen](https://caniuse.com/#feat=fetch)istemcilerde form gönderisini JavaScript 'te gerçekleştirmek için aşağıdaki yaklaşımlardan birini kullanın:

* Fetch Polyfill kullanın (örneğin, [Window. Fetch (GitHub/fetch)](https://github.com/github/fetch)).
* Kullanın `XMLHttpRequest`. Örneğin:

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

Dosya karşıya yüklemelerini desteklemek için HTML formları `multipart/form-data` ' in bir kodlama türü (`enctype`) belirtmelidir.

Birden çok dosyayı karşıya yüklemeyi destekleyecek @no__t 0 giriş öğesi için `<input>` öğesinde `multiple` özniteliği sağlayın:

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

Sunucuya yüklenen tek dosyalara <xref:Microsoft.AspNetCore.Http.IFormFile> kullanılarak [model bağlama](xref:mvc/models/model-binding) aracılığıyla erişilebilir. Örnek uygulama, veritabanı ve fiziksel depolama senaryoları için birden çok arabellekli dosya yüklemeyi gösterir.

> [!WARNING]
> Doğrulaması olmadan <xref:Microsoft.AspNetCore.Http.IFormFile> ' in `FileName` özelliğine güvenmeyin veya güvenmeyin. @No__t-0 özelliği yalnızca görüntüleme amacıyla ve değeri HTML kodladıktan sonra kullanılmalıdır.
>
> Bu nedenle, şu ana kadar dikkate alınması gereken örnekler aşağıda verilmiştir. Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:
>
> * [Güvenlik konuları](#security-considerations)
> * [Doğrulama](#validation)

Model bağlama ve <xref:Microsoft.AspNetCore.Http.IFormFile> kullanarak dosyaları karşıya yüklerken eylem yöntemi şunları kabul edebilir:

* Tek bir <xref:Microsoft.AspNetCore.Http.IFormFile>.
* Birkaç dosyayı temsil eden aşağıdaki koleksiyonlardan herhangi biri:
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * [Liste](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3

> [!NOTE]
> Bağlama, form dosyaları adına göre eşleşir. Örneğin, `<input type="file" name="formFile">` ' deki HTML `name` değeri C# parametre/özellik ile (`FormFile`) eşleşmelidir. Daha fazla bilgi için, [ad öznitelik DEĞERINI Post yönteminin parametre adına eşleştirin](#match-name-attribute-value-to-parameter-name-of-post-method) bölümüne bakın.

Aşağıdaki örnek:

* Karşıya yüklenen bir veya daha fazla dosya üzerinden döngü.
* Dosya adı da dahil olmak üzere bir dosyanın tam yolunu döndürmek için [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanır. 
* Dosyalar, uygulama tarafından oluşturulan bir dosya adı kullanılarak yerel dosya sistemine kaydedilir.
* Karşıya yüklenen dosyaların toplam sayısını ve boyutunu döndürür.

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

Yol olmadan bir dosya adı oluşturmak için `Path.GetRandomFileName` kullanın. Aşağıdaki örnekte, yol yapılandırmadan alınır:

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

@No__t-0 ' a geçirilen yolun dosya adı içermesi *gerekir* . Dosya adı sağlanmazsa, çalışma zamanında bir <xref:System.UnauthorizedAccessException> oluşturulur.

@No__t-0 tekniği kullanılarak yüklenen dosyalar, işlemeden önce sunucuda veya diskte bellek halinde arabelleğe alınır. Eylem yönteminin içinde, <xref:Microsoft.AspNetCore.Http.IFormFile> içerikleri <xref:System.IO.Stream> olarak erişilebilir. Yerel dosya sistemine ek olarak, dosyalar bir ağ paylaşımında veya [Azure Blob depolama](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs)gibi bir dosya depolama hizmetine kaydedilebilir.

Karşıya yüklemek için birden çok dosya üzerinde döngü yapan ve güvenli dosya adları kullanan başka bir örnek için, örnek uygulamadaki *Pages/Bufferedmultiplefileuploadfiziksel. cshtml. cs* dosyasına bakın.

> [!WARNING]
> [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , önceki geçici dosyaları silmeden 65.535 'den fazla dosya oluşturulduysa <xref:System.IO.IOException> oluşturur. 65.535 dosya sınırının sunucu başına sınırı vardır. Windows işletim sistemi için bu sınır hakkında daha fazla bilgi için aşağıdaki konulara bakın:
>
> * [GetTempFileNameA işlevi](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a>Bir veritabanına arabellekli model bağlamaya sahip küçük dosyaları karşıya yükleme

İkili dosya verilerini [Entity Framework](/ef/core/index)kullanarak bir veritabanında depolamak için, varlıkta bir <xref:System.Byte> dizi özelliği tanımlayın:

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

@No__t içeren sınıf için bir sayfa modeli özelliği belirtin-0:

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
> <xref:Microsoft.AspNetCore.Http.IFormFile>, doğrudan bir eylem yöntemi parametresi veya bir bağlantılı model özelliği olarak kullanılabilir. Önceki örnekte, bir bağlantılı model özelliği kullanılmaktadır.

@No__t-0 Razor Pages formunda kullanılır:

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

Form sunucuya gönderildiğinde, <xref:Microsoft.AspNetCore.Http.IFormFile> ' ı bir akışa kopyalayın ve veritabanına bir bayt dizisi olarak kaydedin. Aşağıdaki örnekte, `_dbContext`, uygulamanın veritabanı bağlamını depolar:

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

Yukarıdaki örnek, örnek uygulamada gösterilen senaryoya benzerdir:

* *Pages/BufferedSingleFileUploadDb. cshtml*
* *Pages/BufferedSingleFileUploadDb. cshtml. cs*

> [!WARNING]
> İkili verileri ilişkisel veritabanlarında depolarken dikkatli olun, çünkü performansı olumsuz etkileyebilir.
>
> Doğrulaması olmadan <xref:Microsoft.AspNetCore.Http.IFormFile> ' in `FileName` özelliğine güvenmeyin veya güvenmeyin. @No__t-0 özelliği yalnızca görüntüleme amacıyla ve yalnızca HTML kodlaması sonrasında kullanılmalıdır.
>
> Belirtilen örneklerde dikkate alınması gereken önemli noktalar. Ek bilgiler aşağıdaki bölümler ve [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/)tarafından sağlanır:
>
> * [Güvenlik konuları](#security-considerations)
> * [Doğrulama](#validation)

### <a name="upload-large-files-with-streaming"></a>Akışa sahip büyük dosyaları karşıya yükleme

Aşağıdaki örnek, bir denetleyiciyi bir denetleyici eyleminde akışa almak için JavaScript 'in nasıl kullanılacağını gösterir. Dosyanın antiforgery belirteci özel bir filtre özniteliği kullanılarak oluşturulur ve istek gövdesi yerine istemci HTTP üst bilgilerine geçirilir. Eylem yöntemi karşıya yüklenen verileri doğrudan işlediğinden, form modeli bağlama başka bir özel filtre tarafından devre dışı bırakıldı. Eylem içinde formun içerikleri `MultipartReader` kullanarak okunur, her bir `MultipartSection` ' i okur, dosyayı işliyor veya içeriği uygun şekilde depoluyor. Çok parçalı bölümler okunduktan sonra eylem kendi model bağlamasını gerçekleştirir.

İlk sayfa yanıtı formu yükler ve bir tanımlama bilgisine bir antiforgery belirteci kaydeder (`GenerateAntiforgeryTokenCookieAttribute` özniteliği aracılığıyla). Öznitelik, bir istek belirtecine sahip bir tanımlama bilgisi ayarlamak için ASP.NET Core yerleşik [antiforgery desteğini](xref:security/anti-request-forgery) kullanır:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

@No__t-0, model bağlamayı devre dışı bırakmak için kullanılır:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

Örnek uygulamada `GenerateAntiforgeryTokenCookieAttribute` ve `DisableFormValueModelBindingAttribute`, [@no__t kuralları](xref:razor-pages/razor-pages-conventions)kullanılarak Razor Pages-4 ' te `/StreamedSingleFileUploadDb` ve `/StreamedSingleFileUploadPhysical` ' ün sayfa uygulama modellerine filtre olarak uygulanır:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

Model bağlama formu okumadığından formdan bağlanan parametreler bağlanamaz (sorgu, yol ve başlık çalışmaya devam eder). Action yöntemi, `Request` özelliği ile doğrudan işe yarar. @No__t-0, her bölümü okumak için kullanılır. Anahtar/değer verileri `KeyValueAccumulator` ' da depolanır. Çok parçalı bölümler okunduktan sonra, form verilerini bir model türüne bağlamak için `KeyValueAccumulator` içerikleri kullanılır.

EF Core bir veritabanına akış için tam `StreamingController.UploadDatabase` yöntemi:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

Fiziksel bir konuma akış için tam `StreamingController.UploadPhysical` yöntemi:

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

Örnek uygulamada, doğrulama denetimleri `FileHelpers.ProcessStreamedFile` tarafından işlenir.

## <a name="validation"></a>Doğrulama

Örnek uygulamanın `FileHelpers` sınıfı, arabelleğe alınmış <xref:Microsoft.AspNetCore.Http.IFormFile> ve akış dosya yüklemeleri için birkaç denetim gösterir. Örnek uygulamada <xref:Microsoft.AspNetCore.Http.IFormFile> arabelleğe alınmış dosya yüklemelerini işlemek için, *Utilities/Fileyardımcılar. cs* dosyasındaki `ProcessFormFile` yöntemine bakın. Akış dosyalarını işlemek için aynı dosyada `ProcessStreamedFile` yöntemine bakın.

> [!WARNING]
> Örnek uygulamada gösterilen doğrulama işleme yöntemleri karşıya yüklenen dosyaların içeriğini taramaz. Çoğu üretim senaryosunda, dosyanın kullanıcılara veya diğer sistemlere kullanılabilir hale getirilmesi için dosya üzerinde bir virüs/kötü amaçlı yazılım tarayıcı API 'SI kullanılır.
>
> Konu örneği, doğrulama tekniklerine yönelik çalışan bir örnek sağlasa da şunları gerçekleştirmediğiniz takdirde bir üretim uygulamasında `FileHelpers` sınıfını uygulamayın:
>
> * Uygulamayı tam olarak anlayın.
> * Uygulamayı uygulamanın ortamı ve belirtimleri için uygun şekilde değiştirin.
>
> **Bu gereksinimleri bilmeden bir uygulamada güvenlik kodunu hiçbir şekilde sayısının fark gözetmeden uygulayın.**

### <a name="content-validation"></a>İçerik doğrulama

**Karşıya yüklenen içerikte üçüncü taraf bir virüs/kötü amaçlı yazılım tarama API 'SI kullanın.**

Dosyaları tarama, yüksek hacimli senaryolarda sunucu kaynaklarında yoğun bir şekilde yapılır. Dosya tarama nedeniyle istek işleme performansı azaldığında, tarama işini, muhtemelen uygulamanın sunucusundan farklı bir sunucuda çalışan bir [arka plan hizmetine](xref:fundamentals/host/hosted-services)devredere göz önünde bulundurun. Genellikle, arka plan virüs tarayıcısı tarafından denetlene kadar karşıya yüklenen dosyalar karantinaya alınmış bir alanda tutulur. Bir dosya geçtiğinde dosya normal dosya depolama konumuna taşınır. Bu adımlar genellikle bir dosyanın tarama durumunu gösteren bir veritabanı kaydıyla birlikte gerçekleştirilir. Böyle bir yaklaşım kullanarak, uygulama ve uygulama sunucusu isteklere yanıt vermeye odaklanmaya devam eder.

### <a name="file-extension-validation"></a>Dosya Uzantısı doğrulaması

Karşıya yüklenen dosyanın uzantısı izin verilen uzantılar listesine göre denetlenmelidir. Örneğin:

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a>Dosya imzası doğrulaması

Bir dosyanın imzası, bir dosyanın başlangıcında ilk birkaç bayta göre belirlenir. Bu baytlar, uzantının dosyanın içeriğiyle eşleşip eşleşmediğini göstermek için kullanılabilir. Örnek uygulama, birkaç ortak dosya türü için dosya imzalarını denetler. Aşağıdaki örnekte, bir JPEG görüntüsünün dosya imzası, dosyaya karşı denetlenir:

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

Ek dosya imzaları almak için [Dosya Imzaları veritabanı](https://www.filesignatures.net/) ve resmi dosya belirtimleri bölümüne bakın.

### <a name="file-name-security"></a>Dosya adı güvenliği

Fiziksel depolamaya bir dosyayı kaydetmek için hiçbir şekilde istemci tarafından sağlanan dosya adı kullanmayın. Geçici depolama için tam yol (dosya adı da dahil olmak üzere) oluşturmak için [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) veya [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) kullanarak dosya için güvenli bir dosya adı oluşturun.

Razor otomatik olarak HTML, görüntüleme için özellik değerlerini kodlar. Aşağıdaki kodun kullanımı güvenlidir:

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

Razor dışında, her zaman bir kullanıcının isteğinden <xref:System.Net.WebUtility.HtmlEncode*> dosya adı içeriği.

Birçok uygulama, dosyanın var olduğunu bir denetim içermelidir; Aksi takdirde, dosyanın üzerine aynı ada sahip bir dosya yazılır. Uygulamanızın belirtimlerini karşılamak için ek mantık sağlayın.

### <a name="size-validation"></a>Boyut doğrulaması

Karşıya yüklenen dosyaların boyutunu sınırlayın.

Örnek uygulamada, dosyanın boyutu 2 MB ile sınırlıdır (bayt cinsinden gösterilir). Sınır, *appSettings. JSON* dosyasındaki [yapılandırma](xref:fundamentals/configuration/index) yoluyla sağlanır:

```json
{
  "FileSizeLimit": 2097152
}
```

@No__t-0 `PageModel` sınıflarına eklenmiş:

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

Dosya boyutu sınırı aştığında, dosya reddedilir:

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a>Name öznitelik değerini POST yönteminin Parameter adı ile Eşleştir

Form verileri oluşturan veya JavaScript 'in `FormData` ' ı kullanan Razor olmayan formlarda, formun öğesinde belirtilen adın veya `FormData` ' in, denetleyicinin eyleminde bulunan parametre adıyla aynı olması gerekir.

Aşağıdaki örnekte:

* @No__t-0 öğesi kullanılırken, `name` özniteliği `battlePlans` değerine ayarlanır:

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* JavaScript 'te `FormData` kullanırken, ad `battlePlans` değerine ayarlanır:

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

C# Yöntemin parametresi için eşleşen bir ad kullanın (`battlePlans`):

* @No__t adlı Razor Pages sayfa işleyicisi yöntemi için:

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* MVC POST denetleyicisi eylem yöntemi için:

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a>Sunucu ve uygulama yapılandırması

### <a name="multipart-body-length-limit"></a>Çok parçalı gövde uzunluğu sınırı

<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit>, her bir çok parçalı gövdenin uzunluk sınırını ayarlar. Bu sınırı aşan form bölümleri ayrıştırıldığında <xref:System.IO.InvalidDataException> oluşturur. Varsayılan değer 134.217.728 ' dir (128 MB). @No__t-1 ' deki <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarını kullanarak limiti özelleştirin:

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

<xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute>, tek bir sayfa veya eylem için <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ayarlamak için kullanılır.

Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices` ' deki bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:

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

Razor Pages bir uygulamada veya bir MVC uygulamasında, filtreyi sayfa modeline veya eylem yöntemine uygulayın:

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a>Kestrel maksimum istek gövdesi boyutu

Kestrel tarafından barındırılan uygulamalar için, varsayılan en büyük istek gövdesi boyutu 30.000.000 bayttır ve bu, yaklaşık 28,6 MB 'tır. [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel Server seçeneğini kullanarak sınırı özelleştirin:

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

<xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute>, tek sayfa veya eylem için [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) ayarlamak üzere kullanılır.

Razor Pages bir uygulamada, filtreyi `Startup.ConfigureServices` ' deki bir [kurala](xref:razor-pages/razor-pages-conventions) uygulayın:

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

Bir Razor sayfaları uygulamasında veya bir MVC uygulamasında, filtreyi sayfa işleyici sınıfına veya eylem yöntemine uygulayın:

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a>Diğer Kestrel limitleri

Kestrel tarafından barındırılan uygulamalar için diğer Kestrel limitleri de uygulanabilir:

* [İstemci bağlantıları üst sınırı](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [İstek ve yanıt veri ücretleri](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a>IIS içerik uzunluğu sınırı

Varsayılan istek sınırı (`maxAllowedContentLength`), yaklaşık 28.6 MB olan 30.000.000 bayttır. *Web. config* dosyasında sınırı özelleştirin:

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

Bu ayar yalnızca IIS için geçerlidir. Kestrel üzerinde barındırırken davranış varsayılan olarak gerçekleşmez. Daha fazla bilgi için bkz. [Istek limitleri \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

ASP.NET Core modülündeki sınırlamalar veya IIS Istek filtreleme modülünün varlığı, karşıya yüklemeleri 2 veya 4 GB ile sınırlandırabilir. Daha fazla bilgi için bkz. [2 GB 'tan büyük dosya karşıya yüklenemiyor (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).

## <a name="troubleshoot"></a>Sorun giderme

Dosyaları karşıya yükleme ve olası çözümleri ile çalışırken karşılaşılan bazı yaygın sorunlar aşağıda verilmiştir.

### <a name="not-found-error-when-deployed-to-an-iis-server"></a>Bir IIS sunucusuna dağıtılırken bulunamadı hatası

Aşağıdaki hata karşıya yüklenen dosyanın, sunucunun yapılandırılmış içerik uzunluğunu aştığını gösterir:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

Limiti artırma hakkında daha fazla bilgi için bkz. [IIS içerik uzunluğu sınırı](#iis-content-length-limit) bölümü.

### <a name="connection-failure"></a>Bağlantı hatası

Bir bağlantı hatası ve bir sıfırlama sunucu bağlantısı büyük olasılıkla karşıya yüklenen dosyanın Kestrel 'in en büyük istek gövdesi boyutunu aştığını gösterir. Daha fazla bilgi için, [Kestrel maksimum istek gövdesi boyutu](#kestrel-maximum-request-body-size) bölümüne bakın. Kestrel istemci bağlantı limitleri de ayarlama gerektirebilir.

### <a name="null-reference-exception-with-iformfile"></a>Iformfile ile null başvuru özel durumu

Denetleyici <xref:Microsoft.AspNetCore.Http.IFormFile> kullanarak karşıya yüklenen dosyaları kabul ediyorsanız, ancak değer `null` ise HTML formunun `multipart/form-data` @no__t 2 değerini belirtdiğini doğrulayın. Bu öznitelik `<form>` öğesinde ayarlanmamışsa, dosya karşıya yükleme gerçekleşmez ve herhangi bir bağlı <xref:Microsoft.AspNetCore.Http.IFormFile> bağımsız değişkeni `null` ' dir. Ayrıca, [form verilerinde karşıya yükleme adlandırmasının uygulamanın adlandırmayla eşleştiğinden](#match-name-attribute-value-to-parameter-name-of-post-method)emin olun.

## <a name="additional-resources"></a>Ek kaynaklar

* [Sınırsız dosya karşıya yükleme](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [Azure güvenliği: Güvenlik çerçevesi: Giriş doğrulaması | Azaltmaları @ no__t-0
* [Azure bulut tasarım desenleri: Valet anahtar deseninin @ no__t-0
