---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: "Denetim (VB) için animasyon ekleme | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bu öğreticide gösterilmiştir nasıl..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c2d6971ade89405245c8d23cafb6fd8bb9468639
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-animation-to-a-control-vb"></a>Denetim (VB) için animasyon ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bu öğretici, bu tür animasyonu ayarlamak gösterilmiştir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bu öğretici, bu tür animasyonu ayarlamak gösterilmiştir.

## <a name="steps"></a>Adımlar

İlk adım her zamanki gibi eklemektir `ScriptManager` sayfasındaki ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

Bu senaryoda animasyon şöyle metin panelinin uygulanır:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

İlişkili CSS sınıfı panelinin arka plan rengi ve genişliği tanımlar:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Ardından, ihtiyacımız `AnimationExtender`. Sağlama sonra bir `ID` ve normal `runat="server"`, `TargetControlID` özniteliği örneğimizde paneli animasyon için denetime ayarlanmalıdır:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

Tüm animasyon bildirimli olarak, ne yazık ki şu anda tam olarak Visual Studio'nun IntelliSense tarafından desteklenen bir XML sözdizimi kullanılarak uygulanır. Kök düğüm `<Animations>;` zaman animation(s) yer take(s) belirleyen çeşitli olaylarını bu düğümde izin verilir:

- `OnClick`(fare tıklatın)
- `OnHoverOut`(fare denetim ayrıldığında)
- `OnHoverOver`(fare üzerinde denetim geldiğinde durdurma `OnHoverOut` animasyon)
- `OnLoad`(sayfanın ne zaman yüklendi)
- `OnMouseOut`(fare denetim ayrıldığında)
- `OnMouseOver`(fare üzerinde denetim geldiğinde değil durdurma `OnMouseOut` animasyon)

Framework animasyonları, her biri kendi XML öğesi tarafından temsil edilen bir dizi ile gelir. Bir seçim şöyledir:

- `<Color>`(bir renk değiştirme)
- `<FadeIn>`(yavaş giriş)
- `<FadeOut>`(yavaş çıkış)
- `<Property>`(bir denetimin özelliğini değiştirme)
- `<Pulse>`(pulsating)
- `<Resize>`(boyutunu değiştirme)
- `<Scale>`(orantılı olarak boyutunu değiştirme)

Bu örnekte, bölmenin Kıs. Animasyonun 1.5 saniye sürebilir (`Duration` özniteliği), 24 çerçeveler (animasyon adımları) saniye başına görüntüleme (`Fps` attributs). İçin tam biçimlendirme işte `AnimationExtender` denetimi:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Bu komut dosyasını çalıştırdığınızda paneli görüntülenir ve bir ve bir yarım saniye cinsinden yavaşça.


[![Bölmenin Soluklaşan](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Bölmenin Soluklaşan ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-animation-to-a-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](dynamically-controlling-updatepanel-animations-cs.md)
[sonraki](executing-several-animations-at-the-same-time-vb.md)
