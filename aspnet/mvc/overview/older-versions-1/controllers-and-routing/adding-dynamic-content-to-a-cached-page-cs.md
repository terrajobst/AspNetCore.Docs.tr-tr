---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Önbelleğe alınmış bir sayfaya (C#) dinamik içerik ekleme | Microsoft Docs
author: microsoft
description: Aynı sayfayı dinamik ve önbelleğe alınan içeriğini karıştırmak öğrenin. Sonrası önbellek değiştirme başlığı tanıtımları o gibi dinamik içerik görüntülenecek sağlar...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 5c6d5a223a465a00a08f58a188a16cbf3c310431
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805587"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>Dinamik içerik ekleme önbelleğe alınmış bir sayfaya (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

> Aynı sayfayı dinamik ve önbelleğe alınan içeriğini karıştırmak öğrenin. Sonrası önbellek değiştirme başlık reklamları veya, önbelleğe alınan çıkış bir sayfa içinde olan haber öğelerinin gibi dinamik içerik görüntülemenize olanak sağlar.


Çıkış önbelleğe alma özelliğinden yararlanarak, bir ASP.NET MVC uygulamasının performansını önemli ölçüde artırabilir. Sayfa istenen her zaman bir sayfa üretmek yerine, sayfanın bir kez oluşturulabilir ve birden çok kullanıcı için bellekte önbelleğe alınmış.

Ancak bir sorun oluştu. Peki sayfasında dinamik içerikleri görüntülemek gerekiyor? Örneğin, bir reklam sayfasında görüntülemek istediğinizi düşünün. Her kullanıcı aynı tanıtım görebilmesi için önbelleğe alınacak reklam istemezsiniz. Bu şekilde paranın sabitlenebilen!

Neyse ki, kolay bir çözüm yoktur. Bir özelliğin çağırılır ASP.NET framework'ün yararlanabilirsiniz *sonrası, değiştirme önbelleğe*. Sonrası önbellek değiştirme, bellekte önbelleğe alınmış bir sayfaya dinamik içerik yerine olanak tanır.


Normalde, [OutputCache] özniteliğini kullanarak bir sayfa önbellek çıktısı, sayfa hem sunucu hem de istemci (tarayıcı) önbelleğe alınır. Bir sayfa, sonrası önbellek değiştirme kullandığınızda, yalnızca sunucuda önbelleğe alınır.


#### <a name="using-post-cache-substitution"></a>Sonrası önbellek değiştirme kullanma

Sonrası önbellek değiştirme kullanarak iki adımı gerektirir. İlk olarak, önbelleğe alınan sayfasında görüntülenmesini istediğiniz dinamik içeriği temsil eden bir dize döndüren bir yöntemi tanımlamak gerekir. Ardından, sayfaya dinamik içerik eklemesine HttpResponse.WriteSubstitution() yöntemi çağırın.

Örneğin, rastgele önbelleğe alınmış bir sayfaya farklı haber öğelerini görüntülemek istediğinizi düşünün. 1 listeleme sınıfı, rastgele bir haber öğesi üç haber öğelerinin bir listeden döndüren RenderNews() adlı tek bir yöntemi gösterir.

**1 – Models\News.cs listeleme**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Sonrası önbellek değiştirme yararlanmak için HttpResponse.WriteSubstitution() yöntemi çağırın. WriteSubstitution() yöntemi kodu bölgesi önbelleğe alınmış bir sayfaya dinamik içerik ile değiştirmek için ayarlar. WriteSubstitution() yöntemin rastgele haber öğesinin 2 liste görünümünde görüntülemek için kullanılır.

**2 – Views\Home\Index.aspx listeleme**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

RenderNews yöntemi WriteSubstitution() yöntemine geçirilir. RenderNews yöntemi çağrılmadı dikkat edin (hiçbir parantez vardır). Bunun yerine bir yönteme başvuru için WriteSubstitution() geçirilir.

Dizin görünümünün önbelleğe alınır. Görünüm denetleyicisi listeleme 3'te tarafından döndürülür. İNDİS() eylemi, dizin görünümünün 60 saniye boyunca önbelleğe alınmasına neden olan [OutputCache] özniteliği ile donatılmış dikkat edin.

**3 – Controllers\HomeController.cs listeleme**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Dizin görünümünün önbelleğe alınmış olsa da, dizin sayfası istediğinde farklı rastgele haber öğeler görüntülenir. Dizin Sayfası istediğinde 60 saniye (bkz. Şekil 1) sayfa tarafından görüntülenen zaman değiştirmez. Sayfanın önbelleğe alma süresi değişmez olgu kanıtlar. Ancak, içerik eklenen her istekle WriteSubstitution() yöntemi – rastgele haber öğesinin – değişikliklerden.

**Şekil 1-önbelleğe alınmış bir sayfaya dinamik haber öğeleri ekleme**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Sonrası önbellek değiştirme yardımcı yöntemleri kullanma

Bir özel bir yardımcı yöntem içinde WriteSubstitution() yöntemine çağrı yalıtılacak sonrası önbellek değiştirme yararlanmak için daha kolay bir yolu var. Bu yaklaşım listeleme 4'te yardımcı yöntemi gösterilmiştir.

**4 – AdHelper.cs listeleme**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

4 listeleme içeren iki yöntem gösteren bir statik sınıf: RenderBanner() ve RenderBannerInternal(). RenderBanner() yöntemi gerçek yardımcı yöntemi temsil eder. Bu yöntem, herhangi bir yardımcı yöntemi gibi bir görünümde Html.RenderBanner() çağırabilirsiniz standart ASP.NET MVC HtmlHelper sınıfı genişletir.

RenderBanner() yöntem RenderBannerInternal() yöntemi WriteSubsitution() yöntemine geçirerek HttpResponse.WriteSubstitution() yöntemini çağırır.

Özel bir yöntem RenderBannerInternal() yöntemidir. Bu yöntem, bir yardımcı yöntem sunulmamasını. RenderBannerInternal() yöntemi, üç başlığı reklam görüntü listesinden rastgele bir başlık tanıtım görüntüsünü döndürür.

Listeleme 5'te değiştirilmiş dizin görünümünün RenderBanner() yardımcı yöntemini nasıl kullanabileceğinizi gösterir. Dikkat ek &lt;içeri aktarma % @ %&gt; yönergesi MvcApplication1.Helpers ad alanı içeri aktarmak için görünümün en üstünde bulunur. Bu ad alanı içeri aktarmak yayımlarsanız, RenderBanner() yöntemi Html özelliği üzerinde bir yöntem olarak görünmez.

**5-Views\Home\Index.aspx (RenderBanner() yöntemiyle) listeleme**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Farklı bir reklam listesi 5 görünüm tarafından işlenen sayfa istediğinde, her bir istekle görüntülenir (bkz: Şekil 2). Sayfanın önbelleğe alınır ancak reklam RenderBanner() yardımcı yöntemi tarafından dinamik olarak eklenmiş olur.

**Şekil 2 – rastgele bir reklam görüntüleme dizini görünümü**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Özet

Bu öğreticide, içeriği önbelleğe alınmış bir sayfaya dinamik olarak nasıl güncelleştirebilirsiniz açıklanmıştır. Dinamik içeriği önbelleğe alınmış bir sayfa eklenerek üzere etkinleştirmek için HttpResponse.WriteSubstitution() yöntemi kullanmayı öğrendiniz. Ayrıca bir HTML yardımcı yöntem içinde WriteSubstitution() yöntemine çağrı kapsülleyen öğrendiniz.

Mümkün olduğunda – web uygulamalarınızın performansını önemli bir etkisi olması önbelleğe alma avantajından faydalanın. Bu öğreticide açıklandığı şekilde, hatta sayfalarınıza dinamik içerikleri görüntülemek, ihtiyacınız olduğunda, önbelleğe alma yararlanabilirsiniz.

## 

## 

> [!div class="step-by-step"]
> [Önceki](improving-performance-with-output-caching-cs.md)
> [İleri](creating-a-controller-cs.md)
