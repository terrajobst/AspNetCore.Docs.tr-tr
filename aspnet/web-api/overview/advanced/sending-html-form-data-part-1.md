---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'ASP.NET Web API HTML Form verileri gönderme: Form urlencoded veri | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566388"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>ASP.NET Web API HTML Form verileri gönderme: Form urlencoded veri
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>1. Kısım: Form urlencoded veri

Bu makalede bir Web API denetleyicisi form urlencoded veri göndermek nasıl gösterilmektedir.

- [HTML formları genel bakış](#overview_of_html_forms)
- [Karmaşık türler gönderme](#sending_complex_types)
- [Form verileri AJAX aracılığıyla gönderme](#sending_form_data_via_ajax)
- [Gönderen basit türler](#sending_simple_types)

> [!NOTE]
> [Tamamlanan projenizi indirin](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>HTML formları genel bakış

HTML formları kullanın alın veya sunucuya veri göndermek için gönderin. **Yöntemi** özniteliği **form** öğesi HTTP yöntemini sunar:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

GET için varsayılan yöntemdir. Formun kullanıyorsa, formun URI sorgu dizesi olarak kodlanmış verileri alın. Form POST kullanıyorsa, form verilerini istek gövdesinde yer alır. Deftere nakledilen veriler için **enctype** özniteliği, istek gövdesini biçimi belirtir:

| enctype | Açıklama |
| --- | --- |
| Uygulama/x-www-form-urlencoded | Ad/değer çiftleri, bir URI sorgu dizesinde benzer olarak kodlanmış form verileri. POST için varsayılan biçimi budur. |
| multipart/form-data | Çok parçalı MIME ileti olarak kodlanmış form verileri. Bir dosyayı sunucuya yüklüyorsanız bu biçimini kullanın. |

Bu makalenin bölüm 1 x-www-form-urlencoded biçiminde görünür. [2. bölüm](sending-html-form-data-part-2.md) çok parçalı MIME açıklar.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Karmaşık türler gönderme

Genellikle, birkaç formu denetimlerini gerçekleştirilecek değerleri oluşan bir karmaşık tür gönderir. Bir durum güncelleştirmesi temsil eden aşağıdaki model göz önünde bulundurun:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Kabul eden bir Web API denetleyicisi İşte bir `Update` POST ile nesne.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Bu denetleyici kullanan [eylem tabanlı yönlendirme](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name)rota şablonu gelir &quot;API / {controller} / {action} / {id}&quot;. İstemci verileri gönderecek &quot;/api/updates/complex&quot;.


Şimdi bir durum güncelleştirmesi göndermek kullanıcılar için bir HTML formuna yazalım.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Dikkat **eylem** formdaki bir özniteliktir bizim denetleyici eylemi URI'si. Girdiğiniz bazı değerler formla şöyledir:

![](sending-html-form-data-part-1/_static/image1.png)

Kullanıcı gönderme tıkladığında tarayıcı bir HTTP isteği aşağıdakine benzer gönderir:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

İstek gövdesindeki ad/değer çiftleri olarak biçimlendirilmiş form verileri içerdiğine dikkat edin. Web API örneği otomatik olarak ad/değer çiftleri dönüştürür `Update` sınıfı.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Form verileri AJAX aracılığıyla gönderme

Kullanıcı formu gönderdiğinde, tarayıcı geçerli sayfasından ayrılmayın gider ve yanıt iletisinin gövdesi işler. Yanıt HTML sayfası olduğunda bu normaldir. Bir web API ile ancak yanıt gövdesi genellikle bozulmuş boş veya JSON gibi yapılandırılmış veri içeriyor. Bu durumda, istek bir AJAX kullanarak form verilerini, böylece sayfa yanıt işleyebilir göndermek için daha fazla mantıklıdır.

Aşağıdaki kodu kullanarak jQuery form veri göndermek nasıl gösterir.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

JQuery **gönderme** işlevi ile yeni bir işlev form eylemi yerini alır. Bu, Gönder düğmesine varsayılan davranışını geçersiz kılar. **Seri** işlevi ad/değer çiftlerine form verilerini serileştirir. Form verileri sunucuya göndermek için çağrı `$.post()`.

İstek tamamlandığında `.success()` veya `.error()` işleyici kullanıcıya uygun bir mesaj görüntüler.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Gönderen basit türler

Önceki bölümlerde modeli sınıfının bir örneği için Web API serisi bir karmaşık tür gönderdi. Dize gibi basit türler de gönderebilirsiniz.

> [!NOTE]
> Basit bir tür göndermeden önce bunun yerine bir karmaşık tür değeri kaydırma göz önünde bulundurun. Bu sunucu tarafında model doğrulama avantajları sağlar ve gerekirse modelinizi genişletmek kolaylaştırır.


Basit bir tür göndermek için temel adımlar aynıdır, ancak iki küçük farklılıklar vardır. İlk olarak, denetleyicisi, parametre adı ile tasarlamanız gerekir **FromBody** özniteliği.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Varsayılan olarak, Web API istek URI'si basit türler almaya çalışır. **FromBody** özniteliği söyler Web API isteği gövdesinden değeri okunamıyor.

> [!NOTE]
> Web API yanıt gövdesi en fazla bir kez, böylece yalnızca tek bir eylem parametresinin isteği gövdesinden gelebilir okur. Birden çok değer isteği gövdesinden almanız gerekirse, karmaşık bir tür tanımlayın.


İkinci olarak, istemci aşağıdaki biçimde değeri göndermesi gerekir:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Özellikle, ad/değer çiftinin ad bölümünü basit bir tür için boş olması gerekir. Tüm tarayıcılar bu için HTML formları desteklemez, ancak, bu biçim komut dosyasında aşağıdaki gibi oluşturun:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Bir örnek form şöyledir:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Burada da form değer göndermek için komut dosyası. Yalnızca önceki komut dosyasından içine geçirilen bağımsız değişken farktır **sonrası** işlevi.

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Basit türleri dizisi göndermek için aynı yaklaşımı kullanabilirsiniz:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Ek Kaynaklar

[2. Kısım: Karşıya dosya yükleme ve çok parçalı MIME](sending-html-form-data-part-2.md)
