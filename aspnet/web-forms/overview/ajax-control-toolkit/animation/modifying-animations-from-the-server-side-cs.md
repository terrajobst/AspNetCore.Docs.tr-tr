---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Sunucu tarafı (C#) gelen animasyonları değiştirme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon de olabilir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869016"
---
<a name="modifying-animations-from-the-server-side-c"></a>Değiştirme animasyonları sunucu tarafındaki (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon, sunucu tarafında değiştirilebilir


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyon, sunucu tarafında değiştirilebilir

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

Animasyonun bir panel şöyle metin uygulanır:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Kodun kalan kısmı sunucu tarafında çalıştırır ve biçimlendirme kullanmaz; Bunun yerine, oluşturmak için kod kullanır `AnimationExtender` denetimi:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Ancak, Denetim Araç Seti, tek tek animasyon oluşturmak için API erişimini şu anda sağlamaz. Ancak ayarlamak olası `AnimationExtender`kullanıcının animasyonları dizeye içeren özellik animasyonları bildirimli olarak atarken kullanılan XML biçimlendirme. İçermemelidir XML oluşturmak için `<Animations>` .NET Framework'ün XML kullanabilir öğesi desteklemek veya, aşağıdaki kod olduğu gibi yalnızca bir dize sağlayın:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Son olarak ekleyin `AnimationExtender` geçerli sayfanın içinde kontrol `<form runat="server">` öğesi, animasyon bulunur ve çalıştırılır emin olun:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![Animasyonun sunucu tarafı C# /VB kodu kullanılarak oluşturulur](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

Animasyonun sunucu tarafı C# /VB kodu kullanılarak oluşturulan ([tam boyutlu görüntüyü görüntülemek için tıklatın](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](triggering-an-animation-in-another-control-cs.md)
> [sonraki](executing-animations-using-client-side-code-cs.md)
