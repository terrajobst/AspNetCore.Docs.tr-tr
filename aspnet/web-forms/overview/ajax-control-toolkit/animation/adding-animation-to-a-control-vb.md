---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Animasyon denetimi (VB) ekleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bu öğreticide gösterilmiştir nasıl...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9392b1bab2289d886baf308d05644afbdc42a13a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756280"
---
<a name="adding-animation-to-a-control-vb"></a>(VB) bir denetime animasyon ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bu öğreticide, gibi animasyon ayarlama işlemi gösterilmektedir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bu öğreticide, gibi animasyon ayarlama işlemi gösterilmektedir.

## <a name="steps"></a>Adımlar

İlk adım zamanki dahil etmektir `ScriptManager` sayfasında ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

Bu senaryoda animasyon metin şuna benzer bir panel uygulanır:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

İlişkili bir CSS sınıfı panelinin arka plan rengi ve genişliği tanımlar:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Ardından, ihtiyacımız `AnimationExtender`. Girdikten sonra bir `ID` ve normal `runat="server"`, `TargetControlID` özniteliği örneğimizde panelinde animasyon uygulamak için Denetim ayarlanmalıdır:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

Tüm animasyon bildirimli olarak, ne yazık ki Visual Studio'nun IntelliSense tarafından şu anda tamamen desteklenen XML söz dizimi kullanılarak uygulanır. Kök düğüm `<Animations>;` ne zaman yer animation(s) take(s) belirleyen çeşitli etkinliklerde bu düğümde izin verilir:

- `OnClick` (fare tıklatın)
- `OnHoverOut` (fare bir denetim ayrıldığında)
- `OnHoverOver` (fare bir denetimin üzerine geldiğinde durdurma `OnHoverOut` animasyon)
- `OnLoad` (sayfanın ne zaman yüklendi)
- `OnMouseOut` (fare bir denetim ayrıldığında)
- `OnMouseOver` (fare bir denetimin üzerine geldiğinde değil durdurma `OnMouseOut` animasyon)

Framework, animasyon, her biri kendi XML öğesi tarafından temsil edilen bir dizisiyle gelir. Seçimi şu şekildedir:

- `<Color>` (bir rengi değiştirme)
- `<FadeIn>` (yavaş giriş)
- `<FadeOut>` (yavaş çıkış)
- `<Property>` (bir denetimin özelliğini değiştirme)
- `<Pulse>` (pulsating)
- `<Resize>` (boyutunu değiştirme)
- `<Scale>` (orantılı olarak boyutunu değiştirme)

Bu örnekte, panel Kıs. Animasyon 1.5 saniye almalı (`Duration` özniteliği), 24 çerçeveler (animasyon adımlar) saniye başına görüntüleme (`Fps` attributs). İçin tam biçimlendirmesi şöyledir `AnimationExtender` denetimi:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Bu komut dosyasını çalıştırdığınızda, paneli görüntülenir ve bir buçuk saniyeler içinde silinerek çıkar.


[![Panel Soluklaşan](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Panel Soluklaşan ([tam boyutlu görüntüyü görmek için tıklatın](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](dynamically-controlling-updatepanel-animations-cs.md)
> [İleri](executing-several-animations-at-the-same-time-vb.md)
