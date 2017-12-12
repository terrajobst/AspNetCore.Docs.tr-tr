---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Birim ASP.NET Web API 2 denetleyicisi testi | Microsoft Docs
author: MikeWasson
description: "Bu konu, birim denetleyicileri Web API 2'de test etmek için bazı belirli teknikleri açıklar. Bu konu okumadan önce birim öğretici okumak isteyebilirsiniz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 167cd24d27977c3652f6a8903054654f5edf7756
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Birim ASP.NET Web API 2 denetleyicisi testi
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Bu konu, birim denetleyicileri Web API 2'de test etmek için bazı belirli teknikleri açıklar. Bu konu okumadan önce öğreticiyi okumak isteyebilirsiniz [birim testi ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), çözümünüz için bir birim testi projesi eklemek nasıl gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Moq kullanılan, ancak aynı fikir herhangi mocking framework için geçerlidir. Moq 4.5.30 (ve daha sonra) Visual Studio 2017, Roslyn ve .NET 4.5 ve sonraki sürümleri destekler.

Birim testlerinde ortak bir deseni &quot;Düzenle-act-assert&quot;:

- Düzenleyin: çalıştırmak test için herhangi bir önkoşulu ayarlayın.
- ACT: test gerçekleştirin.
- Assert: sınama başarılı olduğunu doğrulayın.

Yerleştir adımda mock veya sık kullandığınız nesneleri saplama. Test tek şey sınama odaklanmıştır şekilde bağımlılıkları, sayısı, en aza indirir.

Birim testi Web API denetleyicilerinizi gereken bazı noktalar şunlardır:

- Eylem yanıt doğru türünü döndürür.
- Geçersiz parametreler doğru hata yanıtı döndürür.
- Eylem havuzu veya hizmet katmanı doğru yöntemini çağırır.
- Yanıt etki alanı modeli içeriyorsa, model türü doğrulayın.

Test etmek için genel şeylerden bazıları şunlardır ancak özellikleri denetleyicisi uygulamanızı bağlıdır. Denetleyici eylemleri dönüş olup olmadığını özellikle büyük fark kolaylaştırır **httpresponsemessage öğesini** veya **Ihttpactionresult**. Bu sonuç türlerinden hakkında daha fazla bilgi için bkz: [eylem sonuçlarını Web API 2'deki](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Bilgisayarın HttpResponseMessage dönüş eylemleri test etme

İşte bir örnek denetleyicisinin olan eylemler return **httpresponsemessage öğesini**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Bildirim denetleyicisi eklemesine bağımlılık ekleme kullanan bir `IProductRepository`. Sahte bir depo ekleyemezsiniz çünkü denetleyicisi fazla sınanabilir yapıyorsa. Aşağıdaki birim testi doğrular `Get` yöntemi yazma bir `Product` yanıt gövdesi için. Varsayımında `repository` bir mock olduğu `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Ayarlamak önemlidir **isteği** ve **yapılandırma** denetleyicisinde. Aksi takdirde, test başarısız bir **ArgumentNullException** veya **InvalidOperationException**.

## <a name="testing-link-generation"></a>Test bağlantısı oluşturma

`Post` Yöntem çağrılarını **UrlHelper.Link** yanıtta bağlantılar oluşturmak için. Bu, birim testi biraz daha fazla kurulumunda gerektirir:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper** sınıf gereken istek URL'si ve rota verilerini bu değerleri ayarlamak test sahiptir. Başka bir seçenek mock saplama mi **UrlHelper**. Bu yaklaşımda, varsayılan değeri değiştirin [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) sabit bir değer döndürür mock veya saplama sürüme sahip.

Şimdi kullanarak test yeniden [Moq](https://github.com/Moq) framework. Yükleme `Moq` test projesinin NuGet paketi.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

Bu sürümde, herhangi bir rota verileri kurmak çünkü gerekmez mock **UrlHelper** sabit bir dize döndürür.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Ihttpactionresult dönüş eylemleri test etme

Web API 2'de bir denetleyici eylemi döndürebilir **Ihttpactionresult**, benzer olduğu **ActionResult** ASP.NET mvc'de. **Ihttpactionresult** arabirimi HTTP yanıtları oluşturmak için bir komut desen tanımlar. Denetleyici döndürür yanıtı doğrudan oluşturmak yerine, bir **Ihttpactionresult**. Daha sonra ardışık düzen çağırır **Ihttpactionresult** yanıtı oluşturmak için. İçin gerekli Kurulumu çok atlayabilirsiniz çünkü bu yaklaşım, birim testleri yazma kolaylaştırır **httpresponsemessage öğesini**.

Olan eylemler return bir örnek controller işte **Ihttpactionresult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Bu örnek kullanan bazı ortak desenler gösterir **Ihttpactionresult**. Görelim nasıl birimine bunları sınayın.

### <a name="action-returns-200-ok-with-a-response-body"></a>Eylem 200 (Tamam) ile bir yanıt gövdesi döndürür

`Get` Yöntem çağrılarını `Ok(product)` ürün bulunursa. Birim testi dönüş türü olduğundan emin olun **OkNegotiatedContentResult** ve döndürülen ürün sağ kimliği vardır.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Birim testi eylem sonucu yürütmez dikkat edin. Eylem sonucunu doğru HTTP yanıtı oluşturur varsayabilirsiniz. (Neden olan kendi birim testleri Web API çerçevesi sahip!)

### <a name="action-returns-404-not-found"></a>Eylem 404 (bulunamadı) döndürür

`Get` Yöntem çağrılarını `NotFound()` ürün bulunmazsa. Dönüş türü ise bu durumda, yalnızca denetimleri birim test **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Eylem hiçbir yanıt gövdesi ile 200 (Tamam) döndürür

`Delete` Yöntem çağrılarını `Ok()` boş bir HTTP 200 yanıtı dönün. Dönüş türü, birim testi gibi önceki örnekte, bu durumda denetler **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Eylem bir konum üstbilgisi ile 201 (oluşturuldu) döndürür

`Post` Yöntem çağrılarını `CreatedAtRoute` konumu üstbilgisinde bir URI ile HTTP 201 yanıtını dönün. Birim testi eylemi doğru yönlendirme değerlerini ayarlar doğrulayın.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Yanıt gövdesi olan başka bir 2xx eylem döndürür

`Put` Yöntem çağrılarını `Content` yanıt gövdesi HTTP 202 (kabul edilen) yanıtıyla dönün. Bu durumda, 200 (Tamam) döndürmek için benzer ancak birim testi durum kodunu denetlemeniz gerekir.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [Entity Framework mocking zaman birim ASP.NET Web API 2 testi](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Bir ASP.NET Web API hizmeti için testleri yazma](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog yayını Youssef Moussaoui tarafından).
- [ASP.NET Web API rota hata ayıklayıcısı ile hata ayıklama](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
