---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Eylem (VB) oluşturma | Microsoft Docs
author: microsoft
description: ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir eylem için bir yöntem gereksinimleri hakkında bilgi edinin.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 070dd4c9d68327eec52fe385000b9ca3907eaa9f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752174"
---
<a name="creating-an-action-vb"></a>Eylem (VB) oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir eylem için bir yöntem gereksinimleri hakkında bilgi edinin.


Bu öğreticinin amacı, yeni bir denetleyici eylemi nasıl oluşturacağınızı açıklar sağlamaktır. Bir eylem yönteminin gereksinimleri hakkında bilgi edinin. Ayrıca bir yöntem bir eylem olarak açıklanmasını önlemenize öğrenin.

## <a name="adding-an-action-to-a-controller"></a>Denetleyici için eylem ekleme

Bir denetleyici için yeni bir yöntem denetleyiciye ekleyerek yeni bir eylem ekleyin. Örneğin, denetleyici 1 listeleme İNDİS() adlı bir eylem ve SayHello() adlı bir eylem içerir. Her iki yöntem de eylem olarak kullanıma sunulur.

**1 - Controllers\HomeController.vb listeleme**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Bir eylem olarak evreni maruz kalabilir için bir yöntem belirli gereksinimleri karşılamalıdır:

- Yöntemi, ortak olmalıdır.
- Yöntem statik bir yöntemi olamaz.
- Yöntemin bir genişletme yöntemi olamaz.
- Bir oluşturucu, alıcı veya ayarlayıcı yöntemi olamaz.
- Yöntemi, açık genel türler sahip olamaz.
- Yöntemi denetleyici temel sınıfın bir yöntem değil.
- Bir yöntemi içeremez **ref** veya **kullanıma** parametreleri.

Bir denetleyici eylemi dönüş türüne hiçbir kısıtlama olduğuna dikkat edin. Bir denetleyici eylemi bir dize bir DateTime Random sınıfını veya void örneği döndürebilir. ASP.NET MVC çerçevesi bir eylem sonucu bir dizeye değil herhangi bir dönüş türü dönüştürmek ve tarayıcıya dize işleme.

Bu gereksinimleri denetleyicisi ihlal etmemesini herhangi bir yöntemi eklediğinizde, yöntem bir denetleyici eylemi kullanıma sunulur. Burada dikkatli olun. Bir denetleyici eylemi, Internet'e bağlı herhangi bir kişi tarafından çağrılabilir. Örneğin, bir DeleteMyWebsite() denetleyici eylemi oluşturmayın.

## <a name="preventing-a-public-method-from-being-invoked"></a>Genel bir yöntem çağrılmakta olan önleme

Genel bir yöntem bir controller sınıfında oluşturmanız gerekir ve yöntemi bir denetleyici eylemi olarak göstermek istemediğiniz sonra yöntemi kullanarak çağrılmasını engelleyebilir &lt;NonAction&gt; özniteliği. Örneğin, denetleyici listeleme 2 ile donatılmış CompanySecrets() adlı bir genel yöntem içerir &lt;NonAction&gt; özniteliği.

**2 - Controllers\WorkController.vb listeleme**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Tarayıcınızın adres çubuğuna /Work/CompanySecrets yazarak CompanySecrets() denetleyici eylemini çağırma denerseniz, Şekil 1'de hata iletisi alırsınız.


[![NonAction yöntemi çağırma](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Şekil 01**: NonAction yöntemi çağırma ([tam boyutlu görüntüyü görmek için tıklatın](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Önceki](creating-a-controller-vb.md)
> [İleri](aspnet-mvc-controllers-overview-cs.md)
