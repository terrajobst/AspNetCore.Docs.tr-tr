---
title: "ASP.NET Core bir Razor sayfasına dosyaları karşıya yükleme"
author: guardrex
description: "Bir Razor sayfasına dosyaları karşıya yükleme hakkında bilgi edinin."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 184a42b3b6a458a6fbe23c4d54e03abc228b6efa
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>ASP.NET Core bir Razor sayfasına dosyaları karşıya yükleme

Tarafından [Luke Latham](https://github.com/guardrex)

Bu bölümde, Razor sayfasını içeren dosyaları karşıya yükleme gösterilmiştir.

[Razor sayfalarının film örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) dosyaları karşıya yükleme bağlama Bu öğretici kullanan basit modelde çalıştığı iyi küçük dosyaları yüklemek için. Büyük dosyaları akış hakkında daha fazla bilgi için bkz: [akış ile büyük dosyalar](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

Aşağıdaki adımlarda, bir filmi zamanlama dosya karşıya yükleme özelliği örnek uygulaması'na eklenir. Bir filmi zamanlama tarafından temsil edilen bir `Schedule` sınıfı. Sınıfı, zamanlama iki sürümünü içerir. Bir sürüm müşterilere sağlanan `PublicSchedule`. Başka bir sürüm şirket çalışanlarının kullanılan `PrivateSchedule`. Her bir sürümü ayrı bir dosya olarak yüklenir. Öğretici iki dosya yüklemeleriyle tek bir POST ile bir sayfadan sunucuya nasıl gerçekleştirileceğini gösterir.

## <a name="security-considerations"></a>Güvenlik konuları

Kullanıcılar bir sunucuya dosyaları karşıya yükleme olanağı sağlarken dikkat alınması gerekir. Saldırganlar yürütme [hizmet reddi](/windows-hardware/drivers/ifs/denial-of-service) ve bir sistem diğer saldırılar. Başarılı bir saldırı olasılığını azaltmak bazı güvenlik adımlar şunlardır:

* Dosyaları karşıya yükleme adanmış dosya karşıya yükleme alanına sisteminde, karşıya yüklenen içerik üzerinde güvenlik önlemleri zorunlu tuttukları kolaylaştırır. Dosya yüklemeleri sorgulamasına olduğunda, yürütme izinleri emin olun karşıya yükleme konumuna devre dışı bırakılır.
* Uygulamadan değil kullanıcı girişi tarafından belirlenen bir güvenli dosya adı veya karşıya yüklenen dosyanın dosya adı kullanın.
* Yalnızca onaylanan dosya uzantılarını belirli bir dizi izin verir.
* İstemci-tarafı denetimleri sunucu üzerinde gerçekleştirilen doğrulayın. İstemci-tarafı denetimleri aşmak kolaydır.
* Karşıya yükleme boyutunu denetlemek ve beklenenden daha büyük yüklemeler engelleyebilirsiniz.
* Virüs/kötü amaçlı yazılım tarayıcı karşıya yüklenen içerikte çalıştırın.

> [!WARNING]
> Kötü amaçlı kod bir sisteme karşıya sık yapabilirsiniz kod yürütmek için ilk adımdır:
> * Tamamen devralma sistemin.
> * Sistem tamamen başarısız sonucu sistemiyle aşırı yükleme.
> * Kullanıcı veya sistem veri tehlikeye.
> * Graffiti ortak bir arabirim için geçerlidir.

## <a name="add-a-fileupload-class"></a>Dosya yükleme sınıfı ekleme

Dosya yüklemeleri çifti işlemek için bir Razor sayfası oluşturun. Ekleme bir `FileUpload` zamanlama verileri elde etmek için sayfaya bağlı sınıfı. Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adını **dosya yükleme** ve aşağıdaki özellikleri ekleyin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

Sınıfı, zamanlamanın başlık özelliğini ve her iki sürümü zamanlama için bir özelliğine sahiptir. Tüm üç özellikleri gereklidir ve başlığı 3-60 karakter uzunluğunda olmalıdır.

## <a name="add-a-helper-method-to-upload-files"></a>Dosyaları karşıya yüklemek için bir yardımcı yöntemi ekleyin

Karşıya yüklenen zamanlama dosyalarını işlemek için kod yinelemesinden kaçınmak için önce bir statik yardımcı yöntemi ekleyin. Oluşturma bir *yardımcı programları* uygulama klasöründe ve ekleme bir *FileHelpers.cs* dosya aşağıdaki içeriğe sahip. Yardımcı yöntemi `ProcessFormFile`, alan bir [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) ve dosyanın boyutu ve içeriğini içeren bir dize döndürür. İçerik türü ve uzunluğu denetlenir. Dosya bir doğrulama denetimi geçmiyor, bir hata eklenen `ModelState`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a>Dosyayı diske kaydedin

Örnek uygulaması dosyanın içeriğini veritabanı alanına kaydeder. Dosyanın içeriğini diske kaydetmek için kullanın bir [FILESTREAM](/dotnet/api/system.io.filestream):

```csharp
using (var fileStream = new FileStream(filePath, FileMode.Create))
{
    await formFile.CopyToAsync(fileStream);
}
```

Çalışan işlemi tarafından belirtilen konuma yazma izinlerine sahip olmalıdır `filePath`.

### <a name="save-the-file-to-azure-blob-storage"></a>Dosyayı Azure Blob depolama alanına kaydedin

Azure Blob Depolama birimine dosya içerik yüklemek için bkz: [.NET kullanarak Azure Blob Storage ile çalışmaya başlama](/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Nasıl kullanılacağı konusunda ortaya [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) kaydetmek için bir [FILESTREAM](/dotnet/api/system.io.filestream) blob depolama.

## <a name="add-the-schedule-class"></a>Zamanlama sınıfı ekleme

Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adını **zamanlama** ve aşağıdaki özellikleri ekleyin:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

Sınıf kullandığı `Display` ve `DisplayFormat` kolay başlıkları ve zamanlama veri işlendiğinde biçimlendirme oluşturur özniteliklerini.

## <a name="update-the-moviecontext"></a>Güncelleştirme MovieContext

Belirtin bir `DbSet` içinde `MovieContext` (*Models/MovieContext.cs*) tabloları için:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>Zamanlama tablo veritabanına ekleyin

(PMC) Paket Yöneticisi konsolunu açın: **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

PMC aşağıdaki komutları yürütün. Bu komutlar ekleme bir `Schedule` veritabanı tablosuna:

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Bir dosyayı karşıya yükleme Razor sayfası ekleme

İçinde *sayfaları* klasörü oluşturmak bir *zamanlamaları* klasör. İçinde *zamanlamaları* klasörünü adlı bir sayfa oluşturma *Index.cshtml* aşağıdaki içeriğe sahip bir zamanlama karşıya yükleme için:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

Her form grubu içeren bir  **\<etiketi >** her sınıf özelliğinin adını görüntüler. `Display` Öznitelikleri `FileUpload` modeli etiketlerini görüntüleme değerleri belirtin. Örneğin, `UploadPublicSchedule` özelliğin görünen adı ayarlandığında `[Display(Name="Public Schedule")]` ve formu işleyen böylece "Ortak zamanlama" etiketi görüntülenir.

Her form grubu bir doğrulama içeren  **\<span >**. Kullanıcı karşılamak üzere başarısız giriş olması durumunda özellik öznitelikleri kümesinde `FileUpload` sınıfı veya varsa `ProcessFormFile` yöntemi dosya doğrulama başarısız denetler, model doğrulamak başarısız olur. Model doğrulama başarısız olduğunda, bir yardımcı doğrulama ileti kullanıcıya işlenir. Örneğin, `Title` özellik açıklama ile `[Required]` ve `[StringLength(60, MinimumLength = 3)]`. Kullanıcı bir başlık sağlamanız başarısız olursa, bir değer gerekli olduğunu belirten bir ileti alırlar. Kullanıcı değerinden üç veya daha fazla altmış karakter değeri girerse, bunlar değeri yanlış bir uzunluğa sahip olduğunu belirten bir ileti alırsınız. İçeriği yok sağlanan bir dosya varsa, dosyayı boş olduğunu belirten bir ileti görüntülenir.

## <a name="add-the-page-model"></a>Sayfa modeli ekleme

Sayfa modeli ekleme (*Index.cshtml.cs*) için *zamanlamaları* klasörü:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

Sayfa modeli (`IndexModel` içinde *Index.cshtml.cs*) bağlar `FileUpload` sınıfı:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

Model zamanlamalar listesini de kullanır (`IList<Schedule>`) sayfasında veritabanında depolanan zamanlamaları görüntülemek için:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

Sayfa yüklediğinde ile `OnGetAsync`, `Schedules` veritabanından doldurulur ve yüklenen zamanlamalar HTML tablosu oluşturmak için kullanılan:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

Sunucuya form gönderildiğinde `ModelState` denetlenir. Geçersiz, `Schedule` yeniden oluşturulur ve sayfa doğrulamayı neden geçemediğini belirten bir veya daha fazla doğrulama iletileri ile sayfasını işler. Geçerliyse, `FileUpload` özellikleri kullanıldığı *OnPostAsync* zamanlama iki sürümleri için dosya karşıya yükleme işlemini tamamlamak için ve yeni bir oluşturmak için `Schedule` verileri depolamak için nesne. Zamanlama sonra veritabanına kaydedilir:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>Dosya karşıya yükleme Razor sayfasını bağlantı

Açık *_Layout.cshtml* ve bir bağlantı dosya karşıya yükleme sayfasına ulaşmak için gezinti çubuğu ekleyin:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Zamanlama silmeyi onaylamak için bir sayfa ekleyin

Kullanıcı bir zamanlama silinecek tıklattığında işlemi iptal etmek için bir fırsat sağlanır. Silme onayı sayfası ekleme (*Delete.cshtml*) için *zamanlamaları* klasörü:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

Sayfa modeli (*Delete.cshtml.cs*) tarafından tanımlanan tek bir zamanlama yükler `id` isteğin rota verilerindeki. Ekleme *Delete.cshtml.cs* dosya *zamanlamaları* klasörü:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

`OnPostAsync` Yöntemi işler tarafından zamanlama silinirken kendi `id`:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

Zamanlama başarıyla sildikten sonra `RedirectToPage` zamanlamaları için kullanıcı gönderir *Index.cshtml* sayfası.

## <a name="the-working-schedules-razor-page"></a>Zamanlamalar Razor sayfasını çalışma

Etiketleri ve girişleri zamanlama başlık, sayfa yüklendiğinde ortak zamanlama ve özel zamanlama bir gönderme düğmesi ile işlenir:

![Hiçbir doğrulama hataları ve boş alanları ile ilk yükü görülen Razor sayfasını zamanlar](uploading-files/_static/browser1.png)

Seçme **karşıya** tüm alanları doldurmak ihlal olmadan düğmesini `[Required]` model üzerinde öznitelikleri. `ModelState` Geçersiz. Kullanıcı için bir doğrulama hata iletisi görüntülenir:

![Her giriş denetiminin yanındaki bir doğrulama hata iletisi görüntülenir](uploading-files/_static/browser2.png)

İki harf içine yazın **başlık** alan. Başlık 3-60 karakter arasında olması gerektiğini belirtmek için doğrulama ileti değişiklikler:

![Başlık doğrulama ileti değiştirildi](uploading-files/_static/browser3.png)

Bir veya daha fazla Zamanlama yüklenirken **yüklenen zamanlamaları** bölüm yüklenen zamanlamaları işler:

![Yüklenen zamanlamaları, her zamanlamanın başlık gösteren tablosunun tarihi UTC, genel bir sürümü dosya boyutu ve özel sürüm dosya boyutu karşıya](uploading-files/_static/browser4.png)

Kullanıcı tıklatabilirsiniz **silmek** buradan onaylayın ya da silme işlemini iptal etmek için bir fırsat sahip oldukları silme onayı görünüm ulaşmak için bağlantı.

## <a name="troubleshooting"></a>Sorun giderme

Sorun giderme bilgileri ile `IFormFile` yüklemek bkz [dosya yüklemeleri ASP.NET Core: sorun giderme](xref:mvc/models/file-uploads#troubleshooting).

Bu giriş Razor sayfalarının tamamlamak için teşekkür ederiz. Geri bildirim veriyoruz. [MVC ve EF çekirdek kullanmaya başlama](xref:data/ef-mvc/intro) mükemmel bir izleme Bu öğretici kadar olan.

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core dosya yüklemeleri](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[Önceki: doğrulama](xref:tutorials/razor-pages/validation)
