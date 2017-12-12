---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "ASP.NET Web API'de 1 CRUD işlemleri etkinleştirme | Microsoft Docs"
author: MikeWasson
description: "Bu öğreticide, ASP.NET Web API kullanarak, HTTP hizmeti CRUD işlemleri desteklemek gösterilmiştir. Eğitmen Visual Studio 2012 Web AP içinde kullanılan yazılım sürümleri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a91bf065c9ce0fc5bd9b7115340edabea975a7e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="ee883-104">ASP.NET Web API'de 1 CRUD işlemleri etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="ee883-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="ee883-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ee883-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ee883-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="ee883-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="ee883-107">Bu öğreticide, ASP.NET Web API kullanarak, HTTP hizmeti CRUD işlemleri desteklemek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ee883-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ee883-108">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="ee883-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ee883-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ee883-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="ee883-110">Web API 1 (aynı zamanda Web API 2 ile çalışır)</span><span class="sxs-lookup"><span data-stu-id="ee883-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="ee883-111">CRUD anlamına gelir &quot;oluşturma, okuma, güncelleştirme ve silme,&quot; dört temel veritabanı işlemleri olduğu.</span><span class="sxs-lookup"><span data-stu-id="ee883-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="ee883-112">Birçok HTTP Hizmetleri de CRUD işlemleri REST veya gibi REST API'leri aracılığıyla model.</span><span class="sxs-lookup"><span data-stu-id="ee883-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="ee883-113">Bu öğreticide, çok basit bir web API ürünlerin listesini yönetmek için oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="ee883-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="ee883-114">Her ürün adı, fiyat ve kategorisi içerir (gibi &quot;toys&quot; veya &quot;donanım&quot;), artı bir ürün kimliği</span><span class="sxs-lookup"><span data-stu-id="ee883-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="ee883-115">API ürünler, yöntemleri açığa çıkarır.</span><span class="sxs-lookup"><span data-stu-id="ee883-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="ee883-116">Eylem</span><span class="sxs-lookup"><span data-stu-id="ee883-116">Action</span></span> | <span data-ttu-id="ee883-117">HTTP yöntemi</span><span class="sxs-lookup"><span data-stu-id="ee883-117">HTTP method</span></span> | <span data-ttu-id="ee883-118">Göreli URI</span><span class="sxs-lookup"><span data-stu-id="ee883-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee883-119">Tüm ürünlerin listesini al</span><span class="sxs-lookup"><span data-stu-id="ee883-119">Get a list of all products</span></span> | <span data-ttu-id="ee883-120">AL</span><span class="sxs-lookup"><span data-stu-id="ee883-120">GET</span></span> | <span data-ttu-id="ee883-121">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="ee883-121">/api/products</span></span> |
| <span data-ttu-id="ee883-122">Ürün Kimliği tarafından Al</span><span class="sxs-lookup"><span data-stu-id="ee883-122">Get a product by ID</span></span> | <span data-ttu-id="ee883-123">AL</span><span class="sxs-lookup"><span data-stu-id="ee883-123">GET</span></span> | <span data-ttu-id="ee883-124">/api/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="ee883-124">/api/products/*id*</span></span> |
| <span data-ttu-id="ee883-125">Bir ürün kategorisine göre alma</span><span class="sxs-lookup"><span data-stu-id="ee883-125">Get a product by category</span></span> | <span data-ttu-id="ee883-126">AL</span><span class="sxs-lookup"><span data-stu-id="ee883-126">GET</span></span> | <span data-ttu-id="ee883-127">/ api/ürünleri? kategori =*kategorisi*</span><span class="sxs-lookup"><span data-stu-id="ee883-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="ee883-128">Yeni Ürün oluşturma</span><span class="sxs-lookup"><span data-stu-id="ee883-128">Create a new product</span></span> | <span data-ttu-id="ee883-129">YAYINLA</span><span class="sxs-lookup"><span data-stu-id="ee883-129">POST</span></span> | <span data-ttu-id="ee883-130">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="ee883-130">/api/products</span></span> |
| <span data-ttu-id="ee883-131">Bir ürün güncelleştir</span><span class="sxs-lookup"><span data-stu-id="ee883-131">Update a product</span></span> | <span data-ttu-id="ee883-132">PUT</span><span class="sxs-lookup"><span data-stu-id="ee883-132">PUT</span></span> | <span data-ttu-id="ee883-133">/api/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="ee883-133">/api/products/*id*</span></span> |
| <span data-ttu-id="ee883-134">Ürünü silme</span><span class="sxs-lookup"><span data-stu-id="ee883-134">Delete a product</span></span> | <span data-ttu-id="ee883-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="ee883-135">DELETE</span></span> | <span data-ttu-id="ee883-136">/api/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="ee883-136">/api/products/*id*</span></span> |

<span data-ttu-id="ee883-137">Bazı URI'ler ürün kimliği yolundaki dahil dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ee883-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="ee883-138">Örneğin, Kimliğine sahip 28 ürün almak için istemci bir GET isteği gönderir `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="ee883-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="ee883-139">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ee883-139">Resources</span></span>

<span data-ttu-id="ee883-140">API ürünleri URI'ler iki kaynak türleri için tanımlar:</span><span class="sxs-lookup"><span data-stu-id="ee883-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="ee883-141">Kaynak</span><span class="sxs-lookup"><span data-stu-id="ee883-141">Resource</span></span> | <span data-ttu-id="ee883-142">URI</span><span class="sxs-lookup"><span data-stu-id="ee883-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="ee883-143">Tüm ürünler listesi.</span><span class="sxs-lookup"><span data-stu-id="ee883-143">The list of all the products.</span></span> | <span data-ttu-id="ee883-144">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="ee883-144">/api/products</span></span> |
| <span data-ttu-id="ee883-145">Bir ürünün.</span><span class="sxs-lookup"><span data-stu-id="ee883-145">An individual product.</span></span> | <span data-ttu-id="ee883-146">/api/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="ee883-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="ee883-147">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="ee883-147">Methods</span></span>

<span data-ttu-id="ee883-148">Dört ana HTTP yöntemleri (GET, PUT, POST ve Sil) CRUD işlemleri için aşağıdaki gibi eşlenebilir:</span><span class="sxs-lookup"><span data-stu-id="ee883-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="ee883-149">GET Belirtilen URI kaynak gösterimini alır.</span><span class="sxs-lookup"><span data-stu-id="ee883-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="ee883-150">GET sunucu üzerinde hiçbir yan etkisi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ee883-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="ee883-151">Belirtilen URI kaynakta PUT güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="ee883-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="ee883-152">Sunucu yeni URI'ler belirtmek istemcileri izin veriyorsa PUT ayrıca belirtilen bir URI'da, yeni bir kaynak oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ee883-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="ee883-153">Bu öğreticide, API PUT ile oluşturma desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ee883-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="ee883-154">POST yeni bir kaynak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ee883-154">POST creates a new resource.</span></span> <span data-ttu-id="ee883-155">Sunucu yeni bir nesne için URI atar ve yanıt iletisi bir parçası olarak bu URI döndürür.</span><span class="sxs-lookup"><span data-stu-id="ee883-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="ee883-156">Sil, belirtilen URI'ye kaynakta siler.</span><span class="sxs-lookup"><span data-stu-id="ee883-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="ee883-157">Not: Tüm ürün varlık PUT yöntemini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="ee883-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="ee883-158">Diğer bir deyişle, istemci güncelleştirilen ürün eksiksiz bir gösterimini göndermesi beklenir.</span><span class="sxs-lookup"><span data-stu-id="ee883-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="ee883-159">Kısmi güncelleştirmeler desteklemek istiyorsanız, düzeltme eki tercih edilen yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="ee883-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="ee883-160">Bu öğretici, düzeltme eki uygulamıyor.</span><span class="sxs-lookup"><span data-stu-id="ee883-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="ee883-161">Yeni bir Web API projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="ee883-161">Create a New Web API Project</span></span>

<span data-ttu-id="ee883-162">Visual Studio çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="ee883-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="ee883-163">Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="ee883-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="ee883-164">İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü.</span><span class="sxs-lookup"><span data-stu-id="ee883-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ee883-165">Altında **Visual C#**seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="ee883-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ee883-166">Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="ee883-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="ee883-167">Proje adı &quot;ProductStore&quot; tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ee883-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="ee883-168">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Web API** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="ee883-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="ee883-169">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="ee883-169">Adding a Model</span></span>

<span data-ttu-id="ee883-170">A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="ee883-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="ee883-171">ASP.NET Web API, modelleri olarak kesin türü belirtilmiş CLR nesnelerini kullanabilirsiniz ve bunlar otomatik olarak XML veya JSON istemci için seri hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="ee883-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="ee883-172">Adlı yeni bir sınıf oluşturacağız şekilde ProductStore API için verilerimizi ürünlerden, oluşur. `Product`.</span><span class="sxs-lookup"><span data-stu-id="ee883-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="ee883-173">Çözüm Gezgini görünür değilse, **Görünüm** menü ve select **Çözüm Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="ee883-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="ee883-174">Çözüm Gezgini'nde sağ **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="ee883-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ee883-175">Bağlam meny seçin **Ekle**seçeneğini belirleyip **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="ee883-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="ee883-176">Sınıf adını &quot;ürün&quot;.</span><span class="sxs-lookup"><span data-stu-id="ee883-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="ee883-177">Aşağıdaki özellikleri ekleyin `Product` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ee883-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="ee883-178">Bir havuz ekleme</span><span class="sxs-lookup"><span data-stu-id="ee883-178">Adding a Repository</span></span>

<span data-ttu-id="ee883-179">Ürünler koleksiyonu depolamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="ee883-179">We need to store a collection of products.</span></span> <span data-ttu-id="ee883-180">Bizim hizmet uygulaması koleksiyonundan ayırmak için iyi bir fikirdir.</span><span class="sxs-lookup"><span data-stu-id="ee883-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="ee883-181">Böylece, biz yedekleme deposu hizmet sınıfı yeniden yazma işlemi olmadan değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ee883-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="ee883-182">Bu tür bir tasarım adlı *depo* düzeni.</span><span class="sxs-lookup"><span data-stu-id="ee883-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="ee883-183">Depo için genel bir arabirim tanımlayarak başlatın.</span><span class="sxs-lookup"><span data-stu-id="ee883-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="ee883-184">Çözüm Gezgini'nde sağ **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="ee883-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ee883-185">Seçin **Ekle**seçeneğini belirleyip **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="ee883-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="ee883-186">İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve C# düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="ee883-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="ee883-187">C# altında seçin **kod**.</span><span class="sxs-lookup"><span data-stu-id="ee883-187">Under C#, select **Code**.</span></span> <span data-ttu-id="ee883-188">Kod şablonları listesinde seçin **arabirimi**.</span><span class="sxs-lookup"><span data-stu-id="ee883-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="ee883-189">Arabirim adı &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="ee883-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="ee883-190">Aşağıdaki uygulama ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ee883-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="ee883-191">Şimdi adlı modeller klasörü için başka bir sınıf ekleyin &quot;ProductRepository.&quot; Bu sınıf gerçekleştireceksiniz `IProductRespository` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="ee883-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="ee883-192">Aşağıdaki uygulama ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ee883-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="ee883-193">Depo listesi yerel bellekte saklar.</span><span class="sxs-lookup"><span data-stu-id="ee883-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="ee883-194">Bu öğretici için Tamam, ancak gerçek bir uygulamada veri harici olarak ya da bir veritabanı saklayacağından veya Bulut depolama.</span><span class="sxs-lookup"><span data-stu-id="ee883-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="ee883-195">Depo düzeni uygulama daha sonra değiştirmek hale getirir.</span><span class="sxs-lookup"><span data-stu-id="ee883-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="ee883-196">Bir Web API denetleyicisi ekleme</span><span class="sxs-lookup"><span data-stu-id="ee883-196">Adding a Web API Controller</span></span>

<span data-ttu-id="ee883-197">ASP.NET MVC ile çalıştıysanız, ardından denetleyicileriyle bilginiz.</span><span class="sxs-lookup"><span data-stu-id="ee883-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="ee883-198">ASP.NET Web API içinde bir *denetleyicisi* istemciden gelen HTTP isteklerini işleyen sınıftır.</span><span class="sxs-lookup"><span data-stu-id="ee883-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="ee883-199">Proje oluşturduğunuzda Yeni Proje Sihirbazı'nı iki denetleyicileri oluşturulan.</span><span class="sxs-lookup"><span data-stu-id="ee883-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="ee883-200">Bunları görmek için Çözüm Gezgini'nde denetleyicileri klasörünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="ee883-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="ee883-201">HomeController geleneksel bir ASP.NET MVC denetleyicisi değil.</span><span class="sxs-lookup"><span data-stu-id="ee883-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="ee883-202">Site için HTML sayfaları hizmet vermek için sorumludur ve doğrudan sunduğumuz web API ilişkili değil.</span><span class="sxs-lookup"><span data-stu-id="ee883-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="ee883-203">ValuesController örnek Webapı denetleyicisidir.</span><span class="sxs-lookup"><span data-stu-id="ee883-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="ee883-204">Bir tane ValuesController, Çözüm Gezgini'nde dosyaya sağ tıklayıp seçerek silme **silin.**</span><span class="sxs-lookup"><span data-stu-id="ee883-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="ee883-205">Şimdi yeni bir denetleyici aşağıdaki şekilde ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ee883-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="ee883-206">İçinde **Çözüm Gezgini**, sağ denetleyicileri klasör.</span><span class="sxs-lookup"><span data-stu-id="ee883-206">In **Solution Explorer**, right-click the the Controllers folder.</span></span> <span data-ttu-id="ee883-207">Seçin **Ekle** ve ardından **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="ee883-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="ee883-208">İçinde **denetleyici Ekle** Sihirbazı, denetleyici adı &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ee883-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ee883-209">İçinde **şablonu** aşağı açılan listesinden, **boş API denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="ee883-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="ee883-210">Ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="ee883-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="ee883-211">Contollers denetleyicileri adlı bir klasöre yerleştirmek gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="ee883-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="ee883-212">Klasör adı önemli değildir; Bu Kaynak dosyalarınız düzenlemek için yalnızca bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="ee883-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="ee883-213">**Denetleyici Ekle** Sihirbazı denetleyicileri klasöründe ProductsController.cs adlı bir dosya oluşturur.</span><span class="sxs-lookup"><span data-stu-id="ee883-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="ee883-214">Bu dosya zaten açık değilse, açmak için dosyaya çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="ee883-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="ee883-215">Aşağıdakileri ekleyin **kullanarak** deyimi:</span><span class="sxs-lookup"><span data-stu-id="ee883-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="ee883-216">Tutan bir alan ekleyebilmek bir **IProductRepository** örneği.</span><span class="sxs-lookup"><span data-stu-id="ee883-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="ee883-217">Çağırma `new ProductRepository()` belirli bir uygulamaya denetleyicisine bağlar denetleyicide en iyi tasarım olmadığından `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ee883-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="ee883-218">Daha iyi bir yaklaşım için bkz: [Web API bağımlılık çözümleyicisini kullanarak](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ee883-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="ee883-219">Bir kaynak alınıyor</span><span class="sxs-lookup"><span data-stu-id="ee883-219">Getting a Resource</span></span>

<span data-ttu-id="ee883-220">ProductStore API birkaç açığa çıkarır &quot;okuma&quot; HTTP GET yöntemi olarak eylemler.</span><span class="sxs-lookup"><span data-stu-id="ee883-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="ee883-221">Her eylem bir yöntem karşılık gelir `ProductsController` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ee883-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="ee883-222">Eylem</span><span class="sxs-lookup"><span data-stu-id="ee883-222">Action</span></span> | <span data-ttu-id="ee883-223">HTTP yöntemi</span><span class="sxs-lookup"><span data-stu-id="ee883-223">HTTP method</span></span> | <span data-ttu-id="ee883-224">Göreli URI</span><span class="sxs-lookup"><span data-stu-id="ee883-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ee883-225">Tüm ürünlerin listesini al</span><span class="sxs-lookup"><span data-stu-id="ee883-225">Get a list of all products</span></span> | <span data-ttu-id="ee883-226">AL</span><span class="sxs-lookup"><span data-stu-id="ee883-226">GET</span></span> | <span data-ttu-id="ee883-227">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="ee883-227">/api/products</span></span> |
| <span data-ttu-id="ee883-228">Ürün Kimliği tarafından Al</span><span class="sxs-lookup"><span data-stu-id="ee883-228">Get a product by ID</span></span> | <span data-ttu-id="ee883-229">AL</span><span class="sxs-lookup"><span data-stu-id="ee883-229">GET</span></span> | <span data-ttu-id="ee883-230">/api/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="ee883-230">/api/products/*id*</span></span> |
| <span data-ttu-id="ee883-231">Bir ürün kategorisine göre alma</span><span class="sxs-lookup"><span data-stu-id="ee883-231">Get a product by category</span></span> | <span data-ttu-id="ee883-232">AL</span><span class="sxs-lookup"><span data-stu-id="ee883-232">GET</span></span> | <span data-ttu-id="ee883-233">/ api/ürünleri? kategori =*kategorisi*</span><span class="sxs-lookup"><span data-stu-id="ee883-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="ee883-234">Tüm ürünlerin listesini almak için bu yöntemi ekleyin `ProductsController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ee883-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="ee883-235">Yöntem adı ile başlayan &quot;almak&quot;, isteğe bağlı olarak kurala göre GET isteklerinin eşler.</span><span class="sxs-lookup"><span data-stu-id="ee883-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="ee883-236">Ayrıca, bu eşlemeleri içermeyen bir URI yöntemi hiçbir parametre yoktur çünkü bir  *&quot;kimliği&quot;*  yol kesimi.</span><span class="sxs-lookup"><span data-stu-id="ee883-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="ee883-237">Kimliğe göre bir ürün almak için bu yöntemi ekleyin `ProductsController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ee883-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="ee883-238">Bu yöntem adı da ile başlayan &quot;almak&quot;, ancak yöntem adlı bir parametre *kimliği*. Bu parametre eşlenmiş &quot;kimliği&quot; URI yolu kesimi.</span><span class="sxs-lookup"><span data-stu-id="ee883-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="ee883-239">ASP.NET Web API çerçevesi, Kimliğini otomatik olarak doğru veri türüne dönüştürür. (**int**) parametresi için.</span><span class="sxs-lookup"><span data-stu-id="ee883-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="ee883-240">GetProduct yöntemi türünde bir özel durum atar **HttpResponseException** varsa *kimliği* geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="ee883-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="ee883-241">Bu özel durum 404 (bulunamadı) hatası çerçevesi tarafından çevrilir.</span><span class="sxs-lookup"><span data-stu-id="ee883-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="ee883-242">Son olarak, ürün kategorisine göre bulmak için bir yöntem ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ee883-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="ee883-243">İsteğin URI sorgu dizesine sahipse, Web API denetleyicisi yöntemi parametrelerine sorgu parametreleri eşleştirmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="ee883-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="ee883-244">Bu nedenle, bir URI biçiminde "API/ürünleri? kategori =*kategori*" Bu yönteme eşler.</span><span class="sxs-lookup"><span data-stu-id="ee883-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="ee883-245">Kaynak oluşturma</span><span class="sxs-lookup"><span data-stu-id="ee883-245">Creating a Resource</span></span>

<span data-ttu-id="ee883-246">Ardından, bir yönteme ekleyeceğiz `ProductsController` yeni ürün oluşturmak için sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ee883-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="ee883-247">Basit bir yöntemin kullanımı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="ee883-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="ee883-248">Bu yöntem hakkında iki şey dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="ee883-248">Note two things about this method:</span></span>

- <span data-ttu-id="ee883-249">Yöntem adı ile başlayan &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="ee883-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="ee883-250">Yeni bir ürün oluşturmak için istemci bir HTTP POST isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="ee883-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="ee883-251">Yöntem ürün türünde bir parametre alır.</span><span class="sxs-lookup"><span data-stu-id="ee883-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="ee883-252">Web API'de parametreleri Karmaşık türlerle isteği gövdesinden serisi.</span><span class="sxs-lookup"><span data-stu-id="ee883-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="ee883-253">Bu nedenle, XML veya JSON biçiminde bir ürün nesnesinin serileştirilmiş bir gösterimi göndermek için istemci bekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="ee883-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="ee883-254">Bu uygulama çalışır, ancak oldukça tam değil.</span><span class="sxs-lookup"><span data-stu-id="ee883-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="ee883-255">İdeal olarak, aşağıdakiler dahil etmek için HTTP yanıtı isteriz:</span><span class="sxs-lookup"><span data-stu-id="ee883-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="ee883-256">**Yanıt kodu:** varsayılan olarak, Web API çerçevesi yanıt durum kodu 200'e (Tamam) ayarlar.</span><span class="sxs-lookup"><span data-stu-id="ee883-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="ee883-257">Ancak bir POST isteği bir kaynak oluşturulmasında sonuçlandığında HTTP/1.1 protokolünü göre sunucu durum 201 (oluşturuldu) ile yanıtla.</span><span class="sxs-lookup"><span data-stu-id="ee883-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="ee883-258">**Konumu:** sunucu kaynak oluşturduğunda, yanıt konum üstbilgisinin yeni kaynak URI'si içermelidir.</span><span class="sxs-lookup"><span data-stu-id="ee883-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="ee883-259">ASP.NET Web API HTTP yanıt iletisi işlemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="ee883-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="ee883-260">Geliştirilmiş uygulama şöyledir:</span><span class="sxs-lookup"><span data-stu-id="ee883-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="ee883-261">Yöntemin dönüş türü artık olduğuna dikkat edin **httpresponsemessage öğesini**.</span><span class="sxs-lookup"><span data-stu-id="ee883-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="ee883-262">Döndürerek bir **httpresponsemessage öğesini** bir ürün yerine biz durum kodunu ve Location üst bilgisini de dahil olmak üzere HTTP yanıt iletisinin ayrıntılarını kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ee883-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="ee883-263">**CreateResponse** yöntemi oluşturur bir **httpresponsemessage öğesini** ve otomatik olarak serileştirilmiş bir gösterimi Product nesnesinin gövdesi Yazar fo yanıt iletisi.</span><span class="sxs-lookup"><span data-stu-id="ee883-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="ee883-264">Bu örnek doğrulanmadı `Product`.</span><span class="sxs-lookup"><span data-stu-id="ee883-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="ee883-265">Model doğrulama hakkında daha fazla bilgi için bkz: [ASP.NET Web API Model doğrulama](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ee883-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="ee883-266">Bir kaynak güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="ee883-266">Updating a Resource</span></span>

<span data-ttu-id="ee883-267">Bir ürün ile PUT güncelleştirme basittir:</span><span class="sxs-lookup"><span data-stu-id="ee883-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="ee883-268">Yöntem adı ile başlayan &quot;yerleştirin... &quot;, Web API PUT isteklerini eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="ee883-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="ee883-269">Yöntemi iki parametre, ürün kimliği ve güncelleştirilen ürün alır.</span><span class="sxs-lookup"><span data-stu-id="ee883-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="ee883-270">*Kimliği* parametresi URI yolundan alınır ve *ürün* parametre istek gövdesinden serisi.</span><span class="sxs-lookup"><span data-stu-id="ee883-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="ee883-271">Varsayılan olarak, ASP.NET Web API çerçevesi rotadaki basit parametre türleri ve karmaşık türler istek gövdesi alır.</span><span class="sxs-lookup"><span data-stu-id="ee883-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="ee883-272">Bir kaynağı siliniyor</span><span class="sxs-lookup"><span data-stu-id="ee883-272">Deleting a Resource</span></span>

<span data-ttu-id="ee883-273">Bir resourse silmek için "Delete..." yöntemi tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="ee883-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="ee883-274">Bir silme isteği başarılı olursa, bir varlık durumu açıklayan gövdesi ile durum 200 (Tamam) döndürebilir; Durum 202 (silme hala ise, kabul edilen) bekleyen; veya durumu ile hiçbir varlık gövdesini 204 (No içerik).</span><span class="sxs-lookup"><span data-stu-id="ee883-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="ee883-275">Bu durumda, `DeleteProduct` yöntemi sahip bir `void` dönüş türü, durumunu, bu otomatik olarak ASP.NET Web API dönüşür şekilde 204 (No içerik) kodu.</span><span class="sxs-lookup"><span data-stu-id="ee883-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
