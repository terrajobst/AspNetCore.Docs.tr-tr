---
title: Bir ASP.NET Core Razor sayfa dosya yükleme
author: guardrex
description: ASP.NET core'da FileUpload sınıfını kullanarak bir Razor sayfası için dosyaları karşıya yüklemeyi öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 11/10/2018
ms.custom: seodec18
uid: razor-pages/upload-files
ms.openlocfilehash: 80929c6c1a95b46b942958def1540ac8ed5abc81
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121407"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a>Bir ASP.NET Core Razor sayfa dosya yükleme

Tarafından [Luke Latham](https://github.com/guardrex)

Bu konuda bağlı derlemeler [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) içinde <xref:tutorials/razor-pages/razor-pages-start>.

Bu konuda, küçük dosyalar da karşıya yükleme için çalışan basit bir model bağlama dosyaları, karşıya yüklemek için nasıl kullanılacağı gösterilmektedir. Büyük dosyaları akış hakkında daha fazla bilgi için bkz: [akış ile büyük dosyaları karşıya yükleme](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

Aşağıdaki adımlarda, örnek uygulamaya bir film zamanlaması dosyası karşıya yükleme özelliğini eklenir. Bir film zamanlama tarafından temsil edilen bir `Schedule` sınıfı. Sınıfı, zamanlama iki sürümünü içerir. Bir sürüm müşterilere sağlanan `PublicSchedule`. Başka bir sürüm şirket çalışanlarının kullanılan `PrivateSchedule`. Her sürümü ayrı bir dosya olarak yüklenir. Bu öğreticide, tek bir GÖNDERİ ile sayfasından sunucuya iki dosya yüklemelerini gerçekleştirmek gösterilmektedir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>Güvenlik konuları

Kullanıcılar bir sunucuya dosya yüklemek olanağı sağlarken dikkat alınması gerekir. Saldırganlar yürütme [hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) sistemindeki diğer saldırıları belirleyin. Başarılı bir saldırı olasılığını azaltmak bazı güvenlik adımlar şunlardır:

* Daha kolay hale getirir sistem üzerindeki bir adanmış dosya karşıya yükleme alanına karşıya yükleme dosyalarını karşıya yüklenen içerik güvenlik önlemlerinin büyük oranda yansıtmaktadır. Dosya yüklemeleri sorgulamasına, yürütme izinleri emin olun karşıya yükleme konumuna devre dışı bırakılır.
* Uygulamadan değil kullanıcı girişi tarafından belirlenen güvenli dosya adı veya karşıya yüklenen dosya dosya adını kullanın.
* Yalnızca belirli bir onaylı dosya uzantıları kümesini izin verir.
* Sunucu üzerinde istemci tarafı denetimleri yapılır doğrulayın. İstemci tarafı denetimleri sağlamasına kolaydır.
* Karşıya yükleme boyutu denetleyin ve beklenenden daha büyük karşıya engelleyebilirsiniz.
* Virüsten/kötü amaçlı yazılım tarayıcı karşıya yüklenen içerik üzerinde çalıştırın.

> [!WARNING]
> Kötü amaçlı bir kodun bir sisteme karşıya yükleme için kod yürütme için ilk adımı sık şöyledir:
> * Tamamen devralma sistemin.
> * Bir sistemi tamamen başarısız sonucu sistemiyle aşırı yükleme.
> * Kullanıcı veya sistem verilerini tehlikeye.
> * Graffiti ortak bir arabirim için geçerlidir.

## <a name="add-a-fileupload-class"></a>FileUpload sınıfı Ekle

Bir çift dosya yüklemeleri işlemek için bir Razor sayfası oluşturun. Ekleme bir `FileUpload` sayfasına zamanlama verileri almak için bağlanan sınıfı. Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adı **FileUpload** ve aşağıdaki özellikleri ekleyin:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

Sınıfı, zamanlamanın başlık için bir özelliği ve her iki sürümü zamanlama için bir özelliğine sahiptir. Tüm üç özellik gereklidir ve başlığı 3-60 karakter uzunluğunda olmalıdır.

## <a name="add-a-helper-method-to-upload-files"></a>Dosyaları karşıya yüklemek için bir yardımcı yöntemi ekleyin

Karşıya yüklenen zamanlama dosyalarını işlemek için kod yinelemesinden kaçınmak için bir statik yardımcı yöntemi ekleyin. Oluşturma bir *yardımcı programları* uygulama klasöründe ve ekleme bir *FileHelpers.cs* dosya aşağıdaki içeriğe sahip. Yardımcı yöntemi `ProcessFormFile`, alan bir [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) ve dosyanın boyutu ve içeriğini içeren bir dize döndürür. İçerik türünü ve uzunluğu denetlenir. Dosya doğrulama denetimi başarısız olursa hata eklenen `ModelState`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a>Dosyayı diske kaydedin.

Örnek uygulama, veritabanı alanlarına karşıya yüklenen dosyaları kaydeder. Bir dosyayı diske kaydetmek için kullanan bir [FILESTREAM](/dotnet/api/system.io.filestream). Aşağıdaki örnek tarafından tutulan bir dosyayı kopyalar `FileUpload.UploadPublicSchedule` için bir `FileStream` içinde bir `OnPostAsync` yöntemi. `FileStream` Dosyayı diske yazar `<PATH-AND-FILE-NAME>` sağlanan:

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

Çalışan işlemi tarafından belirtilen konuma yazma izinlerine sahip olmalıdır `filePath`.

> [!NOTE]
> `filePath` *Gerekir* dosya adını ekleyin. Dosya adı belirtilmezse, bir [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) çalışma zamanında oluşturulur.

> [!WARNING]
> Hiçbir zaman uygulama olarak aynı dizin ağacında karşıya yüklenen dosyalar kalıcı hale getirin.
>
> Kod örneği, kötü amaçlı dosya yüklemeleri karşı sunucu tarafı koruma sağlar. Kullanıcıların dosyaları kabul ederken saldırı yüzey alanı azaltma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:
>
> * [Sınırsız dosya karşıya yükleme](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Azure güvenlik: uygun denetimleri kullanıcıların dosyaları kabul ederken karşılandığından emin](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a>Dosyayı Azure Blob depolama alanına kaydedin

Dosya içeriği, Azure Blob depolama alanına yüklemek için bkz: [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Konu nasıl kullanılacağını gösteren [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) kaydetmek için bir [FILESTREAM](/dotnet/api/system.io.filestream) blob depolama.

## <a name="add-the-schedule-class"></a>Zamanlama sınıfı Ekle

Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adı **zamanlama** ve aşağıdaki özellikleri ekleyin:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

Sınıfın kullandığı `Display` ve `DisplayFormat` öznitelikleri, kolay başlıklar ve zamanlama verilerini işlendiğinde biçimlendirme oluşturur.

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a>Güncelleştirme RazorPagesMovieContext

Belirtin bir `DbSet` içinde `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) zamanlamalar:

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a>Güncelleştirme MovieContext

Belirtin bir `DbSet` içinde `MovieContext` (*Models/MovieContext.cs*) zamanlamalar:

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a>Zamanlama tablo veritabanına ekleme

Paket Yöneticisi Konsolu (PMC): **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

![PMC menüsü](upload-files/_static/pmc.png)

PMC'de aşağıdaki komutları yürütün. Bu komutlar ekleme bir `Schedule` veritabanı tablosu:

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Dosyayı karşıya yükleme Razor sayfası ekleme

İçinde *sayfaları* klasör oluşturma bir *zamanlamaları* klasör. İçinde *zamanlamaları* klasöründe adlı bir sayfa oluşturun *Index.cshtml* aşağıdaki içeriğe sahip bir zamanlama karşıya yükleme:

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

Her form grubu içeren bir  **\<etiket >** , her sınıf özelliği adı görüntüler. `Display` Öznitelikleri `FileUpload` modeli için etiketleri görüntüleme değerleri sağlar. Örneğin, `UploadPublicSchedule` özellik görünen adı ile ayarlanır `[Display(Name="Public Schedule")]` ve böylece form oluşturulduğunda etikette "Genel zamanlama" görüntüler.

Her form grubu içeren bir doğrulama  **\<span >**. Kullanıcının girişinin karşılamak için başarısız olursa özellik öznitelikleri kümesi'nde `FileUpload` sınıfı veya varsa `ProcessFormFile` yöntemi dosya doğrulama denetimleri başarısız, model doğrulama başarısız olur. Model doğrulama başarısız olduğunda, kullanıcıya yardımcı doğrulama iletisi oluşturulur. Örneğin, `Title` özelliği ile ek açıklamalı `[Required]` ve `[StringLength(60, MinimumLength = 3)]`. Bir başlık sağlamak kullanıcı başarısız olursa, bunlar bir değer gerekli olduğunu belirten bir ileti alırsınız. Kullanıcı Üçten az veya fazla altmış karakter değeri girerse, bunlar değeri yanlış bir uzunluk olduğunu belirten bir ileti alırsınız. Sağlanan içerik olan bir dosya ise, dosya boş olduğunu belirten bir ileti görüntülenir.

## <a name="add-the-page-model"></a>Sayfa modeli ekleme

Sayfa modeli ekleyin (*Index.cshtml.cs*) için *zamanlamaları* klasörü:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

Sayfa modeli (`IndexModel` içinde *Index.cshtml.cs*) bağlar `FileUpload` sınıfı:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

Model zamanlamalar listesini de kullanır (`IList<Schedule>`) sayfasında veritabanında depolanan zamanlamaları görüntülemek için:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

Sayfa yüklediğinde ile `OnGetAsync`, `Schedules` veritabanından doldurulur ve yüklenen zamanlaması ile HTML tablosu oluşturmak için kullanılır:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

Sunucuya form gönderildiğinde `ModelState` denetlenir. Geçersiz olursa `Schedule` yeniden oluşturulur ve sayfa doğrulama başarısız olmasının belirten bir veya daha fazla doğrulama iletilerinin ile sayfasını işler. Geçerliyse, `FileUpload` özellikleri kullanılır *OnPostAsync* dosya karşıya yükleme iki sürümün zamanlamasını tamamlanması ve yeni bir `Schedule` verileri depolamak için nesne. Zamanlama, ardından veritabanına kaydedilir:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a>Dosya karşıya yükleme Razor sayfası bağlantı

Açık *Pages/Shared/_Layout.cshtml* ve zamanlamaları sayfaya gitmek için gezinti çubuğunda bir bağlantı ekleyin:

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

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Zamanlama silme işlemini onaylamak için bir sayfa ekleyin

Bir zamanlama Silinecek kullanıcı tıkladığında işlemi iptal etmek için bir fırsat sağlanır. Bir silme onayı sayfası ekleyin (*Delete.cshtml*) için *zamanlamaları* klasörü:

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

Sayfa modeli (*Delete.cshtml.cs*) tarafından tanımlanmış tek bir zamanlamanın yükler `id` isteğin rota verilerindeki. Ekleme *Delete.cshtml.cs* dosyasını *zamanlamaları* klasörü:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

`OnPostAsync` Yöntemi işler zamanlamayı silme kendi `id`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

Zamanlama başarıyla sildikten sonra `RedirectToPage` kullanıcı tablolarına geri gönderir *Index.cshtml* sayfası.

## <a name="the-working-schedules-razor-page"></a>Çalışan zamanlamaları Razor sayfası

Etiketleri ve zamanlaması başlığı için girişler sayfa yüklendiğinde bir gönderme düğmesi ile genel zamanlama ve özel bir zamanlama oluşturulur:

![Razor sayfası ilk yükü hiçbir doğrulama hatalarını ve boş alanları görüldüğü şekilde zamanlar.](upload-files/_static/browser1.png)

Seçme **karşıya** alanlar doldurma ihlal olmadan düğmesini `[Required]` modelini öznitelikleri. `ModelState` Geçersiz. Doğrulama hatası iletilerini kullanıcıya görüntülenir:

![Her giriş denetim bitişiğinde doğrulama hata iletisi görüntülenir](upload-files/_static/browser2.png)

İçinde iki harf yazın **başlık** alan. Doğrulama iletisi başlığı 3-60 karakter arasında olması gerektiğini belirtmek için değişiklikler:

![Başlık doğrulama iletisi değiştirildi](upload-files/_static/browser3.png)

Bir veya daha fazla zamanlama karşıya yüklendiğinde **yüklenen zamanlamaları** bölümünde yüklenen zamanlamaları işler:

![Yüklenen zamanlaması, her tablosunun başlık gösteren tablo karşıya tarihi UTC, genel sürüm dosya boyutu ve özel sürüm dosya boyutu](upload-files/_static/browser4.png)

Kullanıcının tıklayabileceği **Sil** buradan onaylayın ya da silme işlemini iptal etmek için bir fırsat olduğu bunlar silme onayı görünümü erişmek için bağlantı.

## <a name="troubleshooting"></a>Sorun giderme

Sorun giderme bilgileri ile `IFormFile` yüklemek bkz [dosyasını karşıya yükler, ASP.NET Core: sorun giderme](xref:mvc/models/file-uploads#troubleshooting).
