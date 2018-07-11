---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "ASP.NET Web API'de HTML Form verileri gönderme: Form-urlencoded verileri | Microsoft Docs"
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 617b16da20f448bf86e4b99841ad6eeaf8aafe4d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818080"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>ASP.NET Web API'de HTML Form verileri gönderme: Form-urlencoded verileri
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>1. Bölüm: Form-urlencoded verileri

Bu makalede, bir Web API denetleyicisi için form-urlencoded verileri gönderileceği gösterilmektedir.

- [HTML formu genel bakış](#overview_of_html_forms)
- [Karmaşık türler gönderme](#sending_complex_types)
- [AJAX üzerinden form verileri gönderme](#sending_form_data_via_ajax)
- [Gönderen basit türler](#sending_simple_types)

> [!NOTE]
> [Tamamlanmış projeyi indirmek](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>HTML formu genel bakış

HTML form kullanımı alın veya veri sunucuya göndermek için gönderin. **Yöntemi** özniteliği **form** öğesi HTTP yöntemi sunar:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

GET için varsayılan yöntemdir. Form kullanıyorsa, formun URI sorgu dizesi olarak kodlanmış verileri alın. Form POST kullanıyorsa, form verilerini istek gövdesinde yer alır. Deftere nakledilen veri **Notenctype** özniteliği, istek gövdesi biçimini belirtir:

| Notenctype | Açıklama |
| --- | --- |
| Application/x-www-form-urlencoded işlemek | Ad/değer çiftleri, benzer bir URI sorgu dizesi olarak kodlanmış bir form verileri. Bu GÖNDERİ için varsayılan biçimidir. |
| multipart/form-data | Form verileri çok parçalı MIME ileti olarak kodlanır. Bir dosya sunucusuna yüklüyorsanız bu biçimi kullanın. |

Bu makalede, bölüm 1 x-www-form-urlencoded işlemek biçimi arar. [2. bölüm](sending-html-form-data-part-2.md) çok parçalı MIME açıklar.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Karmaşık türler gönderme

Genellikle, birden çok form denetimlerini alınan değerleri oluşan karmaşık bir tür gönderir. Durum güncelleştirmesi temsil eden şu model göz önünde bulundurun:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Kabul eden bir Web API denetleyicisi İşte bir `Update` POST aracılığıyla nesne.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Bu denetleyicisi kullanan [eylem tabanlı yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), rota şablonu, bu nedenle &quot;API / {denetleyici} / {eylem} / {id}&quot;. İstemci verileri post gerçekleştireceği &quot;/api/updates/complex&quot;.


Artık kullanıcıların durumu güncelleştirmeyi göndermek bir HTML formuna yazalım.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Dikkat **eylem** özniteliktir formdaki denetleyicisi eylemimiz URI'si. Girilen bazı değerler formu şöyledir:

![](sending-html-form-data-part-1/_static/image1.png)

Kullanıcı gönderme tıkladığında tarayıcı bir HTTP isteği aşağıdakine benzer gönderir:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

İstek gövdesini form verileri, ad/değer çiftleri biçimlendirilmiş içerdiğine dikkat edin. Web API'si bir örneğine otomatik olarak ad/değer çiftleri dönüştürür `Update` sınıfı.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>AJAX üzerinden form verileri gönderme

Kullanıcı formu gönderdiğinde, tarayıcı Geçerli sayfadan ayrılmak gider ve yanıt iletisinin gövdesini işler. Yanıtı HTML sayfası Tamam andır. Bir web API ile ancak yanıt gövdesinin genellikle geçerli boş veya JSON gibi yapılandırılmış verilerin içerir. Bu durumda, bu istek bir AJAX kullanarak form verilerini, sayfanın yanıt işleyebilmesi göndermek için daha anlamlı olur.

Aşağıdaki kod, jQuery kullanarak form verileri gönderileceği gösterilmektedir.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery **gönderme** işlevi, form eylemi yeni bir işlev ile değiştirir. Bu, Gönder düğmesinin varsayılan davranışı geçersiz kılar. **Serileştirmek** işlevi ad/değer çiftlerine form verilerini serileştirir. Form verileri sunucuya göndermek için arama `$.post()`.

İstek tamamlandıktan sonra `.success()` veya `.error()` işleyicisi, kullanıcı için uygun bir ileti görüntülenir.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Gönderen basit türler

Önceki bölümlerde, Web API'si için bir model sınıfının bir örneği seri durumdan bir karmaşık tür gönderdik. Ayrıca, bir dize gibi basit türler de gönderebilirsiniz.

> [!NOTE]
> Basit bir tür göndermeden önce değerin bir karmaşık türü yerine sarmalama göz önünde bulundurun. Bu, sunucu tarafında model doğrulama avantajlarını sağlar ve gerekirse modelinizi genişletmek daha kolay hale getirir.


Basit bir tür göndermek için temel adımlar aynıdır, ancak iki küçük farklılıklar vardır. İlk olarak, denetleyicisi, parametre adı ile tasarlamanız gerekir **FromBody** özniteliği.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Varsayılan olarak, Web API'si basit türler istek URI'SİNDEN almaya çalışır. **FromBody** öznitelik değeri gövdeden okunacak Web API söyler.

> [!NOTE]
> Web API yanıt gövdesinin en fazla bir kez, bu nedenle yalnızca tek bir eylem parametresinin gövdeden gelebilir okur. Gövdeden birden çok değer almanız gerekirse, bir karmaşık tür tanımlar.


İkinci olarak, istemci aşağıdaki biçimde değeri göndermesi gerekiyor:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Özellikle, ad/değer çiftinin ad bölümünü basit bir tür için boş olmalıdır. Tüm tarayıcılar bu için HTML formları desteklemez, ancak, bu biçim şu şekilde oluştur:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Bir örnek formu şöyledir:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Ve form değer göndermek için komut dosyası aşağıdadır. Tek fark önceki komut dosyası içine geçirilen bağımsız değişken, **sonrası** işlevi.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Basit bir tür dizisi göndermek için aynı yaklaşımı kullanabilirsiniz:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Ek Kaynaklar

[2. Bölüm: Karşıya dosya yükleme ve çok parçalı MIME](sending-html-form-data-part-2.md)
