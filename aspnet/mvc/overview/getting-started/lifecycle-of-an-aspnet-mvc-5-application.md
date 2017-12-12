---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: "Bir ASP.NET MVC 5 uygulama yaşam döngüsü | Microsoft Docs"
author: cephalin
description: "Bir ASP.NET MVC 5 uygulama yaşam döngüsü grafikleri bir PDF belgesini indirin. Bu yaşam döngüsü belge MVC yaşam döngüsü üst düzey bir görünümünü sunan bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5692c43168eb261c91f40e2046897a1e5d31a028
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Bir ASP.NET MVC 5 uygulama yaşam döngüsü
====================
tarafından [Cephas Bağla](https://github.com/cephalin)

[PDF belgesini indirin](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Burada HTTP almasını her ASP.NET MVC 5 uygulama yaşam döngüsü İstek HTTP yanıtı göndermeyi grafikleri istemciye bir PDF belgesini indirebilirsiniz. ASP.NET MVC için yeni olanlar için eğitim bir aracı hem de ayrıca olarak bir başvuru uygulama belirli yönlerini incelemek için ihtiyacınız olanlar için tasarlanmıştır. PDF belgesini aşağıdaki özelliklere sahiptir:

- İlgili [HttpApplication](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx) MVC tümleştirir burada anlamanıza yardımcı olması için aşamaları [ASP.NET uygulama yaşam döngüsü](https://msdn.microsoft.com/en-us/library/bb470252.aspx).
- İstek işleme ardışık düzeninde her MVC uygulaması geçtiği önemli aşamalar burada anlayabileceği MVC uygulama yaşam döngüsü, üst düzey görünümü.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- İstek işleme ardışık düzenine ayrıntılarını aşağı ayrıntısına gösterir ayrıntı görünümü. Üst düzey görünümü ve yaşam döngüleri ayrıntıları çeşitli aşamaları nasıl toplanır görmek için ayrıntı görünümü karşılaştırabilirsiniz. [PDF'yi indirmek](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) daha büyük bir görmek için.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Yerleştirme ve tüm geçersiz kılınabilir yöntemleri amacı [denetleyicisi](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.aspx) istek işleme ardışık düzeninde nesnesi. Herhangi bir yöntemini geçersiz kılın gerek olmayabilir veya olabilir, ancak düşündüğünüz etkisi için uygun yaşam döngüsü aşamada kod yazabilirsiniz uygulama yaşam döngüsü içindeki rollerine anlaşılması önemlidir.
- Her filtre türü (kimlik doğrulama, yetkilendirme, eylem ve sonuç) nasıl çağrıldığını gösteren attı yukarı diyagramları.
- Ayrıntılar görünümünde ilgilendiğiniz her noktasından bir yararlı makale veya blog bağlayın.


## <a name="next-steps"></a>Sonraki Adımlar

Bu belge, gereksinimini karşılıyor mu? Görüşleriniz bizim için önemlidir. Uygulamanızda, ASP.NET MVC yaşam döngüsü herhangi bir sorunuz varsa [Stackoverflow](http://stackoverflow.com/help) ve [ASP.NET MVC forumları](https://forums.asp.net/1146.aspx) sormak için harika yerlerdir. İzleyin [bana](https://twitter.com/Cephas_MSFT) my son öğreticileri güncelleştirmeleri alabilmek için Twitter'da.
