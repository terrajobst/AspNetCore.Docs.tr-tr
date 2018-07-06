---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: İle Web API 2 OData v3 uç noktası oluşturma | Microsoft Docs
author: MikeWasson
description: Açık Veri Protokolü (OData), web için veri erişim protokolüdür. OData veri yapısı, verileri sorgulamak ve verileri işlemek için Tekdüzen bir yol sağlar...
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 67a41f37a09d089336dc96d393f929e56661c4f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813714"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="8bc98-104">İle Web API 2 OData v3 uç noktası oluşturma</span><span class="sxs-lookup"><span data-stu-id="8bc98-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="8bc98-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8bc98-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8bc98-106">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="8bc98-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="8bc98-107">[Açık veri Protokolü](http://www.odata.org/) (OData) web için bir veri erişim protokolüdür.</span><span class="sxs-lookup"><span data-stu-id="8bc98-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="8bc98-108">OData veri yapısı, verileri sorgulamak ve veri kümesi üzerinden CRUD işlemleri işlemek için Tekdüzen bir yol sağlar (oluşturma, okuma, güncelleştirme ve silme).</span><span class="sxs-lookup"><span data-stu-id="8bc98-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="8bc98-109">OData AtomPub (XML) hem de JSON biçimlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="8bc98-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="8bc98-110">OData veri hakkındaki meta verileri kullanıma sunmak için bir yol da tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8bc98-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="8bc98-111">İstemciler, tür bilgileri ve veri kümesi için ilişkileri keşfetmek için meta verileri kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
> 
> <span data-ttu-id="8bc98-112">ASP.NET Web API OData uç noktası için bir veri kümesi oluşturmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="8bc98-113">Uç nokta tam olarak hangi OData işlemleri destekleyen denetleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8bc98-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="8bc98-114">OData olmayan uç noktaları yanı sıra birden çok OData uç barındırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8bc98-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="8bc98-115">Size, veri modeli, arka uç iş mantığı ve veri katmanı üzerinde tam denetime sahiptir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8bc98-116">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="8bc98-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="8bc98-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8bc98-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="8bc98-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="8bc98-118">Web API 2</span></span>
> - <span data-ttu-id="8bc98-119">OData sürüm 3</span><span class="sxs-lookup"><span data-stu-id="8bc98-119">OData Version 3</span></span>
> - <span data-ttu-id="8bc98-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8bc98-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="8bc98-121">Fiddler'ı Web Proxy (isteğe bağlı) hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="8bc98-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
> 
> <span data-ttu-id="8bc98-122">Web API OData desteği eklendi [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="8bc98-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="8bc98-123">Ancak, Bu öğretici, Visual Studio 2013'te eklenmiş olan yapı iskelesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="8bc98-124">Bu öğreticide, istemciler sorgulayabilir basit bir OData uç noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8bc98-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="8bc98-125">Ayrıca bir C# istemci uç noktası için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8bc98-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="8bc98-126">Bu öğreticiyi tamamladıktan sonra sonraki öğreticiler kümesini Göster varlık ilişkileri, Eylemler dahil olmak üzere daha fazla işlevsellik ekleme ve genişletin $/ $seçin.</span><span class="sxs-lookup"><span data-stu-id="8bc98-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="8bc98-127">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="8bc98-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="8bc98-128">Varlık modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="8bc98-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="8bc98-129">Bir OData denetleyicisi Ekle</span><span class="sxs-lookup"><span data-stu-id="8bc98-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="8bc98-130">EDM ve rota Ekle</span><span class="sxs-lookup"><span data-stu-id="8bc98-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="8bc98-131">(İsteğe bağlı) veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="8bc98-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="8bc98-132">OData uç noktasını keşfetme</span><span class="sxs-lookup"><span data-stu-id="8bc98-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="8bc98-133">OData serileştirme biçimleri</span><span class="sxs-lookup"><span data-stu-id="8bc98-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="8bc98-134">Visual Studio projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="8bc98-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="8bc98-135">Bu öğreticide, temel CRUD işlemleri destekleyen OData uç noktası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8bc98-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="8bc98-136">Uç nokta ürünlerin listesini tek bir kaynak açığa çıkarır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="8bc98-137">Sonraki öğreticiler daha fazla özellik ekler.</span><span class="sxs-lookup"><span data-stu-id="8bc98-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="8bc98-138">Visual Studio'yu başlatın ve seçin **yeni proje** başlangıç sayfasından.</span><span class="sxs-lookup"><span data-stu-id="8bc98-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="8bc98-139">Veya **dosya** menüsünde **yeni** ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="8bc98-140">İçinde **şablonları** bölmesinde **yüklü şablonlar** ve Visual C# düğümünü genişletin.</span><span class="sxs-lookup"><span data-stu-id="8bc98-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="8bc98-141">Altında **Visual C#** seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="8bc98-142">Seçin **ASP.NET Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="8bc98-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="8bc98-143">İçinde **yeni ASP.NET projesi** iletişim kutusunda **boş** şablonu.</span><span class="sxs-lookup"><span data-stu-id="8bc98-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="8bc98-144">Altında &quot;klasörleri ekleyin ve çekirdek başvuruları... &quot;, kontrol **Web API**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="8bc98-145">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="8bc98-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="8bc98-146">Varlık modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="8bc98-146">Add an Entity Model</span></span>

<span data-ttu-id="8bc98-147">A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="8bc98-148">Bu öğreticide, bir ürünü temsil eden bir model oluşturmamız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="8bc98-149">Model OData varlık türüne karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="8bc98-150">Çözüm Gezgini'nde modeller klasörü sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="8bc98-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="8bc98-151">Bağlam menüsünden seçin **Ekle** seçip **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="8bc98-152">İçinde **yeni Ekle** öğesi iletişim, sınıf adını &quot;ürün&quot;.</span><span class="sxs-lookup"><span data-stu-id="8bc98-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="8bc98-153">Kural gereği, model sınıfları modelleri klasörüne yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="8bc98-154">Bu kurala projelerinizi izleyin gerekmez, ancak bu öğretici için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="8bc98-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="8bc98-155">Product.cs dosyasındaki aşağıdaki sınıf tanımını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8bc98-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="8bc98-156">ID özelliği, varlık anahtarı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-156">The ID property will be the entity key.</span></span> <span data-ttu-id="8bc98-157">İstemciler ürün kimliğine göre sorgulayabilir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-157">Clients can query products by ID.</span></span> <span data-ttu-id="8bc98-158">Bu alan ayrıca arka uç veritabanı birincil anahtar olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="8bc98-159">Şimdi, projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="8bc98-159">Build the project now.</span></span> <span data-ttu-id="8bc98-160">Sonraki adımda, ürün türü bulmak için yansıtma kullanan bazı Visual Studio yapı iskelesi kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="8bc98-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="8bc98-161">Bir OData denetleyicisi Ekle</span><span class="sxs-lookup"><span data-stu-id="8bc98-161">Add an OData Controller</span></span>

<span data-ttu-id="8bc98-162">A *denetleyicisi* HTTP isteklerini işleyen sınıftır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="8bc98-163">İçinde OData hizmeti ayarladığınız her varlık için bir ayrı denetleyicisinin tanımlarsınız.</span><span class="sxs-lookup"><span data-stu-id="8bc98-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="8bc98-164">Bu öğreticide, tek bir denetleyici oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="8bc98-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="8bc98-165">Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8bc98-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="8bc98-166">Seçin **Ekle** seçip **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="8bc98-167">İçinde **İskele Ekle** iletişim kutusunda &quot;Web API 2 OData denetleyici Entity Framework kullanarak Eylemler ile&quot;.</span><span class="sxs-lookup"><span data-stu-id="8bc98-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="8bc98-168">İçinde **denetleyici Ekle** iletişim kutusunda, "ProductsController" denetleyicinin adı.</span><span class="sxs-lookup"><span data-stu-id="8bc98-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="8bc98-169">Seçin &quot;zaman uyumsuz denetleyici eylemlerini kullanmak&quot; onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="8bc98-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="8bc98-170">İçinde **modeli** aşağı açılan listesinde, ürün sınıfı seçin.</span><span class="sxs-lookup"><span data-stu-id="8bc98-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="8bc98-171">Tıklayın **yeni veri bağlamı...**  düğmesi.</span><span class="sxs-lookup"><span data-stu-id="8bc98-171">Click the **New data context...** button.</span></span> <span data-ttu-id="8bc98-172">Veri bağlamı türü için varsayılan adı bırakın ve tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="8bc98-173">Denetleyici Ekle iletişim kutusunda denetleyicisi eklemek için Ekle'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8bc98-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="8bc98-174">Not: bir hata iletisi alırsanız bildiren &quot;türü alınırken bir hata oluştu... &quot;, ürün sınıfı eklendikten sonra Visual Studio projesini yerleşik olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="8bc98-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="8bc98-175">Yapı iskelesi sınıfı bulmak için yansıtma kullanır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="8bc98-176">Yapı iskelesi iki kod dosyasını projeye ekler:</span><span class="sxs-lookup"><span data-stu-id="8bc98-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="8bc98-177">OData uç noktasını uygulayan bir Web API denetleyicisi Products.cs tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8bc98-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="8bc98-178">ProductServiceContext.cs Entity Framework kullanarak temel alınan veritabanını sorgulamak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="8bc98-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="8bc98-179">EDM ve rota Ekle</span><span class="sxs-lookup"><span data-stu-id="8bc98-179">Add the EDM and Route</span></span>

<span data-ttu-id="8bc98-180">Çözüm Gezgini'nde uygulama genişletin\_başlangıç klasörü ve WebApiConfig.cs adlı dosyayı açın.</span><span class="sxs-lookup"><span data-stu-id="8bc98-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="8bc98-181">Bu sınıf, Web API'si için yapılandırma kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="8bc98-182">Bu kodu aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="8bc98-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="8bc98-183">Bu kod, iki şey yapar:</span><span class="sxs-lookup"><span data-stu-id="8bc98-183">This code does two things:</span></span>

- <span data-ttu-id="8bc98-184">OData uç noktasını bir varlık veri modeli (EDM) oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8bc98-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="8bc98-185">Uç nokta için bir rota ekler.</span><span class="sxs-lookup"><span data-stu-id="8bc98-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="8bc98-186">Bir EDM soyut bir veri modelidir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="8bc98-187">EDM meta verileri belgesi oluşturun ve hizmet için bir URI'leri tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="8bc98-188">**ODataConventionModelBuilder** varsayılan adlandırma kuralları EDM kümesini kullanarak bir EDM oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8bc98-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="8bc98-189">Bu yaklaşım, en az kod gerektirir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-189">This approach requires the least code.</span></span> <span data-ttu-id="8bc98-190">EDM üzerinde daha fazla denetim istiyorsanız kullanabileceğiniz **ODataModelBuilder** açıkça özellikleri, anahtarları ve gezinti özellikleri ekleyerek EDM oluşturmak için sınıf.</span><span class="sxs-lookup"><span data-stu-id="8bc98-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="8bc98-191">**EntitySet** yöntemi varlık kümesi için EDM ekler:</span><span class="sxs-lookup"><span data-stu-id="8bc98-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="8bc98-192">"Ürünler" dizesini varlık kümesinin adını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8bc98-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="8bc98-193">Denetleyicinin adı, varlık kümesinin adı eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="8bc98-194">Bu öğreticide, varlık kümesini "Ürünler" olarak adlandırılır ve denetleyici adlı `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="8bc98-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="8bc98-195">Adlı varlık "ProductSet" kümesi, denetleyici garip gelse `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="8bc98-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="8bc98-196">Bir uç nokta birden fazla varlık kümesine sahip olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="8bc98-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="8bc98-197">Çağrı **EntitySet&lt;T&gt;**  her varlık için ayarlamak ve karşılık gelen bir denetleyici tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8bc98-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="8bc98-198">**MapODataRoute** yöntemi OData uç noktası için bir rota ekler.</span><span class="sxs-lookup"><span data-stu-id="8bc98-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="8bc98-199">İlk parametre, rota için kolay bir addır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="8bc98-200">Hizmetinizin istemciler bu adı görmez.</span><span class="sxs-lookup"><span data-stu-id="8bc98-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="8bc98-201">Uç noktası için URI öneki ikinci parametredir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="8bc98-202">Bu kodu göz önünde bulundurulduğunda, ürünleri varlık kümesi için http:// URI'dir<em>hostname</em>  /odata/ürünleri.</span><span class="sxs-lookup"><span data-stu-id="8bc98-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="8bc98-203">Uygulamanız birden fazla OData uç noktası olabilir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="8bc98-204">Her uç nokta için çağrı <strong>MapODataRoute</strong> ve benzersiz rota adı ve benzersiz bir URI öneki belirtin.</span><span class="sxs-lookup"><span data-stu-id="8bc98-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="8bc98-205">(İsteğe bağlı) veritabanının çekirdeğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="8bc98-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="8bc98-206">Bu adımda, bazı test verileri ile veritabanının çekirdeğini oluşturma için Entity Framework kullanır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="8bc98-207">Bu adım isteğe bağlıdır, ancak OData uç noktanızı hemen test etmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="8bc98-208">Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-208">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8bc98-209">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="8bc98-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="8bc98-210">Bu, geçiş ve Configuration.cs adlı bir kod dosyası adlı bir klasör ekler.</span><span class="sxs-lookup"><span data-stu-id="8bc98-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="8bc98-211">Bu dosyayı açın ve aşağıdaki kodu ekleyin `Configuration.Seed` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8bc98-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="8bc98-212">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="8bc98-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="8bc98-213">Bu komutlar veritabanı oluşturur ve sonra bu kodu yürüten bir kod oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8bc98-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="8bc98-214">OData uç noktasını keşfetme</span><span class="sxs-lookup"><span data-stu-id="8bc98-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="8bc98-215">Bu bölümde, kullanacağız [Fiddler Web hata ayıklama proxy'si](http://www.fiddler2.com) istekleri uç noktasına göndermesi ve yanıt iletilerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="8bc98-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="8bc98-216">Bu, OData uç noktası yeteneklerini anlamalarına yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="8bc98-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="8bc98-217">Visual Studio'da hata ayıklamayı başlatmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8bc98-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="8bc98-218">Varsayılan olarak, Visual Studio için tarayıcınızı açar `http://localhost:*port*`burada *bağlantı noktası* proje ayarlarında yapılandırılan bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="8bc98-219">Proje ayarlarında bağlantı noktası numarasını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8bc98-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="8bc98-220">Çözüm Gezgini'nde projeye sağ tıklayıp seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="8bc98-221">Özellikler penceresinde seçin **Web**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="8bc98-222">Altında bağlantı noktası numarasını girin **proje URL'si**.</span><span class="sxs-lookup"><span data-stu-id="8bc98-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="8bc98-223">Hizmet belgesi</span><span class="sxs-lookup"><span data-stu-id="8bc98-223">Service Document</span></span>

<span data-ttu-id="8bc98-224">*Hizmet belgesi* OData uç noktası için varlık kümeleri listesini içerir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="8bc98-225">Hizmet belgesi almak için kök URI'sine hizmetinin bir GET isteği gönderin.</span><span class="sxs-lookup"><span data-stu-id="8bc98-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="8bc98-226">Fiddler, kullanarak aşağıdaki URI'de girin **Oluşturucusu** sekmesi: `http://localhost:port/odata/`burada *bağlantı noktası* bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="8bc98-227">Tıklayın **yürütme** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="8bc98-227">Click the **Execute** button.</span></span> <span data-ttu-id="8bc98-228">Fiddler, uygulamanız için bir HTTP GET isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="8bc98-229">Yanıt Web oturum listesinde görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="8bc98-230">Her şeyin çalıştığından, durum kodu 200 olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="8bc98-231">Denetçiler sekmesinde yanıt iletisinin ayrıntıları görmek için Web oturumları listesi yanıtta çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="8bc98-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="8bc98-232">Ham HTTP yanıt iletisi, aşağıdakine benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="8bc98-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="8bc98-233">Varsayılan olarak, Web API, hizmet belgesi AtomPub biçiminde döndürür.</span><span class="sxs-lookup"><span data-stu-id="8bc98-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="8bc98-234">JSON istek için HTTP isteği aşağıdaki üstbilgi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8bc98-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="8bc98-235">Artık HTTP yanıtının JSON yükü içerir:</span><span class="sxs-lookup"><span data-stu-id="8bc98-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="8bc98-236">Hizmet meta verileri belgesi</span><span class="sxs-lookup"><span data-stu-id="8bc98-236">Service Metadata Document</span></span>

<span data-ttu-id="8bc98-237">*Hizmet meta verileri belgesi* kavramsal şema tanım dili (CSDL) adlı bir XML dili kullanarak hizmeti veri modelini açıklar.</span><span class="sxs-lookup"><span data-stu-id="8bc98-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="8bc98-238">Meta veri belgesi hizmette verilerin yapısını gösterir ve istemci kodu oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8bc98-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="8bc98-239">Meta veri belgesi almak için GET isteğini göndermek `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="8bc98-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="8bc98-240">Bu öğreticide gösterilen uç nokta için meta veriler aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="8bc98-241">Varlık kümesi</span><span class="sxs-lookup"><span data-stu-id="8bc98-241">Entity Set</span></span>

<span data-ttu-id="8bc98-242">Ürünleri varlık kümesini almak için bir GET isteğini göndermek `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="8bc98-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="8bc98-243">Varlık</span><span class="sxs-lookup"><span data-stu-id="8bc98-243">Entity</span></span>

<span data-ttu-id="8bc98-244">Tek bir ürün almak için bir GET isteği gönderme `http://localhost:port/odata/Products(1)`, "1" Ürün Kimliği</span><span class="sxs-lookup"><span data-stu-id="8bc98-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="8bc98-245">OData serileştirme biçimleri</span><span class="sxs-lookup"><span data-stu-id="8bc98-245">OData Serialization Formats</span></span>

<span data-ttu-id="8bc98-246">OData çeşitli serileştirme biçimleri destekler:</span><span class="sxs-lookup"><span data-stu-id="8bc98-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="8bc98-247">Atom Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="8bc98-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="8bc98-248">JSON "açık" (OData v3 sürümünde sunulan)</span><span class="sxs-lookup"><span data-stu-id="8bc98-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="8bc98-249">JSON "ayrıntılı" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="8bc98-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="8bc98-250">Varsayılan olarak, Web API'si AtomPubJSON "açık" biçimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-250">By default, Web API uses AtomPubJSON "light" format.</span></span> 

<span data-ttu-id="8bc98-251">Accept üst bilgisi AtomPub biçimi almak için "application/atom + xml" ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8bc98-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="8bc98-252">Bir örnek yanıt gövdesi şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="8bc98-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="8bc98-253">Atom biçimi belirgin bir dezavantajı görebilirsiniz: JSON light çok daha ayrıntılıdır.</span><span class="sxs-lookup"><span data-stu-id="8bc98-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="8bc98-254">AtomPub anlayan bir istemciniz varsa, ancak istemci, biçimi JSON tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8bc98-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="8bc98-255">Aynı varlık JSON light hali aşağıdadır:</span><span class="sxs-lookup"><span data-stu-id="8bc98-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="8bc98-256">JSON light biçimini OData Protokolü 3. sürümü kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="8bc98-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="8bc98-257">Geriye dönük uyumluluk için "ayrıntılı" eski JSON biçiminde bir istemcinin isteyebileceği.</span><span class="sxs-lookup"><span data-stu-id="8bc98-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="8bc98-258">Ayrıntılı JSON istek için Accept üst bilgisi ayarlamak `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="8bc98-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="8bc98-259">Ayrıntılı sürüm şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="8bc98-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="8bc98-260">Bu biçim, önemli ölçüde gider tamamını bir oturum üzerinden ekleyebilirsiniz. yanıt gövdesi içinde daha fazla meta veri sağlar.</span><span class="sxs-lookup"><span data-stu-id="8bc98-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="8bc98-261">Ayrıca, "d" adlı bir özelliğe nesne sarmalama tarafından yöneltme düzeyi ekler.</span><span class="sxs-lookup"><span data-stu-id="8bc98-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bc98-262">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="8bc98-262">Next Steps</span></span>

- [<span data-ttu-id="8bc98-263">Varlık ilişkilerini ekleyin</span><span class="sxs-lookup"><span data-stu-id="8bc98-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="8bc98-264">OData eylemleri ekleme</span><span class="sxs-lookup"><span data-stu-id="8bc98-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="8bc98-265">Bir .NET istemcisinden OData hizmetine çağrı</span><span class="sxs-lookup"><span data-stu-id="8bc98-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
