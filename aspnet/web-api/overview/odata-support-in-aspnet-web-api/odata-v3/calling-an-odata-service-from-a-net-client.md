---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Bir .NET istemcisinden (C#) bir OData hizmeti çağırma | Microsoft Docs
author: MikeWasson
description: Bu öğretici, C# istemci uygulamasından bir OData hizmeti çağırma işlemi gösterilmektedir. Öğretici Visual Studio 2013 (çalışır Visual s... kullanılan yazılım sürümleri
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 75f8e3eab7bd5667bbdcccbb5ae8a8e5b1f5fdba
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912058"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="cd45c-104">Bir OData hizmeti çağırmak bir .NET istemcisinden (C#)</span><span class="sxs-lookup"><span data-stu-id="cd45c-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="cd45c-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cd45c-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="cd45c-106">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="cd45c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="cd45c-107">Bu öğretici, C# istemci uygulamasından bir OData hizmeti çağırma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cd45c-108">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="cd45c-108">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="cd45c-109">[Visual Studio 2013'ün](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (Visual Studio 2012 ile çalışır)</span><span class="sxs-lookup"><span data-stu-id="cd45c-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="cd45c-110">WCF Veri Hizmetleri İstemci Kitaplığı</span><span class="sxs-lookup"><span data-stu-id="cd45c-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="cd45c-111">Web API 2.</span><span class="sxs-lookup"><span data-stu-id="cd45c-111">Web API 2.</span></span> <span data-ttu-id="cd45c-112">(OData hizmeti örnek Web API 2 kullanılarak derlendi ancak istemci uygulaması Web API'ye bağlı değildir.)</span><span class="sxs-lookup"><span data-stu-id="cd45c-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="cd45c-113">Bu öğreticide, ben bir OData hizmetine çağrı yapan bir istemci uygulaması oluşturmada size yardımcı olacağız.</span><span class="sxs-lookup"><span data-stu-id="cd45c-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="cd45c-114">Aşağıdaki varlıkların OData hizmeti sunar:</span><span class="sxs-lookup"><span data-stu-id="cd45c-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="cd45c-115">Aşağıdaki makaleler, Web API OData hizmeti uygulamak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="cd45c-116">(Bu öğreticide, ancak anlamak için bunları okumak gerekmez.)</span><span class="sxs-lookup"><span data-stu-id="cd45c-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="cd45c-117">Web API 2 OData uç noktası oluşturma</span><span class="sxs-lookup"><span data-stu-id="cd45c-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="cd45c-118">Web API 2 OData varlık ilişkileri</span><span class="sxs-lookup"><span data-stu-id="cd45c-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="cd45c-119">Web API 2’de OData Eylemleri</span><span class="sxs-lookup"><span data-stu-id="cd45c-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="cd45c-120">Hizmet proxy'si oluştur</span><span class="sxs-lookup"><span data-stu-id="cd45c-120">Generate the Service Proxy</span></span>

<span data-ttu-id="cd45c-121">İlk adım, bir hizmeti proxy'si oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="cd45c-122">Hizmet proxy'si OData hizmetine erişim yöntemleri tanımlayan bir .NET sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="cd45c-123">Ara sunucu HTTP isteklerinin yöntemi çağrılarını çevirir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="cd45c-124">OData hizmet proje Visual Studio'da açarak işleme başlayın.</span><span class="sxs-lookup"><span data-stu-id="cd45c-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="cd45c-125">IIS Express'te URL'i hizmeti yerel olarak çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="cd45c-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="cd45c-126">Visual Studio atadığı bağlantı noktası numarasını da dahil olmak üzere, yerel adres unutmayın.</span><span class="sxs-lookup"><span data-stu-id="cd45c-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="cd45c-127">Proxy oluşturduğunuzda bu adresi gerekir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="cd45c-128">Ardından, Visual Studio'nun başka bir örneğini açın ve bir konsol uygulama projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cd45c-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="cd45c-129">Konsol uygulaması OData istemci uygulamamız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-129">The console application will be our OData client application.</span></span> <span data-ttu-id="cd45c-130">(, Ayrıca projeyi hizmet olarak aynı çözüme ekleyebilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="cd45c-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="cd45c-131">Konsol projesi kalan adımlara bakın.</span><span class="sxs-lookup"><span data-stu-id="cd45c-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="cd45c-132">Çözüm Gezgini'nde sağ **başvuruları** seçip **hizmet Başvurusu Ekle**.</span><span class="sxs-lookup"><span data-stu-id="cd45c-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="cd45c-133">İçinde **hizmet Başvurusu Ekle** iletişim kutusunda, OData hizmeti adresini yazın:</span><span class="sxs-lookup"><span data-stu-id="cd45c-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="cd45c-134">Burada *bağlantı noktası* bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="cd45c-135">İçin **Namespace**, "ProductService" yazın.</span><span class="sxs-lookup"><span data-stu-id="cd45c-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="cd45c-136">Bu seçenek, ad alanı proxy sınıfını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="cd45c-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="cd45c-137">Tıklayın **Git**.</span><span class="sxs-lookup"><span data-stu-id="cd45c-137">Click **Go**.</span></span> <span data-ttu-id="cd45c-138">Visual Studio service varlıklarda bulmak için OData meta veri belgesini okur.</span><span class="sxs-lookup"><span data-stu-id="cd45c-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="cd45c-139">Tıklayın **Tamam** proxy sınıfı projenize eklenecek.</span><span class="sxs-lookup"><span data-stu-id="cd45c-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="cd45c-140">Hizmeti Proxy sınıfı bir örneğini oluşturun</span><span class="sxs-lookup"><span data-stu-id="cd45c-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="cd45c-141">İçinde `Main` yöntemi, proxy sınıfının yeni bir örneğini şu şekilde oluşturun:</span><span class="sxs-lookup"><span data-stu-id="cd45c-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="cd45c-142">Hizmet uygulamanızın nerede çalıştığına yeniden gerçek bağlantı noktası numarasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="cd45c-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="cd45c-143">Hizmetinizi dağıttığınızda, Canlı hizmeti URI'si kullanır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="cd45c-144">Proxy güncelleştirmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="cd45c-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="cd45c-145">Aşağıdaki kod, istek URI konsol penceresine yazdırır bir olay işleyicisi ekler.</span><span class="sxs-lookup"><span data-stu-id="cd45c-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="cd45c-146">Bu adım gerekli değildir, ancak her sorgu için bir URI'leri görmek ilginç.</span><span class="sxs-lookup"><span data-stu-id="cd45c-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="cd45c-147">Hizmet sorgulama</span><span class="sxs-lookup"><span data-stu-id="cd45c-147">Query the Service</span></span>

<span data-ttu-id="cd45c-148">Aşağıdaki kod, OData hizmeti ürünlerin listesini alır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="cd45c-149">HTTP isteği göndermek veya yanıtları ayrıştırmak için herhangi bir kod yazmanız gerekmez dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="cd45c-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="cd45c-150">Numaralandırma, proxy sınıfı bu otomatik olarak yapar `Container.Products` koleksiyonda **foreach** döngü.</span><span class="sxs-lookup"><span data-stu-id="cd45c-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="cd45c-151">Uygulamayı çalıştırdığınızda, çıktı aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="cd45c-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="cd45c-152">Kimliğe göre varlık almak için kullanmak bir `where` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="cd45c-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="cd45c-153">Bu konunun geri kalanı için ı tamamını gösterilmez `Main` işlev, yalnızca hizmeti çağırmak için gereken kodu.</span><span class="sxs-lookup"><span data-stu-id="cd45c-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="cd45c-154">Geçerli sorgu seçenekleri</span><span class="sxs-lookup"><span data-stu-id="cd45c-154">Apply Query Options</span></span>

<span data-ttu-id="cd45c-155">OData tanımlar [sorgu seçenekleri](../supporting-odata-query-options.md) kullanılabilen filtre, sıralama, sayfa verileri ve benzeri.</span><span class="sxs-lookup"><span data-stu-id="cd45c-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="cd45c-156">Hizmet ara sunucu, çeşitli LINQ ifadelerini kullanarak bu seçenekleri uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd45c-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="cd45c-157">Bu bölümde, ı kısa örnekler göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="cd45c-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="cd45c-158">Daha fazla bilgi için Ek Yardım konusuna [LINQ konuları (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) MSDN'de.</span><span class="sxs-lookup"><span data-stu-id="cd45c-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="cd45c-159">Filtreleme ($filter)</span><span class="sxs-lookup"><span data-stu-id="cd45c-159">Filtering ($filter)</span></span>

<span data-ttu-id="cd45c-160">Filtre uygulamak için kullanmak bir `where` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="cd45c-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="cd45c-161">Aşağıdaki örnek filtreler ürün kategorisine göre.</span><span class="sxs-lookup"><span data-stu-id="cd45c-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="cd45c-162">Bu kod, aşağıdaki OData sorgu için karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="cd45c-163">Proxy dönüştürür bildirimi `where` yan tümcesine bir OData `$filter` ifade.</span><span class="sxs-lookup"><span data-stu-id="cd45c-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="cd45c-164">Sıralama ($orderby)</span><span class="sxs-lookup"><span data-stu-id="cd45c-164">Sorting ($orderby)</span></span>

<span data-ttu-id="cd45c-165">Sıralamak için kullanmak bir `orderby` yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="cd45c-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="cd45c-166">Aşağıdaki örnek, yüksekten en düşüğe fiyata göre sıralar.</span><span class="sxs-lookup"><span data-stu-id="cd45c-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="cd45c-167">Karşılık gelen OData isteği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="cd45c-168">İstemci tarafı sayfalama ($skip ve $top)</span><span class="sxs-lookup"><span data-stu-id="cd45c-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="cd45c-169">Büyük varlık kümeleri için istemci sonuçları sayısını sınırlamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cd45c-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="cd45c-170">Örneğin, bir istemci aynı anda 10 girişleri gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="cd45c-171">Bu adlandırılır *istemci-tarafı sayfalama*.</span><span class="sxs-lookup"><span data-stu-id="cd45c-171">This is called *client-side paging*.</span></span> <span data-ttu-id="cd45c-172">(Ayrıca [sunucu tarafı sayfalama](../supporting-odata-query-options.md#server-paging), burada sunucu sonuçları sayısını sınırlar.) İstemci tarafı sayfalama gerçekleştirmek için LINQ kullanma **atla** ve **ele** yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="cd45c-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="cd45c-173">Aşağıdaki örnek, ilk 40 sonuçları atlar ve sonraki 10 alır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="cd45c-174">Karşılık gelen OData isteği şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="cd45c-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="cd45c-175">($Select) seçme ve genişletme ($expand)</span><span class="sxs-lookup"><span data-stu-id="cd45c-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="cd45c-176">İlişkili varlıkları içermesi için kullanın `DataServiceQuery<t>.Expand` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd45c-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="cd45c-177">Örneğin, dahil etmek için `Supplier` her `Product`:</span><span class="sxs-lookup"><span data-stu-id="cd45c-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="cd45c-178">Karşılık gelen OData isteği şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="cd45c-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="cd45c-179">Yanıtın şeklini değiştirmek için LINQ kullanma **seçin** yan tümcesi.</span><span class="sxs-lookup"><span data-stu-id="cd45c-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="cd45c-180">Aşağıdaki örnek, yalnızca başka hiçbir özellik ile her ürün adını alır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="cd45c-181">Karşılık gelen OData isteği şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="cd45c-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="cd45c-182">Bir select yan tümcesi, ilgili varlıklar içerebilir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-182">A select clause can include related entities.</span></span> <span data-ttu-id="cd45c-183">Bu durumda, çağırmayın **genişletme**; bu durumda, otomatik olarak içerir genişletme proxy.</span><span class="sxs-lookup"><span data-stu-id="cd45c-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="cd45c-184">Aşağıdaki örnek, her ürünün Tedarikçi ve adını alır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="cd45c-185">Karşılık gelen OData isteği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="cd45c-186">İçerdiği bildirimi **$expand** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="cd45c-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="cd45c-187">$Select ve $expand hakkında daha fazla bilgi için genişletme, bkz: [$select kullanarak $expand ve Web API 2 $value](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="cd45c-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="cd45c-188">Yeni varlık Ekle</span><span class="sxs-lookup"><span data-stu-id="cd45c-188">Add a New Entity</span></span>

<span data-ttu-id="cd45c-189">Bir varlık kümesi için yeni bir varlık eklemek için çağrı `AddToEntitySet`burada *EntitySet* varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="cd45c-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="cd45c-190">Örneğin, `AddToProducts` yeni ekler `Product` için `Products` varlık kümesi.</span><span class="sxs-lookup"><span data-stu-id="cd45c-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="cd45c-191">Ara sunucu oluşturduğunuzda, WCF veri hizmetleri otomatik olarak bu türü kesin belirlenmiş oluşturur **AddTo** yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="cd45c-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="cd45c-192">İki varlık arasında bir bağlantı eklemek için **AddLink** ve **SetLink** yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="cd45c-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="cd45c-193">Aşağıdaki kod, yeni bir tedarikçi ve yeni bir ürün ekler ve ardından aralarındaki bağlantıları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cd45c-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="cd45c-194">Kullanım **AddLink** gezinme özelliği, bir koleksiyon olduğunda.</span><span class="sxs-lookup"><span data-stu-id="cd45c-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="cd45c-195">Bu örnekte, bir ürün ekliyoruz `Products` sağlayıcı koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="cd45c-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="cd45c-196">Kullanım **SetLink** gezinti özelliği tek bir varlık olduğunda.</span><span class="sxs-lookup"><span data-stu-id="cd45c-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="cd45c-197">Biz bu örnekte, ayarladığınız `Supplier` ürün özelliği.</span><span class="sxs-lookup"><span data-stu-id="cd45c-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="cd45c-198">Güncelleştirme / düzeltme eki</span><span class="sxs-lookup"><span data-stu-id="cd45c-198">Update / Patch</span></span>

<span data-ttu-id="cd45c-199">Bir varlığı güncelleştirmek için çağrı **UpdateObject** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd45c-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="cd45c-200">Güncelleştirme yapılmadan çağırdığınızda **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="cd45c-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="cd45c-201">Varsayılan olarak, WCF, bir HTTP birleştirme isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="cd45c-202">**PatchOnUpdate** seçeneği, bunun yerine bir HTTP PATCH göndermek için WCF söyler.</span><span class="sxs-lookup"><span data-stu-id="cd45c-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="cd45c-203">Neden birleştirme düzeltme eki?</span><span class="sxs-lookup"><span data-stu-id="cd45c-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="cd45c-204">Özgün HTTP 1.1 belirtimini ([RCF 2616](http://tools.ietf.org/html/rfc2616)) herhangi bir HTTP yöntemi "kısmi güncelleştirme" semantiğine sahip tanımlamıyor.</span><span class="sxs-lookup"><span data-stu-id="cd45c-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="cd45c-205">Kısmi güncelleştirmeleri desteklemek için OData belirtiminden birleştirme yöntemi olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="cd45c-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="cd45c-206">2010'da, [RFC 5789](http://tools.ietf.org/html/rfc5789) kısmi güncelleştirmeler için PATCH yöntemini tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="cd45c-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="cd45c-207">Bu geçmiş bazıları edinebilirsiniz [blog gönderisi](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) WCF Veri Hizmetleri blogunda.</span><span class="sxs-lookup"><span data-stu-id="cd45c-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="cd45c-208">Bugün, düzeltme eki üzerinden birleştirme tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="cd45c-209">Web APİ'si yapı iskelesini tarafından oluşturulan OData denetleyicisi her iki yöntemi destekler.</span><span class="sxs-lookup"><span data-stu-id="cd45c-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="cd45c-210">Tüm varlık (PUT semantiği) değiştirmek istiyorsanız, belirtin **ReplaceOnUpdate** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="cd45c-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="cd45c-211">Bu, bir HTTP PUT isteği göndermek WCF neden olur.</span><span class="sxs-lookup"><span data-stu-id="cd45c-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="cd45c-212">Bir varlığı silme</span><span class="sxs-lookup"><span data-stu-id="cd45c-212">Delete an Entity</span></span>

<span data-ttu-id="cd45c-213">Bir varlığı silmek için çağrı **NesneSil**.</span><span class="sxs-lookup"><span data-stu-id="cd45c-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="cd45c-214">Bir OData eylemini çağırma</span><span class="sxs-lookup"><span data-stu-id="cd45c-214">Invoke an OData Action</span></span>

<span data-ttu-id="cd45c-215">Odata'da, [eylemleri](odata-actions.md) varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="cd45c-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="cd45c-216">OData meta veri belgesi eylemleri açıklar ancak proxy sınıfı kesin türü belirtilmiş yöntemler için bunları oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="cd45c-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="cd45c-217">Genel kullanarak bir OData eylemini yine de çağırabilirsiniz **yürütme** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cd45c-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="cd45c-218">Ancak, parametreler ve dönüş değeri veri türlerini bilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="cd45c-219">Örneğin, `RateProduct` parametre türü "derecesi" adlı bir eylemde `Int32` ve döndüren bir `double`.</span><span class="sxs-lookup"><span data-stu-id="cd45c-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="cd45c-220">Aşağıdaki kod bu eylemi çağırmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="cd45c-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="cd45c-221">Daha fazla bilgi için[hizmet işlemlerini çağırma ve eylemleri](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="cd45c-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="cd45c-222">Bir seçenektir genişletmek için **kapsayıcı** eylemi çağırır kesin türü belirtilmiş bir yöntem sağlar sınıfını:</span><span class="sxs-lookup"><span data-stu-id="cd45c-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
