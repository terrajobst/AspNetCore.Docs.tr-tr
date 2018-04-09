---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Derecelendirme denetim (VB) oluşturma | Microsoft Docs
author: wenz
description: E-ticaret topluluk siteleri için birçok Web sitesi, kullanıcılar oranı makaleler veya öğeleri sunar. Bu genellikle bazı kodlama çaba gerektirir, ancak sahibiz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c19f36dfe1b72a3954db61ff1845e99c02e47c14
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-rating-control-vb"></a>Derecelendirme denetim (VB) oluşturma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> E-ticaret topluluk siteleri için birçok Web sitesi, kullanıcılar oranı makaleler veya öğeleri sunar. Bu genellikle bazı kodlama çaba gerektirir, ancak Denetim Araç Seti bizim elden sunuyoruz.


## <a name="overview"></a>Genel Bakış

E-ticaret topluluk siteleri için birçok Web sitesi, kullanıcılar oranı makaleler veya öğeleri sunar. Bu genellikle bazı kodlama çaba gerektirir, ancak Denetim Araç Seti bizim elden sunuyoruz.

## <a name="steps"></a>Adımlar

Tüm, öncelikle görüntüler (en az) iki tür gerekir: bir doldurulmuş bir derecelendirme öğesi ve boş derecelendirme öğe için bir. Bir derecelendirme genellikle bir yıldız veya bir gülen öğesidir. Bu senaryo için üç dosyaları, smiley.png ve empty.png ve gülen done.png kaynak kodu yüklemeleri Bu öğreticinin bir parçası olarak bulur.

Ardından, yeni bir ASP.NET dosyası oluşturun ve ekleyerek başlayın bir `ScriptManager` kendisine denetimi:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Ardından, ekleyin `Rating` ASP.NET AJAX Denetim araç setinden denetim. Aşağıdaki öznitelikler Bu örnek için ayarlanmış olması gerekir:

- `CurrentRating` kullanılacak ilk derecelendirme
- `MaxRating` en yüksek derecelendirme
- `EmptyStarCssClass` Derecelendirme öğesi (yıldız) boş olduğunda kullanılacak CSS sınıfı
- `FilledStarCssClass` Derecelendirme öğesi (yıldız) doldurulurken kullanılacak CSS sınıfı
- `StarCssClass` görünür stat için kullanılacak CSS sınıfı
- `WaitingStarCssClass` bir yıldız derecesiyle sunucuya geri gönderilirken kullanılacak CSS sınıfı

Burada da bir derecelendirme denetimi ile beşinci oluşturan biçimlendirme hangisinin hiçbiri doldurulur çıkışı başlangıçta öğeleri (smileys):

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

Üç başvurulan CSS sınıfları şimdi uygun görüntü dosyaları göstermek hangi CSS kullanarak yapmak kolaydır gerekir:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Genişlik ve yükseklik üç görüntülerinin sağlar, aksi takdirde görüntü yukarı biraz messed görünebilir emin olun.

Son olarak, derecelendirme sonucunu kullanıcıya görüntüleneceğini (veya, en az bir veritabanına kaydedilir). Bu nedenle bir kısa mesaj ve bir gönderme düğmesi derecelendirme formun sunucuya geri göndermek için çıktısı için bir etiket ekleyin:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

Sunucu tarafı kodu erişim derecelendirme denetimi aracılığıyla kendi `ID` ve ardından erişim kendi `CurrentRating` örneğimizde 0 ile 5 arasında bir değer seçili derecelendirme öğe sayısı olan özelliği.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Sayfa kaydedin ve tarayıcınıza yükleyin. (Başlangıçta boş) derecelendirme öğelerin üzerine geldiğinizde, bir JavaScript efekti oluşur: derecelendirme değişiklikleri. Yıldız kümesinde tıkladığınızda, geçerli derecelendirme korunur. Son olarak, formu gönderdiğinde, sunucu tarafı kodu seçili derecelendirme çıkarır.


[![Bir derecelendirme sistemi ile en az kod oluşturma](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Bir derecelendirme sistemi ile en az kod oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](creating-a-rating-control-cs.md)
