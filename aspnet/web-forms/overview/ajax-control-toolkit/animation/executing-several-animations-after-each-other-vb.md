---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Arkaya (VB) birkaç animasyon yürütme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Severa çalıştırılacak sağlar...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d94619ca40d288f8a5a391ef02103902bd2550c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835961"
---
<a name="executing-several-animations-after-each-other-vb"></a>Arkaya (VB) birkaç animasyon yürütme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Art arda birkaç animasyon bir çalıştırmayı sağlar.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Art arda birkaç animasyon bir çalıştırmayı sağlar.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

Animasyonun bir panel şuna benzer metin uygulanır:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnLoad>` animasyonları sayfanın tam yüklü silindikten sonra çalıştırılacak. Genellikle, `<OnLoad>` yalnızca bir animasyon kabul eder. Animasyon çerçevesi bir kullanarak birkaç animasyon katılmaya sağlar `<Sequence>` öğesi. İçindeki tüm animasyonları `<Sequence>` yürütülen art arda olan. İşte için olası bir biçimlendirme `AnimationExtender` ilk geniş paneli yapma ve ardından yükseklik azalan denetimi:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Bu komut, panel ilk alır ve ardından daha küçük geniş çalıştırıldığında.


[![İlk genişliğini artırılır](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

İlk genişliğini artırılır ([tam boyutlu görüntüyü görmek için tıklatın](executing-several-animations-after-each-other-vb/_static/image3.png))


[![Ardından yüksekliği azalır](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

Yüksekliği azalır sonra ([tam boyutlu görüntüyü görmek için tıklatın](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Önceki](executing-several-animations-at-the-same-time-vb.md)
> [İleri](animation-depending-on-a-condition-vb.md)
