---
title: ASP.NET Core Razor sayfaları için arama Ekle
author: rick-anderson
description: Arama için ASP.NET Core Razor sayfalar ekleme işlemi açıklanır
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 9608f454c2c4ae0f4db1e71200b0ca98fe2cd2ad
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454719"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a>ASP.NET Core Razor sayfaları için arama Ekle

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belgede, arama özelliği tarafından arama filmler sağlayan dizin sayfasına eklenen *Tarz* veya *adı*.

Dizin sayfanın güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

İlk satırı `OnGetAsync` yöntemi oluşturur bir [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) filmler seçmek için sorgu:

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

Sorgu *yalnızca* bu noktada tanımlanan, sahip **değil** veritabanına karşı çalıştırılmış.

Varsa `searchString` parametresi içeren bir dize, filmler sorgu üzerinde arama dizesi filtrelemek için değiştirilir:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` Kodu bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) yöntemi veya `Contains` (Önceki kodda kullanılır). LINQ sorguları tanımlanan ya da bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez (gibi `Where`, `Contains` veya `OrderBy`). Bunun yerine, sorgu yürütme ertelenir. Bir ifade değerlendirmesi üzerinden gerçekleştirilen değerini yinelendiğinde kadar Gecikmeli anlamına veya `ToListAsync` yöntemi çağrılır. Bkz: [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution) daha fazla bilgi için.

**Not:** [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, C# kodu değil, veritabanı üzerinde çalıştırılır. Büyük/küçük harf duyarlılığı sorguda, veritabanı ve harmanlama bağlıdır. SQL Server'da `Contains` eşlendiği [SQL gibi](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu. SQLite içinde varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.

Filmler sayfasına gidin ve bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL'sine (örneğin, `http://localhost:5000/Movies?searchString=Ghost`). Filtrelenmiş filmler görüntülenir.

![Dizini görüntüle](search/_static/ghost.png)

Aşağıdaki rota şablonu dizin sayfasına eklenirse, arama dizesi URL kesimi geçirilebilir (örneğin, `http://localhost:5000/Movies/Ghost`).

```cshtml
@page "{searchString?}"
```

Önceki rota kısıtlaması başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine arama sağlar.  `?` İçinde `"{searchString?}"` Bu, bir isteğe bağlı bir rota parametresini anlamına gelir.

![Dizin görünümünün URL'sini ve döndürülen film listesini Ghostbusters ve Ghostbusters 2 iki filmler eklenen word ghost](search/_static/g2.png)

Ancak, bir filmi için aranacak URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Bu adımda, filmler filtrelemek için kullanıcı Arabirimi eklenir. Rota kısıtlaması eklediyseniz `"{searchString?}"`, bunu kaldırın.

Açık *Pages/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme aşağıdaki kodda vurgulanır:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

HTML `<form>` etiketi kullanan [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-form-tag-helper). Form gönderildiğinde, filtre dizesi gönderilen *filmler/sayfalar/dizin* sayfası. Değişiklikleri kaydetmek ve filtre test edin.

![Dizin görünümünün başlık filtre metin kutusuna yazdığınız word ghost](search/_static/filter.png)

## <a name="search-by-genre"></a>Türe göre ara

Vurgulanan aşağıdaki özelliği ekleyin *Pages/Movies/Index.cshtml.cs*:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end


`SelectList Genres` Türleri listesini içerir. Bu kullanıcının listeden bir türe izin verir.

`MovieGenre` Özelliği, kullanıcı seçer (örneğin, "Batı") belirli Tarz içerir.

Güncelleştirme `OnGetAsync` yöntemini aşağıdaki kod ile:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Tüm türleri veritabanından alır bir LINQ Sorgu kodudur.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

`SelectList` Türleri farklı türleri yansıtma tarafından oluşturulur.

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a>Türe göre arama ekleme

Güncelleştirme *Index.cshtml* gibi:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Uygulama Tarz, film adı ve her ikisi tarafından arama yaparak test edin.

> [!div class="step-by-step"]
> [Önceki: sayfaları güncelleştirme](xref:tutorials/razor-pages/da1)
> [sonraki: yeni alan ekleme](xref:tutorials/razor-pages/new-field)
