---
title: ASP.NET Core uygulaması oluşturulan sayfaları güncelleştirme
author: rick-anderson
description: ASP.NET Core uygulaması oluşturulan sayfaları güncelleştirme hakkında bilgi edinin.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 62385f33dc86609726305728fbc19dd9ff27dc87
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410200"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>ASP.NET Core uygulaması oluşturulan sayfaları güncelleştirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

İskele kurulmuş film uygulaması için iyi bir başlangıç olsa da, sunu ideal değildir. **ReleaseDate** olmalıdır **yayın tarihi** (iki kelimeye).

![Chrome'da açık film uygulaması](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Oluşturulan kodu güncelleştirme

Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterilen vurgulanan satırları ekleyin:

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=12,17)]

`[Column(TypeName = "decimal(18, 2)")]` Veri ek açıklama sağlayan doğru eşlemek Entity Framework Core `Price` veritabanında para birimi. Daha fazla bilgi için [veri türleri](/ef/core/modeling/relational/data-types).

[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide ele alınmıştır. [Görüntüleme](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) öznitelik adı (Bu durumda "ReleaseDate" yerine "yayın tarihi") bir alan için görüntülenecek öğeleri belirtir. [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) öznitelik alanında depolanan saat bilgilerini görüntülenmediğini şekilde (tarih), veri türünü belirtir.

Sayfa/film bulun ve üzerine bir **Düzenle** hedef URL'ye görmek için bağlantı.

![Düzenleme bağlantısını ve bir bağlantı üzerinde fare ile tarayıcı penceresinde URL'sini http://localhost:1234/Movies/Edit/5 gösterilir](~/tutorials/razor-pages/da1/edit7.png)

**Düzenle**, **ayrıntıları**, ve **Sil** tarafından oluşturulan bağlantıları [yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) içinde *sayfaları/filmler / Index.cshtml* dosya.

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir. Önceki kodda, `AnchorTagHelper` dinamik olarak HTML oluşturan `href` Razor (yol göreli) sayfasından, öznitelik değeri `asp-page`ve rota kimliği (`asp-route-id`). Bkz: [sayfaları için URL üretimi](xref:razor-pages/index#url-generation-for-pages) daha fazla bilgi için.

Kullanım **kaynağı görüntüle** oluşturulan biçimlendirme incelemek için sık kullanılan tarayıcınızdan. Oluşturulan HTML değerinin bir bölümü aşağıda gösterilmiştir:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Dinamik olarak oluşturulmuş bağlantıları film Kimliğine sahip bir sorgu dizesi geçirin (örneğin, `?id=1` içinde `https://localhost:5001/Movies/Details?id=1`).

Düzenle, Ayrıntılar ve Razor Sayfaları Sil "{kimliği: int}" rota şablonu kullanmak için güncelleştirin. Her biri bu sayfaları için sayfa yönergesi değiştirme `@page` için `@page "{id:int}"`. Uygulamayı çalıştırın ve ardından kaynağı görüntüleyin. Oluşturulan HTML kimliği URL'nin yol kısmı ekler:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

"{Kimliği: int}" rota şablonuyla yapan sayfasına bir istek **değil** dahil tamsayı HTTP 404 (bulunamadı) hatası döndürür. Örneğin, `http://localhost:5000/Movies/Details` 404 hatası döndürür. Kimliği isteğe bağlı yapmak için URL'ye `?` rota kısıtlaması için:

 ```cshtml
@page "{id:int?}"
```

Davranışını test etmek için `@page "{id:int?}"`:

* Sayfa yönergesi kümesinde *Pages/Movies/Details.cshtml* için `@page "{id:int?}"`.
* Bir kesme noktası kümesinde `public async Task<IActionResult> OnGetAsync(int? id)` (içinde *Pages/Movies/Details.cshtml.cs*).
* `https://localhost:5001/Movies/Details/` sayfasına gidin.

İle `@page "{id:int}"` yönergesi, kesme noktası hiçbir zaman ulaşılır. Yönlendirme altyapısını, HTTP 404 döndürür. Kullanarak `@page "{id:int?}"`, `OnGetAsync` yöntemi döndürür `NotFound` (HTTP 404).

Önerilmese de yazabilirsiniz `OnGetAsync` yöntemi (içinde *Pages/Movies/Delete.cshtml.cs*) olarak:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Delete.cshtml.cs?name=snippet)]

Yukarıdaki kod test edin:

* Seçin bir **Sil** bağlantı.
* URL'deki kimliği kaldırın. Örneğin, değiştirme `https://localhost:5001/Movies/Delete/8` için `https://localhost:5001/Movies/Delete`.
* Hata ayıklayıcı kodda adım adım.

### <a name="review-concurrency-exception-handling"></a>Eşzamanlılık özel durum işleme gözden geçirin

Gözden geçirme `OnPostAsync` yönteminde *Pages/Movies/Edit.cshtml.cs* dosyası:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

Önceki kod, bir istemci film siler ve diğer istemci film değişiklikleri gönderir eşzamanlılık özel durumları algılar.

Test etmek için `catch` engelle:

* Bir kesme noktası ayarlayın `catch (DbUpdateConcurrencyException)`
* Seçin **Düzenle** bir filmi için değişiklik, ancak girmeyin **Kaydet**.
* Başka bir tarayıcı penceresinde seçin **Sil** bağlamak için aynı film ve film silin.
* Önceki tarayıcı penceresinde film değişiklikleri yayınlayın.

Eşzamanlılık çakışmalarını algılamak üzere üretim kodu isteyebilirsiniz. Bkz: [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) daha fazla bilgi için.

### <a name="posting-and-binding-review"></a>Gönderme ve bağlama gözden geçirin

İnceleme *Pages/Movies/Edit.cshtml.cs* dosyası:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

Bir HTTP GET isteği filmler/düzenleme sayfası için yapılan olduğunda (örneğin, `http://localhost:5000/Movies/Edit/2`):

* `OnGetAsync` Yöntemi film veritabanından veri getirir ve döndürür `Page` yöntemi. 
* `Page` Yöntemi işler *Pages/Movies/Edit.cshtml* Razor sayfası. *Pages/Movies/Edit.cshtml* dosyasını içeren model yönergesi (`@model RazorPagesMovie.Pages.Movies.EditModel`), hangi film modeli kullanıma sunar sayfasında.
* Düzenleme formu film değerlerle görüntülenir.

Filmler/düzenleme sayfası gönderildiğinde:

* Form değerlerini sayfasında bağlı olan `Movie` özelliği. `[BindProperty]` Özniteliği etkinleştirir [Model bağlama](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Model durumuna bir hata varsa (örneğin, `ReleaseDate` tarihe dönüştürülür), form gönderilen değerleri görüntülenir.
* Film modeli hata varsa, kaydedilir.

Dizin oluşturma ve silme Razor sayfaları HTTP GET yöntemleri benzer bir desen izleyin. HTTP POST `OnPostAsync` Razor sayfası oluşturma yöntemi için benzer bir desen izler `OnPostAsync` Razor sayfasını Düzenle yöntemi.

Sonraki öğreticide arama eklenir.

> [!div class="step-by-step"]
> [Önceki: Bir veritabanı ile çalışmaya](xref:tutorials/razor-pages/sql)
> [sonraki: Arama ekleme](xref:tutorials/razor-pages/search)
