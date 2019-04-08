---
ms.openlocfilehash: 1c693712cc39c0e01ad19409bd10bc5e24737b3a
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208495"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="fee23-101">Bir ASP.NET Core MVC uygulaması için arama Ekle</span><span class="sxs-lookup"><span data-stu-id="fee23-101">Add search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="fee23-102">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fee23-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fee23-103">Bu bölümde eklediğiniz için arama yeteneğine `Index` olanak sağlayan bir eylem yöntemi tarafından filmler arama *Tarz* veya *adı*.</span><span class="sxs-lookup"><span data-stu-id="fee23-103">In this section you add search capability to the `Index` action method that lets you search movies by *genre* or *name*.</span></span>

<span data-ttu-id="fee23-104">Güncelleştirme `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="fee23-104">Update the `Index` method with the following code:</span></span>
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

<span data-ttu-id="fee23-105">İlk satırı `Index` eylem yöntemi oluşturur bir [LINQ](/dotnet/standard/using-linq) filmler seçmek için sorgu:</span><span class="sxs-lookup"><span data-stu-id="fee23-105">The first line of the `Index` action method creates a [LINQ](/dotnet/standard/using-linq) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="fee23-106">Sorgu *yalnızca* bu noktada tanımlanan, sahip **değil** veritabanına karşı çalıştırılmış.</span><span class="sxs-lookup"><span data-stu-id="fee23-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="fee23-107">Varsa `searchString` parametresi içeren bir dize, filmler sorgu arama dizesinin değerini temel filtrelemek için değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="fee23-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

<span data-ttu-id="fee23-108">`s => s.Title.Contains()` Kodu yukarıdaki bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="fee23-108">The `s => s.Title.Contains()` code above is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="fee23-109">Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](/dotnet/standard/using-linq) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](/dotnet/api/system.linq.enumerable.where) yöntemi veya `Contains` (Yukarıdaki kod kullanılır).</span><span class="sxs-lookup"><span data-stu-id="fee23-109">Lambdas are used in method-based [LINQ](/dotnet/standard/using-linq) queries as arguments to standard query operator methods such as the [Where](/dotnet/api/system.linq.enumerable.where) method or `Contains` (used in the code above).</span></span> <span data-ttu-id="fee23-110">LINQ sorguları tanımlanan ya da bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez `Where`, `Contains` veya `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="fee23-110">LINQ queries are not executed when they're defined or when they're modified by calling a method such as `Where`, `Contains`  or `OrderBy`.</span></span> <span data-ttu-id="fee23-111">Bunun yerine, sorgu yürütme ertelenir.</span><span class="sxs-lookup"><span data-stu-id="fee23-111">Rather, query execution is deferred.</span></span>  <span data-ttu-id="fee23-112">Anlamına gerçekleşen değeri gerçekten üzerinden yinelenir kadar bir ifade değerlendirmesi ertelendi veya `ToListAsync` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fee23-112">That means that the evaluation of an expression is delayed until its realized value is actually iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="fee23-113">Ertelenen sorgu yürütme hakkında daha fazla bilgi için bkz. [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="fee23-113">For more information about deferred query execution, see [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span></span>

<span data-ttu-id="fee23-114">Not: [İçerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, yukarıda gösterilen C# kodunda değil, veritabanı üzerinde çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="fee23-114">Note: The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the c# code shown above.</span></span> <span data-ttu-id="fee23-115">Büyük/küçük harf duyarlılığı sorguda, veritabanı ve harmanlama bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fee23-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="fee23-116">SQL Server'da [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) eşlendiği [SQL gibi](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="fee23-116">On SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="fee23-117">SQLite içinde varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="fee23-117">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="fee23-118">`/Movies/Index` sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="fee23-118">Navigate to `/Movies/Index`.</span></span> <span data-ttu-id="fee23-119">Bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL'si.</span><span class="sxs-lookup"><span data-stu-id="fee23-119">Append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="fee23-120">Filtrelenmiş filmler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fee23-120">The filtered movies are displayed.</span></span>

![Dizini görüntüle](~/tutorials/first-mvc-app/search/_static/ghost.png)

<span data-ttu-id="fee23-122">İmzası değiştirirseniz `Index` adlı bir parametreye sahip yöntemi `id`, `id` parametresi isteğe bağlı olarak eşleşir `{id}` varsayılan için yer tutucu yönlendiren kümesinde *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="fee23-122">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the optional `{id}` placeholder for the default routes set in *Startup.cs*.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
