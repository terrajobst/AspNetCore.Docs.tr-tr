---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Derecelendirme denetimi (VB) oluşturma | Microsoft Docs
author: wenz
description: E-ticaret topluluk sitelerine, birçok Web sitesi kullanıcılarının oranı makaleler veya öğeleri sunar. Bu genellikle bazı bir kodlama çabası gerektirir, ancak sahibiz...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b229d0155fbab0437c644b41424164e4c655f5e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752750"
---
<a name="creating-a-rating-control-vb"></a>Derecelendirme denetimi (VB) oluşturma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> E-ticaret topluluk sitelerine, birçok Web sitesi kullanıcılarının oranı makaleler veya öğeleri sunar. Bu genellikle bazı bir kodlama çabası gerektirir, ancak Denetim Araç Seti için sunduğumuz elden sahibiz.


## <a name="overview"></a>Genel Bakış

E-ticaret topluluk sitelerine, birçok Web sitesi kullanıcılarının oranı makaleler veya öğeleri sunar. Bu genellikle bazı bir kodlama çabası gerektirir, ancak Denetim Araç Seti için sunduğumuz elden sahibiz.

## <a name="steps"></a>Adımlar

Öncelikle, görüntüleri (en az) iki tür ihtiyacınız: bir doldurulmuş bir derecelendirme öğesi ve boş derecelendirme öğenin bir. Bir derecelendirme genellikle bir yıldız veya bir gülen öğesidir. Bu senaryo için üç dosyaları, smiley.png ve empty.png ve gülen done.png Bu öğretici için kaynak kodu indirmeleri bir parçası olarak bulabilirsiniz.

Ardından, yeni bir ASP.NET dosyası oluşturun ve ekleyerek başlayın bir `ScriptManager` denetimi:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Ardından, ekleme `Rating` ASP.NET AJAX Denetim Araç Seti denetim. Aşağıdaki öznitelikler için bu örneği ayarlamanız gerekir:

- `CurrentRating` kullanılacak ilk derecelendirme
- `MaxRating` en yüksek derecelendirme
- `EmptyStarCssClass` CSS sınıfının bir derecelendirme öğesi (star) boş olduğunda kullanın
- `FilledStarCssClass` bir derecelendirme öğesi (star) doldurulurken kullanmak için CSS sınıfı
- `StarCssClass` görünür bir durum için kullanılacak bir CSS sınıfı
- `WaitingStarCssClass` Yıldız derecelendirmesi sunucuya geri gönderilirken kullanılacak CSS sınıfı

Ve derecelendirme denetimi ile beşinci oluşturan biçimlendirmesi şöyledir hangi none doldurulan başlangıçta öğeleri (smileys):

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Üç başvurulan CSS sınıfları artık uygun görüntü dosyaları göstermek, CSS kullanarak yapmak kolaydır gerekir:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Üç görüntü yüksekliğini ve genişliğini sağlarsanız, görüntüyü oluşturan bir bit messed boyutlandırılabilecek emin olun.

Son olarak, derecelendirme sonucunu kullanıcıya görüntüleneceğini (veya, en az bir veritabanında kayıtlı). Bu nedenle sunucu derecelendirme forma geri göndermek için bir kısa mesaj ve bir Gönder düğmesi çıktısı için bir etiket ekleyin:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

Sunucu tarafı kodunda erişim derecelendirme denetimi aracılığıyla kendi `ID` ve ardından erişim kendi `CurrentRating` örneğimizde 0 ile 5 arasında bir değer seçili derecelendirme öğe sayısı olan özelliği.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Sayfayı kaydedin ve tarayıcınıza yükleyin. (Başlangıçta boş) derecelendirmesi öğelerin üzerine geldiğinizde, bir JavaScript etkisi oluşur: derecelendirme değişiklikleri. Yıldızlar kümesini temel tıkladığınızda, geçerli derecelendirme korunur. Son olarak, formu gönderdiğinde, sunucu tarafı kodu seçili derecelendirme çıkarır.


[![En az kodla derecelendirme sistem oluşturma](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

En az kodla derecelendirme sistemi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](creating-a-rating-control-cs.md)
