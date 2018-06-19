---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: İstemci tarafı kodlar (VB) kullanarak animasyonun değiştirme | Microsoft Docs
author: wenz
description: ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyonun da yapabilirsiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f9b72576cc3a9e91827cfb40983821704621060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879159"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>İstemci tarafı kodlar (VB) kullanarak animasyonun değiştirme
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyonun, özel istemci tarafı JavaScript kodu kullanarak da değiştirilebilir.


## <a name="overview"></a>Genel Bakış

ASP.NET AJAX Denetim Araç Seti animasyon denetiminde bir denetimi ancak animasyonları için bir denetim eklemek için tam bir çerçeve değil. Animasyonun, özel istemci tarafı JavaScript kodu kullanarak da değiştirilebilir.

## <a name="steps"></a>Adımlar

İlk olarak dahil `ScriptManager` sayfasında; daha sonra ASP.NET AJAX kitaplığı, Denetim Araç Seti kullanmayı mümkün hale getirme yüklenir:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

Animasyonun bir panel şöyle metin uygulanır:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

İlişkili CSS sınıfı bölmesinin iyi arka plan rengi tanımlayın ve ayrıca sabit genişlikli bölmesinin ayarlayın:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

Gerçek animasyon bir HTML düğmesi tarafından başlatılır:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Ardından, ekleyin `AnimationExtender` sayfasına sağlayan bir `ID`, `TargetControlID` özniteliği ve zorunlu `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Unutmayın hiçbir `<Animations>` düğümde `AnimationExtender` denetim. Özel JavaScript kodu denetimiyle kullanılacak animasyonları sağlamak için kullanılır.

Sunucunun API ile `AnimationExtender`, animasyonun genişletici henüz atamak için kolay bir yolu yoktur. Genişletici okuyup animasyonları yazmak için çeşitli yöntemler ancak kullanıma kayıtlı olan çeşitli olaylar (`OnClick`, `OnLoad`, vb.). Bazı örnekler şunlardır:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Dönüş değerini biçimi `get_*()` işlevleri ve ilgili bağımsız değişken biçimi `set_*()` işlevleri XML Biçimlendirme ne olacağını, bir nesne temsili sağlayan bir JSON dizesi değil. Şu anda bir nesneyi içeri aktarmanız yolu yoktur, ancak bir nesne belirli bir animasyon okumak mümkündür (`get_OnXXXBehavior()` yöntemleri).

Bir JSON dizesinde İşte (sınırlandırma tırnak işaretleri olmadan ve düzgün şekilde biçimlendirilmiş) düğmesini tarafından tetiklenen bir animasyon temsil eden, ancak bunu yeniden boyutlandırma ve aynı anda yavaş çıkış paneli animasyon:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

Şu JavaScript kodunu için bu JSON descripting atar `OnClick` geçerli genişletici animasyon ve bunu çalıştırır:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![Fare olmadan (ve çok az biçimlendirme) animasyon hemen çalışır](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

Animasyonun bir tıklatma olmadan (ve çok az biçimlendirme) hemen çalıştırır ([tam boyutlu görüntüyü görüntülemek için tıklatın](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](executing-animations-using-client-side-code-vb.md)
> [sonraki](animating-an-updatepanel-control-vb.md)
