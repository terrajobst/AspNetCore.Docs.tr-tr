---
uid: mvc/overview/getting-started/introduction/adding-search
title: Arama | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 10457d154f5fda875f7d1054d48daeeba3a50b7c
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
<a name="search"></a><span data-ttu-id="80488-102">Ara</span><span class="sxs-lookup"><span data-stu-id="80488-102">Search</span></span>
====================
<span data-ttu-id="80488-103">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="80488-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="80488-104">Search yöntemini ve arama görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="80488-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="80488-105">Bu bölümde için arama yeteneğine ekleyeceksiniz `Index` olanak sağlayan eylem yöntemi arama filmler Tarz veya adı.</span><span class="sxs-lookup"><span data-stu-id="80488-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="80488-106">Dizin formun güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="80488-106">Updating the Index Form</span></span>

<span data-ttu-id="80488-107">Başlangıç güncelleştirerek `Index` varolan eylem yönteminin `MoviesController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="80488-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="80488-108">Kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="80488-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="80488-109">İlk satırı `Index` yöntemi, aşağıdaki oluşturur [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) filmler seçmek için sorgu:</span><span class="sxs-lookup"><span data-stu-id="80488-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="80488-110">Sorgu, bu noktada tanımlandı ancak veritabanına karşı henüz çalıştırılmadı.</span><span class="sxs-lookup"><span data-stu-id="80488-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="80488-111">Varsa `searchString` parametre içeren bir dize, filmler sorgu aşağıdaki kodu kullanarak, arama dizesi değerini filtre şekilde değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="80488-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="80488-112">`s => s.Title` Kodu yukarıdaki bir [Lambda ifadesi](https://msdn.microsoft.com/en-us/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="80488-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/en-us/library/bb397687.aspx).</span></span> <span data-ttu-id="80488-113">Lambda'lar yöntemi tabanlı içinde kullanılan [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) standart sorgu işleci yöntemlerinden bağımsız değişken olarak gibi sorgular [burada](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.where.aspx) Yukarıdaki kod içinde kullanılan yöntem.</span><span class="sxs-lookup"><span data-stu-id="80488-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="80488-114">LINQ sorgularını değil tanımlanmış olan veya ne zaman bir yöntemi çağrılarak değiştirildiğinde yürütülen `Where` veya `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="80488-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="80488-115">Bunun yerine, sorgu yürütme, gerçekleşen değeri gerçekte üzerinden yinelendiğinde kadar bir ifadenin değerlendirmesine Gecikmeli yani ertelenir veya [ `ToList` ](https://msdn.microsoft.com/en-us/library/bb342261.aspx) yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="80488-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/en-us/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="80488-116">İçinde `Search` örnek, sorgu yürütülürse *Index.cshtml* görünümü.</span><span class="sxs-lookup"><span data-stu-id="80488-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="80488-117">Ertelenmiş sorgu yürütme hakkında daha fazla bilgi için bkz: [sorgu yürütme](https://msdn.microsoft.com/en-us/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="80488-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/en-us/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="80488-118">[İçerir](https://msdn.microsoft.com/en-us/library/bb155125.aspx) yöntemi, veritabanı, c# kod değil yukarıdaki çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="80488-118">The [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="80488-119">Veritabanı üzerindeki [içerir](https://msdn.microsoft.com/en-us/library/bb155125.aspx) eşlendiği [SQL LIKE](https://msdn.microsoft.com/en-us/library/ms179859.aspx), büyük küçük harfe duyarlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="80488-119">On the database, [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/en-us/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="80488-120">Güncelleştirebilirsiniz şimdi `Index` form kullanıcıya görüntüleyecek görünümü.</span><span class="sxs-lookup"><span data-stu-id="80488-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="80488-121">Uygulamayı çalıştırın ve gidin */filmler/dizin*.</span><span class="sxs-lookup"><span data-stu-id="80488-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="80488-122">Bir sorgu dizesi gibi ekleme `?searchString=ghost` URL.</span><span class="sxs-lookup"><span data-stu-id="80488-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="80488-123">Filtrelenmiş filmler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="80488-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="80488-125">İmzası değiştirirseniz `Index` adlı bir parametreye sahip yöntemi `id`, `id` parametresi eşleşir `{id}` varsayılan için yer tutucu yönlendirir kümesinde *uygulama\_Start\ RouteConfig.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="80488-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="80488-126">Özgün `Index` yöntemi şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="80488-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="80488-127">Değiştirilen `Index` yöntemi uygulamamız görünecektir gibi:</span><span class="sxs-lookup"><span data-stu-id="80488-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="80488-128">Bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine olarak arama başlık şimdi geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="80488-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="80488-129">Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz.</span><span class="sxs-lookup"><span data-stu-id="80488-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="80488-130">Size yardımcı olmak için kullanıcı Arabirimi ekleyeceksiniz artık bunu filmler filtre.</span><span class="sxs-lookup"><span data-stu-id="80488-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="80488-131">İmzası değiştirdiyseniz `Index` rota bağlı ID parametresi geçirmek nasıl test etmek için yöntemini değiştirme geri böylece, `Index` yöntemi adlı bir dize parametresi alan `searchString`:</span><span class="sxs-lookup"><span data-stu-id="80488-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="80488-132">Açık *Views\Movies\Index.cshtml* dosya ve henüz sonra `@Html.ActionLink("Create New", "Create")`, aşağıda vurgulanmış form biçimlendirmeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="80488-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="80488-133">`Html.BeginForm` Yardımcı oluşturur açılış `<form>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="80488-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="80488-134">`Html.BeginForm` Yardımcı neden tıklayarak kullanıcı formu gönderdiğinde kendisine gönderme formun **filtre** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="80488-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="80488-135">Visual Studio 2013 görüntülerken ve görünüm dosyalarını düzenleme iyi bir geliştirme vardır.</span><span class="sxs-lookup"><span data-stu-id="80488-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="80488-136">Uygulama, bir görünüm dosyasıyla açık'e çalıştırdığınızda, Visual Studio 2013 görüntülemek için doğru denetleyici eylem yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="80488-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="80488-137">Dizin görünümünün (yukarıdaki resimde gösterildiği gibi) Visual Studio'da açıkken, uygulamayı çalıştırın ve ardından film aramayı deneyin için CTRL F5'e veya F5'e dokunun.</span><span class="sxs-lookup"><span data-stu-id="80488-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="80488-138">Var olan hiçbir `HttpPost` , aşırı `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="80488-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="80488-139">Yöntemi uygulama durumunu değiştirme değil çünkü bu, yalnızca verileri filtreleme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="80488-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="80488-140">Aşağıdaki ekleyebilirsiniz `HttpPost Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="80488-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="80488-141">Bu durumda, eylem Başlatıcısı eşleşir `HttpPost Index` yöntemi ve `HttpPost Index` yöntemi, aşağıdaki resimde gösterildiği gibi çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="80488-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="80488-143">Ancak, bu bile eklerseniz `HttpPost` sürümü `Index` yöntemini nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="80488-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="80488-144">Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklatabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün.</span><span class="sxs-lookup"><span data-stu-id="80488-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="80488-145">HTTP POST isteği için URL GET isteğini (localhost:xxxxx/filmler/dizin) için URL ile aynıdır--URL arama bilgisi yok dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="80488-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="80488-146">Sağ şimdi, arama dizesi bilgileri sunucuya bir form alanı değerini gönderilir.</span><span class="sxs-lookup"><span data-stu-id="80488-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="80488-147">Bu, yer işareti veya bir URL içinde arkadaş göndermek için arama bilgilerini yakalayamazsınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="80488-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="80488-148">Çözüm bir aşırı yüklemesini kullanmaktır `BeginForm` POST isteğini URL'ye arama bilgilerini eklemeniz gerekir ve bunun için yönlendirileceğini belirler `HttpGet` sürümü `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="80488-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="80488-149">Parametresiz varolan `BeginForm` aşağıdaki biçimlendirme yöntemiyle:</span><span class="sxs-lookup"><span data-stu-id="80488-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="80488-151">Şimdi bir arama gönderdiğinizde, URL bir arama sorgu dizesi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="80488-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="80488-152">Arama da gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="80488-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="80488-154">Türe göre arama ekleme</span><span class="sxs-lookup"><span data-stu-id="80488-154">Adding Search by Genre</span></span>

<span data-ttu-id="80488-155">Eklediyseniz `HttpPost` sürümü `Index` yöntemi, şimdi silin.</span><span class="sxs-lookup"><span data-stu-id="80488-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="80488-156">Ardından, filmler için türe göre arama kullanıcıların izin vermek için bir özellik ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="80488-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="80488-157">Değiştir `Index` aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="80488-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="80488-158">Bu sürümü `Index` yöntemi alır ek bir parametre öğesine `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="80488-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="80488-159">İlk birkaç satır kod oluşturma bir `List` film türler veritabanından tutacak nesne.</span><span class="sxs-lookup"><span data-stu-id="80488-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="80488-160">Tüm türler veritabanından alır bir LINQ Sorgu kodudur.</span><span class="sxs-lookup"><span data-stu-id="80488-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="80488-161">Kod kullanan `AddRange` genel yöntemini `List` farklı türler listesine eklemek için koleksiyon.</span><span class="sxs-lookup"><span data-stu-id="80488-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="80488-162">(Olmadan `Distinct` değiştiricisi, yinelenen türler eklenmesi — Örneğin, iki kez örneğimizde Komedi ekleneceği).</span><span class="sxs-lookup"><span data-stu-id="80488-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="80488-163">Kod içinde türler listesi sonra depolar `ViewBag.MovieGenre` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="80488-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="80488-164">Kategori verileri (böyle bir filmi Tarz 's) olarak depolamak bir [SelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlist(v=vs.108).aspx) nesnesinde bir `ViewBag`, MVC uygulamaları için tipik bir yaklaşım ise bir açılır liste kutusu kategori verilere erişme.</span><span class="sxs-lookup"><span data-stu-id="80488-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="80488-165">Aşağıdaki kod nasıl denetleneceğini gösterir `movieGenre` parametresi.</span><span class="sxs-lookup"><span data-stu-id="80488-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="80488-166">Boş değilse, daha fazla kod belirtilen Tarz seçili filmlere sınırlamak için filmler sorgu kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="80488-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="80488-167">Film listesi üzerinden yinelendiğinde kadar daha önce belirtildiği gibi sorgu veri temel çalıştırılmaz (hangi olur görünümünde sonra `Index` eylem yöntemine döndürür).</span><span class="sxs-lookup"><span data-stu-id="80488-167">As stated previously, the query is not run on the data base until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="80488-168">Türe göre arama destekleyecek şekilde dizin görünümünün biçimlendirme ekleme</span><span class="sxs-lookup"><span data-stu-id="80488-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="80488-169">Ekleme bir `Html.DropDownList` yardımcıya *Views\Movies\Index.cshtml* hemen önce dosya `TextBox` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="80488-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="80488-170">Tamamlanan biçimlendirme aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="80488-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="80488-171">Aşağıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="80488-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="80488-172">"MovieGenre" parametresi için anahtar sağlar `DropDownList` bulmaya yardımcı bir `IEnumerable<SelectListItem>` içinde `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="80488-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="80488-173">`ViewBag` Eylem yönteminde doldurulmuş:</span><span class="sxs-lookup"><span data-stu-id="80488-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="80488-174">Parametre "Tümü" bir seçenek etiketini sağlar.</span><span class="sxs-lookup"><span data-stu-id="80488-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="80488-175">Bu seçenek, tarayıcınızda inceleyin, kendi "value" özniteliği boş olduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="80488-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="80488-176">Bizim denetleyicisi filtreler yalnızca bu yana `if` dizesi değil `null` ya da boş bir değer için gönderme boş `movieGenre` tüm türler gösterir.</span><span class="sxs-lookup"><span data-stu-id="80488-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="80488-177">Varsayılan olarak seçili bir seçenek de ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="80488-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="80488-178">Varsayılan seçenek olarak "Komedi" istediyseniz, denetleyici kodda değişeceğinden sözlüğüdür:</span><span class="sxs-lookup"><span data-stu-id="80488-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="80488-179">Uygulamayı çalıştırın ve Gözat */filmler/dizin*.</span><span class="sxs-lookup"><span data-stu-id="80488-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="80488-180">Bir arama Tarz, film adı ve her iki ölçütlere göre deneyin.</span><span class="sxs-lookup"><span data-stu-id="80488-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="80488-181">Bu bölümde bir arama eylem yöntemi ve filmi ve türe göre arama, kullanıcıların izin görünüm oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="80488-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="80488-182">Sonraki bölümde, yeni özellik eklemek nasıl göreceğiz `Movie` modeli ve otomatik olarak bir test veritabanı oluşturacak bir başlatıcı ekleme.</span><span class="sxs-lookup"><span data-stu-id="80488-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="80488-183">[Önceki](examining-the-edit-methods-and-edit-view.md)
[sonraki](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="80488-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
