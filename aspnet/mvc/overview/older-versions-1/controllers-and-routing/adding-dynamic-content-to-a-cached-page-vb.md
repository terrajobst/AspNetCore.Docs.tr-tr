---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: Dinamik içerik önbelleğe alınmış bir sayfa için (VB) ekleme | Microsoft Docs
author: microsoft
description: Dinamik ve önbelleğe alınmış içeriği aynı sayfada karışık öğrenin. Sonrası önbellek değiştirme başlık reklamları o gibi dinamik içerik görüntülemenize olanak sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 89421b4bec2170e408ded87ccc918a7a16844a98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879770"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a>Dinamik içerik önbelleğe alınmış bir sayfa için (VB) ekleme
====================
tarafından [Microsoft](https://github.com/microsoft)

> Dinamik ve önbelleğe alınmış içeriği aynı sayfada karışık öğrenin. Sonrası önbellek değiştirme başlık reklamları veya önbelleğe alınmış çıktı bir sayfa içinde haber öğeleri gibi dinamik içerik görüntülemenize olanak sağlar.


Çıkış önbelleğe alma yararlanarak, bir ASP.NET MVC uygulamanızın performansını önemli ölçüde artırabilir. Sayfa istenen her zaman bir sayfa üretmek yerine sayfa kez oluşturulabilir ve birden çok kullanıcı için bellekte önbelleğe alınmış.

Ancak bir sorun oluştu. Ne dinamik içerik sayfasında görüntülenecek gerekiyor? Örneğin, bir reklam sayfasında görüntülemek istediğinizi varsayalım. Her kullanıcının çok aynı tanıtım görebilmesi için önbelleğe alınacak reklam istemezsiniz. Bu şekilde paranın olun olmayacaktır!

Neyse ki, kolay bir çözüm yoktur. Bir özellik olarak adlandırılan ASP.NET framework'ün yararlanabilir *değiştirme'sonrası önbelleğe*. Sonrası önbellek değiştirme bellekte önbelleğe alınmış bir sayfasında dinamik içerik yerine sağlar.


Çıktı alırken normalde, bir sayfayı kullanarak önbelleğe &lt;OutputCache&gt; hem sunucu hem de istemci (web tarayıcısı) özniteliği, sayfanın önbelleğe alınır. Bir sayfa sonrası önbellek değiştirme kullandığınızda, yalnızca sunucuda önbelleğe alınır.


#### <a name="using-post-cache-substitution"></a>Sonrası önbellek değiştirme kullanma

Sonrası önbellek değiştirme kullanarak iki adımı gerektirir. İlk olarak, önbelleğe alınan sayfasında görüntülenmesini istediğiniz dinamik içerik temsil eden bir dize döndüren bir yöntem tanımlamanız gerekir. Ardından, dinamik içerik sayfaya eklemesine HttpResponse.WriteSubstitution() yöntemini çağırın.

Örneğin, rastgele bir önbelleğe alınan sayfasında farklı haber öğeleri görüntülemek istediğinizi varsayalım. Listeleme 1 sınıfında rastgele üç haber öğe listesinden bir haber öğesi döndüren RenderNews() adlı tek bir yöntemi gösterir.

**1 – Models\News.vb listeleme**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

Sonrası önbellek değiştirme yararlanmak için HttpResponse.WriteSubstitution() yöntemini çağırın. Önbelleğe alınan sayfasının bir bölge ile dinamik içerik değiştirmek için kodu WriteSubstitution() yöntemini ayarlar. WriteSubstitution() yöntemi listeleme 2 görünümünde rastgele haber öğesini görüntülemek için kullanılır.

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

RenderNews yöntemi WriteSubstitution() yöntemine iletilir. RenderNews yöntemi çağrılmazsa dikkat edin. Bunun yerine yöntemi başvuru için WriteSubstitution() AddressOf işleci yardımıyla geçirilir.

Dizin görünümünün önbelleğe alınır. Görünüm listeleme 3'te denetleyicisi tarafından döndürülür. İNDİS() eylem ile donatılmış bildirimi bir &lt;OutputCache&gt; dizin görünümünün 60 saniye boyunca önbelleğe alınmasına neden olan özniteliği.

**Listing 3 – Controllers\HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

Dizin görünümünün önbelleğe olsa da, dizin sayfası istediğinde farklı rastgele haber öğeleri görüntülenir. Dizin Sayfası istediğinde, sayfa tarafından görüntülenen zaman 60 saniye (bkz: Şekil 1) değiştirmez. Süre değişmez olgu sayfası önbelleğe alınır kanıtlar. Ancak, içerik eklenen her istek ile WriteSubstitution() yöntemi – rastgele haber öğesi – değişikliklerden.

**Şekil 1 – önbelleğe alınmış bir sayfa dinamik haber öğeleri Injecting**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Sonrası önbellek değiştirme yardımcı yöntemleri kullanma

Bir özel yardımcı yöntem içinde WriteSubstitution() yöntemine yapılan çağrı kapsülleyen sonrası önbellek değiştirme yararlanmak için daha kolay bir yoludur. Bu yaklaşım listeleme 4 yardımcı yöntemi tarafından gösterilmiştir.

**4 – Helpers\AdHelper.vb listeleme**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

4 listeleme içeren bir Visual Basic modülü iki yöntem sunar: RenderBanner() ve RenderBannerInternal(). RenderBanner() yöntemi gerçek yardımcı yöntemi temsil eder. Böylece bir görünümde herhangi bir yardımcı yöntemi gibi Html.RenderBanner() çağırabilirsiniz bu yöntem standart ASP.NET MVC HtmlHelper sınıfını genişletir.

RenderBanner() yöntemi RenderBannerInternal() yöntemi WriteSubsitution() yönteme geçirme HttpResponse.WriteSubstitution() yöntemini çağırır.

RenderBannerInternal() yöntemi özel bir yöntemdir. Bu yöntem, bir yardımcı yöntem olarak açık olmaz. RenderBannerInternal() yöntemi, üç başlık tanıtım görüntüleri listesinden rastgele bir başlık tanıtım görüntüsünü döndürür.

Listeleme 5 değiştirilmiş dizin görünümünde RenderBanner() yardımcı yönteminin nasıl kullanabileceğinizi gösterir. Dikkat ek bir &lt;% @ alma %&gt; yönergesi MvcApplication1.Helpers ad alanını almak için görünümü üstünde bulunur. Bu ad alanı içe atamazsanız RenderBanner() yöntemi Html özelliği bir yöntem olarak görünmez.

**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

Listeleme 5 görünüm tarafından işlenen sayfanın istediğinde, farklı bir reklam her istek ile gösterilir (bkz: Şekil 2). Sayfanın önbelleğe alınır, ancak reklam RenderBanner() yardımcı yöntemi tarafından dinamik olarak eklenmiş.

**Şekil 2 – rastgele bir reklam görüntüleme dizini görünümü**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>Özet

Bu öğretici, önbelleğe alınmış bir sayfa içeriği nasıl dinamik olarak güncelleştirebilirsiniz açıklanmıştır. Önbelleğe alınmış bir sayfa eklenemeyebilir dinamik içeriği etkinleştirmek için HttpResponse.WriteSubstitution() yöntemini kullanmak üzere öğrendiniz. Ayrıca bir HTML yardımcı yöntemi içinde WriteSubstitution() yöntemine yapılan çağrı kapsülleyen öğrendiniz.

Mümkün olduğunda – web uygulamalarınızın performansını önemli bir etkisi olması önbelleğe alma yararlanabilir. Bu öğreticide açıklandığı gibi hatta sayfalarınızda dinamik içeriği görüntülemek gerektiğinde önbelleğe alma yararlanabilirsiniz.

> [!div class="step-by-step"]
> [Önceki](improving-performance-with-output-caching-vb.md)
> [sonraki](creating-a-controller-vb.md)
