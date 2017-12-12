---
title: "Oluşturulan sayfalarını güncelleştirme"
author: rick-anderson
description: "Oluşturulan sayfalar daha iyi ekranıyla güncelleştiriliyor."
keywords: "ASP.NET Core, Razor sayfaları"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/da1
ms.openlocfilehash: c66bb3a9d766e02c7775906cdd547a0e12c15336
ms.sourcegitcommit: b38796ea3806bf39b89806adfa681b2a33762907
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="updating-the-generated-pages"></a>Oluşturulan sayfalarını güncelleştirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil. Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki sözcük).

![Film uygulaması film verileri gösteren Chrome'da Aç](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Oluşturulan kod güncelleştir

Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterildiği vurgulanan satırları ekleyin:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Bir kırmızı dalgalı satıra sağ tıklayın > ** Hızlı Eylemler ve yapan yeniden düzenlemeler **.

  ![Bağlam menüsü programlarını ** > Hızlı Eylemler ve yapan yeniden düzenlemeler **.](da1/qa.png)

Seçin`using System.ComponentModel.DataAnnotations;`

  ![listesinin başında System.ComponentModel.DataAnnotations kullanma](da1/da.png)

  Visual studio ekler `using System.ComponentModel.DataAnnotations;`.

Şu konulara değineceğiz [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide. [Görüntülemek](https://docs.microsoft.com//aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği ne bir alanın adını (Bu durumda "ReleaseDate" yerine "yayın tarihi") için görüntülenecek belirtir. [DataType](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) öznitelik alanında depolanan saat bilgisi görüntülenmez şekilde (tarih), veri türünü belirtir.

Sayfa/filmlere göz atın ve üzerine gelerek bir **Düzenle** hedef URL görmek için bağlantı.

![Tarayıcı penceresini düzenleme bağlantısını ve bir bağlantı üzerinden fareyle http://localhost:1234/filmler/düzenleme/5 URL'sini gösterilir](da1/edit7.png)

**Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar tarafından üretilen [yer işareti etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) içinde *sayfaları/filmler / Index.cshtml* dosya.

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) sunucu tarafı kodu oluşturma ve Razor dosyalarında HTML öğelerin işlenmesi katılmayı etkinleştir. Önceki kod `AnchorTagHelper` dinamik olarak HTML oluşturan `href` öznitelik değeri Razor (rota göreli) sayfasından `asp-page`ve rota kimliği (`asp-route-id`). Bkz: [sayfaları için URL oluşturma](xref:mvc/razor-pages/index#url-generation-for-pages) daha fazla bilgi için.

Kullanım **kaynağı görüntüle** oluşturulan biçimlendirme incelemek için sık kullanılan tarayıcınızdan. Oluşturulan HTML bir bölümü aşağıda verilmiştir:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Bir sorgu dizesi film Kimliğiyle dinamik olarak üretilen bağlantılar geçirin (örneğin, `http://localhost:5000/Movies/Details?id=2` ). 

Düzenleme, Ayrıntılar ve Razor Sayfaları Sil "{kimliği: int}" rota şablonu kullanmak için güncelleştirin. Sayfa yönergesi her bu sayfalarından değiştirme `@page` için `@page "{id:int}"`. Uygulamayı çalıştırın ve ardından kaynak görüntüleyin. Oluşturulan HTML kimliği URL yolu bölümüne ekler:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

İsteği yapan "{kimliği: int}" rota şablonuyla sayfasına **değil** dahil tamsayı (bulunamadı) HTTP 404 hata döndürür. Örneğin, `http://localhost:5000/Movies/Details` 404 hatası döndürür. Kimliği isteğe bağlı yapmak için ekleme `?` rota kısıtlaması için:

 ```cshtml
@page "{id:int?}"
```

### <a name="update-concurrency-exception-handling"></a>Güncelleştirme eşzamanlılık özel durum işleme

Güncelleştirme `OnPostAsync` yönteminde *Pages/Movies/Edit.cshtml.cs* dosya. Aşağıdaki vurgulanmış kodu değişiklikleri gösterir:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet1&highlight=16-23)]

Önceki kod, yalnızca ilk eşzamanlı istemci film siler ve ikinci eşzamanlı istemci film değişiklikler yazılarını eşzamanlılık algılar.

Test etmek için `catch` engelle:

* Bir kesme noktası ayarlayın`catch (DbUpdateConcurrencyException)`
* Bir filmi düzenleyin.
* Başka bir tarayıcı penceresinde seçin **silmek** bağlamak için aynı film ve film silin.
* Önceki tarayıcı penceresinde film değişiklikler gönderin.

Üretim kodu genellikle eşzamanlılık çakışması iki algılamaz veya daha fazla istemciler eşzamanlı olarak güncelleştirilen bir kaydı. Bkz: [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) daha fazla bilgi için.

### <a name="posting-and-binding-review"></a>Gönderme ve bağlama gözden geçirin

İncelemek *Pages/Movies/Edit.cshtml.cs* dosyası:[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit.cshtml.cs?name=snippet2)]

Ne zaman bir HTTP GET isteği yapıldığında filmler/Düzenle sayfasına (örneğin, `http://localhost:5000/Movies/Edit/2`):

* `OnGetAsync` Yöntemi veritabanından film getirir ve döndürür `Page` yöntemi. 
* `Page` Yöntemi işler *Pages/Movies/Edit.cshtml* Razor sayfası. *Pages/Movies/Edit.cshtml* dosyasını içeren model yönergesi (`@model RazorPagesMovie.Pages.Movies.EditModel`), hangi kullanılabilir hale getirir film modeli sayfasında.
* Düzenleme formu film değerlerle görüntülenir.

Ne zaman filmler/düzenleme sayfasını nakledilir:

* Form değerleri sayfasında bağlı olan `Movie` özelliği. `[BindProperty]` Özniteliği etkinleştirir [bağlama Model](xref:mvc/models/model-binding).

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Model durumuna bir hata varsa (örneğin, `ReleaseDate` bir tarihe dönüştürülemez), formu yeniden gönderilen değerlerle nakledilir.
* Model hatalar varsa, film kaydedilir.

Dizin oluşturma ve silme Razor sayfalarının HTTP GET yöntemlere benzer bir desen izleyin. HTTP POST `OnPostAsync` Razor Sayfa Oluştur yönteminde benzer bir desen izler `OnPostAsync` Razor sayfasını Düzenle yöntemi.

Arama sonraki öğreticide eklenir.

>[!div class="step-by-step"]
[Önceki: SQL Server yerel veritabanı ile çalışma](xref:tutorials/razor-pages/sql)
[arama ekleme](xref:tutorials/razor-pages/search)
