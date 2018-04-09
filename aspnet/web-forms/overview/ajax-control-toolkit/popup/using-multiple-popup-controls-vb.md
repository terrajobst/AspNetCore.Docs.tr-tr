---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Birden çok açılan denetimler (VB) kullanarak | Microsoft Docs
author: wenz
description: AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. M kullanmak da mümkündür...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c57aab3ecf2c02a8488b5ea4e3e0ed33ac5e7fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="using-multiple-popup-controls-vb"></a>Birden çok açılan denetimler (VB) kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. Bir sayfa birden fazla açılan denetiminde kullanmak da mümkündür.


## <a name="overview"></a>Genel Bakış

AJAX Denetim araç setindeki PopupControl genişletici herhangi bir denetimi etkinleştirildiğinde popup tetiklemek için kolay bir yol sunar. Bir sayfa birden fazla açılan denetiminde kullanmak da mümkündür.

## <a name="steps"></a>Adımlar

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Ardından, açılan hizmet veren bir panel ekleyin. Geçerli senaryoda paneli içeren bir `Calendar` denetim. Takvim Geri göndermeler tarafından neden sayfa yenilenir önlemek için bölmenin içinde yerleştirileceği bir `UpdatePanel` denetimi:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

Sayfa iki metin kutusu da içerir. Metin kutusu etkinleştirildikten sonra her metin kutusu için Takvim açılan görünür.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Artık her iki metin kutuları ile genişletmek bir `PopupControlExtender`. `TargetControlID` Özniteliği için genişletici bağlı denetiminin Kimliğini sağlar. `PopupControlID` Özniteliği açılan paneli Kimliğini içerir. Bu durumda, her iki Extender'larının aynı panelini Göster, ancak farklı paneller de olasılığı vardır.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Şimdi bir metin alanı içinde tıklattığınızda, bir takvim bir tarih seçin olanak tanıyan alanın altında görüntülenir. (Seçilen tarih metin kutularına geri alamazsınız. farklı bir öğreticide ele alınacaktır.)


[![Takvim kullanıcı metin kutusuna tıkladığında görüntülenir](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Takvim kullanıcı metin kutusuna tıklattığında görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [sonraki](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
