---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Kaydırıcı denetimi otomatik geri gönderme ile (VB) kullanarak | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopost yapmak mümkündür...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ad701763f5d391a793083a1d81db69e7f712069
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809861"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>Kaydırıcı denetimi kullanarak otomatik geri gönderme ile (VB)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopostback değeri değiştiğinde bir kez yapmak mümkündür.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti kaydırıcı denetimi fareyle denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopostback değeri değiştiğinde bir kez yapmak mümkündür.

## <a name="steps"></a>Adımlar

Kaydırıcı otomatik olarak geri gönderme sırasında bir değişiklik yapmak için iki metin kutusuna öznitelik gerekiyor `AutoPostBack="true"`: kaydırıcı olacak metin kutusu ve kaydırıcının konumunu içeren metin kutusu. Söz konusu gerekli biçimlendirmesi şöyledir:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

`SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden iki metin kutuları kaydırıcı işlevi atar:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Bir ek label öğesini, daha sonra bir geri gönderme kullanıcıya bildirmek için kullanılır:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Son olarak, `ScriptManager` Denetim ASP.NET AJAX Denetim Araç Seti çalışması için gerekli JavaScript yükler:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Kaydırıcı geri gönderen artık; Sunucu tarafında bu olay yakalandı ve izlemede:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Bir geri gönderme kaydırıcıyı hareket tetikler](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Kaydırıcıyı hareket, bir geri gönderme tetikler ([tam boyutlu görüntüyü görmek için tıklatın](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![Daha sonra bu değişiklik tarihini etikette yazılır](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Daha sonra bu değişiklik tarihini etikette yazılır ([tam boyutlu görüntüyü görmek için tıklatın](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Önceki](databinding-the-slider-control-cs.md)
> [İleri](databinding-the-slider-control-vb.md)
