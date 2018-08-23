---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: (C#) animasyon sırasında eylemleri devre dışı bırakma | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Ayrıca, eylem destekler...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4315f06ea1599bacb93a23a3759610e19754cfba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752946"
---
<a name="disabling-actions-during-animation-c"></a>(C#) animasyon sırasında eylemleri devre dışı bırakma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Fare tıklamaları gibi eylemleri de destekler. Ancak bir fare tıklaması bir animasyonu başlattığında, fare tıklamasına animasyon sırasında devre dışı bırakmak için tercih edilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Fare tıklamaları gibi eylemleri de destekler. Ancak bir fare tıklaması bir animasyonu başlattığında, fare tıklamasına animasyon sırasında devre dışı bırakmak için tercih edilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Böyle bir HTML düğmesi animasyonun uygulanır:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Bir geri gönderme oluşturmak için düğmeye istemiyorsanız bu yana bir HTML denetimini yerine Web denetimi kullanıldığını unutmayın; yalnızca bizim için istemci tarafı animasyonu başlatmak.

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

İçinde `<Animations>` düğümünün `<OnClick>` fare tıklatın işlemek için doğru öğedir. Ancak, animasyon sırasında de düğmeye tıkladı. `<EnableAction>` Öğesi dikkatli olun. Ayar `Enabled="false"` düğmeye animasyon bir parçası olarak devre dışı bırakır. (Düğme ve gerçek animasyonlarını devre dışı bırakma), birkaç bireysel animasyon kullandığımızdan `<Parallel>` öğesi birlikte tek bir tek animasyonları yapıştırmak için gereklidir. İçin tam biçimlendirmesi şöyledir `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Ayrıca düğmeyi sonra aşağıdaki XML öğesi listesinin sonunda kullanarak animasyon, yeniden etkinleştirmek mümkün olacaktır:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Ancak belirli bir senaryoda bu gereksiz düğmesidir yavaşça ve animasyon sonunda görünür değil.


[![Animasyon tamamlanmaz düğmesi devre dışıdır](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Animasyon tamamlanmaz düğmesi devre dışıdır ([tam boyutlu görüntüyü görmek için tıklatın](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](animating-in-response-to-user-interaction-cs.md)
> [İleri](triggering-an-animation-in-another-control-cs.md)
