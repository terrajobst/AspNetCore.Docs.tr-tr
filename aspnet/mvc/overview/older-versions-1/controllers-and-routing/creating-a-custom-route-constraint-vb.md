---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Bir özel rota kısıtlaması oluşturma (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir. Biz basit bir uygulama bir yolu olmasını önleyen özel kısıtlaması eşleşen w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 72389d11467cbf7baea4cc9452266edb8ab81125
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840481"
---
<a name="creating-a-custom-route-constraint-vb"></a>Bir özel rota kısıtlaması oluşturma (VB)
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir. Biz, bir tarayıcı isteğini bir uzak bilgisayardan yapıldığında eşleşen bir rota engelleyen basit bir özel kısıtlaması uygular.


Bu öğreticinin özel rota kısıtlaması nasıl oluşturacağınızı göstermek için hedeftir. Özel rota kısıtlaması, bazı özel koşul sisteminkiyle eşleşmediği sürece eşleşen bir rota önlemenize olanak sağlar.

Bu öğreticide, Localhost rota kısıtlaması oluştururuz. Localhost rota kısıtlaması, yalnızca yerel bilgisayardan yapılan istekleri eşleşir. Internet üzerinden uzak isteklerinden karşılaştırılamıyor.

Özel rota kısıtlaması, IRouteConstraint arabirimi uygulayarak uygulayın. Bu tek bir yöntemi açıklayan son derece basit bir arabirim.

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Yöntemi, bir Boole değeri döndürür. False döndürürse, rota kısıtlaması ile ilişkili bir tarayıcı isteğini eşleşmiyor.

Localhost kısıtlaması listeleme 1'de yer alır.

**1 - LocalhostConstraint.vb listeleme**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

1 listeleme kısıtlamasındaki HttpRequest sınıfını tarafından kullanıma sunulan IsLocal özelliği yararlanır. Bu özellik, isteğin IP adresi ya da 127.0.0.1 olduğunda veya isteğin IP sunucunun IP adresi ile aynı olduğunda true değerini döndürür.

Bir rota Global.asax dosyasında tanımlanan özel bir kısıtlamada kullanırsınız. 2 listeleme Global.asax dosyasında Localhost kısıtlaması herhangi bir yönetim sayfası yerel sunucudan istek olmayacaksa istemesini engellemek için kullanır. Örneğin, bir Uzak sunucudan yapıldığında /Admin/DeleteAll isteği başarısız olur.

**2 - Global.asax listeleme**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Localhost kısıtlaması yönetici rota tanımında kullanılır. Bu yol, uzak tarayıcı isteğiyle eşleşen olmaz. Ancak, Global.asax dosyasında tanımlanan diğer yolları aynı istekte eşleşebilir farkında olun. Bir isteği eşleşen dizinden bir kısıtlama belirli bir rota engeller ve tüm yollar Global.asax dosyasında tanımlanmış anlamak önemlidir.

Varsayılan rota Global.asax dosyasında listeleniyor 2'den yorum olarak belirtilmiştir, dikkat edin. Ardından varsayılan yolu içeriyorsa, varsayılan yol yönetim denetleyicisinin istekleri eşleşir. Bu durumda, isteklerin yönetici rota ile eşleşmekte mıydı olsa bile uzak kullanıcılar yine de yönetim denetleyicisinin eylemleri çağırabilirsiniz.

> [!div class="step-by-step"]
> [Önceki](creating-a-route-constraint-vb.md)
