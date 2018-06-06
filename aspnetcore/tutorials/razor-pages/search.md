---
title: Arama ASP.NET Core Razor sayfalara ekleme
author: rick-anderson
description: Arama ASP.NET Core Razor sayfalara eklemek nasıl gösterir
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/search
ms.openlocfilehash: 849ebc1c9e661480f02f80078f2fdad02366b3a5
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582849"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a>Arama ASP.NET Core Razor sayfalara ekleme

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu belgede, arama özelliği tarafından arama filmler etkinleştirir dizin sayfası eklenir *Tarz* veya *adı*.

Dizin sayfasının güncelleştirme `OnGetAsync` aşağıdaki kod ile yöntemi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

İlk satırı `OnGetAsync` yöntemi oluşturur bir [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) filmler seçmek için sorgu:

```csharp
var movies = from m in _context.Movie
             select m;
```

Sorgu *yalnızca* bu noktada tanımlı olan **değil** veritabanına karşı çalışırlar bırakıldı.

Varsa `searchString` parametre içeren bir dize, filmler sorgu filtre arama dizesi şekilde değiştirilir:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

`s => s.Title.Contains()` Kodu bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambda'lar yöntemi tabanlı içinde kullanılan [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) standart sorgu işleci yöntemlerinden bağımsız değişken olarak gibi sorgular [nerede](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) yöntemi veya `Contains` (Yukarıdaki kod içinde kullanılan). LINQ sorgularını değil tanımlanan veya ne zaman bir yöntemini çağırarak değiştirilmeden yürütülen (gibi `Where`, `Contains` veya `OrderBy`). Bunun yerine, sorgu yürütme ertelenir. Bir ifadenin değerlendirmesine üzerinden gerçekleşen değerini yinelendiğinde kadar Gecikmeli anlamına veya `ToListAsync` yöntemi çağrılır. Bkz: [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution) daha fazla bilgi için.

**Not:** [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, C# kodu değil, veritabanında çalıştırılır. Büyük küçük harfe duyarlılığın sorgusu, veritabanı ve harmanlama bağlıdır. SQL Server `Contains` eşlendiği [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu. SQLite içinde varsayılan harmanlaması ile büyük küçük harfe duyarlıdır.

Film sayfasına gidin ve bir sorgu dizesi gibi ilave `?searchString=Ghost` URL (örneğin, `http://localhost:5000/Movies?searchString=Ghost`). Filtrelenmiş filmler görüntülenir.

![Dizin görünümü](search/_static/ghost.png)

Aşağıdaki rota şablonu dizin sayfasına eklediyseniz, arama dizesini bir URL kesimi geçirilebilir (örneğin, `http://localhost:5000/Movies/Ghost`).

```cshtml
@page "{searchString?}"
```

Önceki rota kısıtlaması, bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine unvanını arama sağlar.  `?` İçinde `"{searchString?}"` bu bir isteğe bağlı bir rota parametresini anlamına gelir.

![Url ve iki filmler, Ghostbusters ve Ghostbusters 2 döndürülen film listesi eklenen word hayalet dizin görünümünün](search/_static/g2.png)

Ancak, bir filmi için aranacak URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz. Bu adımda, filmler filtrelemek için kullanıcı Arabirimi eklenir. Rota kısıtlaması eklediyseniz `"{searchString?}"`, kaldırın.

Açık *Pages/Movies/Index.cshtml* dosya ve ekleme `<form>` aşağıdaki kodda vurgulanan biçimlendirme:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

HTML `<form>` etiketi kullanır [Form etiketi yardımcı](xref:mvc/views/working-with-forms#the-form-tag-helper). Form gönderildiğinde, filtre dizesi gönderilen *filmler/sayfalar/dizin* sayfası. Değişiklikleri kaydetmek ve filtre sınayın.

![Word hayalet başlığı filtre metin kutusuna yazdığınız dizin görünümünün](search/_static/filter.png)

## <a name="search-by-genre"></a>Türe göre ara

Aşağıdaki vurgulanan özellikleri ekleyin *Pages/Movies/Index.cshtml.cs*:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]
::: moniker-end


`SelectList Genres` Türler listesini içerir. Bu kullanıcının listeden bir tarzını seçmesine olanak sağlar.

`MovieGenre` Özelliği, kullanıcı seçer (örneğin, "Batı") belirli bir tarzını içerir.

Güncelleştirme `OnGetAsync` aşağıdaki kod ile yöntemi:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

Tüm türler veritabanından alır bir LINQ Sorgu kodudur.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

`SelectList` Türler farklı türler yansıtma tarafından oluşturulur.

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

Genre, filmi ve her ikisi tarafından arayarak uygulamayı test edin.

> [!div class="step-by-step"]
> [Önceki: sayfalarını güncelleştirme](xref:tutorials/razor-pages/da1)
> [sonraki: yeni bir alan ekleme](xref:tutorials/razor-pages/new-field)
