---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: "Animasyonun bir koşul (VB) bağlı olarak | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bir animasyon olup..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8600f33f9c27e1045f5083a126b9d2d1e90303
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-vb"></a>Animasyonun bir koşul (VB) bağlı olarak
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bir animasyon veya çalıştırılan ayrıca bazı JavaScript kodu biçiminde bir koşulunu bağlı olabilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bir animasyon veya çalıştırılan ayrıca bazı JavaScript kodu biçiminde bir koşulunu bağlı olabilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

Animasyonun bir panel şöyle metin uygulanır:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu`runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnLoad>` sayfa tam yüklenmeden silindikten sonra animasyonları çalıştırmak için. Normal bir animasyon birini yerine `<Condition>` öğesi oyuna gelir. Değeri olarak sağlanan JavaScript kodu `ConditionScript` özniteliği çalışma zamanında gerçekleştirilir. Doğru olarak değerlendirir, animasyon, aksi takdirde yürütülemiyor. Aşağıdaki biçimlendirmede bunların rastgele bağlı örneklerinin % 50 yürütülen her birini iki animasyon sağlar. Yalnızca olabilir beri içinde bir animasyon `<OnLoad>`, iki `<Condition>` animasyonları birlikte kullanarak birleştirilir `<Sequence>` öğe:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Unutmayın küçüktür işareti (`<`) içinde `ConditionScript` özniteliği Atlanan () olmalıdır. Ne zaman bu komut, ya da hiçbir animasyon çalıştığında, çalıştırmak veya iki birini desteklemez veya her ikisini birden yapın.


[![İkinci animasyon çalıştığında, ilk kaydetmedi şekilde paneli boyutlandırmadan, yavaş çıkış](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

İkinci animasyon çalıştığında, ilk kaydetmedi şekilde paneli boyutlandırmadan, yavaş çıkış ([tam boyutlu görüntüyü görüntülemek için tıklatın](animation-depending-on-a-condition-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](executing-several-animations-after-each-other-vb.md)
[sonraki](picking-one-animation-out-of-a-list-vb.md)
