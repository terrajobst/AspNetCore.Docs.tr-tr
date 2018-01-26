---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Bir denetleyicisinden modelinizin verilere | Microsoft Docs
author: Rick-Anderson
description: "Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f323fe37da739d957a609dc7ca4e71a3c3ab475e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="ea2f1-104">Bir denetleyicisinden modelinizin verilerine erişme</span><span class="sxs-lookup"><span data-stu-id="ea2f1-104">Accessing Your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="ea2f1-105">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ea2f1-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="ea2f1-106">Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="ea2f1-107">Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="ea2f1-108">Bu bölümde, yeni oluşturacağınız `MoviesController` sınıfı ve film verileri alır ve bir görünüm şablonu kullanarak tarayıcıda görüntüleyen kod yazın.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-108">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="ea2f1-109">**Uygulamayı derleme** sonraki adıma geçmeden önce.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-109">**Build the application** before going on to the next step.</span></span>

<span data-ttu-id="ea2f1-110">Sağ *denetleyicileri* klasörü ve yeni bir `MoviesController` denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-110">Right-click the *Controllers* folder and create a new `MoviesController` controller.</span></span> <span data-ttu-id="ea2f1-111">Aşağıdaki seçenekler, uygulamanızı kadar görünmez.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-111">The options below will not appear until you build your application.</span></span> <span data-ttu-id="ea2f1-112">Aşağıdaki seçenekleri belirleyin:</span><span class="sxs-lookup"><span data-stu-id="ea2f1-112">Select the following options:</span></span>

- <span data-ttu-id="ea2f1-113">Denetleyici adı: **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-113">Controller name: **MoviesController**.</span></span> <span data-ttu-id="ea2f1-114">(Varsayılan değer budur.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-114">(This is the default.</span></span> <span data-ttu-id="ea2f1-115">)</span><span class="sxs-lookup"><span data-stu-id="ea2f1-115">)</span></span>
- <span data-ttu-id="ea2f1-116">Şablonu: **MVC denetleyici Entity Framework kullanarak okuma/yazma eylemleri ve görünümleri ile**.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-116">Template: **MVC Controller with read/write actions and views, using Entity Framework**.</span></span>
- <span data-ttu-id="ea2f1-117">Model sınıfı: **film (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-117">Model class: **Movie (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="ea2f1-118">Veri bağlamı sınıfı: **MovieDBContext (MvcMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-118">Data context class: **MovieDBContext (MvcMovie.Models)**.</span></span>
- <span data-ttu-id="ea2f1-119">Görünümleri: **Razor (CSHTML)**.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-119">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="ea2f1-120">(Varsayılan)</span><span class="sxs-lookup"><span data-stu-id="ea2f1-120">(The default.)</span></span>

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="ea2f1-122">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-122">Click **Add**.</span></span> <span data-ttu-id="ea2f1-123">Visual Studio Express, aşağıdaki dosyaları ve klasörleri oluşturur:</span><span class="sxs-lookup"><span data-stu-id="ea2f1-123">Visual Studio Express creates the following files and folders:</span></span>

- <span data-ttu-id="ea2f1-124">*Bir MoviesController.cs* projenin dosyasında *denetleyicileri* klasör.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-124">*A MoviesController.cs* file in the project's *Controllers* folder.</span></span>
- <span data-ttu-id="ea2f1-125">A *filmler* projenin klasöründe *görünümleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-125">A *Movies* folder in the project's *Views* folder.</span></span>
- <span data-ttu-id="ea2f1-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, ve *Index.cshtml* yeni *Views\Movies* klasör.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-126">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="ea2f1-127">ASP.NET MVC 4 otomatik olarak oluşturulan CRUD (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve sizin için görünümleri (CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını yapı iskelesi olarak bilinir).</span><span class="sxs-lookup"><span data-stu-id="ea2f1-127">ASP.NET MVC 4 automatically created the CRUD (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="ea2f1-128">Artık oluşturma listesinde, düzenleme ve film girişleri silmek olanak sağlayan bir tam olarak işlevsel bir web uygulaması sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-128">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="ea2f1-129">Uygulamayı çalıştırın ve Gözat `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL'ye.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-129">Run the application and browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser.</span></span> <span data-ttu-id="ea2f1-130">Varsayılan yönlendirme uygulama bağlı olduğundan (tanımlanan *Global.asax* dosyası), tarayıcı isteğini `http://localhost:xxxxx/Movies` varsayılan yönlendirilen `Index` eylem yöntemi `Movies` denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-130">Because the application is relying on the default routing (defined in the *Global.asax* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="ea2f1-131">Diğer bir deyişle, tarayıcı isteğini `http://localhost:xxxxx/Movies` tarayıcı isteğini etkili bir şekilde aynıdır `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-131">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="ea2f1-132">Sonuç boş bir liste film, çünkü henüz eklemediniz.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-132">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a><span data-ttu-id="ea2f1-133">Bir filmi oluşturma</span><span class="sxs-lookup"><span data-stu-id="ea2f1-133">Creating a Movie</span></span>

<span data-ttu-id="ea2f1-134">Seçin **Yeni Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-134">Select the **Create New** link.</span></span> <span data-ttu-id="ea2f1-135">Bir filmi bazı ayrıntılarını girin ve ardından **oluşturma** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-135">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image3.png)

<span data-ttu-id="ea2f1-136">Tıklatarak **oluşturma** düğmesi sunucuya film bilgileri veritabanında kaydedildiği postalama forma neden olur.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-136">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="ea2f1-137">Ardından için yönlendirilirsiniz */Movies* burada görebilirsiniz listesinde yeni oluşturulan film URL.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-137">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

<span data-ttu-id="ea2f1-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span><span class="sxs-lookup"><span data-stu-id="ea2f1-138">![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")</span></span>

<span data-ttu-id="ea2f1-139">Daha fazla birkaç film girişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-139">Create a couple more movie entries.</span></span> <span data-ttu-id="ea2f1-140">Deneyin **Düzenle**, **ayrıntıları**, ve **silmek** tüm işlev bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-140">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="ea2f1-141">Oluşturulan kod İnceleme</span><span class="sxs-lookup"><span data-stu-id="ea2f1-141">Examining the Generated Code</span></span>

<span data-ttu-id="ea2f1-142">Açık *Controllers\MoviesController.cs* dosya ve oluşturulan inceleyin `Index` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-142">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="ea2f1-143">Film denetleyicisiyle bir kısmı `Index` yöntemi aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-143">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="ea2f1-144">Aşağıdaki satır `MoviesController` sınıfı, daha önce açıklandığı gibi bir filmi veritabanı bağlamını başlatır.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-144">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="ea2f1-145">Film veritabanı bağlamı, sorgu, düzenlemek ve filmler silmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-145">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

<span data-ttu-id="ea2f1-146">Bir istek `Movies` denetleyicisi döndürür tüm girişler `Movies` film veritabanı tablosunun ve sonuçları geçirir `Index` görünümü.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-146">A request to the `Movies` controller returns all the entries in the `Movies` table of the movie database and then passes the results to the `Index` view.</span></span>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="ea2f1-147">Modelleri'kesin türü belirtilmiş ve @model anahtar sözcüğü</span><span class="sxs-lookup"><span data-stu-id="ea2f1-147">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="ea2f1-148">Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneler görünümü kullanarak şablonu geçirebilirsiniz gördüğünüzü `ViewBag` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-148">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="ea2f1-149">`ViewBag` Bir görünüme bilgi geçirmek için kullanışlı bir geç bağlama yol sağlayan dinamik bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-149">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="ea2f1-150">ASP.NET MVC, veri ya da bir görünüm şablonu nesnelere kesinlikle geçirme özelliği belirtilmiş de sağlar.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-150">ASP.NET MVC also provides the ability to pass strongly typed data or objects to a view template.</span></span> <span data-ttu-id="ea2f1-151">Bu yaklaşım etkinleştirir daha iyi derleme kodu ve Visual Studio düzenleyicisinde daha zengin IntelliSense denetleme zamanı kesin türü belirtilmiş.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-151">This strongly typed approach enables better compile-time checking of your code and richer IntelliSense in the Visual Studio editor.</span></span> <span data-ttu-id="ea2f1-152">Bu yaklaşımda Visual Studio yapı iskelesi yönteminde kullanılan `MoviesController` yöntemleri ve görünümler oluşturduğunuzda sınıfı ve görünüm şablonları.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-152">The scaffolding mechanism in Visual Studio used this approach with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="ea2f1-153">İçinde *Controllers\MoviesController.cs* dosyasını inceleyin oluşturulan `Details` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-153">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="ea2f1-154">Film denetleyicisiyle bir kısmı `Details` yöntemi aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-154">A portion of the movie controller with the `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

<span data-ttu-id="ea2f1-155">Varsa bir `Movie` bulunursa, bir örneğini `Movie` modeli Ayrıntılar görünümüne geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-155">If a `Movie` is found, an instance of the `Movie` model is passed to the Details view.</span></span> <span data-ttu-id="ea2f1-156">İçeriğini incelemek *Views\Movies\Details.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-156">Examine the contents of the *Views\Movies\Details.cshtml* file.</span></span>

<span data-ttu-id="ea2f1-157">Ekleyerek bir `@model` deyimini dosyanın üst kısmındaki görünüm şablonu görünümü bekliyor nesne türünü belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-157">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="ea2f1-158">Film denetleyicisini oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Details.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ea2f1-158">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

<span data-ttu-id="ea2f1-159">Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-159">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="ea2f1-160">Örneğin, *Details.cshtml* şablonu, kodu her film alanına geçirir `DisplayNameFor` ve [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Yardımcıları ile güçlü şekilde yazılan `Model` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-160">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="ea2f1-161">Ayrıca oluşturma ve düzenleme yöntemleri ve görünüm şablonları film model nesnesi geçirin.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-161">The Create and Edit methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="ea2f1-162">İncelemek *Index.cshtml* şablonu görüntüleme ve `Index` yönteminde *MoviesController.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-162">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="ea2f1-163">Kodu nasıl oluşturduğunu fark bir [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) nesne çağırdığında `View` yardımcı yönteminin `Index` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-163">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="ea2f1-164">Kodu daha sonra bu geçirir `Movies` denetleyicisinden listesini görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="ea2f1-164">The code then passes this `Movies` list from the controller to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

<span data-ttu-id="ea2f1-165">Film denetleyicisini oluşturduğunuzda, Visual Studio Express otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ea2f1-165">When you created the movie controller, Visual Studio Express automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="ea2f1-166">Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen filmler listesi erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-166">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="ea2f1-167">Örneğin, *Index.cshtml* şablon kodu döngüler filmler yaparak bir `foreach` kesin türü belirtilmiş üzerinden deyimi `Model` nesnesi:</span><span class="sxs-lookup"><span data-stu-id="ea2f1-167">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="ea2f1-168">Çünkü `Model` nesne kesin türü belirtilmiş (olarak bir `IEnumerable<Movie>` nesnesi), her `item` döngüsünde nesne türü olarak `Movie`.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-168">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="ea2f1-169">Diğer avantajlar arasında bu kod derleme zamanı denetimi Al ve kod düzenleyicisinde IntelliSense desteği tam anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="ea2f1-169">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="ea2f1-171">SQL Server yerel veritabanı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="ea2f1-171">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="ea2f1-172">Entity Framework Code First algılanan sağlanan veritabanı bağlantı dizesi için işaret bir `Movies` Code First veritabanı otomatik olarak oluşturulan, henüz yoksa veritabanı.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-172">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="ea2f1-173">Bakarak oluşturulduktan olmadığını doğrulayabilirsiniz *uygulama\_veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-173">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="ea2f1-174">Görmüyorsanız, *Movies.mdf* dosya, tıklatın **tüm dosyaları göster** düğmesini **Çözüm Gezgini** araç, tıklatın **yenileme** düğmesine tıklayın ve ardından *uygulama\_veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-174">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="ea2f1-175">Çift tıklatın *Movies.mdf* açmak için **DATABASE EXPLORER**,'ni genişletin **tabloları** klasörü filmler tabloya bakın.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-175">Double-click *Movies.mdf* to open **DATABASE EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span>

<span data-ttu-id="ea2f1-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="ea2f1-176">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")</span></span>

> [!NOTE]
> <span data-ttu-id="ea2f1-177">Veritabanı explorer görünmüyorsa, gelen **Araçları** menüsünde, select **veritabanına bağlan**, sonra iptal **veri kaynağı Seç** iletişim.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-177">If the database explorer doesn't appear, from the **TOOLS** menu, select **Connect to Database**, then cancel the **Choose Data Source** dialog.</span></span> <span data-ttu-id="ea2f1-178">Bu veritabanı explorer açık zorlar.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-178">This will force open the database explorer.</span></span>


> [!NOTE]
> <span data-ttu-id="ea2f1-179">VWD veya Visual Studio 2010 kullanarak ve aşağıdaki aşağıdakilerden herhangi birini benzer bir hata iletisi varsa:</span><span class="sxs-lookup"><span data-stu-id="ea2f1-179">If you are using VWD or Visual Studio 2010 and get an error similar to any of the following following:</span></span>
> 
> - <span data-ttu-id="ea2f1-180">Veritabanı ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' sürümü 706 olduğundan açılamıyor.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-180">The database 'C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES.MDF' cannot be opened because it is version 706.</span></span> <span data-ttu-id="ea2f1-181">Bu sunucunun sürümü 655 ve önceki sürümleri destekler.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-181">This server supports version 655 and earlier.</span></span> <span data-ttu-id="ea2f1-182">İndirgeme yolu desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-182">A downgrade path is not supported.</span></span>
> - <span data-ttu-id="ea2f1-183">&quot;InvalidOperation özel durum, kullanıcı kodu tarafından işlenmemiş&quot; sağlanan SqlConnection bir ilk katalog belirtmiyor.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-183">&quot;InvalidOperation Exception was unhandled by user code&quot; The supplied SqlConnection does not specify an initial catalog.</span></span>
> 
> <span data-ttu-id="ea2f1-184">Yüklemenize gerek [SQL Server veri Araçları](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) ve [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span><span class="sxs-lookup"><span data-stu-id="ea2f1-184">You need to install the [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) and [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0).</span></span> <span data-ttu-id="ea2f1-185">Doğrulama `MovieDBContext` önceki sayfada belirtilen bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-185">Verify the `MovieDBContext` connection string specified on the previous page.</span></span>


<span data-ttu-id="ea2f1-186">Sağ `Movies` tablo ve seçin **Show Table Data** oluşturduğunuz verileri görmek için.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-186">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image8.png)

<span data-ttu-id="ea2f1-187">Sağ `Movies` tablo ve seçin **açık tablo tanımı** bu Entity Framework kod sizin için oluşturduğu ilk yapısı tablosunu görmek için.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-187">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

<span data-ttu-id="ea2f1-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span><span class="sxs-lookup"><span data-stu-id="ea2f1-188">![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")</span></span>

![](accessing-your-models-data-from-a-controller/_static/image10.png)

<span data-ttu-id="ea2f1-189">Bildirim nasıl şeması `Movies` tablo eşlenir `Movie` daha önce oluşturduğunuz sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-189">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="ea2f1-190">Entity Framework Code First otomatik olarak oluşturulan bu şemayı dayanarak için `Movie` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-190">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="ea2f1-191">İşlemi tamamladığınızda, bağlantıyı sağ tıklayarak kapatın *MovieDBContext* ve seçerek **kapatmak bağlantı**.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-191">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="ea2f1-192">(Bağlantı kapatmayın, projenin bir sonraki çalıştırmanızda bir hata alabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="ea2f1-192">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

<span data-ttu-id="ea2f1-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span><span class="sxs-lookup"><span data-stu-id="ea2f1-193">![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")</span></span>

<span data-ttu-id="ea2f1-194">Artık bu içeriği görüntülemek için bir basit liste sayfası ve veritabanı vardır.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-194">You now have the database and a simple listing page to display content from it.</span></span> <span data-ttu-id="ea2f1-195">Sonraki öğreticide biz kurulmuş kodu kalan inceleyin ve ekleme bir `SearchIndex` yöntemi ve `SearchIndex` bu veritabanında filmler arama yapmanıza olanak tanıyan görünümü.</span><span class="sxs-lookup"><span data-stu-id="ea2f1-195">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ea2f1-196">[Önceki](adding-a-model.md)
[sonraki](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="ea2f1-196">[Previous](adding-a-model.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
