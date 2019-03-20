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

<span data-ttu-id="9bbcd-101">Artık bir arama gönderdiğinizde, URL'yi arama sorgu dizesi içerir.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="9bbcd-102">Arama de gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![AramaDizesi gösteren tarayıcı penceresinde ghost = URL'sini ve döndürülen, filmler Ghostbusters ve Ghostbusters 2, word ghost içerir](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="9bbcd-104">Aşağıdaki biçimlendirme değişikliği gösterir `form` etiketi:</span><span class="sxs-lookup"><span data-stu-id="9bbcd-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="9bbcd-105">Türe göre arama ekleme</span><span class="sxs-lookup"><span data-stu-id="9bbcd-105">Adding Search by genre</span></span>

<span data-ttu-id="9bbcd-106">Aşağıdaki `MovieGenreViewModel` sınıfının *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="9bbcd-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="9bbcd-107">Film Tarz görünüm modeli içerir:</span><span class="sxs-lookup"><span data-stu-id="9bbcd-107">The movie-genre view model will contain:</span></span>

* <span data-ttu-id="9bbcd-108">Filmler listesi.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-108">A list of movies.</span></span>
* <span data-ttu-id="9bbcd-109">A `SelectList` türleri listesini içeren.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="9bbcd-110">Bu kullanıcının listeden bir türe izin verir.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-110">This allows the user to select a genre from the list.</span></span>
* <span data-ttu-id="9bbcd-111">`MovieGenre`, seçilen türe içerir.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-111">`MovieGenre`, which contains the selected genre.</span></span>
* <span data-ttu-id="9bbcd-112">`SearchString`, kullanıcıların, arama metin kutusuna girdiğiniz metnin içerir.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-112">`SearchString`, which contains the text users enter in the search text box.</span></span>

<span data-ttu-id="9bbcd-113">Değiştirin `Index` yönteminde `MoviesController.cs` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="9bbcd-113">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="9bbcd-114">Aşağıdaki kod bir `LINQ` veritabanından tüm türleri alan sorgu.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-114">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="9bbcd-115">`SelectList` Türleri (biz istemiyorsanız seçin listemize yinelenen tür sahip) farklı tür yansıtma tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-115">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

<span data-ttu-id="9bbcd-116">Kullanıcı için olan öğeyle aradığında, arama kutusuna arama değeri korunur.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-116">When the user searches for the item, the search value is retained in the search box.</span></span> <span data-ttu-id="9bbcd-117">Arama değeri saklamak üzere doldurmak `SearchString` özellik arama değeri.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-117">To retain the search value,  populate the `SearchString` property with the search value.</span></span> <span data-ttu-id="9bbcd-118">Ara değer `searchString` parametresi için `Index` denetleyici eylemi.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-118">The search value is the `searchString` parameter for the `Index` controller action.</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="9bbcd-119">Arama türe göre dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="9bbcd-119">Adding search by genre to the Index view</span></span>

<span data-ttu-id="9bbcd-120">Güncelleştirme `Index.cshtml` gibi:</span><span class="sxs-lookup"><span data-stu-id="9bbcd-120">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="9bbcd-121">Aşağıdaki HTML Yardımcısı kullanılan bir lambda ifadesi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="9bbcd-121">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
<span data-ttu-id="9bbcd-122">Önceki kodda, `DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adını belirlemek için lambda ifadesinde başvurulan özelliği.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-122">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="9bbcd-123">Lambda ifadesi inceledi değerlendirilmesi yerine olduğundan, bir erişim ihlali almadığınız olduğunda `model`, `model.Movies`, veya `model.Movies[0]` olan `null` veya boş.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-123">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="9bbcd-124">Ne zaman lambda ifadesi değerlendirilir (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-124">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="9bbcd-125">Uygulama Tarz, film adı ve her ikisi tarafından arama yaparak test edin.</span><span class="sxs-lookup"><span data-stu-id="9bbcd-125">Test the app by searching by genre, by movie title, and by both.</span></span>
