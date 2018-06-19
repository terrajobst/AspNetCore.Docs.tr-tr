---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Veri bağlama (VB) kaydırıcı denetimi | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar. Geçerli konum bağlamak mümkündür...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ecd8598cd7fdcbbb4812e501bb30fa1f563a054
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879146"
---
<a name="databinding-the-slider-control-vb"></a>Veri bağlama (VB) kaydırıcı denetimi
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar. Başka bir ASP.NET denetimi kaydırıcıyı geçerli konumunu bağlamak mümkündür.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar. Başka bir ASP.NET denetimi kaydırıcıyı geçerli konumunu bağlamak mümkündür.

## <a name="steps"></a>Adımlar

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Ardından, iki eklemek `TextBox` sayfasına denetimler. Bir grafik kaydırıcı dönüştürülür ve diğerinde kaydırıcıyı konumunu tutacaktır.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

Sonraki adım zaten son adımdır. `SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden ilk metin kutusunun dışında bir kaydırıcı yapar ve kaydırıcıyı getirin değiştiğinde ikinci metin kutusu otomatik olarak güncelleştirir. Çalışmak, o sırada `SliderExtender`'s `TargetControlID` özniteliği ilk metin kutusunun; Kimliğine ayarlanmalıdır `BoundControlID` özniteliği ikinci metin kutusunun Kimliğine ayarlanmalıdır.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Veri bağlama tarayıcıda gördüğünüz gibi her iki yönde de çalışır: metin kutusuna yeni bir değer girme kaydırıcının konumunu güncelleştirir. Salt okunur ikinci metin kutusu yaparsanız, böylece kullanıcının orada değerini el ile güncelleştirmeniz daha zayıf bir koruma metin alanı ekleyebilir.


[![Kaydırıcı ve metin kutusu eşitleme](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Kaydırıcı ve metin kutusu eşitlenmiş ([tam boyutlu görüntüyü görüntülemek için tıklatın](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](using-the-slider-control-with-auto-postback-vb.md)
