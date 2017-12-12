---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: "Bir eylem (C#) oluşturma | Microsoft Docs"
author: microsoft
description: "ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir yöntemin bir eylem gereksinimleri hakkında bilgi edinin."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b751dc7e34951be33e7c27a3429c383a3e1e1c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-c"></a>Bir eylem (C#) oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir yöntemin bir eylem gereksinimleri hakkında bilgi edinin.


Bu öğretici, yeni bir denetleyici eylemi nasıl oluşturabileceğinizi açıklamak için hedefidir. Bir eylem yönteminin gereksinimleri hakkında bilgi edinin. Ayrıca bir yöntem bir eylem olarak açıklanmasını önlemenize öğrenin.

## <a name="adding-an-action-to-a-controller"></a>Bir denetleyici için eylem ekleme

Yeni bir eylem denetleyicisi için yeni bir yöntem ekleyerek bir denetleyiciye ekleyin. Örneğin, denetleyici listeleme 1 İNDİS() adlı bir eylem ve SayHello() adlı bir eylem içerir. Her iki yöntem eylemler olarak sunulur.

**1 - Controllers\HomeController.cs listeleme**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

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

Genel yöntem bir controller sınıfında oluşturmanız gerekir ve metodu olarak bir denetleyici eylemi açmak istemiyorsanız [NonAction] özniteliğini kullanarak çağrılmasını yöntemi engelleyebilir. Örneğin, denetleyici listeleme 2'deki [NonAction] özniteliği ile donatılmış CompanySecrets() adlı genel bir yöntem içerir.

**2 - Controllers\WorkController.cs listeleme**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Ardından, tarayıcınızın adres çubuğuna /Work/CompanySecrets yazarak CompanySecrets() denetleyici eylemini çağırma çalışırsanız, Şekil 1'de hata iletisi alırsınız.


[![NonAction yöntemi çağırma](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Şekil 01**: NonAction yöntemi çağırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-an-action-cs/_static/image2.png))

>[!div class="step-by-step"]
[Önceki](creating-a-controller-cs.md)
[sonraki](asp-net-mvc-routing-overview-vb.md)
