---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: "Birkaç animasyon her art arda (C#) yürütme | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Severa çalışmasına izin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 9d4322690132fe3829e3454f0aa7ff38acd8eb04
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="executing-several-animations-after-each-other-c"></a>Birkaç animasyon her art arda (C#) yürütme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bu, birkaç animasyon bir art arda çalıştırılmasına izin verir.


ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bu, birkaç animasyon bir art arda çalıştırılmasına izin verir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

Animasyonun bir panel şöyle metin uygulanır:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu`runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnLoad>` sayfa tam yüklenmeden silindikten sonra animasyonları çalıştırmak için. Genellikle, `<OnLoad>` yalnızca bir animasyon kabul eder. Animasyon çerçevesi bir kullanarak birkaç animasyon katılma sayesinde `<Sequence>` öğesi. İçindeki tüm animasyonları `<Sequence>` yürütülen art arda şunlardır. İşte için olası bir biçimlendirme `AnimationExtender` ilk daha geniş paneli yapma ve yüksekliğinin azalan denetimi:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Bu komut, bölmenin ilk alır ve ardından daha küçük geniş çalıştırdığınızda.


[![İlk genişliği artırılır](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

İlk genişliği artar ([tam boyutlu görüntüyü görüntülemek için tıklatın](executing-several-animations-after-each-other-cs/_static/image3.png))


[![Ardından yüksekliği azalır](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Yükseklik azalır sonra ([tam boyutlu görüntüyü görüntülemek için tıklatın](executing-several-animations-after-each-other-cs/_static/image6.png))

>[!div class="step-by-step"]
[Önceki](executing-several-animations-at-the-same-time-cs.md)
[sonraki](animation-depending-on-a-condition-cs.md)
