# <a name="add-search-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulaması için arama Ekle

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde eklediğiniz için arama yeteneğine `Index` olanak sağlayan bir eylem yöntemi tarafından filmler arama *Tarz* veya *adı*.

Güncelleştirme `Index` yöntemini aşağıdaki kod ile:
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

İlk satırı `Index` eylem yöntemi oluşturur bir [LINQ](/dotnet/standard/using-linq) filmler seçmek için sorgu:

```csharp
var movies = from m in _context.Movie
             select m;
```

Sorgu *yalnızca* bu noktada tanımlanan, sahip **değil** veritabanına karşı çalıştırılmış.

Varsa `searchString` parametresi içeren bir dize, filmler sorgu arama dizesinin değerini temel filtrelemek için değiştirilir:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

`s => s.Title.Contains()` Kodu yukarıdaki bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](/dotnet/standard/using-linq) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](/dotnet/api/system.linq.enumerable.where) yöntemi veya `Contains` (Yukarıdaki kod kullanılır). LINQ sorguları tanımlanan ya da bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez `Where`, `Contains` veya `OrderBy`. Bunun yerine, sorgu yürütme ertelenir.  Anlamına gerçekleşen değeri gerçekten üzerinden yinelenir kadar bir ifade değerlendirmesi ertelendi veya `ToListAsync` yöntemi çağrılır. Ertelenen sorgu yürütme hakkında daha fazla bilgi için bkz. [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Not: [İçerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, yukarıda gösterilen c# kodunda değil, veritabanı üzerinde çalıştırılır. Büyük/küçük harf duyarlılığı sorguda, veritabanı ve harmanlama bağlıdır. SQL Server'da [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) eşlendiği [SQL gibi](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu. SQLlite içinde varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.

`/Movies/Index` sayfasına gidin. Bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL'si. Filtrelenmiş filmler görüntülenir.

![Dizini görüntüle](~/tutorials/first-mvc-app/search/_static/ghost.png)

İmzası değiştirirseniz `Index` adlı bir parametreye sahip yöntemi `id`, `id` parametresi isteğe bağlı olarak eşleşir `{id}` varsayılan için yer tutucu yönlendiren kümesinde *Startup.cs*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
