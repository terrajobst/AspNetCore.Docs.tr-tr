---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: İstemci tarafı kod (C#) kullanarak animasyonları yürütme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon yürütme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="executing-animations-using-client-side-code-c"></a>Yürütülen animasyonları kullanılarak istemci-tarafı kod (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Animasyonun bir panel şöyle metin uygulanır:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnClick>` animasyonları kullanıcı bir kez çalıştırmayı panelde tıklar. Parallelly yürütülecek iki animasyon ekleyin:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Tanıtım amacıyla, bu animasyon (ve Denetim Araç Seti kullanılarak oluşturulan animasyon) sayfayı çalıştıran sonra JavaScript kodu kullanarak yürütülür. Erişim için öncelikle ihtiyacımız `AnimationExtender` denetim. ASP.NET AJAX kitaplığı sağlayan `$find()` işlevi bu görev için:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

`AnimationExtender` Denetim XML biçimlendirmede kullanılan olay işleyicileri aynı adlarla yöntemleri de dahil olmak üzere zengin bir API sunar: `OnClick()`, `OnLoad()`ve benzeri. Örneği için bir çağrı `OnClick()` yöntemini yürütür içinde animasyon `<OnClick>` öğesinin `AnimationExtender` denetimi:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

İşte panelindeki tıklatın sayfa tam yüklenmeden silindikten sonra öykünen tam istemci tarafı JavaScript kodu Not `pageLoad()` işlev adı, ASP.NET AJAX tarafından bir kez sayfası olarak adlandırılır kullanılır ve tüm kitaplıklar olmuştur JavaScript dahil yüklendi.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![Animasyonun bir fare tıklatma hemen çalışır](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

Animasyonun bir tıklatma olmadan hemen çalıştırır ([tam boyutlu görüntüyü görüntülemek için tıklatın](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](modifying-animations-from-the-server-side-cs.md)
> [sonraki](changing-an-animation-using-client-side-code-cs.md)
