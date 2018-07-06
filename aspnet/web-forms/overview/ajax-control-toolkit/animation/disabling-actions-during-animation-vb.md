---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: (VB) animasyon sırasında eylemleri devre dışı bırakma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Ayrıca, eylem destekler...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 18c1eb74c73876b3fc6f1e37f69c31642806d043
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830885"
---
<a name="disabling-actions-during-animation-vb"></a>(VB) animasyon sırasında eylemleri devre dışı bırakma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Fare tıklamaları gibi eylemleri de destekler. Ancak bir fare tıklaması bir animasyonu başlattığında, fare tıklamasına animasyon sırasında devre dışı bırakmak için tercih edilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Fare tıklamaları gibi eylemleri de destekler. Ancak bir fare tıklaması bir animasyonu başlattığında, fare tıklamasına animasyon sırasında devre dışı bırakmak için tercih edilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

Böyle bir HTML düğmesi animasyonun uygulanır:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Bir geri gönderme oluşturmak için düğmeye istemiyorsanız bu yana bir HTML denetimini yerine Web denetimi kullanıldığını unutmayın; yalnızca bizim için istemci tarafı animasyonu başlatmak.

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

İçinde `<Animations>` düğümünün `<OnClick>` fare tıklatın işlemek için doğru öğedir. Ancak, animasyon sırasında de düğmeye tıkladı. `<EnableAction>` Öğesi dikkatli olun. Ayar `Enabled="false"` düğmeye animasyon bir parçası olarak devre dışı bırakır. (Düğme ve gerçek animasyonlarını devre dışı bırakma), birkaç bireysel animasyon kullandığımızdan `<Parallel>` öğesi birlikte tek bir tek animasyonları yapıştırmak için gereklidir. İçin tam biçimlendirmesi şöyledir `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Ayrıca düğmeyi sonra aşağıdaki XML öğesi listesinin sonunda kullanarak animasyon, yeniden etkinleştirmek mümkün olacaktır:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Ancak belirli bir senaryoda bu gereksiz düğmesidir yavaşça ve animasyon sonunda görünür değil.


[![Animasyon tamamlanmaz düğmesi devre dışıdır](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Animasyon tamamlanmaz düğmesi devre dışıdır ([tam boyutlu görüntüyü görmek için tıklatın](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](animating-in-response-to-user-interaction-vb.md)
> [İleri](triggering-an-animation-in-another-control-vb.md)
