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
<a name="call-a-web-api-from-a-net-client-c"></a>Bir .NET İstemci'den (C#) Web API'si çağırma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[Tamamlanan projenizi indirin](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

Bu öğretici bir .NET uygulamasından web API'si çağırma gösterilmektedir kullanarak [System.Net.Http.HttpClient.](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.110).aspx)

Bu öğreticide, bir istemci uygulaması, aşağıdaki web API tüketir yazılır:

| Eylem | HTTP yöntemi | Göreli URI |
| --- | --- | --- |
| Ürün Kimliği tarafından Al | AL | /api/ürünler/*kimliği* |
| Yeni Ürün oluşturma | YAYINLA | / api/ürünleri |
| Bir ürün güncelleştir | PUT | /api/ürünler/*kimliği* |
| Ürünü silme | DELETE | /api/ürünler/*kimliği* |

Bu API ile ASP.NET Web API uygulamak öğrenmek için bkz: [CRUD işlemleri destekler bir Web API oluşturma](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Kolaylık olması için istemci Bu öğreticide bir Windows konsol uygulaması uygulamasıdır. **HttpClient** Windows Phone ve Windows mağazası uygulamaları için de desteklenir. Daha fazla bilgi için bkz: [birden çok platform kullanarak taşınabilir kitaplık için yazma Web API İstemci kodu](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Konsol uygulaması oluşturun

Visual Studio'da adlı yeni bir Windows konsol uygulaması oluşturma **HttpClientSample** ve aşağıdaki kodu yapıştırın:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Önceki kod tam istemci uygulamasıdır.

`RunAsync`çalıştırır ve tamamlanana kadar engeller. Çoğu **HttpClient** yöntemleridir zaman uyumsuz, ağ g/ç gerçekleştirdiğinden. Zaman uyumsuz görevlerin tümünü içinde yapılır `RunAsync`. Bir uygulamanın ana iş parçacığı normalde engellemez, ancak bu uygulama etkileşimi izin vermez.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Web API İstemci kitaplıkları yükleme

NuGet paketi Web API Client Libraries paketini yüklemek için Yöneticisi'ni kullanın.

Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**. Paket Yöneticisi Konsolu (PMC)'da, aşağıdaki komutu yazın:

`Install-Package Microsoft.AspNet.WebApi.Client`

Yukarıdaki komut aşağıdaki NuGet paketlerini projeye ekler:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET, .NET için popüler bir yüksek performanslı JSON çerçevedir.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Bir Model sınıfı ekleme

İncelemek `Product` sınıfı:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Bu sınıf, web API tarafından kullanılan veri modeli ile eşleşir. Bir uygulamayı **HttpClient** okumak için bir `Product` bir HTTP yanıt örneği. Uygulama herhangi bir seri durumdan çıkarma kod yazmak zorunda değildir.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Oluşturma ve HttpClient başlatma

Statik inceleyin **HttpClient** özelliği:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** kez örneğinin oluşturulması için hedeflenen ve uygulama kullanım ömrü yeniden kullanılabilir. Aşağıdaki koşullar sonuçlanabilir **SocketException** hatalar:

* Yeni bir oluşturma **HttpClient** istek başına örnek.
* Sunucu ağır yük altında.

Yeni bir oluşturma **HttpClient** istek başına örnek kullanılabilir yuvaları tüketebilir.

Aşağıdaki kod başlatır **HttpClient** örneği:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Önceki kod:

* HTTP istekleri için taban URI ayarlar. Sunucu uygulamasında kullanılan bağlantı noktası için bağlantı noktası numarasını değiştirin. Sunucu uygulaması için bağlantı noktası kullanılmadığı sürece uygulama çalışmaz.
* "Application/json" için Accept üstbilgisini ayarlar. Bu üst ayarlama JSON biçiminde veri göndermek için sunucunun söyler.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Bir kaynak almak için bir GET isteği gönder

Aşağıdaki kod bir ürün için bir GET isteği gönderir:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync** yöntem, HTTP GET isteği gönderir. Yöntem tamamlandığında, döndürdüğü bir **httpresponsemessage öğesini** HTTP yanıtı içerir. Yanıt durum kodu bir başarı kodu ise, yanıt gövdesi bir ürün JSON gösterimi içerir. Çağrı **ReadAsAsync** JSON yükü seri durumdan çıkarılacak bir `Product` örneği. **ReadAsAsync** yöntemi olduğundan zaman uyumsuz yanıt gövdesi rasgele büyük olabilir.

**HttpClient** HTTP yanıtı bir hata kodu içerdiğinde bir özel durum oluşturmaz. Bunun yerine, **IsSuccessStatusCode** özelliği **false** durumu hata kodu ise. HTTP hata kodları özel durumlar olarak işlemek tercih ederseniz, çağrı [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) yanıt nesnesi üzerinde. `EnsureSuccessStatusCode`bir özel durum kodu 200 aralığının dışında döndürürse&ndash;299. Unutmayın **HttpClient** istisnalar başka nedenlerle atabilirsiniz &mdash; Örneğin, istek zaman aşımına uğrarsa.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Medya türü Biçimlendiricilerini serisini kaldırmak için

Zaman **ReadAsAsync** denir hiçbir parametrelere sahip varsayılan bir kullanır *medya biçimlendiricileri* yanıt gövdesi okunamıyor. Varsayılan biçimlendiricileri JSON, XML ve formu url kodlanmış verileri destekler.

Varsayılan biçimlendiricileri kullanmak yerine, için biçimlendiricileri listesi sağlayabilir **ReadAsAsync** yöntemi.  Kullanarak bir biçimlendiricileri listesi özel medya türü biçimlendiricisi varsa yararlı olur:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Daha fazla bilgi için bkz: [ASP.NET Web API 2'deki medya Biçimlendiricileri](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Bir kaynak oluşturmak için bir POST isteği gönderme

Aşağıdaki kodu içeren bir POST isteği gönderir bir `Product` JSON biçiminde örneği:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync** yöntemi:

* JSON nesneyi serileştirir.
* JSON yükü bir POST isteği gönderir.

İstek başarılı olursa:

* 201 (oluşturuldu) yanıt döndürmelidir.
* Yanıt konumu üstbilgisinde oluşturulan kaynak URL'sini içermelidir.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Bir kaynak güncelleştirmeye yönelik bir PUT isteği gönderme

Aşağıdaki kod bir ürünü güncelleştirmek için bir PUT İsteği gönderir:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync** yöntem çalışır gibi **PostAsJsonAsync**, ancak bu POST yerine bir PUT İsteği gönderir.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Bir kaynak silmek için bir silme isteği gönderme

Aşağıdaki kod bir ürün silmek için bir silme isteği gönderir:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

GET gibi bir istek gövdesi bir silme isteği yok. JSON veya XML biçiminde SİLMEYE devam belirtmeniz gerekmez.

## <a name="test-the-sample"></a>Örnek test

İstemci uygulama test etmek için:

1. [Karşıdan](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) ve server uygulamasını çalıştırın. [Yükleme yönergeleri](https://docs.microsoft.com/en-us/aspnet/core/tutorials/#how-to-download-a-sample). Sunucu uygulamasının çalıştığını doğrulayın. Exaxmple için `http://localhost:64195/api/products` ürünlerin listesini döndürmelidir.
2. HTTP istekleri için ana URI ayarlayın. Sunucu uygulamasında kullanılan bağlantı noktası için bağlantı noktası numarasını değiştirin.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. İstemci uygulaması çalıştırın. Şu çıktı üretilir:

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
