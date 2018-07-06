---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: (C#) başka bir denetimde animasyon tetikleme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Genellikle, başlatma bir...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a7eeb522ae982cd5a84aa9c8e4228871c8c78440
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810808"
---
<a name="triggering-an-animation-in-another-control-c"></a>(C#) başka bir denetimde animasyon tetikleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bir animasyonu başlatmak, aynı denetimi ile kullanıcı etkileşimi tarafından genel olarak, tetiklenir. Ancak aynı zamanda başka bir denetimde bir denetimi ve sonra animasyon ile etkileşim kurmak mümkün.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Bir animasyonu başlatmak, aynı denetimi ile kullanıcı etkileşimi tarafından genel olarak, tetiklenir. Ancak aynı zamanda başka bir denetimde bir denetimi ve sonra animasyon ile etkileşim kurmak mümkün.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Animasyonun bir panel şuna benzer metin uygulanır:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Panel animasyonu başlatmak için bir HTML düğmesi kullanılır. Unutmayın `<input type="button" />` üzerinden favoured `<asp:Button />` beri kullanıcı bu düğmeye tıkladığında bir geri gönderme istiyoruz değil.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`. Ayarlamak önemlidir `TargetControlID` kimliğine (öğesi animasyonun tetiklenmesi), düğmenin paneli (animasyon uygulanan öğesi) kimliği için

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

İçinde `<Animations>` düğümü, her zamanki şekilde Yerleştir animasyonları. Bunları değiştirmek panelin kolaylaştırmak için düğmesini ayarlayın `AnimationTarget` her animasyon öğesi içinde özniteliği `AnimationExtender`. Değeri `AnimationTarget` paneli Elbette kimliğidir. Bu şekilde, animasyon tetikleme düğmesiyle değil paneli ile gerçekleşir. İşte `AnimationExtender` biçimlendirme bu senaryo için:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Tek tek animasyonları göründüğü özel sıra unutmayın. İlk olarak, animasyon çalıştırıldıktan sonra düğme devre dışı. Olduğundan hiçbir `AnimationTarget` özniteliğini `<EnableAction>` öğesi, animasyon, kaynak denetimine uygulanır: düğme. Sonraki iki animasyon adımları parallelly gerçekleştirilebilmesi (`<Parallel>` öğesi). Her ikisi de kendi `AnimationTarget` öznitelikleri kümesine `"Panel1"`, bu nedenle panelini düğme animasyon ekleme.


[![Bir fare tıklaması düğmesine panelinde animasyonun başlatır](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Bir fare tıklaması düğmesine panelinde animasyonun başlar ([tam boyutlu görüntüyü görmek için tıklatın](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](disabling-actions-during-animation-cs.md)
> [İleri](modifying-animations-from-the-server-side-cs.md)
