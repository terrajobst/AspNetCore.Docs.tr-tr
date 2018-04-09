---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Yanıt kullanıcı etkileşimi (C#) için animasyon ekleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon yıldız...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 783563f4e33087e99a071cf829ca6bab246ba3b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="animating-in-response-to-user-interaction-c"></a>Yanıt kullanıcı etkileşimi (C#) için animasyon ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir veya otomatik olarak başlatabilirsiniz.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon kullanıcı etkileşimi tarafından örneğin fareyle tıklatarak tetiklenebilir veya otomatik olarak başlatabilirsiniz.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

Animasyonun bir panel şöyle metin uygulanır:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanıcı etkileşimi aracılığıyla animasyonu başlatmak için beş yolu vardır (eksik öğesi `<OnLoad>` tüm sayfa tam yüklenmeden silindikten sonra yürütülen):

- `<OnClick>` (fare denetimi tıklayın)
- `<OnHoverOut>` (fare denetimi terk)
- `<OnHoverOver>` (fare gezinen durdurma üzerinde bir denetim `<OnHoverOut>` animasyon)
- `<OnMouseOut>` (fare denetim bırakır)
- `<OnMouseOver>` (fare gezinen değil durdurma üzerinde bir denetim `<OnMouseOut>` animasyon)

Bu senaryoda, `<OnClick>` kullanılır. Kullanıcı panelde tıklattığında boyutlandırılır ve aynı anda silinerek çıkar.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


[![Animasyonun bir fare tıklatma başlatır](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Animasyonun bir fare tıklatma başlatır ([tam boyutlu görüntüyü görüntülemek için tıklatın](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](picking-one-animation-out-of-a-list-cs.md)
> [sonraki](disabling-actions-during-animation-cs.md)
