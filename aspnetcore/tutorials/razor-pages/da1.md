---
title: ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirme
author: rick-anderson
description: ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirme hakkında bilgi edinin.
ms.author: riande
ms.date: 12/20/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 0f6535462fe2d308825bf7289c10d2b0690cebd4
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334108"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>ASP.NET Core uygulamasında oluşturulan sayfaları güncelleştirme

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

::: moniker range=">= aspnetcore-3.0"

Yapı iskelesi film uygulamasının iyi bir başlangıcı vardır ancak sunum ideal değildir. **ReleaseDate** **Yayın tarihi** (iki sözcük) olmalıdır.

![Chrome 'da açık film uygulaması](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Oluşturulan kodu Güncelleştir

*Modeller/film. cs* dosyasını açın ve aşağıdaki kodda gösterilen vurgulanmış satırları ekleyin:

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

@No__t-0 veri ek açıklaması, Entity Framework Core `Price` ' i veritabanında para birimine doğru şekilde eşlemesine olanak sağlar. Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).

[Veri açıklamaları](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide ele alınmıştır. [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği bir alanın adı için (Bu durumda "ReleaseDate" yerine "Yayın tarihi") görüntüleneceğini belirtir. [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (Tarih), bu nedenle alanda depolanan zaman bilgileri gösterilmez.

Hedef URL 'yi görmek için sayfalara/filmlere gidin ve bir **düzenleme** bağlantısının üzerine gelin.

![Düzenleme bağlantısı üzerinde fare ile tarayıcı penceresi ve http://localhost:1234/Movies/Edit/5 bağlantı URL 'Si gösteriliyor](~/tutorials/razor-pages/da1/edit7.png)

**Düzenle**, **Ayrıntılar**ve **Sil** bağlantıları, *Sayfalar/filmler/Index. cshtml* dosyasındaki [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) tarafından oluşturulur.

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir. Yukarıdaki kodda `AnchorTagHelper`, Razor sayfasından (yol göreli), `asp-page` ve yol kimliği (`asp-route-id`) HTML `href` öznitelik değerini dinamik olarak oluşturur. Daha fazla bilgi için bkz. [Sayfalar Için URL oluşturma](xref:razor-pages/index#url-generation-for-pages) .

Oluşturulan biçimlendirmeyi incelemek için sık kullandığınız tarayıcıdan **Görünüm kaynağını** kullanın. Oluşturulan HTML 'nin bir bölümü aşağıda gösterilmiştir:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Dinamik olarak oluşturulan bağlantılar film KIMLIĞINI bir sorgu dizesiyle (örneğin, `https://localhost:5001/Movies/Details?id=1` ' de `?id=1`) iletir.

### <a name="add-route-template"></a>Rota şablonu Ekle

"{İd: int}" yol şablonunu kullanmak için Düzenle, Ayrıntılar ve Sil Razor Pages güncelleştirin. Bu sayfaların her biri için Page yönergesini `@page` ' dan `@page "{id:int}"` ' e değiştirin. Uygulamayı çalıştırın ve kaynağı görüntüleyin. Oluşturulan HTML, URL 'nin yol bölümüne KIMLIĞI ekler:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Tamsayıyı **içermeyen "** {id: int}" yol şablonuna sahip sayfaya yönelik bir Istek, HTTP 404 (bulunamadı) hatası döndürüyor. Örneğin, `http://localhost:5000/Movies/Details` bir 404 hatası döndürür. KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:

 ```cshtml
@page "{id:int?}"
```

@No__t davranışını test etmek için-0:

* *Pages/filmler/details. cshtml* içindeki page yönergesini `@page "{id:int?}"` olarak ayarlayın.
* @No__t-0 ' da ( *sayfalarda/filmlerde/details. cshtml. cs*) bir kesme noktası ayarlayın.
* @No__t-0 ' a gidin.

@No__t-0 yönergesi ile, kesme noktası hiçbir şekilde vurılmaz. Yönlendirme Altyapısı HTTP 404 döndürür. @No__t-0 ' ı kullanarak `OnGetAsync` yöntemi `NotFound` (HTTP 404) döndürür.

### <a name="review-concurrency-exception-handling"></a>Eşzamanlılık özel durum işlemeyi gözden geçirme

*Pages/filmler/Edit. cshtml. cs* dosyasında `OnPostAsync` yöntemini gözden geçirin:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Edit.cshtml.cs?name=snippet)]

Önceki kod, bir istemci filmi sildiği ve diğer istemci filmle değişiklik yaptığı zaman eşzamanlılık özel durumlarını algılar.

@No__t-0 bloğunu test etmek için:

* @No__t kesme noktası ayarla-0
* Film için **Düzenle** ' yi seçin, değişiklikler yapın, ancak **Kaydet**' i girmeyin.
* Başka bir tarayıcı penceresinde, aynı filmin **Sil** bağlantısını seçin ve ardından filmi silin.
* Önceki tarayıcı penceresinde filmdeki değişiklikleri gönderin.

Üretim kodu eşzamanlılık çakışmalarını algılamak isteyebilir. Daha fazla bilgi için bkz. [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) .

### <a name="posting-and-binding-review"></a>Gönderme ve bağlama incelemesi

*Pages/filmler/Edit. cshtml. cs* dosyasını inceleyin:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/SnapShots/Edit.cshtml.cs?name=snippet2)]

Filmler/düzenleme sayfasına HTTP GET isteği yapıldığında (örneğin, `http://localhost:5000/Movies/Edit/2`):

* @No__t-0 yöntemi, filmi veritabanından getirir ve `Page` yöntemini döndürür.
* @No__t-0 yöntemi *Sayfalar/filmler/Edit. cshtml* Razor sayfasını işler. *Pages/filmler/Edit. cshtml* dosyası, film modelinin sayfada kullanılabilir olmasını sağlayan model yönergesini (`@model RazorPagesMovie.Pages.Movies.EditModel`) içerir.
* Düzenleme formu filmdeki değerlerle birlikte görüntülenir.

Filmler/Düzenle sayfası gönderildiğinde:

* Sayfadaki form değerleri `Movie` özelliğine bağlıdır. @No__t-0 özniteliği [model bağlamayı](xref:mvc/models/model-binding)mümkün.

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Model durumunda hatalar varsa (örneğin, `ReleaseDate` bir tarihe dönüştürülemez), form gönderilen değerlerle yeniden görüntülenir.
* Model hatası yoksa, film kaydedilir.

Razor sayfalarında Dizin, oluşturma ve silme gibi HTTP GET yöntemleri benzer bir düzende yer alır. Razor Oluştur sayfasındaki HTTP POST `OnPostAsync` yöntemi, Razor düzenleme sayfasındaki `OnPostAsync` yöntemine benzer bir düzen izler.

## <a name="additional-resources"></a>Ek kaynaklar

> [!div class="step-by-step"]
> [Önceki: bir veritabanıyla çalışma](xref:tutorials/razor-pages/sql)
> [Sonraki: Arama ekle](xref:tutorials/razor-pages/search)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Yapı iskelesi film uygulamasının iyi bir başlangıcı vardır ancak sunum ideal değildir. **ReleaseDate** **Yayın tarihi** (iki sözcük) olmalıdır.

![Chrome 'da açık film uygulaması](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Oluşturulan kodu Güncelleştir

*Modeller/film. cs* dosyasını açın ve aşağıdaki kodda gösterilen vurgulanmış satırları ekleyin:

[!code-csharp[Main](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateFixed.cs?name=snippet_1&highlight=3,12,17)]

@No__t-0 veri ek açıklaması, Entity Framework Core `Price` ' i veritabanında para birimine doğru şekilde eşlemesine olanak sağlar. Daha fazla bilgi için bkz. [veri türleri](/ef/core/modeling/relational/data-types).

[Veri açıklamaları](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) sonraki öğreticide ele alınmıştır. [Display](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) özniteliği bir alanın adı için (Bu durumda "ReleaseDate" yerine "Yayın tarihi") görüntüleneceğini belirtir. [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) özniteliği verilerin türünü belirtir (Tarih), bu nedenle alanda depolanan zaman bilgileri gösterilmez.

Hedef URL 'yi görmek için sayfalara/filmlere gidin ve bir **düzenleme** bağlantısının üzerine gelin.

![Düzenleme bağlantısı üzerinde fare ile tarayıcı penceresi ve http://localhost:1234/Movies/Edit/5 bağlantı URL 'Si gösteriliyor](~/tutorials/razor-pages/da1/edit7.png)

**Düzenle**, **Ayrıntılar**ve **Sil** bağlantıları, *Sayfalar/filmler/Index. cshtml* dosyasındaki [tutturucu etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) tarafından oluşturulur.

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=16-18&range=32-)]

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir. Yukarıdaki kodda `AnchorTagHelper`, Razor sayfasından (yol göreli), `asp-page` ve yol kimliği (`asp-route-id`) HTML `href` öznitelik değerini dinamik olarak oluşturur. Daha fazla bilgi için bkz. [Sayfalar Için URL oluşturma](xref:razor-pages/index#url-generation-for-pages) .

Oluşturulan biçimlendirmeyi incelemek için sık kullandığınız tarayıcıdan **Görünüm kaynağını** kullanın. Oluşturulan HTML 'nin bir bölümü aşağıda gösterilmiştir:

```html
<td>
  <a href="/Movies/Edit?id=1">Edit</a> |
  <a href="/Movies/Details?id=1">Details</a> |
  <a href="/Movies/Delete?id=1">Delete</a>
</td>
```

Dinamik olarak oluşturulan bağlantılar film KIMLIĞINI bir sorgu dizesiyle (örneğin, `https://localhost:5001/Movies/Details?id=1` ' de `?id=1`) iletir.

"{İd: int}" yol şablonunu kullanmak için Düzenle, Ayrıntılar ve Sil Razor Pages güncelleştirin. Bu sayfaların her biri için Page yönergesini `@page` ' dan `@page "{id:int}"` ' e değiştirin. Uygulamayı çalıştırın ve kaynağı görüntüleyin. Oluşturulan HTML, URL 'nin yol bölümüne KIMLIĞI ekler:

```html
<td>
  <a href="/Movies/Edit/1">Edit</a> |
  <a href="/Movies/Details/1">Details</a> |
  <a href="/Movies/Delete/1">Delete</a>
</td>
```

Tamsayıyı **içermeyen "** {id: int}" yol şablonuna sahip sayfaya yönelik bir Istek, HTTP 404 (bulunamadı) hatası döndürüyor. Örneğin, `http://localhost:5000/Movies/Details` bir 404 hatası döndürür. KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:

 ```cshtml
@page "{id:int?}"
```

@No__t davranışını test etmek için-0:

* *Pages/filmler/details. cshtml* içindeki page yönergesini `@page "{id:int?}"` olarak ayarlayın.
* @No__t-0 ' da ( *sayfalarda/filmlerde/details. cshtml. cs*) bir kesme noktası ayarlayın.
* @No__t-0 ' a gidin.

@No__t-0 yönergesi ile, kesme noktası hiçbir şekilde vurılmaz. Yönlendirme Altyapısı HTTP 404 döndürür. @No__t-0 ' ı kullanarak `OnGetAsync` yöntemi `NotFound` (HTTP 404) döndürür.

### <a name="review-concurrency-exception-handling"></a>Eşzamanlılık özel durum işlemeyi gözden geçirme

*Pages/filmler/Edit. cshtml. cs* dosyasında `OnPostAsync` yöntemini gözden geçirin:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Edit.cshtml.cs?name=snippet)]

Önceki kod, bir istemci filmi sildiği ve diğer istemci filmle değişiklik yaptığı zaman eşzamanlılık özel durumlarını algılar.

@No__t-0 bloğunu test etmek için:

* @No__t kesme noktası ayarla-0
* Film için **Düzenle** ' yi seçin, değişiklikler yapın, ancak **Kaydet**' i girmeyin.
* Başka bir tarayıcı penceresinde, aynı filmin **Sil** bağlantısını seçin ve ardından filmi silin.
* Önceki tarayıcı penceresinde filmdeki değişiklikleri gönderin.

Üretim kodu eşzamanlılık çakışmalarını algılamak isteyebilir. Daha fazla bilgi için bkz. [eşzamanlılık çakışmalarını işleme](xref:data/ef-rp/concurrency) .

### <a name="posting-and-binding-review"></a>Gönderme ve bağlama incelemesi

*Pages/filmler/Edit. cshtml. cs* dosyasını inceleyin:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Edit21.cshtml.cs?name=snippet2)]

Filmler/düzenleme sayfasına HTTP GET isteği yapıldığında (örneğin, `http://localhost:5000/Movies/Edit/2`):

* @No__t-0 yöntemi, filmi veritabanından getirir ve `Page` yöntemini döndürür. 
* @No__t-0 yöntemi *Sayfalar/filmler/Edit. cshtml* Razor sayfasını işler. *Pages/filmler/Edit. cshtml* dosyası, film modelinin sayfada kullanılabilir olmasını sağlayan model yönergesini (`@model RazorPagesMovie.Pages.Movies.EditModel`) içerir.
* Düzenleme formu filmdeki değerlerle birlikte görüntülenir.

Filmler/Düzenle sayfası gönderildiğinde:

* Sayfadaki form değerleri `Movie` özelliğine bağlıdır. @No__t-0 özniteliği [model bağlamayı](xref:mvc/models/model-binding)mümkün.

  ```csharp
  [BindProperty]
  public Movie Movie { get; set; }
  ```

* Model durumunda hatalar varsa (örneğin, `ReleaseDate` bir tarihe dönüştürülemez), form gönderilen değerlerle birlikte görüntülenir.
* Model hatası yoksa, film kaydedilir.

Razor sayfalarında Dizin, oluşturma ve silme gibi HTTP GET yöntemleri benzer bir düzende yer alır. Razor Oluştur sayfasındaki HTTP POST `OnPostAsync` yöntemi, Razor düzenleme sayfasındaki `OnPostAsync` yöntemine benzer bir düzen izler.

Arama sonraki öğreticiye eklenir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Bu öğreticinin YouTube sürümü](https://youtu.be/yLnnleREMtQ)

> [!div class="step-by-step"]
> [Önceki: bir veritabanıyla çalışma](xref:tutorials/razor-pages/sql)
> [Sonraki: Arama ekle](xref:tutorials/razor-pages/search)

::: moniker-end
