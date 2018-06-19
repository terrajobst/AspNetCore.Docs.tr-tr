---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Eylem sonuçlarını Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: d0db5c6d45020861d7295ab1db989caee525fff9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036472"
---
<a name="action-results-in-web-api-2"></a>Eylem sonuçlarını Web API 2
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu konu, nasıl ASP.NET Web API dönüş değeri bir denetleyici eylemi bir HTTP yanıt iletisine dönüştürür açıklar.

Bir Web API denetleyici eylemi aşağıdakilerden herhangi birini döndürebilirsiniz:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Başka bir türü

Bunlar hangisinin bağlı olarak, Web API HTTP yanıtı oluşturmak için farklı bir mekanizma kullanan döndürülür.

| Dönüş türü | Web API yanıt nasıl oluşturur |
| --- | --- |
| void | Dönüş boş 204 (No içerik) |
| **HttpResponseMessage** | Bir HTTP yanıt iletisini doğrudan dönüştürün. |
| **IHttpActionResult** | Çağrı **ExecuteAsync** oluşturmak için bir **httpresponsemessage öğesini**, bir HTTP yanıt iletisini dönüştürün. |
| Diğer türü | Serileştirilmiş dönüş değeri yanıt gövdesi yazma; 200 (Tamam) döndürür. |

Bu konunun geri kalanında her seçeneği daha ayrıntılı açıklanmıştır.

## <a name="void"></a>void

Dönüş türü ise `void`, Web API yalnızca boş bir HTTP yanıt durum koduyla 204 (No içerik) döndürür.

Örnek denetleyicisi:

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP yanıtı:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Eylem döndürürse bir [httpresponsemessage öğesini](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API dönüştürür dönüş değeri doğrudan bir HTTP yanıt iletisine, özelliklerini kullanarak **httpresponsemessage öğesini** doldurmak için nesne yanıt.

Bu seçenek büyük bir yanıt iletisi üzerinde denetim sağlar. Örneğin, aşağıdaki denetleyici eylemi Cache-Control üstbilgisinin ayarlar.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Yanıtı:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Bir etki alanı modeline geçirirseniz **CreateResponse** yöntemi, Web API'sini kullanan bir [medya biçimlendiricisi](../formats-and-model-binding/media-formatters.md) yanıt gövdesi seri hale getirilmiş modeli yazmak için.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Web API biçimlendirici seçmek için istek kabul etme üstbilgisi kullanır. Daha fazla bilgi için bkz: [içerik anlaşması](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

**Ihttpactionresult** arabirimi, Web API 2'de sunulmuştur. Esas olarak, tanımlayan bir **httpresponsemessage öğesini** üreteci. Kullanmanın bazı avantajları şunlardır **Ihttpactionresult** arabirimi:

- Basitleştirir [birim testi](../testing-and-debugging/unit-testing-controllers-in-web-api.md) denetleyicilerinizi.
- HTTP yanıt ayrı sınıflara oluşturmak için ortak mantığı taşır.
- Yanıt oluşturma alt düzey ayrıntılarını gizleme tarafından NET, denetleyici eylem amacı yapar.

**Ihttpactionresult** tek bir yöntem içerir **ExecuteAsync**, zaman uyumsuz olarak oluşturan bir **httpresponsemessage öğesini** örneği.

[!code-csharp[Main](action-results/samples/sample6.cs)]

Bir denetleyici eylemi döndürürse bir **Ihttpactionresult**, Web API çağrılarının **ExecuteAsync** yöntemi oluşturmak için bir **httpresponsemessage öğesini**. Bunu dönüştüren sonra **httpresponsemessage öğesini** bir HTTP yanıt iletisine.

Basit bir implementaton, işte **Ihttpactionresult** bir düz metin yanıt oluşturur:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Örnek denetleyici eylem:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Yanıtı:

[!code-console[Main](action-results/samples/sample9.cmd)]

Daha sık kullanacağınız **Ihttpactionresult** tanımlanan uygulamaları  **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)**  ad alanı. **ApiController** sınıfı, bu yerleşik eylem sonuçları döndüren Yardımcısı yöntemleri tanımlar.

Aşağıdaki örnekte, denetleyici istek var olan bir ürün kimliği eşleşmiyorsa çağırır [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) 404 (bulunamadı) yanıt oluşturmak için. Aksi takdirde, denetleyici çağırır [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), hangi 200 (Tamam) bir yanıt oluşturan ürün içerir.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Diğer dönüş türleri

Diğer tüm dönüş türleri için Web API'sini kullanan bir [medya biçimlendiricisi](../formats-and-model-binding/media-formatters.md) dönüş değeri serileştirmek için. Web API serileştirilmiş değer yanıt gövdesi yazar. Yanıt durum kodu 200 (Tamam)'dür.

[!code-csharp[Main](action-results/samples/sample11.cs)]

Bu yaklaşımın bir dezavantajı, 404 gibi bir hata kodu doğrudan döndüremez ' dir. Ancak, throw bir **HttpResponseException** hata kodları için. Daha fazla bilgi için bkz: [özel durum işleme ASP.NET Web API içinde](../error-handling/exception-handling.md).

Web API biçimlendirici seçmek için istek kabul etme üstbilgisi kullanır. Daha fazla bilgi için bkz: [içerik anlaşması](../formats-and-model-binding/content-negotiation.md).

Örnek istek

[!code-console[Main](action-results/samples/sample12.cmd)]

Örnek yanıt:

[!code-console[Main](action-results/samples/sample13.cmd)]
