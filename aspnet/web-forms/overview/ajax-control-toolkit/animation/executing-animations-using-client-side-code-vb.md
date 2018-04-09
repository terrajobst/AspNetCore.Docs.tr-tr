---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: İstemci tarafı kodlar (VB) kullanarak animasyonları yürütme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon yürütme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: e2b8c499edaccc70d439a576feb80e91cb28c1c7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="executing-animations-using-client-side-code-vb"></a>İstemci tarafı kodlar (VB) kullanarak animasyonları yürütme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

Animasyonun bir panel şöyle metin uygulanır:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnClick>` animasyonları kullanıcı bir kez çalıştırmayı panelde tıklar. Parallelly yürütülecek iki animasyon ekleyin:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Tanıtım amacıyla, bu animasyon (ve Denetim Araç Seti kullanılarak oluşturulan animasyon) sayfayı çalıştıran sonra JavaScript kodu kullanarak yürütülür. Erişim için öncelikle ihtiyacımız `AnimationExtender` denetim. ASP.NET AJAX kitaplığı sağlayan `$find()` işlevi bu görev için:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

`AnimationExtender` Denetim XML biçimlendirmede kullanılan olay işleyicileri aynı adlarla yöntemleri de dahil olmak üzere zengin bir API sunar: `OnClick()`, `OnLoad()`ve benzeri. Örneği için bir çağrı `OnClick()` yöntemini yürütür içinde animasyon `<OnClick>` öğesinin `AnimationExtender` denetimi:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

İşte panelindeki tıklatın sayfa tam yüklenmeden silindikten sonra öykünen tam istemci tarafı JavaScript kodu Not `pageLoad()` işlev adı, ASP.NET AJAX tarafından bir kez sayfası olarak adlandırılır kullanılır ve tüm kitaplıklar olmuştur JavaScript dahil yüklendi.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![Animasyonun bir fare tıklatma hemen çalışır](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

Animasyonun bir tıklatma olmadan hemen çalıştırır ([tam boyutlu görüntüyü görüntülemek için tıklatın](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](modifying-animations-from-the-server-side-vb.md)
> [sonraki](changing-an-animation-using-client-side-code-vb.md)
