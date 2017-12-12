---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: "Birbirini dışlayan onay kutularını (VB) oluşturma | Microsoft Docs"
author: wenz
description: "Radyo düğmeleri, genellikle bir seçenek kümesi yalnızca biri seçilebilir olduğunda kullanılır. Var olan bir dezavantajı ancak: bir radyo düğmesi grubundaki seçildikten sonra..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 023ca0b145de8147a98e78f4dba20846dc344f06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Birbirini dışlayan onay kutularını (VB) oluşturma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Radyo düğmeleri, genellikle bir seçenek kümesi yalnızca biri seçilebilir olduğunda kullanılır. Var olan bir dezavantajı ancak: bir radyo düğmesi grubundaki seçildikten sonra tüm radyo düğmeleri işaretini mümkün değildir. Onay kutuları herhangi bir zamanda denetlenmeyen olabilir, ancak birbirini dışlamaz. Bu öğreticide her iki yaklaşımın en iyi sağlar: onay kutularını, karşılıklı olarak birbirini dışlar.


## <a name="overview"></a>Genel Bakış

Radyo düğmeleri, genellikle bir seçenek kümesi yalnızca biri seçilebilir olduğunda kullanılır. Var olan bir dezavantajı ancak: bir radyo düğmesi grubundaki seçildikten sonra tüm radyo düğmeleri işaretini mümkün değildir. Onay kutuları herhangi bir zamanda denetlenmeyen olabilir, ancak birbirini dışlamaz. Bu öğreticide her iki yaklaşımın en iyi sağlar: onay kutularını, karşılıklı olarak birbirini dışlar.

## <a name="steps"></a>Adımlar

ASP.NET AJAX Denetim Araç Seti MutuallyExclusiveCheckBox genişletici içerir. Bu bir onay kutusu için bir grup adı atamak programcıları sağlar (`Key` özniteliği). Aynı gruptaki tüm onay kutularından tek seferde seçilebilir.

Yeni bir ASP.NET sayfasında iki onay kutularını koyma ile başlayalım. Daha fazla da olabilir, ancak ikisi ilkesini göstermek için yeterli:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Her iki onay kutularını MutuallyExclusiveCheckBoxExtender denetim sayfada konulmalıdır. Her iki anahtar öznitelik aynı değere, yalnızca HTML radyo düğmesi öğeleri özniteliklerini ait grup belirtmek için özdeş olması gereken değeri olması gerekir. Kimliği onay kutusunun genişletici noktalarına TargetControlID özelliği.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Son olarak, ASP.NET AJAX dahil `ScriptManager` ASP.NET AJAX Denetim Araç Seti tüm öğeleri tarafından gereklidir:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Kaydet ve sayfayı çalıştırın: denetleyin ve her iki onay kutularının işaretini hiçbir zaman her iki onay kutusu ancak denetlenebilir.


[![Aynı anda yalnızca bir onay kutusu denetlenebilir](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Yalnızca bir onay kutusu aynı anda kontrol ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](creating-mutually-exclusive-checkboxes-cs.md)
