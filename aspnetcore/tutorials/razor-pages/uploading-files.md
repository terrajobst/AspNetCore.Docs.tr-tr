---
title: "ASP.NET Core bir Razor sayfasına dosyaları karşıya yükleme"
author: guardrex
description: "Bir Razor sayfasına dosyaları karşıya yükleme hakkında bilgi edinin."
keywords: "ASP.NET Core, Razor, Razor sayfalarının, IFormFile, karşıya dosya yükleme, dosya yükleme"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 3b54bf0b40c396c8c141966219f65231fb362ca4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>ASP.NET Core bir Razor sayfasına dosyaları karşıya yükleme

Tarafından [Luke Latham](https://github.com/guardrex)

Bu bölümde, Razor sayfasını içeren dosyaları karşıya yükleme gösterilmiştir.

[Razor sayfalarının film örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) dosyaları karşıya yükleme bağlama Bu öğretici kullanan basit modelde çalıştığı iyi küçük dosyaları yüklemek için. Büyük dosyaları akış hakkında daha fazla bilgi için bkz: [akış ile büyük dosyalar](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

Aşağıdaki adımlarda örnek uygulama için bir filmi zamanlama dosya karşıya yükleme özelliği ekleyin. Bir filmi zamanlama tarafından temsil edilen bir `Schedule` sınıfı. Sınıfı, zamanlama iki sürümünü içerir. Bir sürüm müşterilere sağlanan `PublicSchedule`. Başka bir sürüm şirket çalışanlarının kullanılan `PrivateSchedule`. Her bir sürümü ayrı bir dosya olarak yüklenir. Öğretici iki dosya yüklemeleriyle tek bir POST ile bir sayfadan sunucuya nasıl gerçekleştirileceğini gösterir.

## <a name="add-a-fileupload-class"></a>Dosya yükleme sınıfı ekleme

Aşağıda, dosya yüklemeleriyle çifti işlemek için bir Razor sayfası oluşturun. Ekleme bir `FileUpload` zamanlama verileri elde etmek için sayfaya bağlı sınıfı. Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adını **dosya yükleme** ve aşağıdaki özellikleri ekleyin:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

Sınıfı, zamanlamanın başlık özelliğini ve her iki sürümü zamanlama için bir özelliğine sahiptir. Tüm üç özellikleri gereklidir ve başlığı 3-60 karakter uzunluğunda olmalıdır.

## <a name="add-a-helper-method-to-upload-files"></a>Dosyaları karşıya yüklemek için bir yardımcı yöntemi ekleyin

Karşıya yüklenen zamanlama dosyalarını işlemek için kod yinelemesinden kaçınmak için önce bir statik yardımcı yöntemi ekleyin. Oluşturma bir *yardımcı programları* uygulama klasöründe ve ekleme bir *FileHelpers.cs* dosya aşağıdaki içeriğe sahip. Yardımcı yöntemi `ProcessFormFile`, alan bir [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) ve dosyanın boyutu ve içeriğini içeren bir dize döndürür. İçerik türü ve uzunluğu denetlenir. Dosya bir doğrulama denetimi geçmiyor, bir hata eklenen `ModelState`.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a>Zamanlama sınıfı ekleme

Sağ tıklayın *modelleri* klasör. Seçin **ekleme** > **sınıfı**. Sınıf adını **zamanlama** ve aşağıdaki özellikleri ekleyin:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

Sınıf kullandığı `Display` ve `DisplayFormat` kolay başlıkları ve zamanlama veri işlendiğinde biçimlendirme oluşturur özniteliklerini.

## <a name="update-the-moviecontext"></a>Güncelleştirme MovieContext

Belirtin bir `DbSet` içinde `MovieContext` (*Models/MovieContext.cs*) tabloları için:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

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

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

Her form grubu içeren bir  **\<etiketi >** her sınıf özelliğinin adını görüntüler. `Display` Öznitelikleri `FileUpload` modeli etiketlerini görüntüleme değerleri belirtin. Örneğin, `UploadPublicSchedule` özelliğin görünen adı ayarlandığında `[Display(Name="Public Schedule")]` ve formu işleyen böylece "Ortak zamanlama" etiketi görüntülenir.

Her form grubu bir doğrulama içeren  **\<span >**. Kullanıcı karşılamak üzere başarısız giriş olması durumunda özellik öznitelikleri kümesinde `FileUpload` sınıfı veya varsa `ProcessFormFile` yöntemi dosya doğrulama başarısız denetler, model doğrulamak başarısız olur. Model doğrulama başarısız olduğunda, bir yardımcı doğrulama ileti kullanıcıya işlenir. Örneğin, `Title` özellik açıklama ile `[Required]` ve `[StringLength(60, MinimumLength = 3)]`. Kullanıcı bir başlık sağlamanız başarısız olursa, bir değer gerekli olduğunu belirten bir ileti alırlar. Kullanıcı değerinden üç veya daha fazla altmış karakter değeri girerse, bunlar değeri yanlış bir uzunluğa sahip olduğunu belirten bir ileti alırsınız. İçeriği yok sağlanan bir dosya varsa, dosyayı boş olduğunu belirten bir ileti görüntülenir.

## <a name="add-the-code-behind-file"></a>Arka plan kod dosyasına ekleyin

Arka plan kod dosyasına ekleyin (*Index.cshtml.cs*) için *zamanlamaları* klasörü:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

Sayfa modeli (`IndexModel` içinde *Index.cshtml.cs*) bağlar `FileUpload` sınıfı:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

Model zamanlamalar listesini de kullanır (`IList<Schedule>`) sayfasında veritabanında depolanan zamanlamaları görüntülemek için:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

Sayfa yüklediğinde ile `OnGetAsync`, `Schedules` veritabanından doldurulur ve yüklenen zamanlamalar HTML tablosu oluşturmak için kullanılan:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

Sunucuya form gönderildiğinde `ModelState` denetlenir. Geçersiz, `Schedule` yeniden oluşturulur ve sayfa doğrulamayı neden geçemediğini belirten bir veya daha fazla doğrulama iletileri ile sayfasını işler. Geçerliyse, `FileUpload` özellikleri kullanıldığı *OnPostAsync* zamanlama iki sürümleri için dosya karşıya yükleme işlemini tamamlamak için ve yeni bir oluşturmak için `Schedule` verileri depolamak için nesne. Zamanlama sonra veritabanına kaydedilir:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>Dosya karşıya yükleme Razor sayfasını bağlantı

Açık *_Layout.cshtml* ve bir bağlantı dosya karşıya yükleme sayfasına ulaşmak için gezinti çubuğu ekleyin:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Zamanlama silmeyi onaylamak için bir sayfa ekleyin

Kullanıcı bir zamanlama silinecek tıkladığında, bunları işlemi iptal etmek için bir fırsat olmasını istiyorsunuz. Silme onayı sayfası ekleme (*Delete.cshtml*) için *zamanlamaları* klasörü:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

Arka plan kod dosyasına (*Delete.cshtml.cs*) tarafından tanımlanan tek bir zamanlama yükler `id` isteğin rota verilerindeki. Ekleme *Delete.cshtml.cs* dosya *zamanlamaları* klasörü:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

`OnPostAsync` Yöntemi işler tarafından zamanlama silinirken kendi `id`:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

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

Bu giriş Razor sayfalarının tamamlamak için teşekkür ederiz. Çıkmadan açıklamaları veriyoruz. [MVC ve EF çekirdek Başlarken](xref:data/ef-mvc/intro) mükemmel bir izleme Bu öğretici kadar olan.

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET Core dosya yüklemeleri](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[Önceki: doğrulama](xref:tutorials/razor-pages/validation)
