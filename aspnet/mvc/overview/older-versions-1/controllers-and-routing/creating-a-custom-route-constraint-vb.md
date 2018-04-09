---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Özel rota kısıtlaması (VB) oluşturma | Microsoft Docs
author: StephenWalther
description: Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir. Biz basit bir uygulama bir rota engeller özel kısıtlama eşleşen w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 094077fa0cb546f4cc91dbf074f8014e62b3b19c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-route-constraint-vb"></a>Özel rota kısıtlaması (VB) oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir. Biz, bir tarayıcı isteğini bir uzak bilgisayardan yapıldığında eşleşen bir rota engeller basit bir özel kısıtlaması uygulamak.


Bu öğreticinin amacı özel rota kısıtlaması nasıl oluşturabileceğinizi gösterir belirlemektir. Özel rota kısıtlaması, bazı özel koşul eşleşen sürece eşleşen bir rota engellemenizi sağlar.

Bu öğreticide, bir Localhost rota kısıtlaması oluşturun. Localhost rota kısıtlaması, yalnızca yerel bilgisayardan yapılan istekleri eşleşir. Internet üzerinden uzaktan isteklerinden eşleşen değil.

Özel rota kısıtlaması IRouteConstraint arabirimi uygulayarak uygulayın. Bu tek bir yöntem tanımlayan çok basit bir arabirim.

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Yöntemi, bir Boole değeri döndürür. False değerini döndürür, kısıtlaması ile ilişkili yol tarayıcı isteğini eşleşmeyecektir.

Localhost kısıtlaması listeleme 1'de yer alır.

**1 - LocalhostConstraint.vb listeleme**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Listeleme 1 kısıtlamasında HttpRequest sınıfı tarafından kullanıma sunulan IsLocal özelliği yararlanır. Bu özellik, istek IP adresini ya da 127.0.0.1 olduğunda veya istek IP sunucunun IP adresi ile aynı olduğunda true değerini döndürür.

Bir rota Global.asax dosyasında tanımlanmış içindeki özel bir kısıtlaması kullanın. Listeleme 2 Global.asax dosyasındaki Localhost kısıtlaması herhangi biri yerel sunucudan istek yaptıkları sürece bir yönetim sayfası istemesini engellemek için kullanır. Örneğin, bir istek /Admin/DeleteAll için uzak bir sunucudan yapıldığında başarısız olur.

**2 - Global.asax listeleme**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Localhost kısıtlama yönetici rota tanımında kullanılır. Bu yol, bir uzak tarayıcı isteğiyle eşleşen olmaz. Ancak, Global.asax dosyasında tanımlanmış diğer yollar aynı istekte eşleşebilir unutmayın. Bir istek eşleşen belirli bir rota kısıtlama engeller ve tüm yollar Global.asax dosyasında tanımlanmış anlamak önemlidir.

Varsayılan rota listeleme 2'deki Global.asax dosyasındaki yorum olarak belirtilmiştir, dikkat edin. Varsayılan rota eklerseniz, varsayılan yol yönetim denetleyicisi için istekleri eşleşir. Bu durumda, kendi isteklerini yönetici rota ile eşleşmekte olmayacaktır olsa da uzak kullanıcıların hala Eylemler yönetim denetleyicisinin çağıramadı.

> [!div class="step-by-step"]
> [Önceki](creating-a-route-constraint-vb.md)
