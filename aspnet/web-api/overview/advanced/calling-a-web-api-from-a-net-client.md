---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: "Bir .NET İstemci'den (C#) Web API'si çağırma | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 41f014e1d23d46ed28c8c1be5ee92f1a6d878ad9
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="1c8bf-102">Bir .NET İstemci'den (C#) Web API'si çağırma</span><span class="sxs-lookup"><span data-stu-id="1c8bf-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="1c8bf-103">tarafından [CAN Wasson](https://github.com/MikeWasson) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1c8bf-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="1c8bf-104">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="1c8bf-104">Download Completed Project</span></span>](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

<span data-ttu-id="1c8bf-105">Bu öğretici bir .NET uygulamasından web API'si çağırma gösterilmektedir kullanarak [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="1c8bf-105">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="1c8bf-106">Bu öğreticide, bir istemci uygulaması, aşağıdaki web API tüketir yazılır:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-106">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="1c8bf-107">Eylem</span><span class="sxs-lookup"><span data-stu-id="1c8bf-107">Action</span></span> | <span data-ttu-id="1c8bf-108">HTTP yöntemi</span><span class="sxs-lookup"><span data-stu-id="1c8bf-108">HTTP method</span></span> | <span data-ttu-id="1c8bf-109">Göreli URI</span><span class="sxs-lookup"><span data-stu-id="1c8bf-109">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1c8bf-110">Ürün Kimliği tarafından Al</span><span class="sxs-lookup"><span data-stu-id="1c8bf-110">Get a product by ID</span></span> | <span data-ttu-id="1c8bf-111">AL</span><span class="sxs-lookup"><span data-stu-id="1c8bf-111">GET</span></span> | <span data-ttu-id="1c8bf-112">/api/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="1c8bf-112">/api/products/*id*</span></span> |
| <span data-ttu-id="1c8bf-113">Yeni Ürün oluşturma</span><span class="sxs-lookup"><span data-stu-id="1c8bf-113">Create a new product</span></span> | <span data-ttu-id="1c8bf-114">YAYINLA</span><span class="sxs-lookup"><span data-stu-id="1c8bf-114">POST</span></span> | <span data-ttu-id="1c8bf-115">/ api/ürünleri</span><span class="sxs-lookup"><span data-stu-id="1c8bf-115">/api/products</span></span> |
| <span data-ttu-id="1c8bf-116">Bir ürün güncelleştir</span><span class="sxs-lookup"><span data-stu-id="1c8bf-116">Update a product</span></span> | <span data-ttu-id="1c8bf-117">PUT</span><span class="sxs-lookup"><span data-stu-id="1c8bf-117">PUT</span></span> | <span data-ttu-id="1c8bf-118">/api/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="1c8bf-118">/api/products/*id*</span></span> |
| <span data-ttu-id="1c8bf-119">Ürünü silme</span><span class="sxs-lookup"><span data-stu-id="1c8bf-119">Delete a product</span></span> | <span data-ttu-id="1c8bf-120">DELETE</span><span class="sxs-lookup"><span data-stu-id="1c8bf-120">DELETE</span></span> | <span data-ttu-id="1c8bf-121">/api/ürünler/*kimliği*</span><span class="sxs-lookup"><span data-stu-id="1c8bf-121">/api/products/*id*</span></span> |

<span data-ttu-id="1c8bf-122">Bu API ile ASP.NET Web API uygulamak öğrenmek için bkz: [CRUD işlemleri destekler bir Web API oluşturma](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="1c8bf-122">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="1c8bf-123">Kolaylık olması için istemci Bu öğreticide bir Windows konsol uygulaması uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-123">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="1c8bf-124">**HttpClient** Windows Phone ve Windows mağazası uygulamaları için de desteklenir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-124">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="1c8bf-125">Daha fazla bilgi için bkz: [birden çok platform kullanarak taşınabilir kitaplık için yazma Web API İstemci kodu](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="1c8bf-125">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="1c8bf-126">Konsol uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="1c8bf-126">Create the Console Application</span></span>

<span data-ttu-id="1c8bf-127">Visual Studio'da adlı yeni bir Windows konsol uygulaması oluşturma **HttpClientSample** ve aşağıdaki kodu yapıştırın:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-127">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="1c8bf-128">Önceki kod tam istemci uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-128">The preceding code is the complete client app.</span></span>

<span data-ttu-id="1c8bf-129">`RunAsync`çalıştırır ve tamamlanana kadar engeller.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-129">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="1c8bf-130">Çoğu **HttpClient** yöntemleridir zaman uyumsuz, ağ g/ç gerçekleştirdiğinden.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-130">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="1c8bf-131">Zaman uyumsuz görevlerin tümünü içinde yapılır `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-131">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="1c8bf-132">Bir uygulamanın ana iş parçacığı normalde engellemez, ancak bu uygulama etkileşimi izin vermez.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-132">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="1c8bf-133">Web API İstemci kitaplıkları yükleme</span><span class="sxs-lookup"><span data-stu-id="1c8bf-133">Install the Web API Client Libraries</span></span>

<span data-ttu-id="1c8bf-134">NuGet paketi Web API Client Libraries paketini yüklemek için Yöneticisi'ni kullanın.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-134">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="1c8bf-135">Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-135">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="1c8bf-136">Paket Yöneticisi Konsolu (PMC)'da, aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-136">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="1c8bf-137">Yukarıdaki komut aşağıdaki NuGet paketlerini projeye ekler:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-137">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="1c8bf-138">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="1c8bf-138">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="1c8bf-139">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="1c8bf-139">Newtonsoft.Json</span></span>

<span data-ttu-id="1c8bf-140">Json.NET, .NET için popüler bir yüksek performanslı JSON çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-140">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="1c8bf-141">Bir Model sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="1c8bf-141">Add a Model Class</span></span>

<span data-ttu-id="1c8bf-142">İncelemek `Product` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-142">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="1c8bf-143">Bu sınıf, web API tarafından kullanılan veri modeli ile eşleşir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-143">This class matches the data model used by the web API.</span></span> <span data-ttu-id="1c8bf-144">Bir uygulamayı **HttpClient** okumak için bir `Product` bir HTTP yanıt örneği.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-144">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="1c8bf-145">Uygulama herhangi bir seri durumdan çıkarma kod yazmak zorunda değildir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-145">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="1c8bf-146">Oluşturma ve HttpClient başlatma</span><span class="sxs-lookup"><span data-stu-id="1c8bf-146">Create and Initialize HttpClient</span></span>

<span data-ttu-id="1c8bf-147">Statik inceleyin **HttpClient** özelliği:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-147">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="1c8bf-148">**HttpClient** kez örneğinin oluşturulması için hedeflenen ve uygulama kullanım ömrü yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-148">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="1c8bf-149">Aşağıdaki koşullar sonuçlanabilir **SocketException** hatalar:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-149">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="1c8bf-150">Yeni bir oluşturma **HttpClient** istek başına örnek.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-150">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="1c8bf-151">Sunucu ağır yük altında.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-151">Server under heavy load.</span></span>

<span data-ttu-id="1c8bf-152">Yeni bir oluşturma **HttpClient** istek başına örnek kullanılabilir yuvaları tüketebilir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-152">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="1c8bf-153">Aşağıdaki kod başlatır **HttpClient** örneği:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-153">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="1c8bf-154">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-154">The preceding code:</span></span>

* <span data-ttu-id="1c8bf-155">HTTP istekleri için taban URI ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-155">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="1c8bf-156">Sunucu uygulamasında kullanılan bağlantı noktası için bağlantı noktası numarasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-156">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="1c8bf-157">Sunucu uygulaması için bağlantı noktası kullanılmadığı sürece uygulama çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-157">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="1c8bf-158">"Application/json" için Accept üstbilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-158">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="1c8bf-159">Bu üst ayarlama JSON biçiminde veri göndermek için sunucunun söyler.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-159">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="1c8bf-160">Bir kaynak almak için bir GET isteği gönder</span><span class="sxs-lookup"><span data-stu-id="1c8bf-160">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="1c8bf-161">Aşağıdaki kod bir ürün için bir GET isteği gönderir:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-161">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="1c8bf-162">**GetAsync** yöntem, HTTP GET isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-162">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="1c8bf-163">Yöntem tamamlandığında, döndürdüğü bir **httpresponsemessage öğesini** HTTP yanıtı içerir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-163">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="1c8bf-164">Yanıt durum kodu bir başarı kodu ise, yanıt gövdesi bir ürün JSON gösterimi içerir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-164">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="1c8bf-165">Çağrı **ReadAsAsync** JSON yükü seri durumdan çıkarılacak bir `Product` örneği.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-165">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="1c8bf-166">**ReadAsAsync** yöntemi olduğundan zaman uyumsuz yanıt gövdesi rasgele büyük olabilir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-166">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="1c8bf-167">**HttpClient** HTTP yanıtı bir hata kodu içerdiğinde bir özel durum oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-167">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="1c8bf-168">Bunun yerine, **IsSuccessStatusCode** özelliği **false** durumu hata kodu ise.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-168">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="1c8bf-169">HTTP hata kodları özel durumlar olarak işlemek tercih ederseniz, çağrı [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) yanıt nesnesi üzerinde.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-169">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="1c8bf-170">`EnsureSuccessStatusCode`bir özel durum kodu 200 aralığının dışında döndürürse&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-170">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="1c8bf-171">Unutmayın **HttpClient** istisnalar başka nedenlerle atabilirsiniz &mdash; Örneğin, istek zaman aşımına uğrarsa.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-171">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="1c8bf-172">Medya türü Biçimlendiricilerini serisini kaldırmak için</span><span class="sxs-lookup"><span data-stu-id="1c8bf-172">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="1c8bf-173">Zaman **ReadAsAsync** denir hiçbir parametrelere sahip varsayılan bir kullanır *medya biçimlendiricileri* yanıt gövdesi okunamıyor.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-173">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="1c8bf-174">Varsayılan biçimlendiricileri JSON, XML ve formu url kodlanmış verileri destekler.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-174">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="1c8bf-175">Varsayılan biçimlendiricileri kullanmak yerine, için biçimlendiricileri listesi sağlayabilir **ReadAsAsync** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-175">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="1c8bf-176">Kullanarak bir biçimlendiricileri listesi özel medya türü biçimlendiricisi varsa yararlı olur:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-176">Using a a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="1c8bf-177">Daha fazla bilgi için bkz: [ASP.NET Web API 2'deki medya Biçimlendiricileri](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="1c8bf-177">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="1c8bf-178">Bir kaynak oluşturmak için bir POST isteği gönderme</span><span class="sxs-lookup"><span data-stu-id="1c8bf-178">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="1c8bf-179">Aşağıdaki kodu içeren bir POST isteği gönderir bir `Product` JSON biçiminde örneği:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-179">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="1c8bf-180">**PostAsJsonAsync** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-180">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="1c8bf-181">JSON nesneyi serileştirir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-181">Serializes an object to JSON.</span></span>
* <span data-ttu-id="1c8bf-182">JSON yükü bir POST isteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-182">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="1c8bf-183">İstek başarılı olursa:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-183">If the request succeeds:</span></span>

* <span data-ttu-id="1c8bf-184">201 (oluşturuldu) yanıt döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-184">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="1c8bf-185">Yanıt konumu üstbilgisinde oluşturulan kaynak URL'sini içermelidir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-185">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="1c8bf-186">Bir kaynak güncelleştirmeye yönelik bir PUT isteği gönderme</span><span class="sxs-lookup"><span data-stu-id="1c8bf-186">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="1c8bf-187">Aşağıdaki kod bir ürünü güncelleştirmek için bir PUT İsteği gönderir:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-187">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="1c8bf-188">**PutAsJsonAsync** yöntem çalışır gibi **PostAsJsonAsync**, ancak bu POST yerine bir PUT İsteği gönderir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-188">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="1c8bf-189">Bir kaynak silmek için bir silme isteği gönderme</span><span class="sxs-lookup"><span data-stu-id="1c8bf-189">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="1c8bf-190">Aşağıdaki kod bir ürün silmek için bir silme isteği gönderir:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-190">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="1c8bf-191">GET gibi bir istek gövdesi bir silme isteği yok.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-191">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="1c8bf-192">JSON veya XML biçiminde SİLMEYE devam belirtmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-192">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="1c8bf-193">Örnek test</span><span class="sxs-lookup"><span data-stu-id="1c8bf-193">Test the sample</span></span>

<span data-ttu-id="1c8bf-194">İstemci uygulama test etmek için:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-194">To test the client app:</span></span>

1. <span data-ttu-id="1c8bf-195">[Karşıdan](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) ve server uygulamasını çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-195">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="1c8bf-196">[Yükleme yönergeleri](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="1c8bf-196">[Download instructions](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="1c8bf-197">Sunucu uygulamasının çalıştığını doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-197">Verify the server app is working.</span></span> <span data-ttu-id="1c8bf-198">Exaxmple için `http://localhost:64195/api/products` ürünlerin listesini döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-198">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="1c8bf-199">HTTP istekleri için ana URI ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-199">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="1c8bf-200">Sunucu uygulamasında kullanılan bağlantı noktası için bağlantı noktası numarasını değiştirin.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-200">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="1c8bf-201">İstemci uygulaması çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1c8bf-201">Run the client app.</span></span> <span data-ttu-id="1c8bf-202">Şu çıktı üretilir:</span><span class="sxs-lookup"><span data-stu-id="1c8bf-202">The following output is produced:</span></span>

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
