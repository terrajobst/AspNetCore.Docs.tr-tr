# <a name="add-search-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulamasına arama ekleyin

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde eklediğiniz için arama yeteneğine `Index` olanak sağlayan eylem yöntemi tarafından filmler arama *Tarz* veya *adı*.

Güncelleştirme `Index` aşağıdaki kod ile yöntemi:
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

İlk satırı `Index` eylem yöntemi oluşturur bir [LINQ](/dotnet/standard/using-linq) filmler seçmek için sorgu:

```csharp
var movies = from m in _context.Movie
             select m;
```

Sorgu *yalnızca* bu noktada tanımlı olan **değil** veritabanına karşı çalışırlar bırakıldı.

Varsa `searchString` parametre içeren bir dize, filmler sorgu arama dizesi değerini filtre şekilde değiştirilir:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

`s => s.Title.Contains()` Kodu yukarıdaki bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambda'lar yöntemi tabanlı içinde kullanılan [LINQ](/dotnet/standard/using-linq) standart sorgu işleci yöntemlerinden bağımsız değişken olarak gibi sorgular [nerede](/dotnet/api/system.linq.enumerable.where) yöntemi veya `Contains` (Yukarıdaki kod kullanılır). LINQ sorgularını değil tanımlanan veya ne zaman bir yöntemi çağrılarak değiştirilmeden yürütülen `Where`, `Contains` veya `OrderBy`. Bunun yerine, sorgu yürütme ertelenir.  Gerçekleşen değeri gerçekte üzerinden yinelendiğinde kadar bir ifadenin değerlendirmesine Gecikmeli anlamına veya `ToListAsync` yöntemi çağrılır. Ertelenmiş sorgu yürütme hakkında daha fazla bilgi için bkz: [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Not: [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, yukarıda gösterilen değil c# kodunda veritabanında çalıştırılır. Büyük küçük harfe duyarlılığın sorgusu, veritabanı ve harmanlama bağlıdır. SQL Server [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) eşlendiği [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu. SQLlite içinde varsayılan harmanlaması ile büyük küçük harfe duyarlıdır.

Gidin `/Movies/Index`. Bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL. Filtrelenmiş filmler görüntülenir.

![Dizin görünümü](~/tutorials/first-mvc-app/search/_static/ghost.png)

İmzası değiştirirseniz `Index` adlı bir parametreye sahip yöntemi `id`, `id` parametre isteğe bağlı eşleşir `{id}` varsayılan için yer tutucu yönlendirir kümesinde *haline*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
