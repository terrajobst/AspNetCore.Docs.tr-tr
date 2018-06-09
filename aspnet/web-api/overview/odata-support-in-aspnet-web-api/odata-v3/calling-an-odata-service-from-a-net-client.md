---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Bir .NET İstemci'den (C#) bir OData hizmeti çağırma | Microsoft Docs
author: MikeWasson
description: Bu öğretici, C# istemci uygulamasından OData hizmetine çağrı gösterilmektedir. Eğitmen Visual Studio 2013 (Visual S. çalışır.. kullanılan yazılım sürümleri
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28042400"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="2f198-104">Bir OData hizmeti çağrılırken bir .NET İstemci'den (C#)</span><span class="sxs-lookup"><span data-stu-id="2f198-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="2f198-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2f198-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2f198-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="2f198-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="2f198-107">Bu öğretici, C# istemci uygulamasından OData hizmetine çağrı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2f198-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2f198-108">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="2f198-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2f198-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 ile çalışır)</span><span class="sxs-lookup"><span data-stu-id="2f198-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="2f198-110">WCF Veri Hizmetleri İstemci Kitaplığı</span><span class="sxs-lookup"><span data-stu-id="2f198-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="2f198-111">Web API 2.</span><span class="sxs-lookup"><span data-stu-id="2f198-111">Web API 2.</span></span> <span data-ttu-id="2f198-112">(OData hizmeti örnek Web API 2 kullanılarak oluşturulur, ancak istemci uygulamasının Web API bağlı değildir.)</span><span class="sxs-lookup"><span data-stu-id="2f198-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="2f198-113">Bu öğreticide, ı OData hizmetine çağıran bir istemci uygulaması oluşturmada size yol.</span><span class="sxs-lookup"><span data-stu-id="2f198-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="2f198-114">OData hizmeti aşağıdaki varlıklar sunar:</span><span class="sxs-lookup"><span data-stu-id="2f198-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="2f198-115">Aşağıdaki makaleler Web API'de OData hizmeti uygulama açıklar.</span><span class="sxs-lookup"><span data-stu-id="2f198-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="2f198-116">(Bu öğretici, ancak anlamak için bunları okuyun gerekmez.)</span><span class="sxs-lookup"><span data-stu-id="2f198-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="2f198-117">Web API 2 OData uç noktası oluşturma</span><span class="sxs-lookup"><span data-stu-id="2f198-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="2f198-118">Web API 2 OData varlık ilişkileri</span><span class="sxs-lookup"><span data-stu-id="2f198-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="2f198-119">Web API 2’de OData Eylemleri</span><span class="sxs-lookup"><span data-stu-id="2f198-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="2f198-120">Hizmet proxy'si oluştur</span><span class="sxs-lookup"><span data-stu-id="2f198-120">Generate the Service Proxy</span></span>

<span data-ttu-id="2f198-121">İlk adım, bir hizmeti proxy'si oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="2f198-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="2f198-122">Hizmet proxy'si OData hizmetine erişmek için yöntemler tanımlayan bir .NET sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="2f198-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="2f198-123">Proxy HTTP istek yöntemi çağrılarını çevirir.</span><span class="sxs-lookup"><span data-stu-id="2f198-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="2f198-124">Visual Studio'da OData hizmeti projesi açarak başlayın.</span><span class="sxs-lookup"><span data-stu-id="2f198-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="2f198-125">IIS Express'te hizmeti yerel olarak çalıştırmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="2f198-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="2f198-126">Visual Studio atar bağlantı noktası numarasını da dahil olmak üzere yerel adres unutmayın.</span><span class="sxs-lookup"><span data-stu-id="2f198-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="2f198-127">Proxy oluşturduğunuzda bu adresi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2f198-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="2f198-128">Ardından, Visual Studio başka bir örneği açın ve konsol uygulama projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2f198-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="2f198-129">Konsol uygulaması OData istemci uygulamamız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2f198-129">The console application will be our OData client application.</span></span> <span data-ttu-id="2f198-130">(Ayrıca projenin hizmet olarak aynı çözüme ekleyebileceğiniz.)</span><span class="sxs-lookup"><span data-stu-id="2f198-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="2f198-131">Geri kalan adımları konsol projesi bakın.</span><span class="sxs-lookup"><span data-stu-id="2f198-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="2f198-132">Çözüm Gezgini'nde sağ **başvuruları** seçip **hizmet Başvurusu Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2f198-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="2f198-133">İçinde **hizmet Başvurusu Ekle** iletişim kutusunda, OData hizmeti adresini yazın:</span><span class="sxs-lookup"><span data-stu-id="2f198-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="2f198-134">Burada *bağlantı noktası* bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="2f198-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="2f198-135">İçin **Namespace**, "ProductService" yazın.</span><span class="sxs-lookup"><span data-stu-id="2f198-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="2f198-136">Bu seçenek proxy sınıfının ad alanını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="2f198-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="2f198-137">tıklatın **Git**.</span><span class="sxs-lookup"><span data-stu-id="2f198-137">Click **Go**.</span></span> <span data-ttu-id="2f198-138">Visual Studio hizmet varlıkları bulmak için OData meta veri belgesini okur.</span><span class="sxs-lookup"><span data-stu-id="2f198-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="2f198-139">Tıklatın **Tamam** proxy sınıfı projenize eklemek için.</span><span class="sxs-lookup"><span data-stu-id="2f198-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="2f198-140">Hizmeti Proxy sınıfı örneği oluşturun</span><span class="sxs-lookup"><span data-stu-id="2f198-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="2f198-141">İçinde `Main` yöntemi, proxy sınıfının yeni bir örneğini gibi oluşturun:</span><span class="sxs-lookup"><span data-stu-id="2f198-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="2f198-142">Yeniden hizmetinizi çalıştığı gerçek bağlantı noktası numarasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="2f198-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="2f198-143">Hizmetinizi dağıttığınızda, Canlı hizmet URI'sini kullanır.</span><span class="sxs-lookup"><span data-stu-id="2f198-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="2f198-144">Proxy güncelleştirme gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="2f198-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="2f198-145">Aşağıdaki kod, istek URI konsol penceresine yazdırır bir olay işleyicisi ekler.</span><span class="sxs-lookup"><span data-stu-id="2f198-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="2f198-146">Bu adım gerekli değildir, ancak her sorgu için URI görmek ilginç olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2f198-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="2f198-147">Hizmet sorgulama</span><span class="sxs-lookup"><span data-stu-id="2f198-147">Query the Service</span></span>

<span data-ttu-id="2f198-148">Aşağıdaki kod OData hizmetinden ürünlerin listesini alır.</span><span class="sxs-lookup"><span data-stu-id="2f198-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="2f198-149">HTTP isteği göndermek veya yanıtı ayrıştırılamadı için herhangi bir kodun yazılması gerekmez dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="2f198-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="2f198-150">Ne zaman, listeleme proxy sınıfı bu otomatik olarak mu `Container.Products` koleksiyonunda **foreach** döngü.</span><span class="sxs-lookup"><span data-stu-id="2f198-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="2f198-151">Uygulamayı çalıştırdığınızda, çıktı aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="2f198-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="2f198-152">Bir varlığı Kimliğe göre almak için bir `where` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="2f198-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="2f198-153">Bu konunun geri kalanı için t tüm göstermeyecektir `Main` işlev, hizmeti çağırmak için gerekli olan kodu.</span><span class="sxs-lookup"><span data-stu-id="2f198-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="2f198-154">Sorgu Seçenekleri Uygula</span><span class="sxs-lookup"><span data-stu-id="2f198-154">Apply Query Options</span></span>

<span data-ttu-id="2f198-155">OData tanımlar [sorgu seçenekleri](../supporting-odata-query-options.md) kullanılabilecek filtre, sıralama, sayfa verilerine ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="2f198-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="2f198-156">Hizmet proxy'si çeşitli LINQ ifadeleri kullanarak bu seçenekleri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f198-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="2f198-157">Bu bölümde, ı kısa örnekler göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="2f198-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="2f198-158">Daha fazla ayrıntı için Ek Yardım konusuna [LINQ konuları (WCF Veri Hizmetleri)](https://msdn.microsoft.com/library/ee622463.aspx) MSDN'de.</span><span class="sxs-lookup"><span data-stu-id="2f198-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="2f198-159">Filtreleme ($filter)</span><span class="sxs-lookup"><span data-stu-id="2f198-159">Filtering ($filter)</span></span>

<span data-ttu-id="2f198-160">Filtrelemek için kullanmak bir `where` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="2f198-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="2f198-161">Aşağıdaki örnek filtreleri ürün kategorisine göre.</span><span class="sxs-lookup"><span data-stu-id="2f198-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="2f198-162">Bu kod, aşağıdaki OData sorgu karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="2f198-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="2f198-163">Proxy dönüştürür bildirimi `where` yan tümcesine bir OData `$filter` ifade.</span><span class="sxs-lookup"><span data-stu-id="2f198-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="2f198-164">Sıralama ($orderby)</span><span class="sxs-lookup"><span data-stu-id="2f198-164">Sorting ($orderby)</span></span>

<span data-ttu-id="2f198-165">Sıralamak için kullanmak bir `orderby` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="2f198-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="2f198-166">Aşağıdaki örnek yüksekten en düşüğe fiyat göre sıralar.</span><span class="sxs-lookup"><span data-stu-id="2f198-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="2f198-167">Burada, karşılık gelen OData isteği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2f198-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="2f198-168">İstemci-tarafı disk belleği ($skip ve $top)</span><span class="sxs-lookup"><span data-stu-id="2f198-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="2f198-169">Büyük varlık kümeleri için istemci sonuç sayısını sınırlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2f198-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="2f198-170">Örneğin, bir istemci bir kerede 10 girişleri gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="2f198-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="2f198-171">Bu adlı *istemci-tarafı disk belleği*.</span><span class="sxs-lookup"><span data-stu-id="2f198-171">This is called *client-side paging*.</span></span> <span data-ttu-id="2f198-172">(Ayrıca [sunucu tarafı disk belleği](../supporting-odata-query-options.md#server-paging), burada sunucu sonuçları sayısını sınırlar.) İstemci-tarafı disk belleği gerçekleştirmek için LINQ kullanın **atla** ve **ele** yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="2f198-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="2f198-173">Aşağıdaki örnek ilk 40 sonuçları atlar ve sonraki 10 alır.</span><span class="sxs-lookup"><span data-stu-id="2f198-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="2f198-174">Karşılık gelen OData isteği şöyledir:</span><span class="sxs-lookup"><span data-stu-id="2f198-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="2f198-175">($Select) seçin ve genişletin ($expand)</span><span class="sxs-lookup"><span data-stu-id="2f198-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="2f198-176">İlgili varlık eklemek için kullanın `DataServiceQuery<t>.Expand` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2f198-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="2f198-177">Örneğin, dahil etmek için `Supplier` her `Product`:</span><span class="sxs-lookup"><span data-stu-id="2f198-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="2f198-178">Karşılık gelen OData isteği şöyledir:</span><span class="sxs-lookup"><span data-stu-id="2f198-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="2f198-179">Yanıt şeklini değiştirmek için LINQ kullanın **seçin** yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="2f198-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="2f198-180">Aşağıdaki örnekte, yalnızca başka hiçbir özellik ile her ürünün adını alır.</span><span class="sxs-lookup"><span data-stu-id="2f198-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="2f198-181">Karşılık gelen OData isteği şöyledir:</span><span class="sxs-lookup"><span data-stu-id="2f198-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="2f198-182">Select yan tümcesi ilgili varlık içerebilir.</span><span class="sxs-lookup"><span data-stu-id="2f198-182">A select clause can include related entities.</span></span> <span data-ttu-id="2f198-183">Bu durumda, çağırmayın **genişletme**; bu durumda, otomatik olarak içeren genişletme proxy.</span><span class="sxs-lookup"><span data-stu-id="2f198-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="2f198-184">Aşağıdaki örnekte, her ürünün sağlayıcısına ve adını alır.</span><span class="sxs-lookup"><span data-stu-id="2f198-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="2f198-185">Burada, karşılık gelen OData isteği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2f198-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="2f198-186">İçerdiği bildirimi **$expand** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="2f198-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="2f198-187">$Select ve $expand hakkında daha fazla bilgi için ' ni genişletin, bkz: [$select kullanarak $genişletin ve Web API 2'deki $value](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="2f198-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="2f198-188">Yeni bir varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="2f198-188">Add a New Entity</span></span>

<span data-ttu-id="2f198-189">Bir varlık kümesine yeni bir varlık eklemek için arama `AddToEntitySet`, burada *EntitySet* varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="2f198-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="2f198-190">Örneğin, `AddToProducts` yeni ekler `Product` için `Products` varlık kümesi.</span><span class="sxs-lookup"><span data-stu-id="2f198-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="2f198-191">Proxy oluşturduğunuzda, WCF veri hizmetleri otomatik olarak bu kesin türü belirtilmiş oluşturur **AddTo** yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="2f198-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="2f198-192">İki varlık arasında bir bağlantı eklemek için kullanın **AddLink** ve **SetLink** yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="2f198-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="2f198-193">Aşağıdaki kod yeni bir sağlayıcının ve yeni bir ürün ekler ve ardından bunları arasındaki bağlantıları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2f198-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="2f198-194">Kullanım **AddLink** gezinti özelliği bir koleksiyon olduğunda.</span><span class="sxs-lookup"><span data-stu-id="2f198-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="2f198-195">Bu örnekte, bir ürün ekliyoruz `Products` sağlayıcı koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="2f198-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="2f198-196">Kullanım **SetLink** gezinti özelliği tek bir varlık olduğunda.</span><span class="sxs-lookup"><span data-stu-id="2f198-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="2f198-197">Bu örnekte, biz ayarlıyorsanız `Supplier` ürün özelliği.</span><span class="sxs-lookup"><span data-stu-id="2f198-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="2f198-198">Güncelleştirme / düzeltme eki</span><span class="sxs-lookup"><span data-stu-id="2f198-198">Update / Patch</span></span>

<span data-ttu-id="2f198-199">Bir varlığı güncelleştirmek için çağrı **UpdateObject** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2f198-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="2f198-200">Çağırdığınızda güncelleştirme yapılmadan **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="2f198-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="2f198-201">Varsayılan olarak, WCF HTTP birleştirme isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="2f198-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="2f198-202">**PatchOnUpdate** seçenek bir HTTP PATCH yerine göndermek için WCF söyler.</span><span class="sxs-lookup"><span data-stu-id="2f198-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="2f198-203">Neden karşı birleştirme düzeltme eki?</span><span class="sxs-lookup"><span data-stu-id="2f198-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="2f198-204">Özgün HTTP 1.1 belirtimini ([RCF 2616](http://tools.ietf.org/html/rfc2616)) "kısmi güncelleştirme" semantiği ile herhangi bir HTTP yöntemini tanımlamıyor.</span><span class="sxs-lookup"><span data-stu-id="2f198-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="2f198-205">Kısmi güncelleştirmeler desteklemek için birleştirme yöntemi OData belirtimi tanımlı.</span><span class="sxs-lookup"><span data-stu-id="2f198-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="2f198-206">2010'da, [RFC 5789](http://tools.ietf.org/html/rfc5789) kısmi güncelleştirmeler için düzeltme eki yöntemi tanımlı.</span><span class="sxs-lookup"><span data-stu-id="2f198-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="2f198-207">Bu geçmiş bazıları okuyabilirsiniz [blog gönderisi](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) WCF Veri Hizmetleri blogunda.</span><span class="sxs-lookup"><span data-stu-id="2f198-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="2f198-208">Bugün, düzeltme eki üzerinden birleştirme tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="2f198-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="2f198-209">Web API yapı iskelesi tarafından oluşturulan OData denetleyicisi her iki yöntemi destekler.</span><span class="sxs-lookup"><span data-stu-id="2f198-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="2f198-210">Tüm varlık (PUT semantiği) değiştirmek istiyorsanız, belirtin **ReplaceOnUpdate** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="2f198-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="2f198-211">Bu, bir HTTP PUT isteği göndermek WCF neden olur.</span><span class="sxs-lookup"><span data-stu-id="2f198-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="2f198-212">Bir varlığı silme</span><span class="sxs-lookup"><span data-stu-id="2f198-212">Delete an Entity</span></span>

<span data-ttu-id="2f198-213">Bir varlığı silmek için çağrı **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="2f198-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="2f198-214">Bir OData eylemini çağırma</span><span class="sxs-lookup"><span data-stu-id="2f198-214">Invoke an OData Action</span></span>

<span data-ttu-id="2f198-215">Odata'da, [Eylemler](odata-actions.md) kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="2f198-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="2f198-216">OData meta veri belgesi eylemleri açıklansa da, proxy sınıfı herhangi kesin türü belirtilmiş bir yöntem bunları oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="2f198-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="2f198-217">Genel kullanarak bir OData eylemini hala çağırabileceği **yürütme** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2f198-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="2f198-218">Ancak, parametreler ve dönüş değeri veri türlerini bilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2f198-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="2f198-219">Örneğin, `RateProduct` eylemde türü "derecesi" adlı parametre `Int32` ve döndüren bir `double`.</span><span class="sxs-lookup"><span data-stu-id="2f198-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="2f198-220">Aşağıdaki kod bu eylemi çağırmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="2f198-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="2f198-221">Daha fazla bilgi için bkz:[hizmet işlemlerini çağırma ve eylemleri](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f198-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="2f198-222">Bir seçenektir genişletmek için **kapsayıcı** sınıfı eylemi çağırır kesin türü belirtilmiş bir yöntem sunar:</span><span class="sxs-lookup"><span data-stu-id="2f198-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
