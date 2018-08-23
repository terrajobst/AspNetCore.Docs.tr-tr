---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Önbelleğe alınmış bir sayfaya (VB) için dinamik içerik ekleme | Microsoft Docs
author: microsoft
description: Aynı sayfayı dinamik ve önbelleğe alınan içeriğini karıştırmak öğrenin. Sonrası önbellek değiştirme başlığı tanıtımları o gibi dinamik içerik görüntülenecek sağlar...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 121a3a35c8255f1423d7008930315f76bbb8e8f9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756512"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a>Önbelleğe alınmış bir sayfaya (VB) için dinamik içerik ekleme
====================
tarafından [Microsoft](https://github.com/microsoft)

> Aynı sayfayı dinamik ve önbelleğe alınan içeriğini karıştırmak öğrenin. Sonrası önbellek değiştirme başlık reklamları veya, önbelleğe alınan çıkış bir sayfa içinde olan haber öğelerinin gibi dinamik içerik görüntülemenize olanak sağlar.


Çıkış önbelleğe alma özelliğinden yararlanarak, bir ASP.NET MVC uygulamasının performansını önemli ölçüde artırabilir. Sayfa istenen her zaman bir sayfa üretmek yerine, sayfanın bir kez oluşturulabilir ve birden çok kullanıcı için bellekte önbelleğe alınmış.

Ancak bir sorun oluştu. Peki sayfasında dinamik içerikleri görüntülemek gerekiyor? Örneğin, bir reklam sayfasında görüntülemek istediğinizi düşünün. Her kullanıcı aynı tanıtım görebilmesi için önbelleğe alınacak reklam istemezsiniz. Bu şekilde paranın sabitlenebilen!

Neyse ki, kolay bir çözüm yoktur. Bir özelliğin çağırılır ASP.NET framework'ün yararlanabilirsiniz *sonrası, değiştirme önbelleğe*. Sonrası önbellek değiştirme, bellekte önbelleğe alınmış bir sayfaya dinamik içerik yerine olanak tanır.


Çıktı alırken genellikle bir sayfasını kullanarak önbelleğe alma &lt;OutputCache&gt; hem sunucu hem de istemci (tarayıcı) özniteliği, sayfanın önbelleğe alınır. Bir sayfa, sonrası önbellek değiştirme kullandığınızda, yalnızca sunucuda önbelleğe alınır.


#### <a name="using-post-cache-substitution"></a>Sonrası önbellek değiştirme kullanma

Sonrası önbellek değiştirme kullanarak iki adımı gerektirir. İlk olarak, önbelleğe alınan sayfasında görüntülenmesini istediğiniz dinamik içeriği temsil eden bir dize döndüren bir yöntemi tanımlamak gerekir. Ardından, sayfaya dinamik içerik eklemesine HttpResponse.WriteSubstitution() yöntemi çağırın.

Örneğin, rastgele önbelleğe alınmış bir sayfaya farklı haber öğelerini görüntülemek istediğinizi düşünün. 1 listeleme sınıfı, rastgele bir haber öğesi üç haber öğelerinin bir listeden döndüren RenderNews() adlı tek bir yöntemi gösterir.

**1 – Models\News.vb listeleme**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Sonrası önbellek değiştirme yararlanmak için HttpResponse.WriteSubstitution() yöntemi çağırın. WriteSubstitution() yöntemi kodu bölgesi önbelleğe alınmış bir sayfaya dinamik içerik ile değiştirmek için ayarlar. WriteSubstitution() yöntemin rastgele haber öğesinin 2 liste görünümünde görüntülemek için kullanılır.

**2 – Views\Home\Index.aspx listeleme**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

RenderNews yöntemi WriteSubstitution() yöntemine geçirilir. RenderNews yöntemi çağrılmadı dikkat edin. Bunun yerine bir yönteme başvuru için WriteSubstitution() AddressOf işleci yardımıyla geçirilir.

Dizin görünümünün önbelleğe alınır. Görünüm denetleyicisi listeleme 3'te tarafından döndürülür. İNDİS() eylem ile donatılmış bildirimi bir &lt;OutputCache&gt; dizin görünümünün 60 saniye boyunca önbelleğe alınmasına neden olan özniteliği.

**3 – Controllers\HomeController.vb listeleme**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Dizin görünümünün önbelleğe alınmış olsa da, dizin sayfası istediğinde farklı rastgele haber öğeler görüntülenir. Dizin Sayfası istediğinde 60 saniye (bkz. Şekil 1) sayfa tarafından görüntülenen zaman değiştirmez. Sayfanın önbelleğe alma süresi değişmez olgu kanıtlar. Ancak, içerik eklenen her istekle WriteSubstitution() yöntemi – rastgele haber öğesinin – değişikliklerden.

**Şekil 1-önbelleğe alınmış bir sayfaya dinamik haber öğeleri ekleme**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Sonrası önbellek değiştirme yardımcı yöntemleri kullanma

Bir özel bir yardımcı yöntem içinde WriteSubstitution() yöntemine çağrı yalıtılacak sonrası önbellek değiştirme yararlanmak için daha kolay bir yolu var. Bu yaklaşım listeleme 4'te yardımcı yöntemi gösterilmiştir.

**4 – Helpers\AdHelper.vb listeleme**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

4 listeleme içeren iki yöntemlerini gösteren bir Visual Basic module'u: RenderBanner() ve RenderBannerInternal(). RenderBanner() yöntemi gerçek yardımcı yöntemi temsil eder. Bu yöntem, herhangi bir yardımcı yöntemi gibi bir görünümde Html.RenderBanner() çağırabilirsiniz standart ASP.NET MVC HtmlHelper sınıfı genişletir.

RenderBanner() yöntem RenderBannerInternal() yöntemi WriteSubsitution() yöntemine geçirerek HttpResponse.WriteSubstitution() yöntemini çağırır.

Özel bir yöntem RenderBannerInternal() yöntemidir. Bu yöntem, bir yardımcı yöntem sunulmamasını. RenderBannerInternal() yöntemi, üç başlığı reklam görüntü listesinden rastgele bir başlık tanıtım görüntüsünü döndürür.

Listeleme 5'te değiştirilmiş dizin görünümünün RenderBanner() yardımcı yöntemini nasıl kullanabileceğinizi gösterir. Dikkat ek &lt;içeri aktarma % @ %&gt; yönergesi MvcApplication1.Helpers ad alanı içeri aktarmak için görünümün en üstünde bulunur. Bu ad alanı içeri aktarmak yayımlarsanız, RenderBanner() yöntemi Html özelliği üzerinde bir yöntem olarak görünmez.

**5-Views\Home\Index.aspx (RenderBanner() yöntemiyle) listeleme**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Farklı bir reklam listesi 5 görünüm tarafından işlenen sayfa istediğinde, her bir istekle görüntülenir (bkz: Şekil 2). Sayfanın önbelleğe alınır ancak reklam RenderBanner() yardımcı yöntemi tarafından dinamik olarak eklenmiş olur.

**Şekil 2 – rastgele bir reklam görüntüleme dizini görünümü**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Özet

Bu öğreticide, içeriği önbelleğe alınmış bir sayfaya dinamik olarak nasıl güncelleştirebilirsiniz açıklanmıştır. Dinamik içeriği önbelleğe alınmış bir sayfa eklenerek üzere etkinleştirmek için HttpResponse.WriteSubstitution() yöntemi kullanmayı öğrendiniz. Ayrıca bir HTML yardımcı yöntem içinde WriteSubstitution() yöntemine çağrı kapsülleyen öğrendiniz.

Mümkün olduğunda – web uygulamalarınızın performansını önemli bir etkisi olması önbelleğe alma avantajından faydalanın. Bu öğreticide açıklandığı şekilde, hatta sayfalarınıza dinamik içerikleri görüntülemek, ihtiyacınız olduğunda, önbelleğe alma yararlanabilirsiniz.

> [!div class="step-by-step"]
> [Önceki](improving-performance-with-output-caching-vb.md)
> [İleri](creating-a-controller-vb.md)
