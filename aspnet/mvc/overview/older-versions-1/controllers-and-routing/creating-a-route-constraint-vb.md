---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: "Bir rota kısıtlaması (VB) oluşturma | Microsoft Docs"
author: StephenWalther
description: "Bu öğreticide, Normal ifadelerle rota kısıtlamalarını oluşturarak tarayıcı eşleşen yolların nasıl istekleri nasıl kontrol edebilir Stephen Walther gösterir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ff2666f4558abd4f8d9bddffd7aef8bb68d7bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-vb"></a>Bir rota kısıtlaması (VB) oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Bu öğreticide, Normal ifadelerle rota kısıtlamalarını oluşturarak tarayıcı eşleşen yolların nasıl istekleri nasıl kontrol edebilir Stephen Walther gösterir.


Rota kısıtlamalarını belirli bir rota ile eşleşmekte tarayıcı isteklerini sınırlamak için kullanın. Bir rota kısıtlamasını belirtmek amacıyla bir normal ifade kullanın.

Örneğin, rota, Global.asax dosyasında listeleniyor 1'de tanımladığınız düşünün.

**1 - Global.asax.vb listeleme**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

1 listeleme ürün adında bir yol içeriyor. Tarayıcı isteklerini listeleme 2'de bulunan ProductController eşlemek için ürün rota kullanabilirsiniz.

**2 - Controllers\ProductController.vb listeleme**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Ürün denetleyicisi tarafından kullanıma sunulan Details() eylem ProductID adlı tek bir parametre kabul eden dikkat edin. Bu parametre bir tamsayı parametredir.

Listeleme 1'de tanımlanan yol aşağıdaki URL'lerden birini eşleşir:

- / Ürün/23
- / Ürün/7

Ne yazık ki, yol aşağıdaki URL'lerle eşleşir:

- / Ürün/filan
- / Ürün/apple

Details() eylemi bir tamsayı parametresi beklediği dışında bir tamsayı değeri içeren bir isteği yapan bir hataya neden olur. URL /Product/apple tarayıcınıza yazın, örneğin, ardından hata sayfası Şekil 1'de alırsınız.


[![Yeni Proje iletişim kutusu](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Şekil 01**: açılımı bir sayfayı görmenizin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-route-constraint-vb/_static/image2.png))


Gerçekten yapmak istediğinize uygun tamsayı ProductID içeren URL'ler yalnızca eşleşen olur. Rota ile eşleşmekte URL'leri kısıtlamak için bir yol tanımlarken bir kısıtlama kullanabilirsiniz. Değiştirilen ürün rota listeleme 3'te yalnızca tamsayı eşleşen bir normal ifade kısıtlaması içerir.

**3 - Global.asax.vb listeleme**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

Normal ifade \d+ bir veya daha fazla tamsayılar eşleşir. Bu sınırlama aşağıdaki URL'ler eşleştirmek ürün yol neden olur:

- / Ürün/3
- / Ürün/8999

Ancak aşağıdaki URL'ler:

- / Ürün/apple
- / Ürün

Bu tarayıcı istekleri başka bir rota tarafından işlenecek veya eşleşen hiçbir yol varsa bir *kaynak bulunamadı* hata döndürülecek.

>[!div class="step-by-step"]
[Önceki](creating-custom-routes-vb.md)
[sonraki](creating-a-custom-route-constraint-vb.md)
