---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: "Dinamik olarak UpdatePanel animasyonları (VB) denetleme | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. İçeriği için bir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 75b1f169724d8d3f8bcbc46198d320eeb8e7260c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Dinamik olarak kontrol eden UpdatePanel animasyonları (VB)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Bir UpdatePanel içeriği için özel bir genişletici yoğun animasyon Framework'te güvenen var: UpdatePanelAnimation. Ayrıca, UpdatePanel Tetikleyicileri birlikte de çalışabilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. İçeriği için bir `UpdatePanel`, özel bir genişletici yoğun animasyon Framework'te güvenen var: `UpdatePanelAnimation`. Ayrıca ile birlikte çalışabilir `UpdatePanel` tetikler.

## <a name="steps"></a>Adımlar

İlk adım her zamanki gibi eklemektir `ScriptManager` sayfasındaki ASP.NET AJAX kitaplığı yüklenir ve Denetim Araç Seti kullanılabilir:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

Bu senaryoda animasyon geçerli saati görüntüye uygulanacak. Bu bilgileri kullanarak bir etiket yazılabilir `Page_Load()` yöntemini veya (basitleştirmek amacıyla) aşağıdaki satır içi kod kullanılır:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Ayrıca, saati güncelleştiriliyor tetiklemek için bir düğmeye oluşturulur:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Bu kod içine konur `<ContentTemplate>` bölümünü bir `UpdatePanel` öğesi. Bölmenin `UpdateMode` özniteliği ayarlanmalıdır `"Conditional"`, yalnızca Tetikleyicileri bölmenin içeriği güncelleştirebilir beri. İçinde `<Triggers>` bölümünü `UpdatePanel`, zaman uyumsuz geri gönderme tetikleyici oluşturulur ve bağlı `Click` düğmesinin olayı. Bu nedenle, kullanıcı düğmesine tıklarsa `UpdatePanel` yenilenmez. İşaretleme için işte `UpdatePanel` denetimi:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Son olarak, `UpdatePanelAnimationExtender` yapılandırılmalıdır: ayarlamak `TargetControlID` özniteliği paneli Kimliğine ve bir animasyon genişletici içinde tanımlayın. İçinde yapar Soluklaşan iyi bir görsel Vurgu güncelleştirilmiş zamanında oluşturur algılama. Genişletici biçimlendirme sonra şu şekilde görünebilir:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Tarayıcıda dosyasını çalıştırın. Düğmesini tıklattığınızda, geçerli saati panelinde, her zaman bir saniye boyunca Soluklaşan gösterilir.


[![Geçerli saati Soluklaşan](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

Geçerli saati Soluklaşan ([tam boyutlu görüntüyü görüntülemek için tıklatın](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](animating-an-updatepanel-control-vb.md)
