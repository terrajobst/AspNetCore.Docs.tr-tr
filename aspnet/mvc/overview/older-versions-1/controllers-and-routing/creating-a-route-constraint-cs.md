---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Rota kısıtlaması (C#) oluşturma | Microsoft Docs
author: StephenWalther
description: Bu öğreticide, Normal ifadelerle rota kısıtlamalarını oluşturarak tarayıcı eşleşen yolların nasıl istekleri nasıl kontrol edebilir Stephen Walther gösterir.
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: f0dbbb880bc679da934444b87ef24d040b69e2f7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828260"
---
<a name="creating-a-route-constraint-c"></a>Rota kısıtlaması (C#) oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Bu öğreticide, Normal ifadelerle rota kısıtlamalarını oluşturarak tarayıcı eşleşen yolların nasıl istekleri nasıl kontrol edebilir Stephen Walther gösterir.


Rota kısıtlamalarını belirli bir rota ile eşleşmekte tarayıcı istekleri kısıtlamak için kullanın. Rota kısıtlaması belirtmek için normal bir ifade kullanabilirsiniz.

Örneğin, rota, Global.asax dosyasında listeleniyor 1'de tanımladığınız düşünün.

**1 - Global.asax.cs listeleme**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

Kod 1 ürün adlı bir rota içerir. Tarayıcı istek listeleme 2'de yer alan ProductController eşlemek için ürün yol kullanabilirsiniz.

**2 - Controllers\ProductController.cs listeleme**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Ürün denetleyicisi tarafından kullanıma sunulan Details() eylem ProductID adlı tek bir parametre kabul ettiğini dikkat edin. Bu parametre, bir tamsayı parametresidir.

Listeleme 1'de tanımlanan yol, aşağıdaki URL'lerden herhangi birini eşleşmesi:

- / Ürün/23
- Ürün/7

Ne yazık ki, yol aşağıdaki URL'ler eşleşir:

- / Ürün/filan
- / Ürün/apple

Bir tamsayı değeri dışında bir şey içeren bir istekte Details() eylemi bir tam sayı parametresi beklediği hataya neden olur. URL /Product/apple tarayıcınıza yazın örneğin, daha sonra hata sayfası Şekil 1'de alırsınız.


[![Yeni Proje iletişim kutusu](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Şekil 01**: explode bir sayfayı görmeden ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-route-constraint-cs/_static/image2.png))


Ne yapmak istediğinizden, yalnızca uygun tamsayı ProductID içeren URL'ler aynı olur. Rota ile eşleşmekte URL'leri kısıtlamak için bir rota tanımlarken bir kısıtlama kullanabilirsiniz. Yalnızca tamsayılara eşleşen bir normal ifade kısıtlaması listeleme 3'te değiştirilmiş ürün yol içerir.

**3 - Global.asax.cs listeleme**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

Normal ifade \d+ tamsayılar bir veya daha fazla eşleşir. Bu sınırlama aşağıdaki URL'ler ile eşleştirilecek ürün yol neden olur:

- / Ürün/3
- / Ürün/8999

Ancak aşağıdaki URL'leri:

- / Ürün/apple
- / Ürün

- Bu tarayıcı istekleri başka bir yol tarafından işlenecek veya eşleşen yol yok, ise bir *kaynak bulunamadı* hata döndürülür.

> [!div class="step-by-step"]
> [Önceki](creating-custom-routes-cs.md)
> [İleri](creating-a-custom-route-constraint-cs.md)
