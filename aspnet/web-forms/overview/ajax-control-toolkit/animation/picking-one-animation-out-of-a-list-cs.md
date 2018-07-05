---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Bir liste (C#) animasyon seçme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Framework ayrıca ver...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 4b0193f47697e072605ed8c01a37fbfa128eb0e0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380752"
---
<a name="picking-one-animation-out-of-a-list-c"></a>Bir liste (C#) animasyon seçme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Framework, bir listeden animasyon animasyon, bazı JavaScript kodları değerlendirmesine bağlı olarak çekmek Programcı de sağlar.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Framework, bir listeden animasyon animasyon, bazı JavaScript kodları değerlendirmesine bağlı olarak çekmek Programcı de sağlar.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

Animasyonun bir panel şuna benzer metin uygulanır:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnLoad>` animasyonları sayfanın tam yüklü silindikten sonra çalıştırılacak. Normal animasyonları birini yerine `<Case>` öğesi oyuna gelir. Kendi SelectScript özniteliğinin değerini değerlendirilir; dönüş değeri sayısal olmalıdır. Bu sayı, içinde subanimations birine bağlı olarak &lt;çalışması&gt; yürütülür. Denetim Araç Seti SelectScript 2 olarak değerlendirilirse, üçüncü animasyon içinde örneği için çalışan &lt;çalışması&gt; (sayım 0 başlar).

Aşağıdaki biçimlendirmede üç subanimations tanımlar: genişliğini yeniden boyutlandırma, yükseklik yeniden boyutlandırma ve yavaş çıkış. JavaScript kodu (`Math.floor(3 * Math.random())`) üç animasyonları birini çalıştırın, ardından 0. ve 2 arasında bir sayı seçer:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


[![Olası üç animasyonları birini: panel geniş alır](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Olası üç animasyonları birini: panel geniş alır ([tam boyutlu görüntüyü görmek için tıklatın](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](animation-depending-on-a-condition-cs.md)
> [İleri](animating-in-response-to-user-interaction-cs.md)
