---
uid: mvc/overview/getting-started/introduction/adding-search
title: Arama | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 31fd35ac63f3eb31d824e1710833ad83a0852ac9
ms.sourcegitcommit: a91e8dd2f4b788114c8bc834507277f4b5e8d6c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712269"
---
<a name="search"></a><span data-ttu-id="98dd6-102">Ara</span><span class="sxs-lookup"><span data-stu-id="98dd6-102">Search</span></span>
====================
<span data-ttu-id="98dd6-103">Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="98dd6-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="98dd6-104">Bir arama yöntemi ve arama görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="98dd6-104">Adding a Search Method and Search View</span></span>

<span data-ttu-id="98dd6-105">Bu bölümde, arama özelliği ekleyeceksiniz `Index` olanak sağlayan bir eylem yöntemi filmler Tarz veya adı arayın.</span><span class="sxs-lookup"><span data-stu-id="98dd6-105">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="updating-the-index-form"></a><span data-ttu-id="98dd6-106">Dizin formun güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="98dd6-106">Updating the Index Form</span></span>

<span data-ttu-id="98dd6-107">Başlangıç güncelleştirerek `Index` mevcut eylem yöntemine `MoviesController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="98dd6-107">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="98dd6-108">Kod aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="98dd6-108">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="98dd6-109">İlk satırı `Index` yöntemi oluşturur aşağıdaki [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) filmler seçmek için sorgu:</span><span class="sxs-lookup"><span data-stu-id="98dd6-109">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="98dd6-110">Sorgu, bu noktada tanımlanır ancak veritabanında henüz çalıştırılmadı.</span><span class="sxs-lookup"><span data-stu-id="98dd6-110">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="98dd6-111">Varsa `searchString` parametresi içeren bir dize, filmler sorgu aşağıdaki kodu kullanarak, bir arama dizesi değerini temel filtrelemek için değiştirilir:</span><span class="sxs-lookup"><span data-stu-id="98dd6-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="98dd6-112">`s => s.Title` Kodu yukarıdaki bir [Lambda ifadesi](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="98dd6-112">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="98dd6-113">Lambda ifadeleri yöntem tabanlı kullanılan [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) gibi sorgularında standart sorgu işleci yöntemlerinin bağımsız değişkenleri olarak [burada](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) Yukarıdaki kod içinde kullanılan yöntem.</span><span class="sxs-lookup"><span data-stu-id="98dd6-113">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="98dd6-114">LINQ sorguları tanımlandıkları veya bunlar bir yöntemi çağırarak değiştirildiğinde yürütülmez `Where` veya `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="98dd6-114">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="98dd6-115">Bunun yerine, sorgu yürütmesi, üzerinden gerçekleştirilen değeri gerçekten yinelendiğinde kadar bir ifade değerlendirmesi ertelendi anlamına gelir ertelenmiştir veya [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="98dd6-115">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="98dd6-116">İçinde `Search` örnek, sorgu yürütülürse *Index.cshtml* görünümü.</span><span class="sxs-lookup"><span data-stu-id="98dd6-116">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="98dd6-117">Ertelenen sorgu yürütme hakkında daha fazla bilgi için bkz. [sorgu yürütme](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="98dd6-117">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="98dd6-118">[İçerir](https://msdn.microsoft.com/library/bb155125.aspx) yöntemi, veritabanı, c# kodunda değil yukarıdaki çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="98dd6-118">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="98dd6-119">Veritabanında [içerir](https://msdn.microsoft.com/library/bb155125.aspx) eşlendiği [SQL gibi](https://msdn.microsoft.com/library/ms179859.aspx), büyük küçük harfe duyarlı olduğu.</span><span class="sxs-lookup"><span data-stu-id="98dd6-119">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="98dd6-120">Şimdi güncelleştirebilirsiniz `Index` kullanıcıya form görüntüleyecek görünümü.</span><span class="sxs-lookup"><span data-stu-id="98dd6-120">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="98dd6-121">Uygulamayı çalıştırmak ve gidin */filmler/dizin*.</span><span class="sxs-lookup"><span data-stu-id="98dd6-121">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="98dd6-122">Bir sorgu dizesi gibi ekleme `?searchString=ghost` URL'si.</span><span class="sxs-lookup"><span data-stu-id="98dd6-122">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="98dd6-123">Filtrelenmiş filmler görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="98dd6-123">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="98dd6-125">İmzası değiştirirseniz `Index` adlı bir parametreye sahip yöntemi `id`, `id` parametresi eşleşir `{id}` varsayılan için yer tutucu yönlendiren kümesinde *uygulama\_Start\ RouteConfig.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="98dd6-125">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.json)]

<span data-ttu-id="98dd6-126">Özgün `Index` yöntemi aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="98dd6-126">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="98dd6-127">Değiştirilmiş `Index` yöntemi şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="98dd6-127">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="98dd6-128">Arama başlığı olarak bir sorgu dizesi değeri olarak rota verilerini (URL kesimi) yerine artık geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="98dd6-128">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="98dd6-129">Ancak, bunlar bir filmi için arama yapmak istediğiniz her zaman URL'sini değiştirmek için kullanıcıların beklediğiniz olamaz.</span><span class="sxs-lookup"><span data-stu-id="98dd6-129">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="98dd6-130">Artık, yardımcı olmak için kullanıcı Arabirimi ekleyeceksiniz filmler filtreleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="98dd6-130">So now you you'll add UI to help them filter movies.</span></span> <span data-ttu-id="98dd6-131">İmzası değiştirdiyseniz `Index` metodu rota bağlı ID parametresine geçirilecek nasıl test etmek için değiştirme geri böylece, `Index` yöntemi adlı bir dize parametresi alır `searchString`:</span><span class="sxs-lookup"><span data-stu-id="98dd6-131">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="98dd6-132">Açık *Views\Movies\Index.cshtml* dosyasını açın ve sonra yalnızca `@Html.ActionLink("Create New", "Create")`, aşağıda Vurgulanan form biçimlendirme ekleyin:</span><span class="sxs-lookup"><span data-stu-id="98dd6-132">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="98dd6-133">`Html.BeginForm` Yardımcısı oluşturur açılış `<form>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="98dd6-133">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="98dd6-134">`Html.BeginForm` Yardımcısı neden tıklayarak kullanıcı formu gönderdiğinde, kendisine göndermek form **filtre** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="98dd6-134">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="98dd6-135">Visual Studio 2013, görüntüleme ve düzenleme görünümü dosyaları iyi bir gelişme vardır.</span><span class="sxs-lookup"><span data-stu-id="98dd6-135">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="98dd6-136">Bir görünümü dosyasıyla uygulamayı açık çalıştırdığınızda Visual Studio 2013 görüntülemek için doğru denetleyici eylem yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="98dd6-136">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="98dd6-137">Dizin görünümünün (yukarıdaki görüntüde gösterildiği gibi), Visual Studio'da açın, uygulamayı çalıştırın ve ardından bir filmi için aramayı deneyin için CTRL-F5 veya F5'e dokunun.</span><span class="sxs-lookup"><span data-stu-id="98dd6-137">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="98dd6-138">Var olan hiçbir `HttpPost` aşırı yükünü `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="98dd6-138">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="98dd6-139">Yöntemi uygulama durumunu değiştirme değildir çünkü, yalnızca verileri filtreleme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="98dd6-139">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="98dd6-140">Aşağıdaki ekleyebilirsiniz `HttpPost Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="98dd6-140">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="98dd6-141">Bu durumda, eylem çağırıcısını BC `HttpPost Index` yöntemi ve `HttpPost Index` yöntemi aşağıdaki resimde gösterildiği gibi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="98dd6-141">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="98dd6-143">Ancak, bu eklediğiniz bile `HttpPost` sürümünü `Index` yöntemi nasıl bu tüm uygulanmıştır içinde bir sınırlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="98dd6-143">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="98dd6-144">Belirli bir arama yer işareti koymak istediğiniz veya bunlar filmler aynı filtrelenmiş listesini görmek için tıklayabilirsiniz arkadaşlarınıza bağlantı göndermek istediğiniz düşünün.</span><span class="sxs-lookup"><span data-stu-id="98dd6-144">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="98dd6-145">HTTP POST isteği URL'sini GET isteği (localhost:xxxxx filmler/dizin) URL'si ile aynıdır; URL içinde hiçbir arama bilgisi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="98dd6-145">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="98dd6-146">Sağ artık, arama dizesi bilgilerini sunucuya biçiminde alan değeri gönderilir.</span><span class="sxs-lookup"><span data-stu-id="98dd6-146">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="98dd6-147">Bu, yer işareti veya bir URL içinde arkadaş göndermek için bu arama bilgileri yakalayamazsınız anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="98dd6-147">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="98dd6-148">Çözüm bir aşırı yüklemesini kullanmaktır `BeginForm` POST isteği URL'sini arama bilgilerini eklemeniz gerekir ve bunun için yönlendirileceğini belirtir `HttpGet` sürümünü `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="98dd6-148">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="98dd6-149">Parametresiz varolan `BeginForm` aşağıdaki işaretlemeyle yöntemi:</span><span class="sxs-lookup"><span data-stu-id="98dd6-149">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="98dd6-151">Artık bir arama gönderdiğinizde, URL'yi bir arama sorgu dizesi içerir.</span><span class="sxs-lookup"><span data-stu-id="98dd6-151">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="98dd6-152">Arama de gider için `HttpGet Index` eylem yöntemi, varsa bile bir `HttpPost Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="98dd6-152">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="98dd6-154">Türe göre arama ekleme</span><span class="sxs-lookup"><span data-stu-id="98dd6-154">Adding Search by Genre</span></span>

<span data-ttu-id="98dd6-155">Eklediyseniz `HttpPost` sürümünü `Index` yöntemi, şimdi silin.</span><span class="sxs-lookup"><span data-stu-id="98dd6-155">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="98dd6-156">Ardından, kullanıcıların filmler için türe göre arama bir özellik ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="98dd6-156">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="98dd6-157">Değiştirin `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="98dd6-157">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="98dd6-158">Bu sürümü `Index` yöntemi alır ek bir parametre, yani `movieGenre`.</span><span class="sxs-lookup"><span data-stu-id="98dd6-158">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="98dd6-159">İlk birkaç kod satırlarının bir `List` film türleri veritabanından tutacak nesne.</span><span class="sxs-lookup"><span data-stu-id="98dd6-159">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="98dd6-160">Tüm türleri veritabanından alır bir LINQ Sorgu kodudur.</span><span class="sxs-lookup"><span data-stu-id="98dd6-160">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="98dd6-161">Kod `AddRange` genel yöntemini `List` farklı türleri listesine eklemek için koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="98dd6-161">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="98dd6-162">(Olmadan `Distinct` değiştiricisi, yinelenen tür eklenecek — Örneğin, iki kez örneğimizi Komedi eklenir).</span><span class="sxs-lookup"><span data-stu-id="98dd6-162">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="98dd6-163">Kod içinde türleri listesini ardından depolar `ViewBag.MovieGenre` nesne.</span><span class="sxs-lookup"><span data-stu-id="98dd6-163">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="98dd6-164">Kategori veri (böyle bir film türe ait) olarak depolama bir [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) nesnesine bir `ViewBag`, MVC uygulamaları için tipik bir yaklaşım ise bir açılan liste kutusunda kategori verilere erişme.</span><span class="sxs-lookup"><span data-stu-id="98dd6-164">Storing category data (such a movie genre's) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="98dd6-165">Aşağıdaki kod nasıl kontrol edileceğini göstermektedir `movieGenre` parametresi.</span><span class="sxs-lookup"><span data-stu-id="98dd6-165">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="98dd6-166">Boş değilse, belirtilen türe seçili film sınırlamak için filmler sorgu kodu daha ayrıntılı kısıtlar.</span><span class="sxs-lookup"><span data-stu-id="98dd6-166">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="98dd6-167">Film listesi üzerinden yinelenir kadar daha önce bahsedildiği gibi sorgu veritabanında çalıştırılmaz (hangi olur Görünümü'nde sonra `Index` eylem yöntemine döndürür).</span><span class="sxs-lookup"><span data-stu-id="98dd6-167">As stated previously, the query is not run on the database until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="98dd6-168">Biçimlendirme türe göre ara desteklemek için dizini görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="98dd6-168">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="98dd6-169">Ekleme bir `Html.DropDownList` yardımcıya *Views\Movies\Index.cshtml* hemen önce dosya `TextBox` Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="98dd6-169">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="98dd6-170">Tamamlanan biçimlendirme aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="98dd6-170">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="98dd6-171">Aşağıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="98dd6-171">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="98dd6-172">"MovieGenre" parametresi için anahtar sağlar `DropDownList` bulmaya yardımcı bir `IEnumerable<SelectListItem>` içinde `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="98dd6-172">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="98dd6-173">`ViewBag` Eylem yönteminde doldurulup doldurulmadığına:</span><span class="sxs-lookup"><span data-stu-id="98dd6-173">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="98dd6-174">Parametre bir seçenek etiketini "Tüm" sağlar.</span><span class="sxs-lookup"><span data-stu-id="98dd6-174">The parameter "All" provides an option label.</span></span> <span data-ttu-id="98dd6-175">Bu seçenek, tarayıcınızda inceleyin, kendi "value" özniteliği boş olduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="98dd6-175">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="98dd6-176">Denetleyici yalnızca filtreler bu yana `if` dize değil `null` veya göndermek için boş değer boş, `movieGenre` tüm türleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="98dd6-176">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="98dd6-177">Varsayılan olarak seçilecek bir seçenek de ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="98dd6-177">You can also set an option to be selected by default.</span></span> <span data-ttu-id="98dd6-178">Varsayılan seçenek olarak "Komedi" istediyseniz, denetleyici kodda değiştirirsiniz şu şekilde:</span><span class="sxs-lookup"><span data-stu-id="98dd6-178">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="98dd6-179">Uygulamayı çalıştırmak ve göz atın */filmler/dizin*.</span><span class="sxs-lookup"><span data-stu-id="98dd6-179">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="98dd6-180">Türe, film adı ve her iki ölçütlere göre bir arama deneyin.</span><span class="sxs-lookup"><span data-stu-id="98dd6-180">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="98dd6-181">Bu bölümde bir arama eylem yöntemi ve kullanıcıların filmi Tarz arama görünüm oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="98dd6-181">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="98dd6-182">Sonraki bölümde, bir özelliğin ekleneceği nasıl şu konuları inceleyeceğiz `Movie` modeli ve bir test veritabanı otomatik olarak oluşturacak bir başlatıcı ekleme.</span><span class="sxs-lookup"><span data-stu-id="98dd6-182">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="98dd6-183">[Önceki](examining-the-edit-methods-and-edit-view.md)
> [İleri](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="98dd6-183">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
