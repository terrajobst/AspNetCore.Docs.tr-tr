---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: "Web API 2 OData v3 uç noktası oluşturma | Microsoft Docs"
author: MikeWasson
description: "Açık Veri Protokolü (OData), web için veri erişim protokolüdür. OData veri yapısı, verileri sorgulamak ve verileri işlemek için Tekdüzen bir yol sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 33fe4d764bf9bf64c852f1269255925b5cc42536
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2018
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="a0294-104">Web API 2 OData v3 uç noktası oluşturma</span><span class="sxs-lookup"><span data-stu-id="a0294-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="a0294-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a0294-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a0294-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="a0294-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="a0294-107">[Açık veri Protokolü](http://www.odata.org/) (OData) web için bir veri erişim protokolüdür.</span><span class="sxs-lookup"><span data-stu-id="a0294-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="a0294-108">OData veri yapısı, verileri sorgulamak ve CRUD işlemleri üzerinden veri kümesi işlemek için Tekdüzen bir yol sağlar (oluşturma, okuma, güncelleştirme ve silme).</span><span class="sxs-lookup"><span data-stu-id="a0294-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="a0294-109">OData AtomPub (XML) ve JSON biçimleri destekler.</span><span class="sxs-lookup"><span data-stu-id="a0294-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="a0294-110">OData meta veriler hakkında kullanıma sunmak için bir yol da tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a0294-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="a0294-111">İstemciler, veri kümesi için ilişkileri ve tür bilgileri bulmak için meta verileri kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="a0294-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="a0294-112">ASP.NET Web API veri kümesi için bir OData uç nokta oluşturmak kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="a0294-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="a0294-113">Uç nokta tam olarak hangi OData işlemlerini destekleyen kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0294-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="a0294-114">Birden fazla OData uç noktası, OData olmayan uç noktaları yanında barındırabilir.</span><span class="sxs-lookup"><span data-stu-id="a0294-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="a0294-115">Veri modeli, arka uç iş mantığı ve veri katmanı üzerinde tam denetime sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a0294-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a0294-116">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="a0294-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="a0294-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a0294-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a0294-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="a0294-118">Web API 2</span></span>
> - <span data-ttu-id="a0294-119">OData sürüm 3</span><span class="sxs-lookup"><span data-stu-id="a0294-119">OData Version 3</span></span>
> - <span data-ttu-id="a0294-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a0294-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="a0294-121">Fiddler Web Proxy (isteğe bağlı) hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="a0294-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="a0294-122">Web API OData desteği eklendi [ASP.NET ve Web Araçları 2012.2 güncelleştirmesi](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="a0294-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="a0294-123">Ancak, bu öğreticide Visual Studio 2013'te eklendi yapı iskelesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="a0294-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="a0294-124">Bu öğreticide, istemciler sorgulayabilir basit bir OData uç noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a0294-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="a0294-125">Ayrıca bir C# istemci uç noktası için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a0294-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="a0294-126">Bu öğreticiyi tamamladıktan sonra sonraki öğreticileri kümesini Göster varlık ilişkileriyle, Eylemler, dahil olmak üzere daha fazla işlevsellik ekleme ve genişletin $/ $seçin.</span><span class="sxs-lookup"><span data-stu-id="a0294-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="a0294-127">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a0294-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="a0294-128">Bir varlık modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="a0294-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="a0294-129">Bir OData denetleyicisi ekleme</span><span class="sxs-lookup"><span data-stu-id="a0294-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="a0294-130">EDM ve rota ekleme</span><span class="sxs-lookup"><span data-stu-id="a0294-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="a0294-131">Çekirdek veritabanı (isteğe bağlı)</span><span class="sxs-lookup"><span data-stu-id="a0294-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="a0294-132">OData uç noktasını keşfetme</span><span class="sxs-lookup"><span data-stu-id="a0294-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="a0294-133">OData serileştirme biçimleri</span><span class="sxs-lookup"><span data-stu-id="a0294-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="a0294-134">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a0294-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="a0294-135">Bu öğreticide, temel CRUD işlemleri destekleyen OData uç noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a0294-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="a0294-136">Uç nokta ürünlerin listesini tek bir kaynak açığa çıkarır.</span><span class="sxs-lookup"><span data-stu-id="a0294-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="a0294-137">Sonraki öğreticileri daha fazla özellik ekler.</span><span class="sxs-lookup"><span data-stu-id="a0294-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="a0294-138">Visual Studio'yu başlatın ve seçin **yeni proje** başlangıç sayfasından.</span><span class="sxs-lookup"><span data-stu-id="a0294-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="a0294-139">Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="a0294-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="a0294-140">İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve Visual C# düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="a0294-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="a0294-141">Altında **Visual C#**seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="a0294-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="a0294-142">Seçin **ASP.NET Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="a0294-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="a0294-143">İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.</span><span class="sxs-lookup"><span data-stu-id="a0294-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="a0294-144">Altında &quot;klasörler ekleme ve çekirdek başvurular için... &quot;, denetleme **Web API**.</span><span class="sxs-lookup"><span data-stu-id="a0294-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="a0294-145">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a0294-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="a0294-146">Bir varlık modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="a0294-146">Add an Entity Model</span></span>

<span data-ttu-id="a0294-147">A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="a0294-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="a0294-148">Bu öğreticide, bir ürün temsil eden bir modeli gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0294-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="a0294-149">Model bizim OData varlık türüne karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a0294-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="a0294-150">Çözüm Gezgini'nde modeller klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a0294-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="a0294-151">Bağlam menüsünden seçin **Ekle** seçip **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="a0294-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="a0294-152">İçinde **yeni Ekle** öğesi iletişim, sınıf adını &quot;ürün&quot;.</span><span class="sxs-lookup"><span data-stu-id="a0294-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="a0294-153">Kurala göre modeli sınıfları modelleri klasörüne yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a0294-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="a0294-154">Bu kural, kendi projelerine izleyin gerekmez, ancak bu öğretici için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="a0294-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="a0294-155">Product.cs dosyasındaki şu sınıf tanımını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a0294-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="a0294-156">ID özelliği, varlık anahtarı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a0294-156">The ID property will be the entity key.</span></span> <span data-ttu-id="a0294-157">İstemcileri ürünleri kimliğe göre sorgulama yapabilirsiniz</span><span class="sxs-lookup"><span data-stu-id="a0294-157">Clients can query products by ID.</span></span> <span data-ttu-id="a0294-158">Bu alan, aynı zamanda arka uç veritabanı birincil anahtarda olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a0294-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="a0294-159">Projeyi şimdi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a0294-159">Build the project now.</span></span> <span data-ttu-id="a0294-160">Sonraki adımda, yansıma ürün türü bulmak için kullandığı bazı Visual Studio yapı iskelesi kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="a0294-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="a0294-161">Bir OData denetleyicisi ekleme</span><span class="sxs-lookup"><span data-stu-id="a0294-161">Add an OData Controller</span></span>

<span data-ttu-id="a0294-162">A *denetleyicisi* HTTP isteklerini işleyen sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a0294-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="a0294-163">Her varlık size OData hizmeti kümesi için ayrı bir denetleyici tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="a0294-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="a0294-164">Bu öğreticide, tek bir denetleyici oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="a0294-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="a0294-165">Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a0294-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="a0294-166">Seçin **Ekle** ve ardından **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="a0294-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="a0294-167">İçinde **İskele Ekle** iletişim kutusunda &quot;Web API 2 OData denetleyici Entity Framework kullanarak Eylemler ile&quot;.</span><span class="sxs-lookup"><span data-stu-id="a0294-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="a0294-168">İçinde **denetleyici Ekle** iletişim kutusunda, adı "ProductsController" denetleyici.</span><span class="sxs-lookup"><span data-stu-id="a0294-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="a0294-169">Seçin &quot;zaman uyumsuz denetleyici eylemlerini kullanmak&quot; onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="a0294-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="a0294-170">İçinde **modeli** aşağı açılan listesinde, ürün sınıfı seçin.</span><span class="sxs-lookup"><span data-stu-id="a0294-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="a0294-171">Tıklatın **yeni veri bağlamı...**  düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a0294-171">Click the **New data context...** button.</span></span> <span data-ttu-id="a0294-172">Veri içerik türü için varsayılan adı bırakın ve tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a0294-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="a0294-173">Denetleyicisi eklemek için denetleyici Ekle iletişim kutusunda Ekle'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a0294-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="a0294-174">Not: bir hata iletisi alırsanız bildiren &quot;türü alınırken bir hata oluştu... &quot;, ürün sınıfı eklendikten sonra Visual Studio projesi yerleşik emin olun.</span><span class="sxs-lookup"><span data-stu-id="a0294-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="a0294-175">Yapı iskelesi yansıma olan sınıfı bulmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="a0294-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="a0294-176">Yapı iskelesi iki kod dosyaları projeye ekler:</span><span class="sxs-lookup"><span data-stu-id="a0294-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="a0294-177">OData uç noktasını uygulayan Web API denetleyicisi Products.cs tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a0294-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="a0294-178">ProductServiceContext.cs Entity Framework kullanarak temel veritabanını sorgulamak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="a0294-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="a0294-179">EDM ve rota ekleme</span><span class="sxs-lookup"><span data-stu-id="a0294-179">Add the EDM and Route</span></span>

<span data-ttu-id="a0294-180">Çözüm Gezgini'nde, uygulama genişletin\_başlangıç klasörü ve WebApiConfig.cs adlı dosyayı açın.</span><span class="sxs-lookup"><span data-stu-id="a0294-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="a0294-181">Bu sınıf için Web API yapılandırma kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="a0294-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="a0294-182">Bu kodu aşağıdakilerle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="a0294-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="a0294-183">Bu kod iki işlemi yapar:</span><span class="sxs-lookup"><span data-stu-id="a0294-183">This code does two things:</span></span>

- <span data-ttu-id="a0294-184">OData uç noktası için varlık veri modeli (EDM) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a0294-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="a0294-185">Uç noktası için bir yol ekler.</span><span class="sxs-lookup"><span data-stu-id="a0294-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="a0294-186">Bir EDM verilerin soyut bir modelidir.</span><span class="sxs-lookup"><span data-stu-id="a0294-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="a0294-187">EDM meta veri belgesi oluşturmak ve hizmet için URI tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a0294-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="a0294-188">**ODataConventionModelBuilder** varsayılan adlandırma kuralları EDM kümesi kullanarak bir EDM oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a0294-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="a0294-189">Bu yaklaşım, en az kod gerektirir.</span><span class="sxs-lookup"><span data-stu-id="a0294-189">This approach requires the least code.</span></span> <span data-ttu-id="a0294-190">EDM üzerinde daha fazla denetim isterseniz, kullanabileceğiniz **ODataModelBuilder** açıkça özellikleri, anahtarları ve gezinti özellikleri ekleyerek EDM oluşturmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a0294-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="a0294-191">**EntitySet** yöntemi, bir varlık için EDM kümesine ekler:</span><span class="sxs-lookup"><span data-stu-id="a0294-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="a0294-192">"Ürünler" dizesini varlık kümesinin adını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="a0294-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="a0294-193">Denetleyicinin adını varlık kümesinin adı eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="a0294-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="a0294-194">Bu öğreticide, varlık kümesini "Ürünler" olarak adlandırılır ve denetleyici adlı `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="a0294-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="a0294-195">Adlı "ProductSet" varlık kümesi, denetleyici adı `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="a0294-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="a0294-196">Bir uç nokta birden çok varlık kümesine sahip olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a0294-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="a0294-197">Çağrı **EntitySet&lt;T&gt;**  her varlık için ayarlamak ve karşılık gelen bir denetleyici tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="a0294-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="a0294-198">**MapODataRoute** yöntemi OData uç noktası için bir yol ekler.</span><span class="sxs-lookup"><span data-stu-id="a0294-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="a0294-199">İlk parametre, yol için bir kolay addır.</span><span class="sxs-lookup"><span data-stu-id="a0294-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="a0294-200">Hizmetinizin istemciler bu adı görmezsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0294-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="a0294-201">İkinci parametre uç noktası için URI öneki ' dir.</span><span class="sxs-lookup"><span data-stu-id="a0294-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="a0294-202">Bu kod verildiğinde, ürünleri varlık kümesi için http:// URI'dir*ana bilgisayar adı*  /odata/ürünler.</span><span class="sxs-lookup"><span data-stu-id="a0294-202">Given this code, the URI for the Products entity set is http://*hostname*/odata/Products.</span></span> <span data-ttu-id="a0294-203">Uygulamanız birden fazla OData uç noktası olabilir.</span><span class="sxs-lookup"><span data-stu-id="a0294-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="a0294-204">Her uç noktası için çağrı **MapODataRoute** ve benzersiz rota adı ve benzersiz bir URI öneki belirtin.</span><span class="sxs-lookup"><span data-stu-id="a0294-204">For each endpoint, call **MapODataRoute** and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="a0294-205">Çekirdek veritabanı (isteğe bağlı)</span><span class="sxs-lookup"><span data-stu-id="a0294-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="a0294-206">Bu adımda, bazı test verilerini ile veritabanı oluşturmak için Entity Framework kullanır.</span><span class="sxs-lookup"><span data-stu-id="a0294-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="a0294-207">Bu adım isteğe bağlıdır, ancak OData uç noktanızı hemen test etmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a0294-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="a0294-208">Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="a0294-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a0294-209">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="a0294-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="a0294-210">Geçişler ve Configuration.cs adlı bir kod dosyası adlı bir klasör ekler.</span><span class="sxs-lookup"><span data-stu-id="a0294-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="a0294-211">Bu dosyayı açın ve aşağıdaki kodu ekleyin `Configuration.Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a0294-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="a0294-212">Paket Yöneticisi konsol penceresinde aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="a0294-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="a0294-213">Bu komutlar veritabanını oluşturur ve ardından bu kodu yürütür kodu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a0294-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="a0294-214">OData uç noktasını keşfetme</span><span class="sxs-lookup"><span data-stu-id="a0294-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="a0294-215">Bu bölümde, kullanacağız [Fiddler Web hata ayıklama Proxy](http://www.fiddler2.com) uç noktasına istekleri göndermek ve yanıt iletilerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="a0294-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="a0294-216">Bu, bir OData uç noktası özelliklerini anlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a0294-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="a0294-217">Visual Studio'da hata ayıklamayı başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="a0294-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="a0294-218">Varsayılan olarak, Visual Studio için tarayıcınızın açılır `http://localhost:*port*`, burada *bağlantı noktası* proje ayarlarında yapılandırılan bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="a0294-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="a0294-219">Bağlantı noktası numarası proje ayarları değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0294-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="a0294-220">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="a0294-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="a0294-221">Özellikler penceresinde seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="a0294-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="a0294-222">Altında bağlantı noktası numarası girin **proje URL'sini**.</span><span class="sxs-lookup"><span data-stu-id="a0294-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="a0294-223">Hizmet belgesi</span><span class="sxs-lookup"><span data-stu-id="a0294-223">Service Document</span></span>

<span data-ttu-id="a0294-224">*Hizmet belgesini* OData uç noktası için varlık kümeleri listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="a0294-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="a0294-225">Hizmet belgesini almak için hizmet URI'sini köküne GET isteği gönderin.</span><span class="sxs-lookup"><span data-stu-id="a0294-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="a0294-226">Fiddler, kullanarak girin aşağıdaki URI'de **Oluşturucu** sekmesi: `http://localhost:port/odata/`, burada *bağlantı noktası* bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="a0294-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="a0294-227">Tıklatın **yürütme** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a0294-227">Click the **Execute** button.</span></span> <span data-ttu-id="a0294-228">Fiddler uygulamanız için bir HTTP GET isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="a0294-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="a0294-229">Yanıt Web oturum listesinde görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a0294-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="a0294-230">Her şeyi çalışıyorsa, durum kodu 200 olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a0294-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="a0294-231">Denetçiler sekmesindeki yanıt iletisi ayrıntılarını görmek için Web oturumlarının listesini yanıtta çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="a0294-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="a0294-232">Ham HTTP yanıt iletisi aşağıdakine benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="a0294-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="a0294-233">Varsayılan olarak, Web API hizmet belgesini AtomPub biçiminde döndürür.</span><span class="sxs-lookup"><span data-stu-id="a0294-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="a0294-234">JSON İstek HTTP isteği aşağıdaki üstbilgi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a0294-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="a0294-235">Artık HTTP yanıtının JSON yükü içerir:</span><span class="sxs-lookup"><span data-stu-id="a0294-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="a0294-236">Hizmet meta veri belgesi</span><span class="sxs-lookup"><span data-stu-id="a0294-236">Service Metadata Document</span></span>

<span data-ttu-id="a0294-237">*Hizmeti meta veri belgesi* kavramsal şema tanım dili (CSDL) adlı bir XML dili kullanarak hizmeti veri modelini açıklar.</span><span class="sxs-lookup"><span data-stu-id="a0294-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="a0294-238">Meta veri belgesi hizmetinde verilerin yapısını gösterir ve istemci kodu oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a0294-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="a0294-239">Meta veri belgesi almak için bir GET isteği Gönder `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="a0294-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="a0294-240">Bu öğreticide gösterilen uç nokta için meta verileri aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="a0294-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="a0294-241">Varlık kümesi</span><span class="sxs-lookup"><span data-stu-id="a0294-241">Entity Set</span></span>

<span data-ttu-id="a0294-242">Ürünler varlık kümesini almak için bir GET isteği Gönder `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="a0294-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="a0294-243">Varlık</span><span class="sxs-lookup"><span data-stu-id="a0294-243">Entity</span></span>

<span data-ttu-id="a0294-244">Ayrı bir ürün almak için bir GET isteği Gönder `http://localhost:port/odata/Products(1)`, "1" Ürün Kimliği</span><span class="sxs-lookup"><span data-stu-id="a0294-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="a0294-245">OData serileştirme biçimleri</span><span class="sxs-lookup"><span data-stu-id="a0294-245">OData Serialization Formats</span></span>

<span data-ttu-id="a0294-246">OData birkaç seri hale getirme biçimi destekler:</span><span class="sxs-lookup"><span data-stu-id="a0294-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="a0294-247">Atom Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="a0294-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="a0294-248">JSON "ışık (OData v3 sunulan)"</span><span class="sxs-lookup"><span data-stu-id="a0294-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="a0294-249">JSON "ayrıntılı" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="a0294-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="a0294-250">Varsayılan olarak, Web API AtomPubJSON "açık" biçimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="a0294-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="a0294-251">Accept üstbilgisi AtomPub biçimi almak için "application/atom + xml için" ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a0294-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="a0294-252">Bir örnek yanıt gövdesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="a0294-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="a0294-253">Atom biçimi belirgin bir dezavantajı görebilirsiniz: JSON light çok daha ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="a0294-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="a0294-254">AtomPub özelliğini algılayan bir istemciniz varsa, ancak, istemci bu biçimi JSON tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a0294-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="a0294-255">Aynı varlık JSON light sürümü şöyledir:</span><span class="sxs-lookup"><span data-stu-id="a0294-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="a0294-256">JSON light biçimi sürüm OData Protokolü 3 sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="a0294-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="a0294-257">Geriye dönük uyumluluk için bir istemci eski "ayrıntılı" JSON biçimi isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="a0294-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="a0294-258">Ayrıntılı JSON istek için Accept üstbilgisi ayarlamak `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="a0294-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="a0294-259">Ayrıntılı sürüm şöyledir:</span><span class="sxs-lookup"><span data-stu-id="a0294-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="a0294-260">Bu biçim önemli ölçüde gider tüm oturumu üzerinden ekleyebilirsiniz yanıt gövdesi içinde daha fazla meta veri iletir.</span><span class="sxs-lookup"><span data-stu-id="a0294-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="a0294-261">Ayrıca, "d" adlı bir özellik nesne kaydırma tarafından yöneltme düzeyini yükseltir.</span><span class="sxs-lookup"><span data-stu-id="a0294-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0294-262">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="a0294-262">Next Steps</span></span>

- [<span data-ttu-id="a0294-263">Varlık İlişkileriyle ekleme</span><span class="sxs-lookup"><span data-stu-id="a0294-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="a0294-264">OData eylem ekleyin</span><span class="sxs-lookup"><span data-stu-id="a0294-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="a0294-265">OData hizmeti bir .NET istemciden çağırın</span><span class="sxs-lookup"><span data-stu-id="a0294-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
