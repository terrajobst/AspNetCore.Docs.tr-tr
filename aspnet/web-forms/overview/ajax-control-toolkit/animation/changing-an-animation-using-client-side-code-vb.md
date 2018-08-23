---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: İstemci tarafı kod (VB) kullanarak bir animasyonu değiştirme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Ayrıca animasyon...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: bfce413cd45daab62a247d596d9b51cb82cc7f97
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755340"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>İstemci tarafı kod (VB) kullanarak bir animasyonu değiştirme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyon, özel istemci tarafı JavaScript kodu kullanarak da değiştirilebilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyon, özel istemci tarafı JavaScript kodu kullanarak da değiştirilebilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

Animasyonun bir panel şuna benzer metin uygulanır:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

Gerçek animasyon tarafından bir HTML düğmesi başlatılır:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Unutmayın hiçbir `<Animations>` düğümünde `AnimationExtender` denetimi. Özel bir JavaScript kodu denetimiyle kullanılacak animasyonları sağlamak için kullanılır.

Sunucunun API ile `AnimationExtender`, animasyon için genişletici henüz atamak için kolay bir yolu yoktur. Ancak genişletici okuma ve yazma animasyonları çeşitli yöntemleri açığa kaydedilen çeşitli olayları (`OnClick`, `OnLoad`, vb.). Bazı örnekler şunlardır:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Dönüş değeri biçimi `get_*()` işlevler için bağımsız değişken biçimlerinin ve kimliklerinin `set_*()` işlevleri, bir nesne temsili, XML işaretlemesini ne olacağını sağlayan bir JSON dizesi. Şu anda, nesneyi geçirmek için bir yolu yoktur, ancak belirli bir animasyon bir nesne okumak mümkündür (`get_OnXXXBehavior()` yöntemleri).

Bir JSON dizesi İşte (sınırlandırma tırnak işaretleri olmadan düzgün şekilde biçimlendirilmiş) düğmesi tarafından tetiklenen animasyon temsil eden, ancak yeniden boyutlandırdıktan ve aynı anda yavaş çıkış panelinde animasyon ekleme:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

İçin bu JSON descripting aşağıdaki JavaScript kodunu atar `OnClick` geçerli genişletici animasyon ve çalıştırır:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![Fare tıklatın olmadan (ve çok az biçimlendirme) animasyon hemen çalışır](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

Fare tıklatın olmadan (ve çok az biçimlendirme ile) animasyon hemen çalışır ([tam boyutlu görüntüyü görmek için tıklatın](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](executing-animations-using-client-side-code-vb.md)
> [İleri](animating-an-updatepanel-control-vb.md)
