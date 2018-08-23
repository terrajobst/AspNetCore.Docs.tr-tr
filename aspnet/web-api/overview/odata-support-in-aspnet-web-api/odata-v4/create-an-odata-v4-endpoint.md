---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: ASP.NET Web API 2.2 kullanarak bir OData v4 uç noktası oluşturma | Microsoft Docs
author: MikeWasson
description: Açık Veri Protokolü (OData), web için veri erişim protokolüdür. OData sorgu ve veri kümeleri aracılığıyla CRUD işlemleri işlemek için Tekdüzen bir yol sağlar...
ms.author: riande
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 7f2d0b8fa8ac290e5018cb5237b1fedb5f40eeb0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754681"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="440cb-104">ASP.NET Web API 2.2 kullanarak bir OData v4 uç noktası oluşturma</span><span class="sxs-lookup"><span data-stu-id="440cb-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="440cb-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="440cb-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="440cb-106">Açık Veri Protokolü (OData), web için veri erişim protokolüdür.</span><span class="sxs-lookup"><span data-stu-id="440cb-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="440cb-107">OData sorgu ve veri kümeleri aracılığıyla CRUD işlemleri işlemek için Tekdüzen bir yol sağlar (oluşturma, okuma, güncelleştirme ve silme).</span><span class="sxs-lookup"><span data-stu-id="440cb-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="440cb-108">ASP.NET Web API, hem v3 hem de v4 protokolü destekler.</span><span class="sxs-lookup"><span data-stu-id="440cb-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="440cb-109">Yan yana çalışan bir v4 uç noktası bile olabilir v3 uç noktası ile.</span><span class="sxs-lookup"><span data-stu-id="440cb-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="440cb-110">Bu öğreticide, CRUD işlemleri destekleyen OData v4 uç noktası oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="440cb-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="440cb-111">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="440cb-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="440cb-112">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="440cb-112">Web API 2.2</span></span>
> - <span data-ttu-id="440cb-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="440cb-113">OData v4</span></span>
> - [<span data-ttu-id="440cb-114">Visual Studio 2013 Güncelleştirme 2</span><span class="sxs-lookup"><span data-stu-id="440cb-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="440cb-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="440cb-115">Entity Framework 6</span></span>
> - <span data-ttu-id="440cb-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="440cb-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="440cb-117">Öğretici sürümleri</span><span class="sxs-lookup"><span data-stu-id="440cb-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="440cb-118">OData sürüm 3 için bkz: [OData v3 uç noktası oluşturma](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="440cb-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="440cb-119">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="440cb-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="440cb-120">Visual Studio'da gelen **dosya** menüsünde **yeni** &gt; **proje**.</span><span class="sxs-lookup"><span data-stu-id="440cb-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="440cb-121">Genişletin **yüklü** &gt; **şablonları** &gt; **Visual C#** &gt; **Web**, seçin **ASP.NET Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="440cb-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="440cb-122">Projeyi adlandırın &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="440cb-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="440cb-123">İçinde **yeni proje** iletişim kutusunda **boş** şablonu.</span><span class="sxs-lookup"><span data-stu-id="440cb-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="440cb-124">Altında &quot;klasörleri ekleyin ve çekirdek başvuruları... &quot;, tıklayın **Web API**.</span><span class="sxs-lookup"><span data-stu-id="440cb-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="440cb-125">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="440cb-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="440cb-126">OData paketleri yükleme</span><span class="sxs-lookup"><span data-stu-id="440cb-126">Install the OData Packages</span></span>

<span data-ttu-id="440cb-127">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** &gt; **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="440cb-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="440cb-128">Paket Yöneticisi konsolu penceresinde yazın:</span><span class="sxs-lookup"><span data-stu-id="440cb-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="440cb-129">Bu komut, en son OData NuGet paketlerini yükler.</span><span class="sxs-lookup"><span data-stu-id="440cb-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="440cb-130">Bir Model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="440cb-130">Add a Model Class</span></span>

<span data-ttu-id="440cb-131">A *modeli* uygulamanızda bir veri varlığı temsil eden bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="440cb-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="440cb-132">Çözüm Gezgini'nde modeller klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="440cb-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="440cb-133">Bağlam menüsünden seçin **Ekle** &gt; **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="440cb-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="440cb-134">Kural olarak, model sınıfları modelleri klasörüne yerleştirilir ancak kendi projelerinizde bu kurala uymayan gerekmez.</span><span class="sxs-lookup"><span data-stu-id="440cb-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="440cb-135">Sınıf adını `Product`.</span><span class="sxs-lookup"><span data-stu-id="440cb-135">Name the class `Product`.</span></span> <span data-ttu-id="440cb-136">Product.cs dosyasındaki ortak kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="440cb-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="440cb-137">`Id` Özelliği Varlık anahtarı.</span><span class="sxs-lookup"><span data-stu-id="440cb-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="440cb-138">İstemciler varlıkları anahtara göre sorgulayabilir.</span><span class="sxs-lookup"><span data-stu-id="440cb-138">Clients can query entities by key.</span></span> <span data-ttu-id="440cb-139">Örneğin, 5 Kimliğine sahip bir ürün almak için URI değil `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="440cb-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="440cb-140">`Id` Özelliği birincil anahtar arka uç veritabanı olarak da olacaktır.</span><span class="sxs-lookup"><span data-stu-id="440cb-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="440cb-141">Entity Framework etkinleştir</span><span class="sxs-lookup"><span data-stu-id="440cb-141">Enable Entity Framework</span></span>

<span data-ttu-id="440cb-142">Bu öğreticide, Entity Framework (EF) Code First arka uç veritabanı oluşturmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="440cb-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="440cb-143">Web API OData EF gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="440cb-143">Web API OData does not require EF.</span></span> <span data-ttu-id="440cb-144">Veritabanı varlıklarını modellerini çevirebilir herhangi bir veri erişim katmanı'nı kullanın.</span><span class="sxs-lookup"><span data-stu-id="440cb-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="440cb-145">İlk olarak EF için NuGet paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="440cb-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="440cb-146">Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** &gt; **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="440cb-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="440cb-147">Paket Yöneticisi konsolu penceresinde yazın:</span><span class="sxs-lookup"><span data-stu-id="440cb-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="440cb-148">Web.config dosyasını açın ve içine aşağıdaki bölümü ekleyin **yapılandırma** öğesi, sonra **configSections** öğesi.</span><span class="sxs-lookup"><span data-stu-id="440cb-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="440cb-149">Bu ayar için bir LocalDB veritabanına bir bağlantı dizesi ekler.</span><span class="sxs-lookup"><span data-stu-id="440cb-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="440cb-150">Bu veritabanı, uygulamayı yerel olarak çalıştırdığınızda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="440cb-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="440cb-151">Ardından, adında bir sınıf ekleyin `ProductsContext` modeller klasörü için:</span><span class="sxs-lookup"><span data-stu-id="440cb-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="440cb-152">Oluşturucuya `"name=ProductsContext"` bağlantı dizesinin adını verir.</span><span class="sxs-lookup"><span data-stu-id="440cb-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="440cb-153">OData uç noktasını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="440cb-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="440cb-154">Uygulama dosyasını açın\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="440cb-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="440cb-155">Aşağıdaki **kullanarak** ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="440cb-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="440cb-156">Ardından aşağıdaki kodu ekleyin **kaydetme** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="440cb-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="440cb-157">Bu kod, iki şey yapar:</span><span class="sxs-lookup"><span data-stu-id="440cb-157">This code does two things:</span></span>

- <span data-ttu-id="440cb-158">Bir varlık veri modeli (EDM) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="440cb-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="440cb-159">Bir rota ekler.</span><span class="sxs-lookup"><span data-stu-id="440cb-159">Adds a route.</span></span>

<span data-ttu-id="440cb-160">Bir EDM soyut bir veri modelidir.</span><span class="sxs-lookup"><span data-stu-id="440cb-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="440cb-161">EDM hizmet meta verileri belgesi oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="440cb-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="440cb-162">**ODataConventionModelBuilder** sınıf varsayılan adlandırma kurallarını kullanarak bir EDM oluşturur.</span><span class="sxs-lookup"><span data-stu-id="440cb-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="440cb-163">Bu yaklaşım, en az kod gerektirir.</span><span class="sxs-lookup"><span data-stu-id="440cb-163">This approach requires the least code.</span></span> <span data-ttu-id="440cb-164">EDM üzerinde daha fazla denetim istiyorsanız kullanabileceğiniz **ODataModelBuilder** açıkça özellikleri, anahtarları ve gezinti özellikleri ekleyerek EDM oluşturmak için sınıf.</span><span class="sxs-lookup"><span data-stu-id="440cb-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="440cb-165">A *rota* Web API uç noktasına HTTP istekleri yönlendirme anlatır.</span><span class="sxs-lookup"><span data-stu-id="440cb-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="440cb-166">Bir OData v4 rota oluşturmak için arama **MapODataServiceRoute** genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="440cb-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="440cb-167">Uygulamanızı birden çok OData uç noktaları varsa, her biri için ayrı bir yol oluşturun.</span><span class="sxs-lookup"><span data-stu-id="440cb-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="440cb-168">Her yol, bir benzersiz rota adı ve ön eki ekleyin.</span><span class="sxs-lookup"><span data-stu-id="440cb-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="440cb-169">OData denetleyicisi Ekle</span><span class="sxs-lookup"><span data-stu-id="440cb-169">Add the OData Controller</span></span>

<span data-ttu-id="440cb-170">A *denetleyicisi* HTTP isteklerini işleyen sınıftır.</span><span class="sxs-lookup"><span data-stu-id="440cb-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="440cb-171">OData hizmetinizi her varlık için bir ayrı denetleyicisinin oluşturduğunuz.</span><span class="sxs-lookup"><span data-stu-id="440cb-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="440cb-172">Bu öğreticide, bir denetleyici için oluşturacağınız `Product` varlık.</span><span class="sxs-lookup"><span data-stu-id="440cb-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="440cb-173">Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın ve seçin **Ekle** &gt; **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="440cb-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="440cb-174">Sınıf adını `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="440cb-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="440cb-175">Bu öğretici için OData v3 kullanan sürümünü **denetleyici Ekle** yapı iskelesi.</span><span class="sxs-lookup"><span data-stu-id="440cb-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="440cb-176">Şu anda hiçbir yapı iskelesi OData v4 için yoktur.</span><span class="sxs-lookup"><span data-stu-id="440cb-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="440cb-177">ProductsController.cs Demirbaş kodu aşağıdakiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="440cb-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="440cb-178">Denetleyici kullandığı `ProductsContext` EF kullanarak veritabanına erişmek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="440cb-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="440cb-179">Denetleyici geçersiz kılan bildirimi **Dispose** yöntemi elden çıkarmak **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="440cb-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="440cb-180">Bu denetleyici için başlangıç noktasıdır.</span><span class="sxs-lookup"><span data-stu-id="440cb-180">This is the starting point for the controller.</span></span> <span data-ttu-id="440cb-181">Ardından, tüm CRUD işlemleri için yöntemler ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="440cb-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="440cb-182">Varlık kümesi sorgulama</span><span class="sxs-lookup"><span data-stu-id="440cb-182">Querying the Entity Set</span></span>

<span data-ttu-id="440cb-183">Aşağıdaki yöntemi ekleyin `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="440cb-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="440cb-184">Parametresiz sürümünü `Get` yöntemi tüm ürünleri koleksiyonu döndürür.</span><span class="sxs-lookup"><span data-stu-id="440cb-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="440cb-185">`Get` Yöntemi ile bir *anahtarı* parametresi anahtara göre bir ürün görünür (Bu durumda, `Id` özelliği).</span><span class="sxs-lookup"><span data-stu-id="440cb-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="440cb-186">**[EnableQuery]** özniteliği $filter $sort ve $page gibi sorgu Seçenekleri'ni kullanarak sorgu değiştirmek istemcileri etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="440cb-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="440cb-187">Daha fazla bilgi için [OData sorgu seçeneklerini destekleme](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="440cb-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="440cb-188">Varlık kümesine varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="440cb-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="440cb-189">Yeni ürün eklemek istemcileri etkinleştirmek için aşağıdaki yöntemi ekleyin. `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="440cb-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="440cb-190">Bir varlığı güncelleştirmek</span><span class="sxs-lookup"><span data-stu-id="440cb-190">Updating an Entity</span></span>

<span data-ttu-id="440cb-191">OData, düzeltme eki ve PUT bir varlığı güncelleştirmek için iki farklı semantikler destekler.</span><span class="sxs-lookup"><span data-stu-id="440cb-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="440cb-192">Düzeltme eki kısmi bir güncelleştirmesini yapar.</span><span class="sxs-lookup"><span data-stu-id="440cb-192">PATCH performs a partial update.</span></span> <span data-ttu-id="440cb-193">İstemci, yalnızca güncelleştirilecek özelliklerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="440cb-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="440cb-194">Tüm varlık PUT değiştirir.</span><span class="sxs-lookup"><span data-stu-id="440cb-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="440cb-195">PUT bir dezavantajı, istemci varlıkta değil değiştirme değerleri dahil olmak üzere tüm özellikler için değerleri göndermelisiniz olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="440cb-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="440cb-196">[OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) düzeltme eki tercih edilen olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="440cb-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="440cb-197">Herhangi bir durumda, hem düzeltme eki hem de PUT yöntemleri için kod şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="440cb-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="440cb-198">Düzeltme eki söz konusu olduğunda denetleyicisi kullanan **Delta&lt;T&gt;**  değişiklikleri izlemek için türü.</span><span class="sxs-lookup"><span data-stu-id="440cb-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="440cb-199">Bir varlığı silme</span><span class="sxs-lookup"><span data-stu-id="440cb-199">Deleting an Entity</span></span>

<span data-ttu-id="440cb-200">Bir ürün veritabanından silmek istemcileri etkinleştirmek için aşağıdaki yöntemi ekleyin. `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="440cb-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
