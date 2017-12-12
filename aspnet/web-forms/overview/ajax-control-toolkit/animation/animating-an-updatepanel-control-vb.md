---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Bir UpdatePanel denetimi (VB) animasyon | Microsoft Docs
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. İçeriği için bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 0d1056fc798e22254e94e5cad54436576a297f7d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="animating-an-updatepanel-control-vb"></a>Bir UpdatePanel denetimi (VB) animasyon ekleme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bir UpdatePanel içeriği için özel bir genişletici yoğun animasyon Framework'te güvenen var: UpdatePanelAnimation. Bu öğretici, bu tür bir animasyon için bir UpdatePanel ayarlamak gösterilmiştir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. İçeriği için bir `UpdatePanel`, özel bir genişletici yoğun animasyon Framework'te güvenen var: `UpdatePanelAnimation`. Bu öğretici, bu tür bir animasyon için ayarlamak üzere gösterilmiştir bir `UpdatePanel`.

## <a name="steps"></a>Adımlar

İlk adım her zamanki gibi eklemektir `ScriptManager` sayfasındaki ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

Bu senaryoda animasyon bir ASP.NET uygulanacak `Wizard` web denetimi bulunan bir `UpdatePanel`. Üç (isteğe bağlı) adımı Geri göndermeler tetiklemek için yeterli seçenekleri sağlar:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

İşaretleme için gerekli `UpdatePanelAnimationExtender` denetimi için kullanılan biçimlendirme oldukça benzer `AnimationExtender`. İçinde `TargetControlID` sağladığımız özniteliği `ID` , `UpdatePanel` animasyon için; içinde `UpdatePanelAnimationExtender` denetimi `<Animations>` öğesi animation(s) için XML biçimlendirmesini tutar. Ancak bir fark yoktur: miktarını olayların ve olay işleyicileri comparison için sınırlı `AnimationExtender`. İçin `UpdatePanels`, yalnızca iki bunları var:

- `<OnUpdated>`UpdatePanel ne zaman güncelleştirildiğini
- `<OnUpdating>`Güncelleştirme UpdatePanel başladığında

Bu senaryoda, yeni içeriği `UpdatePanel` (sonra geri gönderme) belirerek. Bu, söz konusu gerekli biçimlendirme oluşur:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Geri gönderimin içinde UpdatePanel oluştuğunda artık yeni içeriği bölmenin yavaşça.


[![Sonraki sihirbaz adımı Soluklaşan](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

Sonraki sihirbaz adımı Soluklaşan ([tam boyutlu görüntüyü görüntülemek için tıklatın](animating-an-updatepanel-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](changing-an-animation-using-client-side-code-vb.md)
[sonraki](dynamically-controlling-updatepanel-animations-vb.md)
