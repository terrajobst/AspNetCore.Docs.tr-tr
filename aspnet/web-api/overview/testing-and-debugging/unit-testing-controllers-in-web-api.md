---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Test denetleyicileri ASP.NET Web API 2 birim | Microsoft Docs
author: MikeWasson
description: Bu konu, Web API 2'de test denetleyicileri birim için belirli bazı teknikleri açıklar. Bu konuda okumadan önce öğretici birim okumak isteyebilirsiniz...
ms.author: aspnetcontent
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bc6d4d7e65c84036dc35fe39ed25685ed8b213b3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823695"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Test denetleyicileri ASP.NET Web API 2 birim
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Bu konu, Web API 2'de test denetleyicileri birim için belirli bazı teknikleri açıklar. Bu konuda okumadan önce öğretici okumak isteyebilirsiniz [birim testi ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), nasıl bir birim test projesi çözümünüze ekleyin.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Moq kullandım, ancak aynı fikir sahte herhangi bir çerçeveyi için geçerlidir. Moq 4.5.30 (ve sonraki sürümler) Visual Studio 2017, Roslyn ve .NET 4.5 ve sonraki sürümleri destekler.

Birim testlerinde bir ortak desendir &quot;düzenleme-act-assert&quot;:

- Düzenleme: testin çalışması için tüm önkoşulları ayarlayın.
- ACT: test gerçekleştirin.
- Assert: test başarılı olduğunu doğrulayın.

Düzenle adımda sahte veya sık kullandığınız saptama nesneleri. Bir şey testlere test odaklı için bağımlılık sayısı, en aza indirir.

Birim testi, Web APİ'si denetleyicilerinin içinde gereken bazı noktalar şunlardır:

- Eylem türü doğru yanıtı döndürür.
- Geçersiz parametreler doğru hata yanıtı döndürür.
- Eylem deposuna veya hizmet katmanı doğru yöntemi çağırır.
- Yanıt, bir etki alanı modeli içeriyorsa, model türü doğrulayın.

Bazı test etmek için gereken genel noktalar şunlardır ancak özellikleri denetleyicisi uygulamanıza bağlıdır. Denetleyici eylemleri dönüş özellikle büyük bir fark netleştirir **HttpResponseMessage** veya **Ihttpactionresult**. Bu sonuç türleri hakkında daha fazla bilgi için bkz. [Web API 2'de eylem sonuçları](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Test HttpResponseMessage dönüş eylemleri

İşte bir örnek etki alanı denetleyicisinin, Eylemler dönüş **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Bildirim denetleyicisi eklemesine bağımlılık ekleme kullanan bir `IProductRepository`. Sahte bir depo ekleyemezsiniz çünkü, denetleyici daha test edilebilir hale getirir. Aşağıdaki birim sınama doğrular `Get` yönteminin yazma bir `Product` yanıt gövdesi için. Varsayımında `repository` bir sahte olduğu `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

Ayarlamak önemlidir **istek** ve **yapılandırma** denetleyicisinde. Aksi halde, test başarısız olur bir **ArgumentNullException** veya **InvalidOperationException**.

## <a name="testing-link-generation"></a>Sınama bağlantısı oluşturma

`Post` Yöntem çağrılarını **UrlHelper.Link** yanıtta bağlantılar oluşturmak için. Bu işlem biraz daha fazla birim testi ayarında gerektirir:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper** sınıfı istek URL'si ve rota verilerini test için bu değerleri ayarlamak sahip olması gerekiyor. Saplama sahte veya başka bir seçenek olan **UrlHelper**. Bu yaklaşımda, varsayılan değeri değiştirin [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) sabit bir değer döndüren saptama sahte veya sürüme sahip.

Şimdi test kullanarak yeniden [Moq](https://github.com/Moq) framework. Yükleme `Moq` test projesinde NuGet paketi.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

Bu sürümde, tüm rota verileri, ayarlayın çünkü gerekmez sahte **UrlHelper** sabit bir dize döndürür.


## <a name="testing-actions-that-return-ihttpactionresult"></a>Test Ihttpactionresult dönüş eylemleri

Web API 2'de bir denetleyici eylemi döndürebilir **Ihttpactionresult**, alınmak üzere olduğu **ActionResult** ASP.NET mvc'de. **Ihttpactionresult** arabirimi HTTP yanıt oluşturmak için bir komut desen tanımlar. Yanıt doğrudan oluşturmak yerine, denetleyici döndürür bir **Ihttpactionresult**. Daha sonra işlem hattını çağıran **Ihttpactionresult** yanıt oluşturmak için. Kurulum için gereken çok fazla atlayabilirsiniz çünkü bu yaklaşım, birim testleri yazma kolaylaştırır **HttpResponseMessage**.

Olan eylemler dönüş örnek controller işte **Ihttpactionresult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Bu örnek kullanan bazı ortak desenleri gösterir **Ihttpactionresult**. Bakalım nasıl birimine bunları test edin.

### <a name="action-returns-200-ok-with-a-response-body"></a>Eylem 200 (Tamam) ile bir yanıt gövdesini döndürür

`Get` Yöntem çağrılarını `Ok(product)` ürün bulunursa. Birim test, dönüş türü olduğundan emin olun **OkNegotiatedContentResult** ve döndürülen ürün doğru kimliği.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Birim testi dikkat edin, eylem sonucu yürütülmez. Eylem sonucu doğru HTTP yanıtını oluşturur varsayabilirsiniz. (Nedeni kendi birim testleri Web API çerçevesi olan!)

### <a name="action-returns-404-not-found"></a>Eylem 404 (bulunamadı) döndürür.

`Get` Yöntem çağrılarını `NotFound()` ürüne bulunamazsa. Bu durumda, dönüş türü ise yalnızca denetimleri birim testi **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>Eylem hiç yanıt gövdesi ile 200 (Tamam) döndürür.

`Delete` Yöntem çağrılarını `Ok()` boş bir HTTP 200 yanıtı dönün. Bu durumda dönüş türü, önceki örnekteki gibi birim testi denetler **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>Eylemi bir konum üst bilgisi ile 201 (oluşturuldu) döndürür.

`Post` Yöntem çağrılarını `CreatedAtRoute` konum üst bilgisinde bir URI ile bir HTTP 201 yanıtı dönün. Birim test, eylemin doğru yönlendirme değerleri ayarlar doğrulayın.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>Yanıt gövdesi olan başka bir 2xx eylem döndürür

`Put` Yöntem çağrılarını `Content` yanıt gövdesi ile bir HTTP 202 (kabul edildi) yanıtı dönün. Bu durumda, 200 (Tamam) döndürmek için benzer, ancak birim testi ayrıca durum kodunu denetleyin.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [Sahte Entity Framework, birim testi ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Bir ASP.NET Web API'si hizmeti için testleri yazma](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog gönderisi Youssef Moussaoui tarafından).
- [ASP.NET Web API rota hata ayıklayıcısı ile hata ayıklama](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
