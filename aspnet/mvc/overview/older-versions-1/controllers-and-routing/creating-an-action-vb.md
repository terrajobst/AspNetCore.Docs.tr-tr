---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Bir eylem (VB) oluşturma | Microsoft Docs
author: microsoft
description: ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir yöntemin bir eylem gereksinimleri hakkında bilgi edinin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: c77e4738444c61d60bdd78a50b36f98be41fc271
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-vb"></a>Bir eylem (VB) oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir yöntemin bir eylem gereksinimleri hakkında bilgi edinin.


Bu öğretici, yeni bir denetleyici eylemi nasıl oluşturabileceğinizi açıklamak için hedefidir. Bir eylem yönteminin gereksinimleri hakkında bilgi edinin. Ayrıca bir yöntem bir eylem olarak açıklanmasını önlemenize öğrenin.

## <a name="adding-an-action-to-a-controller"></a>Bir denetleyici için eylem ekleme

Yeni bir eylem denetleyicisi için yeni bir yöntem ekleyerek bir denetleyiciye ekleyin. Örneğin, denetleyici listeleme 1 İNDİS() adlı bir eylem ve SayHello() adlı bir eylem içerir. Her iki yöntem eylemler olarak sunulur.

**Listing 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Bir eylem olarak universe maruz kalabilir için bir yöntem belirli gereksinimleri karşılaması gerekir:

- Metodu genel olmalıdır.
- Yöntem statik yöntemi olamaz.
- Yöntemi, bir genişletme yöntemi olamaz.
- Yöntem oluşturucusu, alıcı veya ayarlayıcı olamaz.
- Yöntemi, açık genel türleri sahip olamaz.
- Yöntemi denetleyici temel sınıfın bir yöntem değildir.
- Yöntem içeremez **ref** veya **çıkışı** parametreleri.

Bir denetleyici eylemi dönüş türü üzerinde herhangi bir kısıtlama olduğuna dikkat edin. Bir denetleyici eylemi bir dize bir DateTime rastgele sınıfının veya void bir örnek döndürebilir. ASP.NET MVC çerçevesi bir eylem sonucu bir dizeye olmayan herhangi bir dönüş türü dönüştürün ve tarayıcı dizeye işleme.

Bu gereksinimleri denetleyicisi ihlal olmayan herhangi bir yöntemini eklediğinizde, yöntemi bir denetleyici eylemi sunulur. Burada dikkatli olun. Bir denetleyici eylemi, Internet'e bağlı herhangi bir kişi tarafından çağrılabilir. Örneğin, bir DeleteMyWebsite() denetleyici eylemi oluşturmayın.

## <a name="preventing-a-public-method-from-being-invoked"></a>Genel yöntem çağrılmasını önleme

Genel yöntem bir controller sınıfında oluşturmanız gerekir ve yöntemi bir denetleyici eylemi olarak kullanıma sunmak istemediğiniz ardından yöntemi kullanarak çağrılmasını engelleyebilir &lt;NonAction&gt; özniteliği. Örneğin, denetleyici listeleme 2 ile donatılmış CompanySecrets() adlı genel bir yöntem içerir &lt;NonAction&gt; özniteliği.

**2 - Controllers\WorkController.vb listeleme**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Ardından, tarayıcınızın adres çubuğuna /Work/CompanySecrets yazarak CompanySecrets() denetleyici eylemini çağırma çalışırsanız, Şekil 1'de hata iletisi alırsınız.


[![NonAction yöntemi çağırma](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Şekil 01**: NonAction yöntemi çağırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Önceki](creating-a-controller-vb.md)
> [sonraki](aspnet-mvc-controllers-overview-cs.md)
