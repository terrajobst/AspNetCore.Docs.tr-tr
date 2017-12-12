---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: "Bir animasyon listesi (VB) dışında çekme | Microsoft Docs"
author: wenz
description: "ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Framework de izin ver..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: dd2cfd512b03fa1d1f7754d9f86d080c1977695d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Bir animasyon listesi (VB) dışında çekme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Çerçeve ayrıca bazı JavaScript kodları değerlendirmesine bağlı olarak bir animasyon listesi dışında bir animasyon seçmek Programcı sağlar.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Çerçeve ayrıca bazı JavaScript kodları değerlendirmesine bağlı olarak bir animasyon listesi dışında bir animasyon seçmek Programcı sağlar.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

Animasyonun bir panel şöyle metin uygulanır:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu`runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnLoad>` sayfa tam yüklenmeden silindikten sonra animasyonları çalıştırmak için. Normal bir animasyon birini yerine `<Case>` öğesi oyuna gelir. Kendi SelectScript özniteliğinin değeri değerlendirilir; dönüş değeri sayısal olmalıdır. Bu sayı, içinde subanimations birini bağlı olarak &lt;durumda&gt; yürütülür. SelectScript 2 olarak değerlendirilirse, Denetim Araç Seti içinde üçüncü animasyon örneği için çalışan &lt;durumda&gt; (sayım 0 başlar).

Aşağıdaki biçimlendirmede üç subanimations tanımlar: genişliği yeniden boyutlandırma, yüksekliği yeniden boyutlandırma ve yavaş çıkış. JavaScript kodu (`Math.floor(3 * Math.random())`) üç animasyon birini çalışabilmesi 0 ve 2 ' arasında bir sayı seçer:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Olası üç animasyon birini: paneli daha geniş alır](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Olası üç animasyon birini: paneli daha geniş alır ([tam boyutlu görüntüyü görüntülemek için tıklatın](picking-one-animation-out-of-a-list-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](animation-depending-on-a-condition-vb.md)
[sonraki](animating-in-response-to-user-interaction-vb.md)
