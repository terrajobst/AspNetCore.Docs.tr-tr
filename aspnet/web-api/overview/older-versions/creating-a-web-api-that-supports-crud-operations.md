---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: ASP.NET Web API 1'de CRUD işlemlerini etkinleştirme | Microsoft Docs
author: MikeWasson
description: Bu öğreticide, ASP.NET Web API'sini kullanarak, HTTP hizmeti CRUD işlemlerini desteklemek gösterilir. Öğretici Visual Studio 2012 Web AP içinde kullanılan yazılım sürümleri...
ms.author: riande
ms.date: 01/28/2012
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: ba061b26b8527e447f25f6046057542a54f989a8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756517"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="204b0-104">ASP.NET Web API 1'de CRUD işlemlerini etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="204b0-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="204b0-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="204b0-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="204b0-106">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="204b0-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="204b0-107">Bu öğreticide, ASP.NET Web API'sini kullanarak, HTTP hizmeti CRUD işlemlerini desteklemek gösterilir.</span><span class="sxs-lookup"><span data-stu-id="204b0-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="204b0-108">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="204b0-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="204b0-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="204b0-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="204b0-110">Web API 1 (aynı zamanda Web API 2 ile çalışır)</span><span class="sxs-lookup"><span data-stu-id="204b0-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="204b0-111">CRUD anlamına gelen &quot;oluşturma, okuma, güncelleştirme ve silme,&quot; dört temel veritabanı işlemlerinin olduğu.</span><span class="sxs-lookup"><span data-stu-id="204b0-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="204b0-112">Birçok HTTP Hizmetleri de CRUD işlemlerini REST veya gibi REST API'leri aracılığıyla model.</span><span class="sxs-lookup"><span data-stu-id="204b0-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="204b0-113">Bu öğreticide, çok basit bir web API ürünlerin listesini yönetmek için oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="204b0-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="204b0-114">Her ürün adı, fiyatı ve kategorisi içerir (gibi &quot;toys&quot; veya &quot;donanım&quot;), artı ürün kimliği</span><span class="sxs-lookup"><span data-stu-id="204b0-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="204b0-115">API ürünler, yöntemleri açığa çıkarır.</span><span class="sxs-lookup"><span data-stu-id="204b0-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="204b0-116">Eylem</span><span class="sxs-lookup"><span data-stu-id="204b0-116">Action</span></span> | <span data-ttu-id="204b0-117">HTTP yöntemi</span><span class="sxs-lookup"><span data-stu-id="204b0-117">HTTP method</span></span> | <span data-ttu-id="204b0-118">Göreli URI'si</span><span class="sxs-lookup"><span data-stu-id="204b0-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="204b0-119">Tüm ürünlerin listesini alın</span><span class="sxs-lookup"><span data-stu-id="204b0-119">Get a list of all products</span></span> | <span data-ttu-id="204b0-120">AL</span><span class="sxs-lookup"><span data-stu-id="204b0-120">GET</span></span> | <span data-ttu-id="204b0-121">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="204b0-121">/api/products</span></span> |
| <span data-ttu-id="204b0-122">Bir ürün Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="204b0-122">Get a product by ID</span></span> | <span data-ttu-id="204b0-123">AL</span><span class="sxs-lookup"><span data-stu-id="204b0-123">GET</span></span> | <span data-ttu-id="204b0-124">/API'si/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="204b0-124">/api/products/*id*</span></span> |
| <span data-ttu-id="204b0-125">Bir ürün kategorisine göre Al</span><span class="sxs-lookup"><span data-stu-id="204b0-125">Get a product by category</span></span> | <span data-ttu-id="204b0-126">AL</span><span class="sxs-lookup"><span data-stu-id="204b0-126">GET</span></span> | <span data-ttu-id="204b0-127">/ api/ürünleri? kategori =*kategorisi*</span><span class="sxs-lookup"><span data-stu-id="204b0-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="204b0-128">Yeni Ürün oluşturma</span><span class="sxs-lookup"><span data-stu-id="204b0-128">Create a new product</span></span> | <span data-ttu-id="204b0-129">YAYINLA</span><span class="sxs-lookup"><span data-stu-id="204b0-129">POST</span></span> | <span data-ttu-id="204b0-130">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="204b0-130">/api/products</span></span> |
| <span data-ttu-id="204b0-131">Bir ürün güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="204b0-131">Update a product</span></span> | <span data-ttu-id="204b0-132">PUT</span><span class="sxs-lookup"><span data-stu-id="204b0-132">PUT</span></span> | <span data-ttu-id="204b0-133">/API'si/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="204b0-133">/api/products/*id*</span></span> |
| <span data-ttu-id="204b0-134">Bir ürün Sil</span><span class="sxs-lookup"><span data-stu-id="204b0-134">Delete a product</span></span> | <span data-ttu-id="204b0-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="204b0-135">DELETE</span></span> | <span data-ttu-id="204b0-136">/API'si/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="204b0-136">/api/products/*id*</span></span> |

<span data-ttu-id="204b0-137">Bir URI'leri bazıları ürün kimliği yolundaki Ekle dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="204b0-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="204b0-138">Örneğin, 28 Kimliğine sahip ürün almak için istemci bir GET isteği gönderir `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="204b0-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="204b0-139">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="204b0-139">Resources</span></span>

<span data-ttu-id="204b0-140">API ürünleri iki kaynak türleri için URI tanımlar:</span><span class="sxs-lookup"><span data-stu-id="204b0-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="204b0-141">Kaynak</span><span class="sxs-lookup"><span data-stu-id="204b0-141">Resource</span></span> | <span data-ttu-id="204b0-142">URI</span><span class="sxs-lookup"><span data-stu-id="204b0-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="204b0-143">Tüm ürünler listesi.</span><span class="sxs-lookup"><span data-stu-id="204b0-143">The list of all the products.</span></span> | <span data-ttu-id="204b0-144">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="204b0-144">/api/products</span></span> |
| <span data-ttu-id="204b0-145">Tek bir ürün.</span><span class="sxs-lookup"><span data-stu-id="204b0-145">An individual product.</span></span> | <span data-ttu-id="204b0-146">/API'si/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="204b0-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="204b0-147">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="204b0-147">Methods</span></span>

<span data-ttu-id="204b0-148">Dört ana HTTP yöntemleri (GET, PUT, POST ve DELETE) için CRUD işlemleri gibi eşlenebilir:</span><span class="sxs-lookup"><span data-stu-id="204b0-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="204b0-149">GET, belirtilen URI'de kaynağın bir gösterimini alır.</span><span class="sxs-lookup"><span data-stu-id="204b0-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="204b0-150">GET sunucu üzerinde hiçbir yan etkisinin olmaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="204b0-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="204b0-151">PUT, belirtilen URI'de bir kaynağı güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="204b0-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="204b0-152">Sunucu yeni bir URI'leri belirtmek istemcileri izin veriyorsa PUT bir belirtilen URI'de yeni bir kaynak oluşturmak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="204b0-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="204b0-153">Bu öğreticide, API ile PUT oluşturma desteklemez.</span><span class="sxs-lookup"><span data-stu-id="204b0-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="204b0-154">POST, yeni bir kaynak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="204b0-154">POST creates a new resource.</span></span> <span data-ttu-id="204b0-155">Sunucu yeni bir nesne için URI atar ve bu URI, yanıt iletisinin bir parçası olarak döndürür.</span><span class="sxs-lookup"><span data-stu-id="204b0-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="204b0-156">SİLME, belirtilen URI'de bir kaynak siler.</span><span class="sxs-lookup"><span data-stu-id="204b0-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="204b0-157">Not: Tüm ürün varlığı PUT yöntemini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="204b0-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="204b0-158">Diğer bir deyişle, güncelleştirilen ürün tam bir temsilini göndermek için istemci bekleniyor.</span><span class="sxs-lookup"><span data-stu-id="204b0-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="204b0-159">Kısmi güncelleştirmeleri desteklemek istiyorsanız, PATCH yöntemini tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="204b0-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="204b0-160">Bu öğreticide, düzeltme eki uygulamıyor.</span><span class="sxs-lookup"><span data-stu-id="204b0-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="204b0-161">Yeni bir Web API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="204b0-161">Create a New Web API Project</span></span>

<span data-ttu-id="204b0-162">Visual Studio çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="204b0-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="204b0-163">Veya **dosya** menüsünde **yeni** ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="204b0-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="204b0-164">İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="204b0-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="204b0-165">Altında **Visual C#** seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="204b0-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="204b0-166">Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="204b0-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="204b0-167">Projeyi adlandırın &quot;ProductStore&quot; tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="204b0-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="204b0-168">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Web API** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="204b0-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="204b0-169">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="204b0-169">Adding a Model</span></span>

<span data-ttu-id="204b0-170">A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="204b0-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="204b0-171">ASP.NET Web API, türü kesin belirlenmiş CLR nesne modelleri olarak kullanabilirsiniz ve bunlar otomatik olarak XML veya JSON istemci için seri hale.</span><span class="sxs-lookup"><span data-stu-id="204b0-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="204b0-172">Adlı yeni bir sınıf oluşturacağız. böylece ProductStore API'si için verilerimizi, ürünlerinin aynısından. `Product`.</span><span class="sxs-lookup"><span data-stu-id="204b0-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="204b0-173">Çözüm Gezgini görünür değilse, **görünümü** menü ve select **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="204b0-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="204b0-174">Çözüm Gezgini'nde sağ **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="204b0-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="204b0-175">Bağlam meny seçin **Ekle**, ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="204b0-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="204b0-176">Sınıf adı &quot;ürün&quot;.</span><span class="sxs-lookup"><span data-stu-id="204b0-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="204b0-177">Aşağıdaki özellikleri `Product` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="204b0-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="204b0-178">Bir depo ekleme</span><span class="sxs-lookup"><span data-stu-id="204b0-178">Adding a Repository</span></span>

<span data-ttu-id="204b0-179">Ürünleri koleksiyonunu depolamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="204b0-179">We need to store a collection of products.</span></span> <span data-ttu-id="204b0-180">Hizmet kararlılığımızın koleksiyondan ayırmak için iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="204b0-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="204b0-181">Bu şekilde yedekleme deposu hizmet sınıfı yeniden yazma olmadan Değiştirebiliriz.</span><span class="sxs-lookup"><span data-stu-id="204b0-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="204b0-182">Bu tür bir tasarım adlı *depo* deseni.</span><span class="sxs-lookup"><span data-stu-id="204b0-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="204b0-183">Depo için genel bir arabirim tanımlayarak başlatın.</span><span class="sxs-lookup"><span data-stu-id="204b0-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="204b0-184">Çözüm Gezgini'nde sağ **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="204b0-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="204b0-185">Seçin **Ekle**, ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="204b0-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="204b0-186">İçinde **şablonları** bölmesinde **yüklü şablonlar** ve C# düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="204b0-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="204b0-187">C# altında seçin **kod**.</span><span class="sxs-lookup"><span data-stu-id="204b0-187">Under C#, select **Code**.</span></span> <span data-ttu-id="204b0-188">Kod şablonları listesinde seçin **arabirimi**.</span><span class="sxs-lookup"><span data-stu-id="204b0-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="204b0-189">Arabirim adı &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="204b0-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="204b0-190">Aşağıdaki uygulama ekleyin:</span><span class="sxs-lookup"><span data-stu-id="204b0-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="204b0-191">Artık başka bir sınıf adlı modelleri klasörüne ekleyin &quot;ProductRepository.&quot; Bu sınıf uygulayacak `IProductRespository` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="204b0-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="204b0-192">Aşağıdaki uygulama ekleyin:</span><span class="sxs-lookup"><span data-stu-id="204b0-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="204b0-193">Depoyu yerel belleğinde listesini tutar.</span><span class="sxs-lookup"><span data-stu-id="204b0-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="204b0-194">Bu öğretici için Tamam olduğunu, ancak gerçek bir uygulamada verilerin harici olarak ya da bir veritabanını saklayacağından veya Bulut depolama.</span><span class="sxs-lookup"><span data-stu-id="204b0-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="204b0-195">Depo düzeni daha sonra uygulama değiştirme hale getirir.</span><span class="sxs-lookup"><span data-stu-id="204b0-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="204b0-196">Web API denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="204b0-196">Adding a Web API Controller</span></span>

<span data-ttu-id="204b0-197">ASP.NET MVC ile çalıştıysanız, ardından denetleyicileriyle bilginiz.</span><span class="sxs-lookup"><span data-stu-id="204b0-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="204b0-198">ASP.NET Web API, bir *denetleyicisi* istemciden gelen HTTP isteklerini işleyen sınıftır.</span><span class="sxs-lookup"><span data-stu-id="204b0-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="204b0-199">Proje oluşturduğunuz sırada yeni proje sihirbazını iki denetleyici oluşturulan.</span><span class="sxs-lookup"><span data-stu-id="204b0-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="204b0-200">Bunları görmek için Çözüm Gezgini'nde denetleyicileri klasörünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="204b0-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="204b0-201">HomeController, geleneksel bir ASP.NET MVC denetleyicisi değil.</span><span class="sxs-lookup"><span data-stu-id="204b0-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="204b0-202">HTML sayfaları site için hizmet vermek için sorumludur ve müşterilerimizin web API'sine doğrudan ilgili değildir.</span><span class="sxs-lookup"><span data-stu-id="204b0-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="204b0-203">ValuesController bir örnek Webapı denetleyicisi değil.</span><span class="sxs-lookup"><span data-stu-id="204b0-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="204b0-204">Devam edin ve ValuesController, Çözüm Gezgini'nde dosyaya sağ tıklatıp seçerek silme **silin.**</span><span class="sxs-lookup"><span data-stu-id="204b0-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="204b0-205">Şimdi yeni bir denetleyici aşağıdaki şekilde ekleyin:</span><span class="sxs-lookup"><span data-stu-id="204b0-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="204b0-206">İçinde **Çözüm Gezgini**, denetleyicileri klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="204b0-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="204b0-207">Seçin **Ekle** seçip **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="204b0-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="204b0-208">İçinde **denetleyici Ekle** Sihirbazı, denetleyici adı &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="204b0-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="204b0-209">İçinde **şablon** aşağı açılan listesinden **boş API denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="204b0-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="204b0-210">Ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="204b0-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="204b0-211">Contollers denetleyicileri adlı klasöre koyun gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="204b0-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="204b0-212">Klasör adı önemli değildir; Bu kaynak dosyaları düzenlemek için yalnızca bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="204b0-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="204b0-213">**Denetleyici Ekle** Sihirbazı ProductsController.cs denetleyicileri klasöründe adlı bir dosya oluşturur.</span><span class="sxs-lookup"><span data-stu-id="204b0-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="204b0-214">Bu dosya zaten açık değilse, açmak için dosyaya çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="204b0-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="204b0-215">Aşağıdaki **kullanarak** deyimi:</span><span class="sxs-lookup"><span data-stu-id="204b0-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="204b0-216">Tutan bir alan eklemek bir **IProductRepository** örneği.</span><span class="sxs-lookup"><span data-stu-id="204b0-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="204b0-217">Çağırma `new ProductRepository()` özel uygulanışı denetleyiciye bölümlere denetleyicide en iyi tasarım olmadığından `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="204b0-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="204b0-218">Daha iyi bir yaklaşım için bkz. [Web API bağımlılık çözümleyicisini kullanarak](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="204b0-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="204b0-219">Bir kaynak alma</span><span class="sxs-lookup"><span data-stu-id="204b0-219">Getting a Resource</span></span>

<span data-ttu-id="204b0-220">ProductStore API birkaç açığa çıkarır &quot;okuma&quot; Eylemler HTTP GET yöntemleri olarak.</span><span class="sxs-lookup"><span data-stu-id="204b0-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="204b0-221">Her eylem bir yöntemde karşılık gelir `ProductsController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="204b0-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="204b0-222">Eylem</span><span class="sxs-lookup"><span data-stu-id="204b0-222">Action</span></span> | <span data-ttu-id="204b0-223">HTTP yöntemi</span><span class="sxs-lookup"><span data-stu-id="204b0-223">HTTP method</span></span> | <span data-ttu-id="204b0-224">Göreli URI'si</span><span class="sxs-lookup"><span data-stu-id="204b0-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="204b0-225">Tüm ürünlerin listesini alın</span><span class="sxs-lookup"><span data-stu-id="204b0-225">Get a list of all products</span></span> | <span data-ttu-id="204b0-226">AL</span><span class="sxs-lookup"><span data-stu-id="204b0-226">GET</span></span> | <span data-ttu-id="204b0-227">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="204b0-227">/api/products</span></span> |
| <span data-ttu-id="204b0-228">Bir ürün Kimliğine göre Al</span><span class="sxs-lookup"><span data-stu-id="204b0-228">Get a product by ID</span></span> | <span data-ttu-id="204b0-229">AL</span><span class="sxs-lookup"><span data-stu-id="204b0-229">GET</span></span> | <span data-ttu-id="204b0-230">/API'si/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="204b0-230">/api/products/*id*</span></span> |
| <span data-ttu-id="204b0-231">Bir ürün kategorisine göre Al</span><span class="sxs-lookup"><span data-stu-id="204b0-231">Get a product by category</span></span> | <span data-ttu-id="204b0-232">AL</span><span class="sxs-lookup"><span data-stu-id="204b0-232">GET</span></span> | <span data-ttu-id="204b0-233">/ api/ürünleri? kategori =*kategorisi*</span><span class="sxs-lookup"><span data-stu-id="204b0-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="204b0-234">Tüm ürünlerin listesini almak için bu yönteme ekleme `ProductsController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="204b0-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="204b0-235">Yöntem adı ile başlayan &quot;alma&quot;, isteğe bağlı olarak Kural gereği GET isteklerinin eşler.</span><span class="sxs-lookup"><span data-stu-id="204b0-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="204b0-236">Ayrıca, bu eşler içermeyen bir URI yöntemin parametre olduğundan, bir *&quot;kimliği&quot;* yolunda kesimi.</span><span class="sxs-lookup"><span data-stu-id="204b0-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="204b0-237">Kimliğe göre bir ürün almak için bu yönteme ekleme `ProductsController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="204b0-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="204b0-238">Bu yöntem adı ile başlayan ayrıca &quot;alma&quot;, ancak yöntem adlı bir parametre *kimliği*. Bu parametre eşlenen &quot;kimliği&quot; URI yolu kesimi.</span><span class="sxs-lookup"><span data-stu-id="204b0-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="204b0-239">ASP.NET Web API çerçevesi, kimlik otomatik olarak doğru veri türüne dönüştürür. (**int**) parametresi için.</span><span class="sxs-lookup"><span data-stu-id="204b0-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="204b0-240">Türünde bir özel durum GetProduct yöntemi oluşturur **HttpResponseException** varsa *kimliği* geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="204b0-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="204b0-241">Bu özel durumun framework tarafından bir 404 (bulunamadı) hatası çevrilir.</span><span class="sxs-lookup"><span data-stu-id="204b0-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="204b0-242">Son olarak, kategoriye göre ürünleri bulmak için bir yöntem ekleyin:</span><span class="sxs-lookup"><span data-stu-id="204b0-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="204b0-243">İstek URI'SİNDEKİ bir sorgu dizesine sahipse, Web API denetleyici yönteminin parametreleri sorgu parametreleri eşleştirmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="204b0-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="204b0-244">Bu nedenle, bir URI biçiminde "API/ürünleri? kategori =*kategori*" Bu yönteme eşler.</span><span class="sxs-lookup"><span data-stu-id="204b0-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="204b0-245">Kaynak oluşturma</span><span class="sxs-lookup"><span data-stu-id="204b0-245">Creating a Resource</span></span>

<span data-ttu-id="204b0-246">Ardından, bir yönteme ekleyeceğiz `ProductsController` sınıfının yeni bir ürün oluşturun.</span><span class="sxs-lookup"><span data-stu-id="204b0-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="204b0-247">Basit bir yöntem uygulaması şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="204b0-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="204b0-248">Bu yöntem hakkında iki şeyi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="204b0-248">Note two things about this method:</span></span>

- <span data-ttu-id="204b0-249">Yöntem adı ile başlayan &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="204b0-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="204b0-250">Yeni ürün oluşturmak için istemci bir HTTP POST isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="204b0-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="204b0-251">Yöntem ürün türünde bir parametre alır.</span><span class="sxs-lookup"><span data-stu-id="204b0-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="204b0-252">Web API'de gövdeden parametrelerle karmaşık türleri seri.</span><span class="sxs-lookup"><span data-stu-id="204b0-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="204b0-253">Bu nedenle, XML veya JSON biçiminde bir ürün nesnesinin serileştirilmiş bir gösterimi göndermek için istemci bekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="204b0-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="204b0-254">Bu uygulama çalışır, ancak henüz tamamlanmadı.</span><span class="sxs-lookup"><span data-stu-id="204b0-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="204b0-255">İdeal olarak, aşağıdakiler dahil etmek için HTTP yanıt istiyoruz:</span><span class="sxs-lookup"><span data-stu-id="204b0-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="204b0-256">**Yanıt kodu:** varsayılan olarak, Web API çerçevesi yanıt durum kodu 200 (Tamam) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="204b0-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="204b0-257">Ancak, bir POST isteği bir kaynak oluşturulmasını içinde sonuçlandığında göre HTTP/1.1 protokolünü, sunucunun 201 (oluşturuldu) durumu ile yanıtla.</span><span class="sxs-lookup"><span data-stu-id="204b0-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="204b0-258">**Konum:** sunucu kaynak oluşturduğunda, yeni kaynağın URI'sini yanıtının Location üst bilgisini içermelidir.</span><span class="sxs-lookup"><span data-stu-id="204b0-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="204b0-259">ASP.NET Web API HTTP yanıt iletisini işlemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="204b0-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="204b0-260">Gelişmiş uygulama şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="204b0-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="204b0-261">Yöntem dönüş türü artık olduğuna dikkat edin **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="204b0-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="204b0-262">Döndürerek bir **HttpResponseMessage** yerine bir ürün, biz durum kodunu ve Location üst bilgisini dahil olmak üzere HTTP yanıt iletisinin ayrıntıları kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="204b0-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="204b0-263">**CreateResponse** yöntemi oluşturur bir **HttpResponseMessage** ve otomatik olarak serileştirilmiş bir gösterimi olan ürün nesne gövdesine Yazar fo yanıt iletisi.</span><span class="sxs-lookup"><span data-stu-id="204b0-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="204b0-264">Bu örnekte doğrulamamanız `Product`.</span><span class="sxs-lookup"><span data-stu-id="204b0-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="204b0-265">Model doğrulama hakkında daha fazla bilgi için bkz. [ASP.NET Web API'de Model doğrulama](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="204b0-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="204b0-266">Bir kaynak güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="204b0-266">Updating a Resource</span></span>

<span data-ttu-id="204b0-267">Bir ürün ile PUT güncelleştirmek kolaydır:</span><span class="sxs-lookup"><span data-stu-id="204b0-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="204b0-268">Yöntem adı ile başlayan &quot;yerleştirin... &quot;, Web API'si için PUT isteklerini eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="204b0-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="204b0-269">Yöntem iki parametre, ürün kimliği ve güncelleştirilen ürün alır.</span><span class="sxs-lookup"><span data-stu-id="204b0-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="204b0-270">*Kimliği* parametresi URI yolundan alınır ve *ürün* gövdeden parametresi seri durumdan.</span><span class="sxs-lookup"><span data-stu-id="204b0-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="204b0-271">Varsayılan olarak, ASP.NET Web API çerçevesi rotadaki basit parametre türleri ve karmaşık türler istek gövdesinden alır.</span><span class="sxs-lookup"><span data-stu-id="204b0-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="204b0-272">Kaynak siliniyor</span><span class="sxs-lookup"><span data-stu-id="204b0-272">Deleting a Resource</span></span>

<span data-ttu-id="204b0-273">Bir resourse silmek için "Sil..." yöntemi tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="204b0-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="204b0-274">Bir silme isteği başarılı olursa, bir varlık durumunu açıklayan gövdesi ile 200 (Tamam) durum döndürebilir; Durum 202 (kabul edildi silme işlemi hala ise) bekleyen; veya durumu ile hiçbir varlık gövdesini 204 (içerik yok).</span><span class="sxs-lookup"><span data-stu-id="204b0-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="204b0-275">Bu durumda, `DeleteProduct` yöntemi olan bir `void` dönüş türü, durumuna, bu otomatik olarak ASP.NET Web API dönüşür şekilde 204 (içerik yok) kodu.</span><span class="sxs-lookup"><span data-stu-id="204b0-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
