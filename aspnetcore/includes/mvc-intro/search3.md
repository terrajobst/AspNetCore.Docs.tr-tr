---
ms.openlocfilehash: 0a9b5bc4a91607306a9f4a23756a740366b24933
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58207972"
---
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Artık bir arama gönderdiğinizde, URL'yi arama sorgu dizesi içerir. Arama de gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.

![AramaDizesi gösteren tarayıcı penceresinde ghost = URL'sini ve döndürülen, filmler Ghostbusters ve Ghostbusters 2, word ghost içerir](~/tutorials/first-mvc-app/search/_static/search_get.png)

Aşağıdaki biçimlendirme değişikliği gösterir `form` etiketi:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Türe göre arama ekleme

Aşağıdaki `MovieGenreViewModel` sınıfının *modelleri* klasörü:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Film Tarz görünüm modeli içerir:

* Filmler listesi.
* A `SelectList` türleri listesini içeren. Bu kullanıcının listeden bir türe izin verir.
* `MovieGenre`, seçilen türe içerir.
* `SearchString`, kullanıcıların, arama metin kutusuna girdiğiniz metnin içerir.

Değiştirin `Index` yönteminde `MoviesController.cs` aşağıdaki kod ile:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Aşağıdaki kod bir `LINQ` veritabanından tüm türleri alan sorgu.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

`SelectList` Türleri (biz istemiyorsanız seçin listemize yinelenen tür sahip) farklı tür yansıtma tarafından oluşturulur.

Kullanıcı için olan öğeyle aradığında, arama kutusuna arama değeri korunur. Arama değeri saklamak üzere doldurmak `SearchString` özellik arama değeri. Ara değer `searchString` parametresi için `Index` denetleyici eylemi.

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a>Arama türe göre dizini görünümü ekleme

Güncelleştirme `Index.cshtml` gibi:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Aşağıdaki HTML Yardımcısı kullanılan bir lambda ifadesi inceleyin:

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
Önceki kodda, `DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adını belirlemek için lambda ifadesinde başvurulan özelliği. Lambda ifadesi inceledi değerlendirilmesi yerine olduğundan, bir erişim ihlali almadığınız olduğunda `model`, `model.Movies`, veya `model.Movies[0]` olan `null` veya boş. Ne zaman lambda ifadesi değerlendirilir (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.

Uygulama Tarz, film adı ve her ikisi tarafından arama yaparak test edin.
