---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Bir koşul (C#) bağlı animasyon | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bir animasyon olup...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: cb08c330d6fbc86035a2f21ad382cc009411bcd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808430"
---
<a name="animation-depending-on-a-condition-c"></a>Bir koşul (C#) bağlı animasyon
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bir animasyon çalışıp ayrıca formunda bazı JavaScript kodunun bir koşul bağlı olabilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bir animasyon çalışıp ayrıca formunda bazı JavaScript kodunun bir koşul bağlı olabilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

Animasyonun bir panel şuna benzer metin uygulanır:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnLoad>` animasyonları sayfanın tam yüklü silindikten sonra çalıştırılacak. Normal animasyonları birini yerine `<Condition>` öğesi oyuna gelir. Değeri olarak sağlanan JavaScript kodu `ConditionScript` özniteliği, çalışma zamanında yürütülür. True olarak değerlendirilirse animasyon, aksi takdirde yürütülmez. Her biri %50 rastgele sırasında çalışmalarının yürütülmekte olan iki animasyon aşağıdaki biçimlendirme sağlar. İçinde bir animasyon bulunabilir olduğundan `<OnLoad>`, iki `<Condition>` animasyonları birlikte kullanarak birleştirilir `<Sequence>` öğesi:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Unutmayın küçüktür işareti (`<`) içinde `ConditionScript` özniteliğinin Atlanan () olması gerekir. Ne zaman ya da animasyon çalıştırma yok, bu betiği çalıştırın veya bir iki desteklemez veya her ikisini birden yapın.


[![Birinci ikinci animasyon çalıştırmaların dolayısıyla paneli yeniden boyutlandırma olmadan, yavaş çıkış](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Birinci ikinci animasyon çalıştırmaların dolayısıyla paneli yeniden boyutlandırma olmadan, yavaş çıkış ([tam boyutlu görüntüyü görmek için tıklatın](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](executing-several-animations-after-each-other-cs.md)
> [İleri](picking-one-animation-out-of-a-list-cs.md)
