---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Eylem (C#) oluşturma | Microsoft Docs
author: microsoft
description: ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir eylem için bir yöntem gereksinimleri hakkında bilgi edinin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: d6f981201b93e3650b18f112dd9330f1470aa573
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372504"
---
<a name="creating-an-action-c"></a>Eylem (C#) oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir eylem için bir yöntem gereksinimleri hakkında bilgi edinin.


Bu öğreticinin amacı, yeni bir denetleyici eylemi nasıl oluşturacağınızı açıklar sağlamaktır. Bir eylem yönteminin gereksinimleri hakkında bilgi edinin. Ayrıca bir yöntem bir eylem olarak açıklanmasını önlemenize öğrenin.

## <a name="adding-an-action-to-a-controller"></a>Denetleyici için eylem ekleme

Bir denetleyici için yeni bir yöntem denetleyiciye ekleyerek yeni bir eylem ekleyin. Örneğin, denetleyici 1 listeleme İNDİS() adlı bir eylem ve SayHello() adlı bir eylem içerir. Her iki yöntem de eylem olarak kullanıma sunulur.

**1 - Controllers\HomeController.cs listeleme**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

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

Ardından bir controller sınıfında genel bir yöntem oluşturmanız gerekir ve yöntemi bir denetleyici eylemi olarak kullanıma sunmak istemiyorsanız, yöntemi [NonAction] özniteliğini kullanarak çağrılmasını engelleyebilir. Örneğin, denetleyici listeleme 2'deki [NonAction] özniteliği ile donatılmış CompanySecrets() adlı bir genel yöntem içerir.

**2 - Controllers\WorkController.cs listeleme**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Tarayıcınızın adres çubuğuna /Work/CompanySecrets yazarak CompanySecrets() denetleyici eylemini çağırma denerseniz, Şekil 1'de hata iletisi alırsınız.


[![NonAction yöntemi çağırma](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Şekil 01**: NonAction yöntemi çağırma ([tam boyutlu görüntüyü görmek için tıklatın](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Önceki](creating-a-controller-cs.md)
> [İleri](asp-net-mvc-routing-overview-vb.md)
