---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Sunucu tarafı (VB) tarafından animasyonları değiştirme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyonları da olabilir...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 5ff8685b44e7d0c2261ea7b9f1ca6397ae05ce53
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811172"
---
<a name="modifying-animations-from-the-server-side-vb"></a>Sunucu tarafı (VB) tarafından animasyonları değiştirme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyonları, sunucu tarafında değiştirilebilir


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetimi yalnızca bir denetim, ancak bir denetime animasyon eklemek için tam bir çerçeve değil. Animasyonları, sunucu tarafında değiştirilebilir

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; ardından, ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale yüklenir:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

Animasyonun bir panel şuna benzer metin uygulanır:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

İlişkili CSS sınıfı paneli için iyi bir arka plan rengi tanımlayın ve ayrıca panelinin sabit genişlikte ayarlayın:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Kodun geri kalanını sunucu tarafında çalışan ve biçimlendirme kullanmaz; Bunun yerine, oluşturmak için kod kullanır `AnimationExtender` denetimi:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Bununla birlikte, Denetim Araç Seti, tek tek animasyon oluşturmak için bir API erişimi şu anda sağlamaz. Ancak ayarlamak olası `AnimationExtender`projenin bir dizeye animasyon özelliğini kapsayan animasyonları bildirimli olarak atarken kullanılan XML biçimlendirmesi. Değil içermesi gereken bir XML dosyası oluşturmak için `<Animations>` .NET Framework'ün XML kullanabilirsiniz öğesi desteklemek veya, aşağıdaki kod olduğu gibi yalnızca dize sağlayın:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Son olarak, ekleme `AnimationExtender` içinde geçerli sayfaya, Denetim `<form runat="server">` öğesi, animasyon bulunur ve çalıştırılır emin olun:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![Animasyon sunucu tarafı C# /VB kod kullanarak oluşturulur.](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

Animasyon, sunucu tarafı C# /VB kod kullanarak oluşturulur ([tam boyutlu görüntüyü görmek için tıklatın](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](triggering-an-animation-in-another-control-vb.md)
> [İleri](executing-animations-using-client-side-code-vb.md)
