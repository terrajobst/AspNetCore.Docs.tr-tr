---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: (C#) aynı anda birkaç animasyon yürütme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Severa çalıştırılacak sağlar...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: 017a37533ab055ad149cf0de3a5892ae92741b84
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816856"
---
<a name="executing-several-animations-at-the-same-time-c"></a>(C#) aynı anda birkaç animasyon yürütme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Birkaç animasyon paralel bir şekilde çalışmasına olanak sağlar.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Birkaç animasyon paralel bir şekilde çalışmasına olanak sağlar.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

Animasyonun bir panel şuna benzer metin uygulanır:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnLoad>` animasyonları sayfanın tam yüklü silindikten sonra çalıştırılacak. Genellikle, `<OnLoad>` yalnızca bir animasyon kabul eder. Animasyon çerçevesi bir kullanarak birkaç animasyon katılmaya sağlar `<Parallel>` öğesi. İçindeki tüm animasyonları `<Parallel>` aynı zamanda yürütülür.

İşte için olası bir biçimlendirme `AnimationExtender` yavaş çıkış ve aynı anda paneli yeniden boyutlandırma denetimi:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

Ve gerçekten: Bu komut dosyasını çalıştırdığınızda, paneli görüntülenir ve ardından yeniden boyutlandırır (threefolding fazlasını halfing genişlik ve yükseklik) ve aynı anda silinerek çıkar.


[![Panel yavaş çıkış ve yeniden boyutlandırma (tarayıcının işleme altyapısı sayesinde, içeriği dahil)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

Panel yavaş çıkış ve (tarayıcının işleme altyapısı sayesinde, içeriği dahil) yeniden boyutlandırma ([tam boyutlu görüntüyü görmek için tıklatın](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](adding-animation-to-a-control-cs.md)
> [İleri](executing-several-animations-after-each-other-cs.md)
