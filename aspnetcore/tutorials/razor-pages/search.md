---
title: ASP.NET Core Razor sayfaları için arama Ekle
author: rick-anderson
description: Arama için ASP.NET Core Razor sayfalar ekleme işlemi açıklanır
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.date: 12/3/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 8e047024180b20e3b649085647a9136140911fee
ms.sourcegitcommit: 8a65f6c2cbe290fb2418eed58f60fb74c95392c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52892074"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a>ASP.NET Core Razor sayfaları için arama Ekle

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

Aşağıdaki bölümlerde, arama tarafından filmler *Tarz* veya *adı* eklenir.

Vurgulanan aşağıdaki özelliği ekleyin *Pages/Movies/Index.cshtml.cs*:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* `SearchString`: kullanıcıların, arama metin kutusuna girdiğiniz metnin içerir. `SearchString` ile donatılmış [ `[BindProperty]` ](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) özniteliği. `[BindProperty]` Form değerlerini ve sorgu dizeleriyle özelliğiyle aynı ada bağlar. `(SupportsGet = true)` GET isteklerinde bağlama için gereklidir.
* `Genres`: tür listesini içerir. `Genres` kullanıcının listeden bir türe izin verir. `SelectList` gerektirir `using Microsoft.AspNetCore.Mvc.Rendering;`
* `MovieGenre`: belirli bir türe kullanıcı seçer (örneğin, "Batı") içeriyor.
* `Genres` ve `MovieGenre` Bu öğreticinin ilerleyen bölümlerinde kullanılır.

[!INCLUDE[](~/includes/bind-get.md)]

Dizin sayfanın güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

İlk satırı `OnGetAsync` yöntemi oluşturur bir [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) filmler seçmek için sorgu:

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

Sorgu *yalnızca* bu noktada tanımlanan, sahip **değil** veritabanına karşı çalıştırılmış.

Varsa `SearchString` özelliği null veya boş değil, filmler sorgu üzerinde arama dizesi filtrelemek için değiştirilir:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` Kodu bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) yöntemi veya `Contains` (Önceki kodda kullanılır). LINQ sorguları tanımlanan ya da bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez (gibi `Where`, `Contains` veya `OrderBy`). Bunun yerine, sorgu yürütme ertelenir. Bir ifade değerlendirmesi üzerinden gerçekleştirilen değerini yinelendiğinde kadar Gecikmeli anlamına veya `ToListAsync` yöntemi çağrılır. Bkz: [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution) daha fazla bilgi için.

**Not:** [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, C# kodu değil, veritabanı üzerinde çalıştırılır. Büyük/küçük harf duyarlılığı sorguda, veritabanı ve harmanlama bağlıdır. SQL Server'da `Contains` eşlendiği [SQL gibi](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu. SQLite içinde varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.

Filmler sayfasına gidin ve bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL'sine (örneğin, `https://localhost:5001/Movies?searchString=Ghost`). Filtrelenmiş filmler görüntülenir.

![Dizini görüntüle](search/_static/ghost.png)

Aşağıdaki rota şablonu dizin sayfasına eklenirse, arama dizesi URL kesimi geçirilebilir (örneğin, `https://localhost:5001/Movies/Ghost`).

```cshtml
@page "{searchString?}"
```

Önceki rota kısıtlaması başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine arama sağlar.  `?` İçinde `"{searchString?}"` Bu, bir isteğe bağlı bir rota parametresini anlamına gelir.

![Dizin görünümünün URL'sini ve döndürülen film listesini Ghostbusters ve Ghostbusters 2 iki filmler eklenen word ghost](search/_static/g2.png)

ASP.NET Core çalışma zamanı kullanan [model bağlama](xref:mvc/models/model-binding) değerini ayarlamak için `SearchString` Sorgu dizesinden özelliği (`?searchString=Ghost`) veya verilerini yönlendirme (`https://localhost:5001/Movies/Ghost`). Model bağlama büyük/küçük harfe duyarlı değildir.

Ancak, bir filmi için aranacak URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Bu adımda, filmler filtrelemek için kullanıcı Arabirimi eklenir. Rota kısıtlaması eklediyseniz `"{searchString?}"`, bunu kaldırın.

Açık *Pages/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme aşağıdaki kodda vurgulanır:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

HTML `<form>` etiketi kullanan aşağıdaki [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro):

* [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper). Form gönderildiğinde, filtre dizesi gönderilen *filmler/sayfalar/dizin* sayfası aracılığıyla sorgu dizesi.
* [Giriş Etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-input-tag-helper)

Değişiklikleri kaydetmek ve filtre test edin.

![Dizin görünümünün başlık filtre metin kutusuna yazdığınız word ghost](search/_static/filter.png)

## <a name="search-by-genre"></a>Türe göre ara

Güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Tüm türleri veritabanından alır bir LINQ Sorgu kodudur.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

`SelectList` Türleri farklı türleri yansıtma tarafından oluşturulur.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a>Razor sayfası için türe göre arama Ekle

Güncelleştirme *Index.cshtml* gibi:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Uygulama Tarz, film adı ve her ikisi tarafından arama yaparak test edin.

> [!div class="step-by-step"]
> [Önceki: sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)
> [sonraki: yeni alan ekleme](xref:tutorials/razor-pages/new-field)
