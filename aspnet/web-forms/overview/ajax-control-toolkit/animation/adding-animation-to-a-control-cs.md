---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: (C#) bir denetime animasyon ekleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bu öğreticide gösterilmiştir nasıl...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 80c982e041af2d0b9ee789665613ced0311dfcc9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396578"
---
<a name="adding-animation-to-a-control-c"></a>(C#) bir denetime animasyon ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bu öğreticide, gibi animasyon ayarlama işlemi gösterilmektedir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bu öğreticide, gibi animasyon ayarlama işlemi gösterilmektedir.

## <a name="steps"></a>Adımlar

İlk adım zamanki dahil etmektir `ScriptManager` sayfasında ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

Bu senaryoda animasyon metin şuna benzer bir panel uygulanır:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

İlişkili bir CSS sınıfı panelinin arka plan rengi ve genişliği tanımlar:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Ardından, ihtiyacımız `AnimationExtender`. Girdikten sonra bir `ID` ve normal `runat="server"`, `TargetControlID` özniteliği örneğimizde panelinde animasyon uygulamak için Denetim ayarlanmalıdır:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

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

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Bu komut dosyasını çalıştırdığınızda, paneli görüntülenir ve bir buçuk saniyeler içinde silinerek çıkar.


[![Panel Soluklaşan](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Panel Soluklaşan ([tam boyutlu görüntüyü görmek için tıklatın](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](executing-several-animations-at-the-same-time-cs.md)
