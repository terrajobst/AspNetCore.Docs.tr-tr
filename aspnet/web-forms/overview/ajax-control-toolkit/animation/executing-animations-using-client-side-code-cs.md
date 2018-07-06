---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Animasyonları istemci tarafı kod (C#) kullanarak yürütme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyon yürütme...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: ae8c4df3c5852e9ee95f6184a86059859c56b128
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824564"
---
<a name="executing-animations-using-client-side-code-c"></a>Animasyonları istemci tarafı kod (C#) kullanarak yürütme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyon yürütme, özel istemci tarafı JavaScript kodu kullanarak da tetiklenebilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Animasyonun bir panel şuna benzer metin uygulanır:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Ardından, ekleme `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve bömesinde `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

İçinde `<Animations>` düğümü, kullanım `<OnClick>` animasyonlar kullanıcı bir kez çalıştırılacak panelinde tıklar. Parallelly yürütülecek iki animasyon ekleyin:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Gösterim amacıyla, bu animasyonu (ve Denetim Araç Seti kullanılarak oluşturulan animasyon) sayfanın çalıştırıldıktan sonra JavaScript kodu kullanarak yürütülür. İlk olarak erişim ihtiyacımız `AnimationExtender` denetimi. ASP.NET AJAX kitaplığı sağlar `$find()` işlevi bu görev için:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

`AnimationExtender` Denetim XML biçimlendirmede kullanılan olay işleyicilerine benzer adlarla yöntemleri dahil olmak üzere zengin bir API sunar: `OnClick()`, `OnLoad()`ve benzeri. Örneği için bir çağrı `OnClick()` yöntemini yürütür içinde animasyon `<OnClick>` öğesinin `AnimationExtender` denetimi:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

İşte sayfanın tam olarak yüklendikten sonra tıklayarak Panodaki öykünen tüm istemci tarafı JavaScript kod Not `pageLoad()` işlev adı ASP.NET AJAX ile bir kez sayfa çağrılan kullanılır ve dahil olan tüm JavaScript kitaplıkları yapıldı yüklendi.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![Animasyonun bir fare tıklaması hemen çalışır](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

Fare tıklatın olmadan animasyon hemen çalışır ([tam boyutlu görüntüyü görmek için tıklatın](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](modifying-animations-from-the-server-side-cs.md)
> [İleri](changing-an-animation-using-client-side-code-cs.md)
