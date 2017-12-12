---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: "Eylemler (VB) animasyon sırasında devre dışı bırakma | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Ayrıca, eylem destekler..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 0f68d27f60e9b224a7cc0d598962553afeb3f28f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="disabling-actions-during-animation-vb"></a>Eylemler (VB) animasyon sırasında devre dışı bırakma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Fare tıklamaları gibi eylemleri de destekler. Ancak fare animasyonu başlattığında, fare tıklamaları animasyon sırasında devre dışı bırakmak için tercih edilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Fare tıklamaları gibi eylemleri de destekler. Ancak fare animasyonu başlattığında, fare tıklamaları animasyon sırasında devre dışı bırakmak için tercih edilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

Animasyonun bir HTML düğmesi bu gibi uygulanır:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Bir HTML denetimini geri gönderimin oluşturmak için düğmesini istemiyorsanız bu yana bir Web denetimi yerine kullanıldığını unutmayın; yalnızca istemci tarafında animasyonun bize başlatın.

Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

İçinde `<Animations>` düğümü `<OnClick>` fare tıklatma işlemeye sağ öğedir. Ancak, animasyon sırasında da düğmesine tıklanana. `<EnableAction>` Öğesi dikkatli olun. Ayarı `Enabled="false"` animasyonun bir parçası olarak düğmesini devre dışı bırakır. (Düğme ve gerçek animasyonlarını devre dışı bırakma), birkaç ayrı animasyon kullanıyoruz beri `<Parallel>` öğesi, tek bir animasyon birleştirerek bir Birleştirici için gereklidir. İçin tam biçimlendirme işte `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Ayrıca düğmesine listesinin sonuna aşağıdaki XML öğesi kullanarak animasyon sonra yeniden etkinleştirmek mümkün olacaktır:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Ancak verilen senaryoda bu düğmesi gereksiz olacaktır yavaşça ve animasyon sonunda görünür değil.


[![Animasyonun tamamlanır tamamlanmaz düğmesi devre dışı bırakılır](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Animasyonun tamamlanır tamamlanmaz düğmesi devre dışı ([tam boyutlu görüntüyü görüntülemek için tıklatın](disabling-actions-during-animation-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](animating-in-response-to-user-interaction-vb.md)
[sonraki](triggering-an-animation-in-another-control-vb.md)
