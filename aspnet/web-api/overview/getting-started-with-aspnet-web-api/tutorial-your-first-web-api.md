---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: "ASP.NET Web API 2 (C#) ile çalışmaya başlama"
author: MikeWasson
description: "HTTP web sayfaları yalnızca hizmet vermek için değil. Hizmetler ve veri kullanıma API'ları oluşturmak için de güçlü bir platformdur. HTTP basit, esnek ve ubiq ise..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 6ff9fd279a03197f761454bba3f180d7428b1b1f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="2c617-105">ASP.NET Web API 2 (C#) ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="2c617-105">Getting Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="2c617-106">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2c617-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2c617-107">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="2c617-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="2c617-108">HTTP web sayfaları yalnızca hizmet vermek için değil.</span><span class="sxs-lookup"><span data-stu-id="2c617-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="2c617-109">Ayrıca Hizmetleri ve veriler kullanıma API'ları oluşturmak için güçlü bir platform HTTP'dir.</span><span class="sxs-lookup"><span data-stu-id="2c617-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="2c617-110">Basit, esnek ve her yerden HTTP'dir.</span><span class="sxs-lookup"><span data-stu-id="2c617-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="2c617-111">HTTP Hizmetleri çok çeşitli istemciler, tarayıcılar, mobil cihazlar ve geleneksel masaüstü uygulamaları gibi ulaşabilmeniz için bir HTTP kitaplığı düşünebilirsiniz neredeyse herhangi bir platform sahiptir.</span><span class="sxs-lookup"><span data-stu-id="2c617-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>
 
<span data-ttu-id="2c617-112">ASP.NET Web API, web API'leri .NET Framework üzerine oluşturmaya yönelik bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="2c617-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="2c617-113">Bu öğreticide, bir web ürünlerin listesini döndürür API oluşturmak için ASP.NET Web API kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c617-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

 
 ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2c617-114">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="2c617-114">Software versions used in the tutorial</span></span>
  
 - [<span data-ttu-id="2c617-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2c617-115">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)
 - <span data-ttu-id="2c617-116">Web API 2</span><span class="sxs-lookup"><span data-stu-id="2c617-116">Web API 2</span></span>

<span data-ttu-id="2c617-117">Bkz: [ASP.NET Core ve Windows için Visual Studio ile web API oluşturma](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) Bu öğreticide daha yeni bir sürümü için.</span><span class="sxs-lookup"><span data-stu-id="2c617-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="2c617-118">Bir Web API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="2c617-118">Create a Web API Project</span></span>

<span data-ttu-id="2c617-119">Bu öğreticide, bir web ürünlerin listesini döndürür API oluşturmak için ASP.NET Web API kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c617-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="2c617-120">Ön uç web sayfası jQuery sonuçları görüntülemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c617-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="2c617-121">Visual Studio'yu başlatın ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="2c617-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="2c617-122">Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="2c617-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2c617-123">İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="2c617-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2c617-124">Altında **Visual C#**seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="2c617-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="2c617-125">Proje şablonları listesinde seçin **ASP.NET Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="2c617-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="2c617-126">"ProductsApp" adını verin ve projeyi tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="2c617-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="2c617-127">İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.</span><span class="sxs-lookup"><span data-stu-id="2c617-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="2c617-128">Altında &quot;klasörler ekleme ve çekirdek için başvurular&quot;, denetleme **Web API**.</span><span class="sxs-lookup"><span data-stu-id="2c617-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="2c617-129">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2c617-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="2c617-130">Kullanarak bir Web API projesi oluşturabilirsiniz &quot;Web API&quot; şablonu.</span><span class="sxs-lookup"><span data-stu-id="2c617-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="2c617-131">Web API şablon ASP.NET MVC API yardım sayfalarına sağlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="2c617-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="2c617-132">MVC olmadan Web API göstermek istediklerinden Bu öğretici için boş şablonu kullanıyorum.</span><span class="sxs-lookup"><span data-stu-id="2c617-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="2c617-133">Genel olarak, ASP.NET MVC Web API kullanılacağını bilmeniz gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="2c617-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="2c617-134">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="2c617-134">Adding a Model</span></span>

<span data-ttu-id="2c617-135">A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="2c617-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="2c617-136">ASP.NET Web API otomatik olarak modelinize JSON, XML veya başka bir biçime serileştirir ve ardından HTTP yanıt iletisinin gövdesine seri hale getirilmiş veri yazar.</span><span class="sxs-lookup"><span data-stu-id="2c617-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="2c617-137">Bir istemci seri hale getirme biçimi okuyabilirler sürece, nesneyi seri durumdan.</span><span class="sxs-lookup"><span data-stu-id="2c617-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="2c617-138">Çoğu istemcileri, XML veya JSON ayrıştıramıyor.</span><span class="sxs-lookup"><span data-stu-id="2c617-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="2c617-139">Ayrıca, istemci HTTP istek iletisinde Accept üstbilgisi ayarlayarak istediği hangi biçimini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c617-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="2c617-140">Bir ürün temsil eden basit bir modeli oluşturarak başlayalım.</span><span class="sxs-lookup"><span data-stu-id="2c617-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="2c617-141">Çözüm Gezgini görünür değilse, **Görünüm** menü ve select **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="2c617-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="2c617-142">Çözüm Gezgini'nde modeller klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2c617-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="2c617-143">Bağlam menüsünden seçin **Ekle** seçip **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="2c617-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="2c617-144">Sınıf adını &quot;ürün&quot;.</span><span class="sxs-lookup"><span data-stu-id="2c617-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="2c617-145">Aşağıdaki özellikleri ekleyin `Product` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2c617-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="2c617-146">Bir denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="2c617-146">Adding a Controller</span></span>

<span data-ttu-id="2c617-147">Web API içinde bir *denetleyicisi* HTTP isteklerini işleyen bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="2c617-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="2c617-148">Ürünlerin listesini ya da kimliğine göre belirtilen tek ürün döndürebilir denetleyicisi ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="2c617-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="2c617-149">ASP.NET MVC kullandıysanız, denetleyicileriyle bilginiz.</span><span class="sxs-lookup"><span data-stu-id="2c617-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="2c617-150">Web API denetleyicilerinin MVC denetleyicileri için benzerdir, ancak devral **ApiController** sınıfına **denetleyicisi** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2c617-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="2c617-151">İçinde **Çözüm Gezgini**, denetleyicileri klasörünü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2c617-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="2c617-152">Seçin **Ekle** ve ardından **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="2c617-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="2c617-153">İçinde **İskele Ekle** iletişim kutusunda **Web API denetleyici - boş**.</span><span class="sxs-lookup"><span data-stu-id="2c617-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="2c617-154">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2c617-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="2c617-155">İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="2c617-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="2c617-156">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2c617-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="2c617-157">Yapı iskelesi denetleyicileri klasöründe ProductsController.cs adlı bir dosya oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2c617-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="2c617-158">Denetleyicilerinizi denetleyicileri adlı bir klasöre yerleştirin gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="2c617-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="2c617-159">Klasör adı, kaynak dosyaları düzenlemek için yalnızca bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="2c617-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="2c617-160">Bu dosya zaten açık değilse, açmak için dosyaya çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2c617-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="2c617-161">Bu dosyadaki kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="2c617-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="2c617-162">Örneği basit tutmak için ürünleri sabit bir dizi denetleyici sınıfı içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="2c617-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="2c617-163">Elbette, gerçek bir uygulamada bunu bir veritabanını sorgulamak veya başka bir dış veri kaynağını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c617-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="2c617-164">Denetleyici ürünleri dönüş iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2c617-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="2c617-165">`GetAllProducts` Yöntemi ürünlerinin tam listesini döndürür bir **IEnumerable&lt;ürün&gt;**  türü.</span><span class="sxs-lookup"><span data-stu-id="2c617-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="2c617-166">`GetProduct` Tek bir ürün, kimliğe göre arar yöntemi</span><span class="sxs-lookup"><span data-stu-id="2c617-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="2c617-167">İşte bu kadar!</span><span class="sxs-lookup"><span data-stu-id="2c617-167">That's it!</span></span> <span data-ttu-id="2c617-168">Çalışma web API'si var.</span><span class="sxs-lookup"><span data-stu-id="2c617-168">You have a working web API.</span></span> <span data-ttu-id="2c617-169">Denetleyicisinde her bir yöntemi, bir veya daha fazla URI'ler karşılık gelir:</span><span class="sxs-lookup"><span data-stu-id="2c617-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="2c617-170">Denetleyici yöntemi</span><span class="sxs-lookup"><span data-stu-id="2c617-170">Controller Method</span></span> | <span data-ttu-id="2c617-171">URI</span><span class="sxs-lookup"><span data-stu-id="2c617-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="2c617-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="2c617-172">GetAllProducts</span></span> | <span data-ttu-id="2c617-173">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="2c617-173">/api/products</span></span> |
| <span data-ttu-id="2c617-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="2c617-174">GetProduct</span></span> | <span data-ttu-id="2c617-175">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="2c617-175">/api/products/*id*</span></span> |

<span data-ttu-id="2c617-176">İçin `GetProduct` yöntemi, *kimliği* URI'de bir yer tutucudur.</span><span class="sxs-lookup"><span data-stu-id="2c617-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="2c617-177">Örneğin, 5 kimlikli ürün almak için URI. `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="2c617-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="2c617-178">Nasıl Web API denetleyici yöntemlerine HTTP isteklerini yolları hakkında daha fazla bilgi için bkz: [ASP.NET Web API'de yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2c617-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="2c617-179">Javascript ve Jquery'de ile Web API çağırma</span><span class="sxs-lookup"><span data-stu-id="2c617-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="2c617-180">Bu bölümde, web API'sini çağırmak için AJAX kullanan bir HTML sayfası ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="2c617-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="2c617-181">JQuery AJAX çağrıları yapma ve sayfa sonuçlarıyla güncelleştirmek için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2c617-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="2c617-182">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Ekle**seçeneğini belirleyip **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="2c617-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="2c617-183">İçinde **Yeni Öğe Ekle** iletişim kutusunda **Web** düğümü altında **Visual C#**ve ardından **HTML sayfası** öğesi.</span><span class="sxs-lookup"><span data-stu-id="2c617-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="2c617-184">Sayfa adı &quot;index.html&quot;.</span><span class="sxs-lookup"><span data-stu-id="2c617-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="2c617-185">Bu dosyadaki her şeyi aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="2c617-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="2c617-186">JQuery almak için birkaç yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="2c617-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="2c617-187">Bu örnekte, kullandım [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="2c617-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="2c617-188">Ayrıca buradan indirebilirsiniz [http://jquery.com/](http://jquery.com/)ve "Web API'sini" ASP.NET proje şablonu jQuery de içerir.</span><span class="sxs-lookup"><span data-stu-id="2c617-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="2c617-189">Ürünlerin listesini alma</span><span class="sxs-lookup"><span data-stu-id="2c617-189">Getting a List of Products</span></span>

<span data-ttu-id="2c617-190">Ürünlerin listesini almak için bir HTTP GET isteği Gönder &quot;/api/ürünleri&quot;.</span><span class="sxs-lookup"><span data-stu-id="2c617-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="2c617-191">JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) işlevi bir AJAX isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="2c617-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="2c617-192">Yanıt için JSON nesnelerinin dizisi içerir.</span><span class="sxs-lookup"><span data-stu-id="2c617-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="2c617-193">`done` İstek başarılı olursa çağrılan bir geri çağırma işlevini belirtiyor.</span><span class="sxs-lookup"><span data-stu-id="2c617-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="2c617-194">Geri arama, ürün bilgilerle DOM güncelleştiriyoruz.</span><span class="sxs-lookup"><span data-stu-id="2c617-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="2c617-195">Ürün Kimliği ile Başlarken</span><span class="sxs-lookup"><span data-stu-id="2c617-195">Getting a Product By ID</span></span>

<span data-ttu-id="2c617-196">Kimliğe göre bir ürün almak için bir HTTP GET isteği Gönder &quot;/api/ürünler/*kimliği*&quot;, burada *kimliği* ürün kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="2c617-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="2c617-197">Hala diyoruz `getJSON` AJAX isteği, ancak bu kez göndermeyi biz kimliği URI isteğinde yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="2c617-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="2c617-198">Bu istek yanıtı tek bir ürün JSON gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="2c617-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="2c617-199">Uygulamayı Çalıştırma</span><span class="sxs-lookup"><span data-stu-id="2c617-199">Running the Application</span></span>

<span data-ttu-id="2c617-200">Uygulama hata ayıklamayı başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="2c617-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="2c617-201">Web sayfasını aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="2c617-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="2c617-202">Kimliğe göre bir ürün almak için kimliği girin ve Ara'yı tıklatın:</span><span class="sxs-lookup"><span data-stu-id="2c617-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="2c617-203">Geçersiz kimlik girerseniz, sunucunun bir HTTP hata döndürür:</span><span class="sxs-lookup"><span data-stu-id="2c617-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="2c617-204">HTTP istek ve yanıt görüntülemek için F12 kullanma</span><span class="sxs-lookup"><span data-stu-id="2c617-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="2c617-205">Bir HTTP hizmeti ile çalışırken, HTTP isteği görmek ve iletileri istemek çok kullanışlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2c617-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="2c617-206">Internet Explorer 9 ' F12 geliştirici araçlarını kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c617-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="2c617-207">Internet Explorer 9 ' basın **F12** araçlarını açmak için.</span><span class="sxs-lookup"><span data-stu-id="2c617-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="2c617-208">Tıklatın **ağ** sekmesi ve tuşuna **Başlat yakalama**.</span><span class="sxs-lookup"><span data-stu-id="2c617-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="2c617-209">Şimdi geri dönerek basın ve web sayfası **F5** web sayfasını yeniden yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="2c617-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="2c617-210">Internet Explorer tarayıcı ile web sunucusu arasındaki HTTP trafiğini yakalar.</span><span class="sxs-lookup"><span data-stu-id="2c617-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="2c617-211">Özet görünümü bir sayfa için tüm ağ trafiğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="2c617-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="2c617-212">İçin göreli URI girişini bulun "API/Ürünler /".</span><span class="sxs-lookup"><span data-stu-id="2c617-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="2c617-213">Bu girişi seçin ve tıklatın **ayrıntılı görünümüne gidin**.</span><span class="sxs-lookup"><span data-stu-id="2c617-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="2c617-214">Ayrıntılı görünümde gövdeleri ve istek ve yanıt üst bilgileri görüntülemek için sekmeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="2c617-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="2c617-215">Örneğin, tıklatırsanız **istek üst** sekmesinde, istemci istenilen görebilirsiniz &quot;uygulama/json&quot; Accept üstbilgisindeki.</span><span class="sxs-lookup"><span data-stu-id="2c617-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="2c617-216">Yanıt gövdesi sekmesini tıklatırsanız, ürün listesinin nasıl JSON olarak serileştirilmiş görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c617-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="2c617-217">Diğer tarayıcılarda benzer işlevlere sahip.</span><span class="sxs-lookup"><span data-stu-id="2c617-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="2c617-218">Başka bir yararlı bir araçtır [Fiddler](http://www.fiddler2.com/fiddler2/), bir web proxy hata ayıklama.</span><span class="sxs-lookup"><span data-stu-id="2c617-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="2c617-219">Fiddler HTTP trafiğini görüntülemek için ve ayrıca HTTP isteklerini oluşturmak için istek HTTP üstbilgilerini üzerinde tam denetim imkanı sağlayan kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c617-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="2c617-220">Azure üzerinde çalışan bu uygulamayı bakın</span><span class="sxs-lookup"><span data-stu-id="2c617-220">See this App Running on Azure</span></span>

<span data-ttu-id="2c617-221">Canlı web uygulaması çalışan tamamlanmış site görmek ister misiniz?</span><span class="sxs-lookup"><span data-stu-id="2c617-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="2c617-222">Aşağıdaki düğmeyi tıklatarak, uygulama tam sürümü Azure hesabınızda dağıtabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c617-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="2c617-223">Bu çözüm Azure'a dağıtmak için bir Azure hesabınız olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2c617-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="2c617-224">Bir hesap zaten yoksa, aşağıdaki seçenekler vardır:</span><span class="sxs-lookup"><span data-stu-id="2c617-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="2c617-225">[Ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -krediler alırsınız, ücretli Azure hizmetlerini denemek için kullanabileceğiniz ve hatta kullanıldıktan sonra en fazla hesabı tutabilir ve ücretsiz Azure hizmetlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c617-225">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="2c617-226">[MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -MSDN aboneliğiniz size kredi verir Ücretli Azure hizmetlerinizi kullanabildiğiniz her ay.</span><span class="sxs-lookup"><span data-stu-id="2c617-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c617-227">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="2c617-227">Next Steps</span></span>

- <span data-ttu-id="2c617-228">POST, PUT ve silme eylemlerini destekler ve bu veritabanına yazan bir HTTP hizmeti daha eksiksiz bir örnek için bkz: [Entity Framework 6 ile Web API 2 kullanarak](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="2c617-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="2c617-229">Bir HTTP hizmeti üstünde esnektir, hızlı yanıt web uygulamaları oluşturma hakkında daha fazla bilgi için bkz: [ASP.NET tek sayfa uygulaması](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="2c617-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="2c617-230">Visual Studio web projesini Azure App Service'e dağıtma hakkında daha fazla bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="2c617-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
