---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Bir .NET istemcisinden (C#) bir Web API'si çağırma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6b034642f1616a9d5c6495b65d4adeeae9a6f2c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821397"
---
<a name="call-a-web-api-from-a-net-client-c"></a>Bir .NET istemcisinden (C#) bir Web API'si çağırma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

[Tamamlanmış projeyi indirmek](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Yükleme yönergeleri](/aspnet/core/tutorials/#how-to-download-a-sample). 

Bu öğretici, bir .NET uygulamasından web API'si çağırma nasıl gösterir kullanarak [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

Bu öğreticide, aşağıdaki web API'si kullanan bir istemci uygulaması yazılır:

| Eylem | HTTP yöntemi | Göreli URI'si |
| --- | --- | --- |
| Bir ürün Kimliğine göre Al | AL | /API'si/ürünler/*kimliği* |
| Yeni Ürün oluşturma | YAYINLA | / api/ürünleri |
| Bir ürün güncelleştirmesi | PUT | /API'si/ürünler/*kimliği* |
| Bir ürün Sil | DELETE | /API'si/ürünler/*kimliği* |

Bu API ile ASP.NET Web API'si uygulama hakkında bilgi edinmek için bkz: [bu destekler CRUD işlemleri bir Web API'si oluşturma](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Kolaylık olması için istemci uygulaması Bu öğreticide bir Windows konsol uygulamasıdır. **HttpClient** Windows Phone ve Windows Store uygulamaları için de desteklenir. Daha fazla bilgi için [yazma Web API İstemci kodu birden çok platformları kullanarak taşınabilir kitaplıklar için](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Konsol uygulaması oluşturun

Visual Studio'da adlı yeni bir Windows konsol uygulaması oluşturma **HttpClientSample** ve aşağıdaki kodu yapıştırın:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Yukarıdaki kod tam istemci uygulaması gereklidir.

`RunAsync` çalıştırır ve işlem tamamlanana kadar engeller. Çoğu **HttpClient** yöntemlerdir zaman uyumsuz, bunların ağ g/ç gerçekleştirdiğinden. Zaman uyumsuz görevlerin tümünü içinde yapılır `RunAsync`. Bir uygulama ana iş parçacığı normalde engellemez, ancak bu uygulama etkileşimi izin vermez.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Web API İstemci kitaplıklarını yükleme

NuGet paketi Web API İstemci kitaplıkları paketini yüklemek için Yöneticisi'ni kullanın.

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**. Paket Yöneticisi Konsolu (PMC'de), aşağıdaki komutu yazın:

`Install-Package Microsoft.AspNet.WebApi.Client`

Önceki komutta aşağıdaki NuGet paketleri projeye ekler:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET, .NET için popüler bir yüksek performanslı JSON çerçevedir.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Bir Model sınıfı ekleme

İnceleme `Product` sınıfı:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Bu sınıf, web API'si tarafından kullanılan veri modeli ile eşleşir. Bir uygulama kullanabilirsiniz **HttpClient** okumak için bir `Product` bir HTTP yanıt örneği. Uygulama seri durumundan çıkarma kod yazmanız gerekmez.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Oluşturma ve HttpClient başlatma

Statik inceleyin **HttpClient** özelliği:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** kez oluşturulacak hedeflenen ve bir uygulamanın ömrü yeniden. Aşağıdaki koşullar sonuçlanabilir **SocketException** hataları:

* Yeni bir oluşturma **HttpClient** istek başına örnek.
* Ağır yük altında sunucu.

Yeni bir oluşturma **HttpClient** istek başına örnek kullanılabilir yuva tüketebilir.

Aşağıdaki kod başlatır **HttpClient** örneği:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Yukarıdaki kod:

* Taban URI HTTP istekleri için ayarlar. Sunucu uygulamasında kullanılan bağlantı noktası için bağlantı noktası numarasını değiştirin. Uygulama bağlantı noktası sunucu uygulaması için kullanıldıkları sürece çalışmaz.
* "Application/json" için Accept üstbilgisini ayarlar. Bu üst bilgi ayarlama, JSON biçiminde veri göndermek sunucuyı söyler.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Kaynak almak için bir GET isteği gönder

Aşağıdaki kod, bir ürün için bir GET isteği gönderir:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync** yöntem, HTTP GET isteği gönderir. Yöntemi tamamlandığında döndürür bir **HttpResponseMessage** , HTTP yanıtı içerir. Yanıt durum kodu bir başarı kodu ise yanıt gövdesi bir ürün JSON gösterimini içerir. Çağrı **ReadAsAsync** JSON yükü için seri durumdan çıkarılacak bir `Product` örneği. **ReadAsAsync** yanıt gövdesi büyük olabileceğinden yöntemi zaman uyumsuz.

**HttpClient** HTTP yanıtı bir hata kodu içeriyorsa bir özel durum oluşturmaz. Bunun yerine, **IsSuccessStatusCode** özelliği **false** durum bir hata kodu ise. HTTP hata kodları özel durumlar olarak değerlendirilecek tercih ederseniz, çağrı [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) yanıt nesnesinde. `EnsureSuccessStatusCode` Durum kodu 200 aralığın dışında kalan bir özel durum oluşturur&ndash;299. Unutmayın **HttpClient** diğer nedenlerle özel durumlar oluşturabilecek &mdash; Örneğin, istek zaman aşımına uğrar.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Medya türü Biçimlendiricileri serisini kaldırmak için

Zaman **ReadAsAsync** çağrılır hiçbir parametre olmadan varsayılan kullandığı *medya biçimlendiricileri* yanıt gövdesini okumak için. Varsayılan biçimlendiricileri JSON, XML ve formu url kodlanmış verileri destekler.

Varsayılan biçimlendiricileri kullanmak yerine, için biçimlendiricileri listesi sağlayabilirsiniz **ReadAsAsync** yöntemi.  Biçimlendiricileri listesi kullanarak bir özel medya türü biçimlendiricisi varsa yararlı olur:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Daha fazla bilgi için [ASP.NET Web API 2'deki medya Biçimlendiricileri](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Bir kaynak oluşturmak için bir POST isteği gönderme

Aşağıdaki kodu içeren bir POST isteği gönderir. bir `Product` JSON biçimindeki örneği:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync** yöntemi:

* Bir nesne json'a seri hale getirir.
* JSON yükü bir POST isteği gönderir.

İstek başarılı olursa:

* 201 (oluşturuldu) yanıt döndürmesi gerekir.
* Yanıt URL'si oluşturulan kaynakların konumu üst bilgisini içermelidir.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Bir kaynağı güncelleştirmek için PUT isteği gönderme

Aşağıdaki kod, bir ürünü güncelleştirmek için PUT İsteği gönderir:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync** yöntem çalışır gibi **PostAsJsonAsync**dışında posta yerine bir PUT İsteği gönderir.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Bir kaynağı silmek için bir silme isteği gönderiliyor

Aşağıdaki kod, bir ürünü silmek için bir silme isteği gönderir:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

GET gibi bir istek gövdesi bir silme isteği yok. DELETE ile JSON veya XML biçiminde belirtmeniz gerekmez.

## <a name="test-the-sample"></a>Örnek test

İstemci uygulamayı test etmek için:

1. [İndirme](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) ve sunucu uygulamasını çalıştırın. [Yükleme yönergeleri](/aspnet/core/tutorials/#how-to-download-a-sample). Sunucu uygulamasının çalıştığı doğrulayın. Exaxmple için `http://localhost:64195/api/products` ürünlerin listesini döndürmelidir.
2. HTTP isteklerini temel URI'sini ayarlayın. Sunucu uygulamasında kullanılan bağlantı noktası için bağlantı noktası numarasını değiştirin.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. İstemci uygulaması çalıştırın. Aşağıdaki çıkış üretilir:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
