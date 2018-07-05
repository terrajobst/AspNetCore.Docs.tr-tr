---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Özel rota kısıtlaması (C#) oluşturma | Microsoft Docs
author: StephenWalther
description: Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir. Biz basit bir uygulama bir yolu olmasını önleyen özel kısıtlaması eşleşen w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: a77a25672c94d6b706af0cc36807b11297bc573a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400009"
---
<a name="creating-a-custom-route-constraint-c"></a>Özel rota kısıtlaması (C#) oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir. Biz, bir tarayıcı isteğini bir uzak bilgisayardan yapıldığında eşleşen bir rota engelleyen basit bir özel kısıtlaması uygular.


Bu öğreticinin özel rota kısıtlaması nasıl oluşturacağınızı göstermek için hedeftir. Özel rota kısıtlaması, bazı özel koşul sisteminkiyle eşleşmediği sürece eşleşen bir rota önlemenize olanak sağlar.

Bu öğreticide, Localhost rota kısıtlaması oluştururuz. Localhost rota kısıtlaması, yalnızca yerel bilgisayardan yapılan istekleri eşleşir. Internet üzerinden uzak isteklerinden karşılaştırılamıyor.

Özel rota kısıtlaması, IRouteConstraint arabirimi uygulayarak uygulayın. Bu tek bir yöntemi açıklayan son derece basit bir arabirim.

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Yöntemi, bir Boole değeri döndürür. False döndürürse, rota kısıtlaması ile ilişkili bir tarayıcı isteğini eşleşmiyor.

Localhost kısıtlaması listeleme 1'de yer alır.

**1 - LocalhostConstraint.cs listeleme**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

1 listeleme kısıtlamasındaki HttpRequest sınıfını tarafından kullanıma sunulan IsLocal özelliği yararlanır. Bu özellik, isteğin IP adresi ya da 127.0.0.1 olduğunda veya isteğin IP sunucunun IP adresi ile aynı olduğunda true değerini döndürür.

Bir rota Global.asax dosyasında tanımlanan özel bir kısıtlamada kullanırsınız. 2 listeleme Global.asax dosyasında Localhost kısıtlaması herhangi bir yönetim sayfası yerel sunucudan istek olmayacaksa istemesini engellemek için kullanır. Örneğin, bir Uzak sunucudan yapıldığında /Admin/DeleteAll isteği başarısız olur.

**2 - Global.asax listeleme**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Localhost kısıtlaması yönetici rota tanımında kullanılır. Bu yol, uzak tarayıcı isteğiyle eşleşen olmaz. Ancak, Global.asax dosyasında tanımlanan diğer yolları aynı istekte eşleşebilir farkında olun. Bir isteği eşleşen dizinden bir kısıtlama belirli bir rota engeller ve tüm yollar Global.asax dosyasında tanımlanmış anlamak önemlidir.

Varsayılan rota Global.asax dosyasında listeleniyor 2'den yorum olarak belirtilmiştir, dikkat edin. Ardından varsayılan yolu içeriyorsa, varsayılan yol yönetim denetleyicisinin istekleri eşleşir. Bu durumda, isteklerin yönetici rota ile eşleşmekte mıydı olsa bile uzak kullanıcılar yine de yönetim denetleyicisinin eylemleri çağırabilirsiniz.

> [!div class="step-by-step"]
> [Önceki](creating-a-route-constraint-cs.md)
> [İleri](asp-net-mvc-controller-overview-vb.md)
