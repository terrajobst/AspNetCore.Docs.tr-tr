---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: "Kaydırıcı denetimi otomatik-geri gönderme ile (VB) kullanarak | Microsoft Docs"
author: wenz
description: "AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopost yapmak mümkündür..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: fedd3ff947c6f5d5d4d00791087e9fd825fdf9d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>Otomatik-geri gönderme ile (VB) kaydırıcı denetimi kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopostback kez kendi değer değişiklik mümkündür.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti, kaydırıcı denetimi fare kullanılarak denetlenebilir bir grafik kaydırıcı sağlar. Kaydırıcı autopostback kez kendi değer değişiklik mümkündür.

## <a name="steps"></a>Adımlar

Kaydırıcıyı bir değişikliklerinde otomatik olarak geri gönderme yapmak için her iki metin kutuları öznitelik gereksinim `AutoPostBack="true"`: kaydırıcı olacak metin kutusu ve kaydırıcının konumunu içeren metin kutusu. Söz konusu gerekli biçimlendirme aşağıdaki gibidir:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

`SliderExtender` ASP.NET AJAX Denetim Araç Seti denetiminden iki metin kutuları kaydırıcı işlevselliği atar:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Bir ek label öğesi, daha sonra geri gönderimin kullanıcı bildirmek için kullanılır:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Son olarak, `ScriptManager` Denetim ASP.NET AJAX Denetim Araç Seti çalışması için gerekli JavaScript yükler:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Şimdi kaydırıcıyı geri nakil; Sunucu tarafında bu olay yakalanan ve üzerinde işlem:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Kaydırıcıyı hareket geri gönderimin tetikler](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Kaydırıcıyı hareket tetikler geri gönderimin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![Daha sonra bu değişiklik tarihi etiketinde yazılır](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Daha sonra bu değişiklik tarihi etiketinde yazılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

>[!div class="step-by-step"]
[Önceki](databinding-the-slider-control-cs.md)
[sonraki](databinding-the-slider-control-vb.md)
