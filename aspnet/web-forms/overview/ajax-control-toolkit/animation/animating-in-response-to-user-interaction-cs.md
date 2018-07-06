---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: (C#) kullanıcı etkileşimine karşılık olarak animasyon ekleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyonları yıldız...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e8ebf5ec7fc0875e0eb43923321513bf0a08899
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826137"
---
<a name="animating-in-response-to-user-interaction-c"></a>(C#) kullanıcı etkileşimine karşılık olarak animasyon ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyonların otomatik olarak başlayabilir veya kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyonların otomatik olarak başlayabilir veya kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

Animasyonun bir panel şuna benzer metin uygulanır:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanıcı etkileşimi aracılığıyla animasyonu başlatmak için beş yol vardır (eksik öğe `<OnLoad>` sayfanın tamamını tam yüklü silindikten sonra yürütülen):

- `<OnClick>` (Denetim fare tıklatın)
- `<OnHoverOut>` (fare denetimi terk)
- `<OnHoverOver>` (fare durdurulurken bir denetimin üzerine geldiğinde `<OnHoverOut>` animasyon)
- `<OnMouseOut>` (fare, denetimin bırakır)
- `<OnMouseOver>` (fare değil durdurulurken bir denetimin üzerine geldiğinde `<OnMouseOut>` animasyon)

Bu senaryoda, `<OnClick>` kullanılır. Kullanıcı panelinde tıkladığında boyutlandırılır ve aynı anda silinerek çıkar.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


[![Fare tıklatın animasyonu başlatır](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Animasyonun bir fare tıklaması başlar ([tam boyutlu görüntüyü görmek için tıklatın](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](picking-one-animation-out-of-a-list-cs.md)
> [İleri](disabling-actions-during-animation-cs.md)
