<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

Şimdi bir arama gönderdiğinizde, URL arama sorgu dizesi içeriyor. Arama da gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.

![Aramasını gösteren bir tarayıcı penceresi hayalet = Url ve döndürülen, filmler Ghostbusters ve Ghostbusters 2, word hayalet içerir](../../tutorials/first-mvc-app/search/_static/search_get.png)

Aşağıdaki biçimlendirmede değişiklik gösterir `form` etiketi:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Türe göre arama ekleme

Aşağıdakileri ekleyin `MovieGenreViewModel` sınıfının *modelleri* klasörü:

[!code-csharp[Ana](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Film Tarz görünüm modeli içerir:

   * Film listesi.
   * A `SelectList` türler listesini içeren. Bu kullanıcının listeden bir tarzını seçmesine izin verir.
   * `movieGenre`, seçilen Tarz içerir.

Değiştir `Index` yönteminde `MoviesController.cs` aşağıdaki kod ile:

[!code-csharp[Ana](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Aşağıdaki kod bir `LINQ` tüm türler veritabanından alır sorgu.

[!code-csharp[Ana](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

`SelectList` Türler (sizi istemiyorsanız yinelenen türler için bizim seçim listesi) farklı türler yansıtma tarafından oluşturulur.

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a>Arama türe göre dizini görünümü ekleme

Güncelleştirme `Index.cshtml` gibi:

[!code-HTML[Ana](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Aşağıdaki HTML Yardımcısı kullanılan lambda ifadesi inceleyin:

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
Önceki kod `DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adı belirlemek için lambda ifadesinde başvurulan özelliği. Lambda ifadesi Denetlenmekte değerlendirilmesi yerine olduğundan, bir erişim ihlali almadığınız zaman `model`, `model.movies`, veya `model.movies[0]` olan `null` veya boş. Lambda ifadesi ne zaman değerlendirildiği (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri olarak değerlendirilir.

Genre, filmi ve her ikisi tarafından arayarak uygulamayı test edin.
