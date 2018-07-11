---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: (C#) kaydırıcı denetiminde veri bağlama | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar. Geçerli konum bağlamak mümkündür...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: b7aebb8dd180113b011ac038e8da4a3baa701485
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818669"
---
<a name="databinding-the-slider-control-c"></a>Veri bağlama kaydırıcı denetimi (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar. Geçerli konumun slider'ın başka bir ASP.NET denetime bağlamak mümkündür.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar. Geçerli konumun slider'ın başka bir ASP.NET denetime bağlamak mümkündür.

## <a name="steps"></a>Adımlar

ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

Ardından, iki ekleyin `TextBox` sayfasına denetimleri. Bir grafik bir kaydırıcı dönüştürülür ve diğer bir kaydırıcının konumu tutar.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

Sonraki adım zaten son adımdır. `SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden dışında ilk metin kutusuna bir kaydırıcı yapar ve kaydırıcıyı değişiklikleri getirdiğinizde ikinci metin kutusunda otomatik olarak güncelleştirir. Çalışmak, o sırada `SliderExtender`'s `TargetControlID` özniteliği ilk metin kutusuna; Kimliğine ayarlanmalıdır `BoundControlID` özniteliği ikinci metin kutusunda Kimliğine ayarlanmalıdır.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Tarayıcıda gördüğünüz gibi her iki yönde veri bağlama çalışır: yeni bir değer metin kutusuna girerek kaydırıcının konumunu güncelleştirir. İkinci metin kutusunda salt okunur yaparsanız, böylece orada değeri el ile güncelleştirmek kullanıcının daha zayıf bir koruma metin alanına ekleyebilirsiniz.


[![Kaydırıcı ve metin kutusu eşitlenmiş durumda](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

Kaydırıcı ve metin kutusu eşitlenmiş ([tam boyutlu görüntüyü görmek için tıklatın](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](using-the-slider-control-with-auto-postback-cs.md)
> [İleri](using-the-slider-control-with-auto-postback-vb.md)
