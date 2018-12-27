---
title: Bir ASP.NET Core MVC uygulaması için arama Ekle
author: rick-anderson
description: Basit bir ASP.NET Core MVC uygulaması için arama ekleme işlemi açıklanır
ms.author: riande
ms.date: 12/25/2018
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 3814acf0f1cf1572f1c30ce952df910ce40b4141
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381835"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="2e81a-103">Bir ASP.NET Core MVC uygulaması için arama Ekle</span><span class="sxs-lookup"><span data-stu-id="2e81a-103">Add search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="2e81a-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2e81a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2e81a-105">Bu bölümde eklediğiniz için arama yeteneğine `Index` olanak sağlayan bir eylem yöntemi tarafından filmler arama *Tarz* veya *adı*.</span><span class="sxs-lookup"><span data-stu-id="2e81a-105">In this section you add search capability to the `Index` action method that lets you search movies by *genre* or *name*.</span></span>

<span data-ttu-id="2e81a-106">Güncelleştirme `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="2e81a-106">Update the `Index` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

<span data-ttu-id="2e81a-107">İlk satırı `Index` eylem yöntemi oluşturur bir [LINQ](/dotnet/standard/using-linq) filmler seçmek için sorgu:</span><span class="sxs-lookup"><span data-stu-id="2e81a-107">The first line of the `Index` action method creates a [LINQ](/dotnet/standard/using-linq) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="2e81a-108">Sorgu *yalnızca* bu noktada tanımlanan, sahip **değil** veritabanına karşı çalıştırılmış.</span><span class="sxs-lookup"><span data-stu-id="2e81a-108">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="2e81a-109">Varsa `searchString` parametresi içeren bir dize, filmler sorgu arama dizesinin değerini temel filtrelemek için değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="2e81a-109">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

<span data-ttu-id="2e81a-110">`s => s.Title.Contains()` Kodu yukarıdaki bir [Lambda ifadesi](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="2e81a-110">The `s => s.Title.Contains()` code above is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="2e81a-111">Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](/dotnet/standard/using-linq) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](/dotnet/api/system.linq.enumerable.where) yöntemi veya `Contains` (Yukarıdaki kod kullanılır).</span><span class="sxs-lookup"><span data-stu-id="2e81a-111">Lambdas are used in method-based [LINQ](/dotnet/standard/using-linq) queries as arguments to standard query operator methods such as the [Where](/dotnet/api/system.linq.enumerable.where) method or `Contains` (used in the code above).</span></span> <span data-ttu-id="2e81a-112">LINQ sorguları tanımlanan ya da bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez `Where`, `Contains` veya `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="2e81a-112">LINQ queries are not executed when they're defined or when they're modified by calling a method such as `Where`, `Contains`  or `OrderBy`.</span></span> <span data-ttu-id="2e81a-113">Bunun yerine, sorgu yürütme ertelenir.</span><span class="sxs-lookup"><span data-stu-id="2e81a-113">Rather, query execution is deferred.</span></span>  <span data-ttu-id="2e81a-114">Anlamına gerçekleşen değeri gerçekten üzerinden yinelenir kadar bir ifade değerlendirmesi ertelendi veya `ToListAsync` yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2e81a-114">That means that the evaluation of an expression is delayed until its realized value is actually iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="2e81a-115">Ertelenen sorgu yürütme hakkında daha fazla bilgi için bkz. [sorgu yürütme](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="2e81a-115">For more information about deferred query execution, see [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span></span>

<span data-ttu-id="2e81a-116">Not: [İçerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) yöntemi, yukarıda gösterilen c# kodunda değil, veritabanı üzerinde çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="2e81a-116">Note: The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the c# code shown above.</span></span> <span data-ttu-id="2e81a-117">Büyük/küçük harf duyarlılığı sorguda, veritabanı ve harmanlama bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="2e81a-117">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="2e81a-118">SQL Server'da [içerir](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) eşlendiği [SQL gibi](/sql/t-sql/language-elements/like-transact-sql), büyük küçük harfe duyarlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="2e81a-118">On SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="2e81a-119">SQLlite içinde varsayılan harmanlama ile büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="2e81a-119">In SQLlite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="2e81a-120">`/Movies/Index` sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="2e81a-120">Navigate to `/Movies/Index`.</span></span> <span data-ttu-id="2e81a-121">Bir sorgu dizesi gibi ekleme `?searchString=Ghost` URL'si.</span><span class="sxs-lookup"><span data-stu-id="2e81a-121">Append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="2e81a-122">Filtrelenmiş filmler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2e81a-122">The filtered movies are displayed.</span></span>

![Dizini görüntüle](~/tutorials/first-mvc-app/search/_static/ghost.png)

<span data-ttu-id="2e81a-124">İmzası değiştirirseniz `Index` adlı bir parametreye sahip yöntemi `id`, `id` parametresi isteğe bağlı olarak eşleşir `{id}` varsayılan için yer tutucu yönlendiren kümesinde *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2e81a-124">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the optional `{id}` placeholder for the default routes set in *Startup.cs*.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="2e81a-125">Parametre değiştirme `id` ve tüm oluşumlarını `searchString` değiştirmek `id`.</span><span class="sxs-lookup"><span data-stu-id="2e81a-125">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

<span data-ttu-id="2e81a-126">Önceki `Index` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="2e81a-126">The previous `Index` method:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="2e81a-127">Güncelleştirilmiş `Index` yöntemiyle `id` parametresi:</span><span class="sxs-lookup"><span data-stu-id="2e81a-127">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

<span data-ttu-id="2e81a-128">Arama başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine artık geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2e81a-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![Dizin görünümünün URL'sini ve döndürülen film listesini Ghostbusters ve Ghostbusters 2 iki filmler eklenen word ghost](~/tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="2e81a-130">Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz.</span><span class="sxs-lookup"><span data-stu-id="2e81a-130">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="2e81a-131">Şimdi, filmler filtre yardımcı olmak için kullanıcı Arabirimi öğeleri ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="2e81a-131">So now you'll add UI elements to help them filter movies.</span></span> <span data-ttu-id="2e81a-132">İmzası değiştirdiyseniz `Index` rota bağlı nasıl test etmek için yöntemi `ID` parametresi, yeniden adlandırılmış bir parametre alır, böylece değişiklik `searchString`:</span><span class="sxs-lookup"><span data-stu-id="2e81a-132">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="2e81a-133">Açık *Views/Movies/Index.cshtml* dosya ve ekleme `<form>` biçimlendirme vurgulanmış aşağıda:</span><span class="sxs-lookup"><span data-stu-id="2e81a-133">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="2e81a-134">HTML `<form>` etiketi kullanan [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms), formu gönderdiğinde, filtre dizesi nakledilir `Index` denetleyici filmler eylemi.</span><span class="sxs-lookup"><span data-stu-id="2e81a-134">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="2e81a-135">Yaptığınız değişiklikleri kaydedin ve ardından filtre sınayın.</span><span class="sxs-lookup"><span data-stu-id="2e81a-135">Save your changes and then test the filter.</span></span>

![Dizin görünümünün başlık filtre metin kutusuna yazdığınız word ghost](~/tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="2e81a-137">Var olan hiçbir `[HttpPost]` aşırı yükünü `Index` yöntemi olarak, beklediğiniz.</span><span class="sxs-lookup"><span data-stu-id="2e81a-137">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="2e81a-138">Yöntemi, uygulama durumunu değiştirme değildir çünkü, yalnızca verileri filtreleme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2e81a-138">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="2e81a-139">Aşağıdaki ekleyebilirsiniz `[HttpPost] Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2e81a-139">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="2e81a-140">`notUsed` Parametresi için bir aşırı yükleme oluşturmak için kullanılan `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2e81a-140">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="2e81a-141">Öğreticinin ilerleyen bölümlerinde, hakkında konuşacağız.</span><span class="sxs-lookup"><span data-stu-id="2e81a-141">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="2e81a-142">Eylem çağırıcısı, bu yöntem eklerseniz, BC `[HttpPost] Index` yöntemi ve `[HttpPost] Index` yöntemi aşağıdaki resimde gösterildiği gibi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2e81a-142">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![Tarayıcı penceresi ile HttpPost dizininin'ndan uygulama yanıt: ghost filtreleme](~/tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="2e81a-144">Ancak, bu eklediğiniz bile `[HttpPost]` sürümünü `Index` yöntemi nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="2e81a-144">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="2e81a-145">Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklayabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün.</span><span class="sxs-lookup"><span data-stu-id="2e81a-145">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="2e81a-146">HTTP POST isteği URL'sini GET isteği (localhost:xxxxx filmler/dizin) URL'si ile aynıdır; hiçbir arama bilgisi URL'sindeki dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2e81a-146">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="2e81a-147">Arama dizesi bilgilerini sunucuya olarak gönderilen bir [form alanının değeri](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span><span class="sxs-lookup"><span data-stu-id="2e81a-147">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="2e81a-148">Tarayıcının geliştirici araçları veya mükemmel ile doğrulayabilirsiniz [Fiddler aracı](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="2e81a-148">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="2e81a-149">Aşağıdaki resimde Chrome tarayıcı geliştirici araçları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="2e81a-149">The image below shows the Chrome browser Developer tools:</span></span>

![Ghost AramaDizesi değeri istek gövdesi gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi](~/tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="2e81a-151">Arama parametresi görebilirsiniz ve [XSRF](xref:security/anti-request-forgery) istek gövdesindeki belirteci.</span><span class="sxs-lookup"><span data-stu-id="2e81a-151">You can see the search parameter and [XSRF](xref:security/anti-request-forgery) token in the request body.</span></span> <span data-ttu-id="2e81a-152">Unutmayın, önceki öğreticide belirtildiği gibi [Form etiketi Yardımcısı](xref:mvc/views/working-with-forms) oluşturur bir [XSRF](xref:security/anti-request-forgery) sahteciliğe karşı koruma belirteci.</span><span class="sxs-lookup"><span data-stu-id="2e81a-152">Note, as mentioned in the previous tutorial, the [Form Tag Helper](xref:mvc/views/working-with-forms) generates an [XSRF](xref:security/anti-request-forgery) anti-forgery token.</span></span> <span data-ttu-id="2e81a-153">Denetleyici yönteminin belirteci doğrulamak gerekmez için biz veri, değişiklik yaptığınız değil.</span><span class="sxs-lookup"><span data-stu-id="2e81a-153">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="2e81a-154">İstek gövdesini ve URL'yi arama parametresi olduğundan, bu arama bilgilere yer işareti yakalamak veya başkalarıyla paylaşmak.</span><span class="sxs-lookup"><span data-stu-id="2e81a-154">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="2e81a-155">Düzeltme isteği belirterek olmalıdır `HTTP GET`:</span><span class="sxs-lookup"><span data-stu-id="2e81a-155">Fix this by specifying the request should be `HTTP GET`:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGet.cshtml?highlight=12&range=1-23)]

<span data-ttu-id="2e81a-156">Artık bir arama gönderdiğinizde, URL'yi arama sorgu dizesi içerir.</span><span class="sxs-lookup"><span data-stu-id="2e81a-156">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="2e81a-157">Arama de gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2e81a-157">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![AramaDizesi gösteren tarayıcı penceresinde ghost = URL'sini ve döndürülen, filmler Ghostbusters ve Ghostbusters 2, word ghost içerir](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="2e81a-159">Aşağıdaki biçimlendirme değişikliği gösterir `form` etiketi:</span><span class="sxs-lookup"><span data-stu-id="2e81a-159">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="add-search-by-genre"></a><span data-ttu-id="2e81a-160">Türe göre arama Ekle</span><span class="sxs-lookup"><span data-stu-id="2e81a-160">Add Search by genre</span></span>

<span data-ttu-id="2e81a-161">Aşağıdaki `MovieGenreViewModel` sınıfının *modelleri* klasörü:</span><span class="sxs-lookup"><span data-stu-id="2e81a-161">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="2e81a-162">Film Tarz görünüm modeli içerir:</span><span class="sxs-lookup"><span data-stu-id="2e81a-162">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="2e81a-163">Filmler listesi.</span><span class="sxs-lookup"><span data-stu-id="2e81a-163">A list of movies.</span></span>
   * <span data-ttu-id="2e81a-164">A `SelectList` türleri listesini içeren.</span><span class="sxs-lookup"><span data-stu-id="2e81a-164">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="2e81a-165">Bu kullanıcının listeden bir türe izin verir.</span><span class="sxs-lookup"><span data-stu-id="2e81a-165">This allows the user to select a genre from the list.</span></span>
   * <span data-ttu-id="2e81a-166">`MovieGenre`, seçilen türe içerir.</span><span class="sxs-lookup"><span data-stu-id="2e81a-166">`MovieGenre`, which contains the selected genre.</span></span>
   * <span data-ttu-id="2e81a-167">`SearchString`, kullanıcıların, arama metin kutusuna girdiğiniz metnin içerir.</span><span class="sxs-lookup"><span data-stu-id="2e81a-167">`SearchString`, which contains the text users enter in the search text box.</span></span>

<span data-ttu-id="2e81a-168">Değiştirin `Index` yönteminde `MoviesController.cs` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="2e81a-168">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="2e81a-169">Aşağıdaki kod bir `LINQ` veritabanından tüm türleri alan sorgu.</span><span class="sxs-lookup"><span data-stu-id="2e81a-169">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="2e81a-170">`SelectList` Türleri (biz istemiyorsanız seçin listemize yinelenen tür sahip) farklı tür yansıtma tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2e81a-170">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

<span data-ttu-id="2e81a-171">Kullanıcı için olan öğeyle aradığında, arama kutusuna arama değeri korunur.</span><span class="sxs-lookup"><span data-stu-id="2e81a-171">When the user searches for the item, the search value is retained in the search box.</span></span>

## <a name="add-search-by-genre-to-the-index-view"></a><span data-ttu-id="2e81a-172">Arama dizini görünümü türe göre Ekle</span><span class="sxs-lookup"><span data-stu-id="2e81a-172">Add search by genre to the Index view</span></span>

<span data-ttu-id="2e81a-173">Güncelleştirme `Index.cshtml` gibi:</span><span class="sxs-lookup"><span data-stu-id="2e81a-173">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="2e81a-174">Aşağıdaki HTML Yardımcısı kullanılan bir lambda ifadesi inceleyin:</span><span class="sxs-lookup"><span data-stu-id="2e81a-174">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movies[0].Title)`

<span data-ttu-id="2e81a-175">Önceki kodda, `DisplayNameFor` HTML Yardımcısı inceler `Title` görünen adını belirlemek için lambda ifadesinde başvurulan özelliği.</span><span class="sxs-lookup"><span data-stu-id="2e81a-175">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="2e81a-176">Lambda ifadesi inceledi değerlendirilmesi yerine olduğundan, bir erişim ihlali almadığınız olduğunda `model`, `model.Movies`, veya `model.Movies[0]` olan `null` veya boş.</span><span class="sxs-lookup"><span data-stu-id="2e81a-176">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="2e81a-177">Ne zaman lambda ifadesi değerlendirilir (örneğin, `@Html.DisplayFor(modelItem => item.Title)`), modelin özellik değerleri değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="2e81a-177">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="2e81a-178">Uygulamayı Tarz, film adı ve her ikisi tarafından arama yaparak test edin:</span><span class="sxs-lookup"><span data-stu-id="2e81a-178">Test the app by searching by genre, by movie title, and by both:</span></span>

![Tarayıcı penceresini gösteren sonuçları https://localhost:5001/Movies?MovieGenre=Comedy&SearchString=2](~/tutorials/first-mvc-app/search/_static/s2.png)

> [!div class="step-by-step"]
> <span data-ttu-id="2e81a-180">[Önceki](controller-methods-views.md)
> [İleri](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="2e81a-180">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
