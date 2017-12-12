---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: "Başka bir denetim (C#) animasyonda tetikleme | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Genellikle, başlatma bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d243eebc42b66f1e86b38a1b7531e527144ea7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-c"></a>Başka bir denetim (C#) animasyonda tetikleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bir animasyon başlatarak, aynı denetimi ile kullanıcı etkileşimi tarafından genellikle tetiklenir. Ancak ayrıca bir denetim ve sonra da animasyon başka bir denetim etkileşime mümkün.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bir animasyon başlatarak, aynı denetimi ile kullanıcı etkileşimi tarafından genellikle tetiklenir. Ancak ayrıca bir denetim ve sonra da animasyon başka bir denetim etkileşime mümkün.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Animasyonun bir panel şöyle metin uygulanır:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Bölmenin animasyon başlamak için bir HTML düğmesi kullanılır. Unutmayın `<input type="button" />` üzerinden favoured `<asp:Button />` kullanıcı o düğmesine tıkladığında biz geri gönderimin istemiyorsanız bu yana.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`. Ayarlamak önemlidir `TargetControlID` (animasyonu tetikleme öğe) düğmeye kimliği (Hareketli öğesi) paneli Kimliğini uygulanmaz

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

İçinde `<Animations>` düğümü, normal olarak yer animasyonları. Bölmenin değişiklik yapmanız için düğmeyi değil ayarlamanız `AnimationTarget` her animasyon öğesinin içinde özniteliğinin `AnimationExtender`. Değeri `AnimationTarget` Masası Elbette kimliğidir. Bu şekilde animasyonları paneliyle tetikleme düğmesiyle değil gerçekleşir. Burada `AnimationExtender` biçimlendirme bu senaryo için:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Tek tek animasyonları göründüğü özel sıra unutmayın. İlk olarak animasyon çalışır sonra düğmesi devre dışı. Olduğundan hiçbir `AnimationTarget` özniteliğini `<EnableAction>` öğesi, bu animasyon kaynak denetimine uygulanır: düğme. Sonraki iki animasyon adımları parallelly gerçekleştirilebilmesi (`<Parallel>` öğesi). Hem de bunların `AnimationTarget` öznitelikleri ayarlamak `"Panel1"`, böylece düğmesi paneli animasyon ekleme.


[![Fare tıklatma düğmesine paneli animasyonu başlatır](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Fare tıklatma düğmesine paneli animasyonu başlatır ([tam boyutlu görüntüyü görüntülemek için tıklatın](triggering-an-animation-in-another-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](disabling-actions-during-animation-cs.md)
[sonraki](modifying-animations-from-the-server-side-cs.md)
